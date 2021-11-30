# Git 的一些简单用法

## **配置用户名和邮箱**

初次在本地使用 Git，要先配置用户名和邮箱，命令如下：
```
git commit --global user.name "用户名"
git commit --global user.email "邮箱"
```

## **日常执行命令**
工作环境中提交代码到本地仓库
```
git add .  // 有新文件提交时要先执行这句
git commit -m "日志"
```

本地仓库同步到远程仓库的分支上（可选）
```
git push [remote] [branch]
```

远程仓库同步更新到工作环境
```
git pull [remote] [branch]
```

合并指定分支到当前分支
```
git merge [branch]
```

切换分支
```
git checkout [branch]
```

显示有变更的文件
```
git status
```

显示当前分支的版本历史
```
git log
```

恢复暂存区的指定文件到工作区
```
git checkout [file]
```

恢复暂存区的所有文件到工作区
```
git checkout .
```

下载远程仓库的所有变动到本地仓库
```
git fetch [remote]
```

追加提交
```
git add [file]
git commit --amend
git push --force
```

删除最后一次提交(不保留痕迹)
```
git reset --hard HEAD^
git push origin [branch] -f
```

## .gitignore 忽略特殊文件
有些不必要的，如编译后产生的文件不需要提交上仓库中，可以使用 .gitignore 来忽略这些文件的提交

新建一个 .gitignore 文件，在 windows 改名为 .gitignore 会提示你必须输入文件名，这时可以在文本编辑器里'保存'或'另存为'就可以把文件保存为 .gitignore 了

忽略特殊文件或文件夹的写法:
```
# 忽略文件
*.后缀名

# 忽略文件夹
文件夹名

```

## **一些常见错误及解决方法**

**问题**

使用 git push 的时候报错 failed to push some refs to 'github.com....'

**解决方法**

这是因为远程仓库有更新，要先更新再提交，以下命令：
```
git pull --rebase
git push
```

**问题**

使用 git checkout 切换分支的时候报错 "error: cannot stat 'file': Permission denied"

**解决方法**

据说这个错误只是在 Windows 下发生，应该是有什么其它应用程序打开了这个文件。所以试着关闭编辑器、任务管理器、命令行或者 FTP 服务之类的，百试百灵，总能解决问题

**问题**

git add 的时候报 "LF will be replaced by CRLF" 的警告

**解决方法**

这是因为在 Windows 中的换行符为 CRLF，而在 Linux 中的换行符为 LF。在 git 创建的项目中换行符为 LF，而执行 git add 操作时，系统会提示 LF 将被自动转为 CRLF。所以只需禁止 git 的自动转换就可以了：
```
git config --global core.autocrlf false
```
