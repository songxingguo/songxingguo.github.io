title: 博客部署到Github Pages
author: songxingguo
tags:
  - github
  - Hexo
  - ''
categories:
  - 博客搭建
date: 2018-06-06 21:43:00
---
- ### 前提

 拥有github账号
 已经搭建了一个Hexo博客

- ### 前景知识
 Github Pages是一个静态页面服务器，可以用来展示托管在gitbub仓库的静态页面。

 Github Pages 提供了三种方式展示静态页面。

  * 仓库以 ** xx.github.io ** 命名
  * 仓库创建gh-pages分支

 下面以第一种方式推送到Gitbub Pages

<!-- more-->

- ### 创建仓库

 登录github账号,点击**New repository**

 ![登录github](https://graphbed.qiniu.songxingguo.com/%E7%99%BB%E5%BD%95github.png)

 创建仓库, 输入名称 **hexoblog.github.io**

 ![创建仓库](https://graphbed.qiniu.songxingguo.com/%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93.png)

 点击 **Create repository** ,创建成功

 ![创建成功](https://graphbed.qiniu.songxingguo.com/%E5%88%9B%E5%BB%BA%E6%88%90%E5%8A%9F.png)

- ### 修改博客配置信息 config.yml

 打开配置文件

 ![打开配置文件](https://graphbed.qiniu.songxingguo.com/%E6%89%93%E5%BC%80%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.png)

 将代码拉到最后找到**deploy**修改成下面内容:
```
deploy:
  type: git
  repository: https://github.com/songxingguo/hexoblog.github.io.git
  branch: master
```
 repository : 上面创建的仓库地址。
 branch : 提交到仓库的那个分支

 修改后,保存

 ![修改后](https://graphbed.qiniu.songxingguo.com/%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.png)

- ### 部署博客

 打开命令行，进入博客项目目录

 ![进入项目目录](https://graphbed.qiniu.songxingguo.com/%E8%BF%9B%E5%85%A5%E7%9B%AE%E5%BD%95.png)

 安装hexo-deployer-git
```
npm install hexo-deployer-git --save
```
 ![安装hexo-deployer-git](https://graphbed.qiniu.songxingguo.com/hexogit%E5%AE%89%E8%A3%85.png)

 然后依次执行下面代码：

 * 清除缓存文件 (db.json) 和已生成的静态文件 (public)
  ```
  $ hexo clean 
  ```
 * 生成静态文件
  ```
  $ hexo g
  ```
 * 部署网站
  ```
  $ hexo d
  ```
 出现下面界面就部署成功了

 ![部署 成功](https://graphbed.qiniu.songxingguo.com/%E9%83%A8%E7%BD%B2%E5%AE%8C%E6%88%90.png)

- ### 修改Github Pages设置

 进入github查看部署的代码

 ![进入github查看](https://graphbed.qiniu.songxingguo.com/%E6%9F%A5%E7%9C%8BgithubHexoBlog%E4%BB%A3%E7%A0%81.png)

 点击 **setting**

 ![点击setting](https://graphbed.qiniu.songxingguo.com/%E7%82%B9%E5%87%BBSetting.png)

 然后往下拉，找到 **Github Pages**

 ![找到Github pages](https://graphbed.qiniu.songxingguo.com/%E6%89%BE%E5%88%B0githubpages.png)

 选择 **master branch** 之后保存

 ![选择master branch](https://graphbed.qiniu.songxingguo.com/%E7%82%B9%E5%87%BBmaster%E4%BF%9D%E5%AD%98.png)

 点击 **Github Pages**上的链接，查看博客效果

 ![点击链接](https://graphbed.qiniu.songxingguo.com/%E7%82%B9%E5%87%BB%E9%93%BE%E6%8E%A5.png)

 ![查看博客效果](https://graphbed.qiniu.songxingguo.com/%E6%9F%A5%E7%9C%8B%E5%8D%9A%E5%AE%A2%E6%95%88%E6%9E%9C.png)