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

<br />

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
