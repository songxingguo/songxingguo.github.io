title: Hello World
tags:
  - Hexo
categories:
  - 博客搭建
date: 2018-05-18 23:45:00
---
# 快速开始

- ### 前提
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：

 [Node.js](https://nodejs.org/en/)

 [Git](https://git-scm.com/)

 Windows系统

- ### 安装hexo脚手架

 打开命令行工具，全局安装hexo脚手架
  ```
   npm install -g hexo-cli
  ```
  <!-- more -->

 安装成功：
  ![安装成功](https://graphbed.qiniu.songxingguo.com/pasted-0.png)

- ### 新建博客

 进入项目存放的目录，执行代码：
  ```
   hexo init hexo-blog
   cd hexo-blog
   npm install
  ```
  hexo-blog 为博客写项目的名称

 文件目录：
 ![文件目录](https://graphbed.qiniu.songxingguo.com/%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95.png)

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
   1. _config.yml 博客配置信息。

   2. package.json 应用程序的信息。

   3. scaffolds 模版文件夹。

   4. source 资源文件夹是存放用户资源的地方。

   5. themes 主题文件夹。Hexo 会根据主题来生成静态页面。


- ### 启动服务
  代码
  ``` 
   hexo server
  ```
   或
  ``` 
   hexo s
  ```
 运行命令
 ![启动服务](https://graphbed.qiniu.songxingguo.com/%E5%9C%A84000%E7%AB%AF%E5%8F%A3%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1.png)

 访问端口 [http://localhost:4000/](http://localhost:4000/)

 ![预览效果](https://graphbed.qiniu.songxingguo.com/%E5%9C%A84000%E7%AB%AF%E5%8F%A3%E9%A2%84%E8%A7%88.png)