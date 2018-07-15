title: 跨域问题
author: songxingguo
tags:
  - http
categories:
  - 前端技术
date: 2018-07-15 07:48:00
---
### 写在前面

前后端分离的到来，给我们工作上带来了一下便利，但是也随之产生了一些问题，前后端分离意味这个在实际的工作中会存在客户端域名和和服务器端域名，而客服端向服务器端发出请求也就存在跨域的问题。

如果大家有兴趣可以看一下后面这篇文章，了解前后端分离的的历史原由。[前后端分离](https://blog.csdn.net/L_apple8/article/details/80782603)

<!-- more -->

---
### 什么是跨域
  
  - 域名地址的组成
  
  ![域名组成](http://p9myzkds7.bkt.clouddn.com/cross-domain/%E8%B7%A8%E5%9F%9F.jpg)
  
 `图片来自慕课网`
 
  - 跨域错误代码
  
  ![跨域错误代码](http://p9myzkds7.bkt.clouddn.com/cross-domain/%E8%B7%A8%E5%9F%9F%E9%94%99%E8%AF%AF%E4%BB%A3%E7%A0%81.jpg)
  
  `图片来自慕课网`
 
  - 跨域的定义
  
   跨域是JavaScript出于安全方面的考虑，不允许跨域调用其他页面对象。是JavaScript同源策略的限制。
   
  - 跨域的类型
  
   ![跨域类型](http://p9myzkds7.bkt.clouddn.com/cross-domain/%E8%B7%A8%E5%9F%9F%E7%9A%84%E7%B1%BB%E5%9E%8B.jpg)
   
   `图片来自慕课网`
   
---
### 处理跨域的方法

 #### **方法一：代理**
 
 服务器后台做代理，a.com 调用 b.com的服务时不直接调用，而是通过a.com的服务器后台去调用b.com的服务。
 
 ![服务器代理解决跨域](http://p9myzkds7.bkt.clouddn.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%BB%A3%E7%90%86%E8%A7%A3%E5%86%B3%E8%B7%A8%E5%9F%9Fjpg.jpg)
 `图片来自慕课网`
 
 ---
 #### **方法二：JSONP**
 
 用JSONP定义参数名称，只支持GET请求。
 
 ![JSONP跨域处理](http://p9myzkds7.bkt.clouddn.com/cross-domain/JSONP%E8%B7%A8%E5%9F%9F%E5%A4%84%E7%90%86.jpg)
 `图片来自慕课网`
 
 ---
 #### **方法三： XHR2**
 
 ![XHR2跨域解决](http://p9myzkds7.bkt.clouddn.com/cross-domain/XHR2%E8%B7%A8%E5%9F%9F%E8%A7%A3%E5%86%B3.jpg) 
  `图片来自慕课网`
 