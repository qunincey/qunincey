---
title: git知识总结
date: 2020-05-21
tags:
 - git
categories:
 - other
---

# git相关知识总结

## git add

git add .：将工作空间新增和被修改的文件添加的暂存区   
git add -u:将工作空间被修改和被删除的文件添加到暂存区(不包含没有纳入Git管理的新增文件)   
git add -A：将文件的修改，文件的删除，文件的新建，添加到暂存区。

## git重命名文件

 git mv 旧文件名 新文件名 eg:  git mv readme Rename 
 实际操作过可以直接改文件名，然后走add  commit流程效果是一样的。


## git log查看日志文件

git log --oneline 把日志简化后显示
git log -n3 显示最近的3个提交信息  n后面可加任何数字
git log -n3 --oneline 最近的3个提交信息  简化显示
git log --all 查看所有分支的提交信息
git log --graph 图形化提交信息

## gitk
gitk可视化工具