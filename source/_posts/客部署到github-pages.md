title: 博客部署到Github Pages
author: songxingguo
date: 2018-06-06 21:43:23
tags:
---
- ### 前提

 拥有github账号
 已经搭建了一个Hexo博客
 
- ### 前景知识
 Github Pages是一个静态页面服务器，可以用来展示托管在gitbub仓库的静态页面。
 
 Github Pages 提供了三种方式展示静态页面。
 
  * 仓库以 ** xx.github.io ** 命名
  * 仓库创建gh-pages分支
  
- ### 创建仓库

 登录github账号,点击**New repository**
 
 ![登录github](http://p9myzkds7.bkt.clouddn.com/%E7%99%BB%E5%BD%95github.png)

 创建仓库
 
 ![创建仓库](http://p9myzkds7.bkt.clouddn.com/%E5%88%9B%E5%BB%BA%E4%BB%93%E5%BA%93.png)
 
创建成功

![创建成功](http://p9myzkds7.bkt.clouddn.com/%E5%88%9B%E5%BB%BA%E6%88%90%E5%8A%9F.png)

- ### 修改博客配置信息 config.yml

打开配置文件
![打开配置文件](http://p9myzkds7.bkt.clouddn.com/%E6%89%93%E5%BC%80%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.png)

将代码拉到最后找到**deploy**修改成下面内容:
```
deploy:
  type: git
  repository: https://github.com/songxingguo/hexoblog.github.io.git
  branch: master
```
repository为上面创建的仓库地址。

修改后,保存
![修改后](http://p9myzkds7.bkt.clouddn.com/%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.png)

打开命令行，进入博客项目目录

![进入项目目录](http://p9myzkds7.bkt.clouddn.com/%E8%BF%9B%E5%85%A5%E7%9B%AE%E5%BD%95.png)

初始化git 
```
git init 
```

安装hexo-deployer-git

```
npm install hexo-deployer-git --save
```
![安装hexo-deployer-git](http://p9myzkds7.bkt.clouddn.com/hexogit%E5%AE%89%E8%A3%85.png)

然后依次执行下面代码：

清除缓存文件 (db.json) 和已生成的静态文件 (public)
```
$ hexo clean 
```
生成静态文件
```
$ hexo g
```
部署网站
```
$ hexo d
```
