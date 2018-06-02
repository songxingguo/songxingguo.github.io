title: 从零开始搭建一个Hexo博客
id: 1527000654062
author: songxingguo
tags:
  - Hexo
  - github
  - ''
categories:
  - 博客搭建
date: 2018-05-22 22:52:00
---
### 简介

 大三的我，做过项目，平时也看了不少的书，但是能记住却少之有少。于是我希望通过博客将学过的知识整理下来，形成自己的知识体系。在今年上半年开始就打算搭建博客，在网上百度了各种资料，找到博客三种搭建方式: Wordpress、Jekyll、Hexo。其中Wordpress是需要服务器的，作为学生党的我当然没人买服务器了，于是果断放弃了Wordpress,那就再看看Jekeyll吧，就来到了[Jekyll的官网](https://www.jekyll.com.cn) ，看了一下是不需要服务器的 ，那就撸起袖子开干吧，按着官方的教程往下弄，但Jekyll是设计是基于Ruby的，而我又没有Ruby的环境，那就顺理成章的来到了[Ruby的官网](http://www.ruby-lang.org/en/downloads/)找到了Windows系统的[下载连接](https://rubyinstaller.org/downloads)。但说起最气的就是安装Ruby了，下载一个只有8兆左右的却怎么也下载不下来，下载了一半结果进度就不动了，接着试了几次，都没下载下来。那个真是把我气的那是不要不要的。这儿东西光是下载下来就花了我接近一个小时。最后在千辛万苦下终于还是弄出来了一个[Jekeyll的博客](http://blog.songxingguo.com)。当然了，前面的说所的一切都是我们今天的主角Hexo做铺垫的。下面我们正式进入正题吧。
 
<!-- more -->
 
### 什么是Hexo

[Hexo官网](https://hexo.io/zh-cn/docs/)

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 安装

#### 安装前提
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
[Node.js](https://nodejs.org/en/)
[Git](https://git-scm.com/)

接下来只需要使用 npm 即可完成 Hexo 脚手架的安装。
```bash
 $ npm install -g hexo-cli
```
效果：
![安装成功](http://p9myzkds7.bkt.clouddn.com/pasted-0.png)

### 建站

请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
$ hexo init hexo-blog
$ cd hexo-blog
$ npm install
```

新建完成后，指定文件夹的目录如下：

```bash
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

##### _config.yml

网址配置信息。

##### package.json

应用程序的信息。

##### scaffolds

模版 文件夹。

##### source

资源文件夹是存放用户资源的地方。

##### themes

主题 文件夹。Hexo 会根据主题来生成静态页面。


未完待续。。。