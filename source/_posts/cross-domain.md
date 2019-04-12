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

  ![域名组成](https://graphbed.qiniu.songxingguo.com/cross-domain/%E8%B7%A8%E5%9F%9F.jpg)

 `图片来自慕课网`

  - 跨域错误代码

  ![跨域错误代码](https://graphbed.qiniu.songxingguo.com/cross-domain/%E8%B7%A8%E5%9F%9F%E9%94%99%E8%AF%AF%E4%BB%A3%E7%A0%81.jpg)

  `图片来自慕课网`

  - 跨域的定义

   跨域是JavaScript出于安全方面的考虑，不允许跨域调用其他页面对象。是JavaScript同源策略的限制。

  - 常见跨域场景

   ![跨域类型](https://graphbed.qiniu.songxingguo.com/cross-domain/%E8%B7%A8%E5%9F%9F%E7%9A%84%E7%B1%BB%E5%9E%8B.jpg)

   `图片来自慕课网`

---
### 处理跨域的方法

 #### **方法一：代理**

 服务器后台做代理，a.com 调用 b.com的服务时不直接调用，而是通过a.com的服务器后台去调用b.com的服务。

 ![服务器代理解决跨域](https://graphbed.qiniu.songxingguo.com/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%BB%A3%E7%90%86%E8%A7%A3%E5%86%B3%E8%B7%A8%E5%9F%9Fjpg.jpg)
 `图片来自慕课网`

---
 #### **方法二：JSONP**

 用JSONP定义参数名称，只支持GET请求。

 ![JSONP跨域处理](https://graphbed.qiniu.songxingguo.com/cross-domain/JSONP%E8%B7%A8%E5%9F%9F%E5%A4%84%E7%90%86.jpg)
 `图片来自慕课网`

---
 #### **方法三： XHR2**

  跨域资源共享（CORS）

普通跨域请求：只服务端设置Access-Control-Allow-Origin即可，前端无须设置。
带cookie请求：前后端都需要设置字段，另外需注意：所带cookie为跨域请求接口所在域的cookie，而非当前页。
目前，所有浏览器都支持该功能(IE8+：IE8/9需要使用XDomainRequest对象来支持CORS）)，CORS也已经成为主流的跨域解决方案。

 ![XHR2跨域解决](https://graphbed.qiniu.songxingguo.com/cross-domain/XHR2%E8%B7%A8%E5%9F%9F%E8%A7%A3%E5%86%B3.jpg) 
  `图片来自慕课网`

---
#### **方法四： nginx代理**

- nginx配置解决iconfont跨域

   浏览器跨域访问js、css、img等常规静态资源被同源策略许可，但iconfont字体文件(eot|otf|ttf|woff|svg)例外，此时可在nginx的静态资源服务器中加入以下配置

```
location / {
  add_header Access-Control-Allow-Origin *;
}
```
- nginx反向代理接口跨域

   跨域原理： 同源策略是浏览器的安全策略，不是HTTP协议的一部分。服务器端调用HTTP接口只是使用HTTP协议，不会执行JS脚本，不需要同源策略，也就不存在跨越问题。

   实现思路：通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录。

  nginx具体配置：
```
#proxy服务器
server {
    listen       81;
    server_name  www.domain1.com;

    location / {
        proxy_pass   http://www.domain2.com:8080;  #反向代理
        proxy_cookie_domain www.domain2.com www.domain1.com; #修改cookie里域名
        index  index.html index.htm;

        # 当用webpack-dev-server等中间件代理接口访问nignx时，此时无浏览器参与，故没有同源限制，下面的跨域配置可不启用
        add_header Access-Control-Allow-Origin http://www.domain1.com;  #当前端只跨域不带cookie时，可为*
        add_header Access-Control-Allow-Credentials true;
    }
}
```
---
#### **方法五：nodejs中间件** 

node中间件实现跨域代理，原理大致与nginx相同，都是通过启一个代理服务器，实现数据的转发。

---
#### **方法六：WebSocket**

WebSocket protocol是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现。原生WebSocket API使用起来不太方便，我们使用Socket.io，它很好地封装了webSocket接口，提供了更简单、灵活的接口，也对不支持webSocket的浏览器提供了向下兼容。

---
### 参考教程

[九种跨域方式实现原理（完整版）](https://mp.weixin.qq.com/s/zU6EyuuIqDbowAUMZwlH9Q)

[处理跨域方式 慕课网](https://www.imooc.com/video/6238)

[前端常见跨域解决方案（全）](https://www.cnblogs.com/roam/p/7520433.html)