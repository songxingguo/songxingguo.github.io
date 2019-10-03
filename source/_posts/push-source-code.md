title: 将博客源码推送到github上
author: songxingguo
tags:
  - github
  - Hexo
  - 博客搭建
categories:
  - 开发者手册
date: 2018-06-18 15:49:00
---
- ### 前提

   已经搭建了一个Hexo博客
  
- ### 背景
  
   &emsp;&emsp;使用命令 `Hexo d` 推送到github上的是Hexo生成的静态文件，而如果我们 换一台电脑就无法编辑博客了，所以我们需要把Hexo博客的源码推送到github上，方便我们在其他电脑上也能进行博客的编辑。也保证博客文章不会丢失。(但需要注意的是github上的仓库是开源的，也就是说所有人的都可以看到你的源代码）

 <!-- more -->

- ### 新建source-code分支

 - github上的博客仓库新建 **source-code** 分支
    ![新建分支](https://graphbed.qiniu.songxingguo.com/%E6%96%B0%E5%BB%BA%E5%88%86%E6%94%AF.png)
  
 - 将 **source-code** 设置为默认分支
    ![设置为默认分支](https://graphbed.qiniu.songxingguo.com/%E9%BB%98%E8%AE%A4%E5%88%86%E6%94%AF.png)
  
- ### 将源代码推送到 **source-code** 分支上
 - 初始化git仓库
  ```
  git init
  ```
 - 关联远程仓库
  ```
  git remote add origin https://github.com/songxingguo/hexoblog.github.io.git
  ```
 - 从远程分支拉取代码,并查看远程分支
 ```
 git pull
 ```
 - 查看远程分支
 ```
 git branch --remote
 ```
 - 查看本地分支
 ```
 git branch
 ```
 - 检出远程分支中的 **source-code** 分支
 ```
 git checkout origin/source-code
 ```
 - 进入博客根路径，然后执行下面代码
  ```
  git status 
  git add -A
  git commit -m "source code"
  git push
  ```

 - 源码推送到了github的 **source-code** 分支上了
