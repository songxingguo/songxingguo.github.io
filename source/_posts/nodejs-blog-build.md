title: NodeJS博客搭建
author: songxingguo
tags:
  - Node
  - 博客搭建
categories:
  - 开发者手册
date: 2018-06-13 11:14:00
---
### pm2启动服务

 [pm2官网](http://pm2.keymetrics.io/docs/usage/cluster-mode/)

- 安装pm2

 ```
 npm install -g pm2
 ```
 [使用 PM2 管理nodejs进程](https://www.cnblogs.com/liusixin/p/7007340.html)
 
- 启动服务

  配置pm2.json文件
  
  ```
  {
	"name": "dynablog.songxingguo.com",
	"script": "bin/www",
	"log_date_format": "YYYY-MM-DD HH:mm Z",
	"exec_mode": "cluster",
	"instances": 2,
	"error_file": "logs/stderr.log",
	"out_file": "logs/stdout.log"
  }
  ```
  加载pm2.json文件
  
  ```
  pm2 reload pm2.json
  ```
  或
  ```
  pm2 start pm2.json
  ```


 [常用命令](https://www.cnblogs.com/jtnote/p/6230720.html)
