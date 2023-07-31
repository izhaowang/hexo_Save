---
title: git
tags:
  - git
categories:
  - web前端
  - git
date: 2023-05-09 21:39:43
---

# 分支
## 查看本地分支
+ git branch 
+ git branch -r 查看远程分支
+ git branch -a 查看远程分支和本地分支
+ git checkout  name 创建分支
+ git checkout -b 创建分支并切换到该分支上
+ git merge name 合并分支
+ git branch -d 删除本地分支 删除全会检查merge状态
+ git branch -D 是git branch --delete --force 的简写
+ git push --delete origin name 删除远程分支
+ 当你在工作区域修改了内容，此时想在其他分支上解决小问题时。可以使用git stash 。 然后切换到其他分支上解决了小问题后。然后回到你的分支上。用git stash pop 还原你的修改内容
+ 当主分支的bug修改过后。 你想在你的个人分支也想同步一下这些bug的修改代码时，你可以使用git cherry-pick <commit-id>

# git 配置别名 
如果敲git st就表示git status那就简单多了，当然这种偷懒的办法我们是极力赞成的。 我们只需要敲一行命令，告诉Git，以后st就表示status：会将修改保存到~/.gitconfig里
/*
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
*/
