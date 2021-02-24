---
title: Git常用命令速查
tags:
  - git
id: '1137'
categories:
  - - 周边技术
date: 2016-08-26 15:49:22
---

[![git](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/08/git.jpeg)](http://qxu2059920095.my3w.com/blog/wp-content/uploads/2016/08/git.jpeg)git clone http://..git #复制远程库下来
git remote rm orgin #断开远程库

git init #初始化init
git remote add http://..git #添加远程库连接
git push -u origin master #推送到远程库，第一次加 -u
git pull #拉去远程库更新

ssh-keygen -t rsa -C "email@163.com" #生成ssh key

git log # 日志
git log --graph --pretty=oneline --abbrev-commit #日志，图形化，只显示一行, commit\_id缩写
git status #当前状态

git add #添加到暂存去，空文件夹不行
git commit -m "message"

git reset --hard HEAD^ #回退到上个版本， --hard 是强制同步修改工作区的文件
git reflog #版本操作的编号
git reset --hard <comond\_id> #版本回退后还能恢复

git reset HEAD <file> #从暂存区移除文件
git checkout -- <file> #撤销工作区文件的修改


git branch -b dev #新建并切换到dev分支
git branch dev #新建dev分支
git checkout dev #切换到dev分支

git branch -d dev 删除dev分支

git merge --no-ff -m "message" dev #合并分支dev到当前 带no-ff，删除分支后依然可以看到log历史记录


git stash #保存现场
git stash -u #强制保存现场，包括未add追踪的文件
git stash pop #恢复现场，并删除stash的记录
git stash list #查看stash


git tag v0.1 #创建标签，默认HEAD
git tag v0.2 354qwe4 #可指定commit\_id来创建
git push origin v0.1 #推送标签到远程
git push origin --tag #推送所有未推送的标签到远程
git tag -d v0.1 #删除标签
git push origin :refs/tags/v0.1 #将删除的标签同步删除到远程


git config --global alias.st status #全局别名，git st = git status
#超强log，别名lg
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset
-%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"