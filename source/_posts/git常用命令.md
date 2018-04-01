title: git常用命令
author: songxingguo
tags:
  - git
categories:
  - git
date: 2018-03-25 15:25:00
---
查看状态
```bash
$ git status
```
把项目添加到仓库
```bash
$ git add 【-A】|【文件名称】|$ git add .
```
提交代码
```bash
$ git commit -m "文件描述"
```
推送到远程仓库
```bash
$ git push 【remote orgin mater】
```
将本地仓库和远程仓库进行关联
```bash
$ git remote add origin https://github.com/guyibang/TEST2.git
```
把本地仓库的项目推送到远程仓库
```bash
git push -u origin master
```