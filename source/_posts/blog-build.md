title: 博客搭建
author: songxingguo
tags:
  - Hexo
  - 博客搭建
categories:
  - 开发者手册
date: 2018-06-02 17:11:00
---
### 前言

大三的我，做过项目，平时也看了不少的书，但是能记住却少之有少。于是我希望通过博客将学过的知识整理下来，形成自己的知识体系。在今年上半年开始就打算搭建博客，在网上百度了各种资料，找到博客三种搭建方式: Wordpress、Jekyll、Hexo。其中Wordpress是需要服务器的，作为学生党的我当然没钱买服务器了，于是果断放弃了Wordpress,那就再看看Jekeyll吧，就来到了[Jekyll的官网](https://www.jekyll.com.cn) ，看了一下是不需要服务器的 ，那就撸起袖子开干吧，按着官方的教程往下弄，但Jekyll是设计是基于Ruby的，而我又没有Ruby的环境，那就顺理成章的来到了[Ruby的官网](http://www.ruby-lang.org/en/downloads/)找到了Windows系统的[下载连接](https://rubyinstaller.org/downloads)。但说起最气的就是安装Ruby了，下载一个只有8兆左右的却怎么也下载不下来，下载了一半结果进度就不动了，接着试了几次，都没下载下来。那个真是把我气的那是不要不要的。这东西光是下载下来就花了我接近一个小时。最后在千辛万苦下终于还是弄出来了一个[Jekeyll的博客](http://blog.songxingguo.com)。当然了，前面的说所的一切都是我们今天的主角Hexo做铺垫的。下面我们正式进入正题吧。

<!-- more -->

### 大纲

   - [Hello Wrold](https://www.songxingguo.com/2018/05/18/hello-world/)
   - [博客部署到Github Pages](https://www.songxingguo.com/2018/06/06/%E5%AE%A2%E9%83%A8%E7%BD%B2%E5%88%B0github-pages/)
   - [更换主题](https://www.songxingguo.com/2018/06/06/hexo-theme/)
   - [可视化编写文档]()
   - [将源代码推送到github相同仓库的不同分支](https://www.songxingguo.com/2018/06/06/deploy-github-pages/)
   - [使用Travis CI 进行自动发布代码](https://www.songxingguo.com/2018/05/19/TravisCI/)
   - 将博客推送给百度
   - [使用Coding作为国内访问站点](https://www.songxingguo.com/2018/05/19/TravisCI/)
   - 实现windows开机启动Hexo 服务
   - [图床七牛云](https://www.songxingguo.com/2018/12/20/qiniu/)
   
### 优秀教程

- #### 统计访问量

  [Hexo搭建博客系列：（五）Hexo添加不蒜子和LeanCloud统计无标题文章](https://www.jianshu.com/p/702a7aec4d00)
 
- #### gh-pages 展示项目

  [如何用Github的gh-pages分支展示自己的项目](https://www.cnblogs.com/MuYunyun/p/6082359.html)
 
- #### Travis CI 自动部署

  [使用 Travis CI 自动更新 GitHub Pages](https://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)
  
- #### 阅读量统计

 [为NexT主题添加文章阅读量统计功能](https://notes.doublemine.me/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud)
 
 [Hexo个人免费博客(三) next主题、评论、阅读量统计和站内搜索](https://blog.csdn.net/linshuhe1/article/details/52424573)
 
 [百度统计官网](https://tongji.baidu.com/web/welcome/login)
