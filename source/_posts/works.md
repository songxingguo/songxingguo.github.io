title: 作品展示搭建
author: songxingguo
tags: []
categories: []
date: 2018-08-02 09:01:00
---
# 作品展示搭建

## 生成项目

```
npm install koa-generator -g
koa2 works
```

## 系统架构

前端： HTML5、CSS3、JavaScript
后端： Koa

## 写在前面

系统中已经安装了 node.js 。

## node.js 应用组成

- 引入required模块：我们可以使用 require 指令来载入 Node.js 模块。
- 创建服务器：服务器可以监听客户端的请求。
- 接收请求与响应请求：服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

## 初始化项目

```
npm init
```
生成 package.json 文件

```
{
  "name": "works",
  "version": "1.0.0",
  "description": "作品展示",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git@git.coding.net:songxingguo/works.git"
  },
  "keywords": [
    "sxg"
  ],
  "author": "songxingguo",
  "license": "MIT"
}
```
<!-- more -->

## 创建服务器

> **思路**
- 使用 require 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http ；
- 使用http.creatServer() 方法创建服务器 ；
- 使用 listen() 方法绑定8080端口；
- 函数通过 request , response 参数来接收和响应数据。

新建 server.js 文件

```
var http = require('http');

http.createServer(function (request, response) {
    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8080);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8080/');
```
## 启动服务

```
node server.js
```

## 新建文件 index.html

