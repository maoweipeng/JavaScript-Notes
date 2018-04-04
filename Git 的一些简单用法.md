# Git 的一些简单用法

### **配置用户名和邮箱**

初次在本地使用 Git，要先配置用户名和邮箱，命令如下：
```
git commit --global user.name "用户名"
git commit --global user.email "邮箱"
```

### **日常执行命令**
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




