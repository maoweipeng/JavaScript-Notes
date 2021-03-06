# Node.js 项目部署到服务器

---
### **配置好基础工具**
先把 yum 更新到最新版本
```
yum -y update
```

安装一组用来编译源代码的开发工具，因为我们要通过编译 Node.js 源代码的方式来安装
```
yum -y groupinstall "Development Tools"
```

### **安装 Node.js**
先进入 /usr/src 文件夹，这个文件夹通常用来存放软件源代码，然后从 nodejs.org 中获取压缩源文件，截止目前2018.4.12，Nodejs的最新稳定版是 v9.10.1
```
wget http://nodejs.org/dist/v9.10.1/node-v9.10.1.tar.gz
```

下完解压压缩文件，并且进入到压缩后的文件夹中

```
tar zxf node-v9.10.1.tar.gz
cd node-v9.10.1
```

执行配置脚本进行编译预处理
```
./configure
```

开始编译源代码
```
make
```

编译完成后，安装
```
make install
```

安装玩 Node.js 后就开始部署一些必要模块，如 Express （我项目中用到的框架） 和 forever （一个非常有用的模块，可以确保应用程序一直在服务器上跑，而且重启也很方便）
```
npm -g install express forever
```

建立超级链接，不然 sudo node 时会报 "command not found"
```
sudo ln -s /usr/local/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/node /usr/lib/node
sudo ln -s /usr/local/bin/npm /usr/bin/npm
sudo ln -s /usr/local/bin/node-waf /usr/bin/node-waf
sudo ln -s /usr/local/bin/forever /usr/bin/forever
```

### **安装 mongodb** （非必须，如果你项目有用到就安装）
进入 /usr/local 文件夹后，下载 mongodb 源码
```
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-amazon-3.6.3.tgz
```

解压安装包，重命名文件夹为 mongodb
```
tar zxvf mongodb-linux-x86_64-amazon-3.6.3.tgz
mv mongodb-linux-x86_64-amazon-3.6.3 mongodb
```

在 var 文件夹里面建立 mongodb 文件夹，并分别建立文件夹 data 用于存放数据，logs用于存放日志
```
mkdir /var/mongodb
mkdir /var/mongodb/data
mkdir /var/mongodb/logs
```

打开 rc.local 文件，添加 CentOS 开机启动项
```
vim /etc/rc.d/rc.local
```

将 mongodb 启动命令追加到本文件中，让 mongodb 开机自启动
```
/usr/local/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log -fork
```

启动 mongodb
```
/usr/local/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log -fork
```

看到如下信息说明启动成功
```
forked process: 18394
all output going to: /var/mongodb/logs/log.log
```

上传代码用 ftp、svn 或者 git，喜欢哪个用哪个

### **启动应用**
进入应用目录，开启服务
```
sudo forever start app.js
```

检查运行中的服务
```
sudo forever list
```

关闭应用
```
sudo forever stop id
```

至此，项目部署成功

### **使用 Github 管理服务器的项目**
之前更新服务器上的项目的方法是，在 Windows 上开发，然后用 FTP 上传项目到服务器上，然后更新应用，这样做比较麻烦，不太好管理项目，每次更新要么就是把整个项目删掉然后替换一个新的项目，要么就是自己手动一个个替换那些修改过的文件。无论怎样，这种做法都是不好的，所以使用 Git 或 svn 这类版本控制工具无疑是较好的选择，以下以 Github 为例

首先先安装个 git
```
sudo yum install git
```

默认都有 Github 账号，并且选择 ssh 的连接方式，所以要生成 ssh key:
```
ssh-keygen -t rsa -C "your_email@youremail.com"
```
没特殊情况的话一直回车就好

成功生成 ssh 密钥对，默认路径是 /root/.ssh/，打开 /root/.ssh/id_rsa.pub，把里面的公钥复制一下，登录自己的 Github，Setting -> SSH and GPG keys -> new SSH key。把公钥粘贴上去，title 随便写，Add 一下就成功添加了

回到服务器上，输入：
```
ssh -T git@github.com
```

如果显示
```
Hi XXX! You've successfully authenticated, but GitHub does not provide shell access.
```

就说明成功建立连接，有些情况会出现以下错误：
```
no such identity: /root/.ssh/github_rsa: No such file or directory
Permission denied (publickey).
```

这个错误是 SSH 配置文件不匹配导致的，打开 /root/.ssh/config，可以看到 "identityFile = ~/.ssh/github_rsa"，然鹅，你压根没这个文件，而是有 id_rsa，所以改为以下配置就好了：
```
identityFile = ~/.ssh/id_rsa
```

接下来就是走老规矩路线配置 username 和 email
```
github config --global user.name "your name"
github config --global user.email "your email"
```

把项目上传到 github 上，然后在服务器克隆下来。以后的开发维护过程就是，在工作终端上修改代码，提交到 github，服务器更新代码，重启服务，OK

### **其它补充**

**Linux 下升级 Node.js**

检查当前版本号
```
node -v
```

清除 npm cache
```
sudo npm cache clean -f
```

安装 n 模块
```
sudo npm install -g n
```

升级到最新稳定版本
```
sudo n stable
```

安装二进制链接（注意路径）
```
sudo ln -sf /usr/local/n/versions/node/6.0.0/bin/node /usr/bin/node
```

或者用前面说的方法，下载 Nodejs 最新的源代码，编译安装

**npm 安装模块出错：Error: SSL Error: CERT_UNTRUSTED**

在安装 Express 和 forever 模块的那一步报 SSL 错误，可以使用以下命令绕过 https：
```
npm config set strict-ssl false
```

或者从https或http设置注册表URL，如下：
```
npm config set registry="http://registry.npmjs.org/"
```

当然，绕过 https 不是真正的解决方法，但是能很好地解决问题

后续研究如何让 nodejs 和 nginx 协同工作












#
