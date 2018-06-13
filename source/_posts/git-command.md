title: git常用命令
author: songxingguo
tags:
  - git
categories:
  - 常用命令
  - ''
date: 2018-03-25 15:25:00
---
查看状态
```git
$ git status
```
把项目添加到仓库
```git
$ git add 【-A】|【文件名称】|$ git add .
```
提交代码
```git
$ git commit -m "文件描述"
```
<!-- more -->

推送到默认分支
```git
$ git push
```
推送到远程分支
```git
$ git push origin master
```
将本地仓库和远程仓库进行关联
```git
$ git remote add origin https://github.com/guyibang/TEST2.git
```
把本地仓库的项目推送到远程仓库
```git
$ git push -u origin master
```
移除远程仓库
```git
$ git remote rm origin
```
查看远程分支
```git
$ git branch --remote
```