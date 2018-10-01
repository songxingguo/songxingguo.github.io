title: HTTP教程
author: songxingguo
tags:
  - HTTP
  - ''
categories:
  - 读书笔记
date: 2018-10-01 14:52:00
---
# HTTP 教程

HTTP协议（HyperText Transfer Protocol，超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的WWW文件都必须遵守这个标准。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

<!-- mmore -->

## HTTP 简介

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

### HTTP 工作原理
HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。

Web服务器有：Apache服务器，IIS服务器（Internet Information Services）等。

Web服务器根据接收到的请求后，向客户端发送响应信息。

HTTP默认端口号为80，但是你也可以改为8080或者其他端口。

HTTP三点注意事项：

- HTTP是无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- HTTP是媒体独立的：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。
- HTTP是无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

以下图表展示了HTTP协议通信流程：

![HTTP协议通信流程](http://p9myzkds7.bkt.clouddn.com/HTTP/cgiarch.gif)

## HTTP 消息结构

HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

一个HTTP"客户端"是一个应用程序（Web浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。

一个HTTP"服务器"同样也是一个应用程序（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。

一旦建立连接后，数据消息就通过类似Internet邮件所使用的格式[RFC5322]和多用途Internet邮件扩展（MIME）[RFC2045]来传送。

### 客户端请求消息

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成，下图给出了请求报文的一般格式。

![请求报文的一般格式](http://p9myzkds7.bkt.clouddn.com/HTTP/%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87%E7%9A%84%E4%B8%80%E8%88%AC%E6%A0%BC%E5%BC%8F.png)

### 服务器响应消息

HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。

![HTTP响应也由四个部分组成](http://p9myzkds7.bkt.clouddn.com/HTTP/HTTP%E5%93%8D%E5%BA%94%E4%B9%9F%E7%94%B1%E5%9B%9B%E4%B8%AA%E9%83%A8%E5%88%86%E7%BB%84%E6%88%90.jpg)

下面实例是一点典型的使用GET来传递数据的实例：

客户端请求：

```
GET /hello.txt HTTP/1.1
User-Agent: curl/7.16.3 libcurl/7.16.3 OpenSSL/0.9.7l zlib/1.2.3
Host: www.example.com
Accept-Language: en, mi
```
服务端响应:

```
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
ETag: "34aa387-d-1568eb00"
Accept-Ranges: bytes
Content-Length: 51
Vary: Accept-Encoding
Content-Type: text/plain
```
输出结果：

```
Hello World! My payload includes a trailing CRLF.
```
## HTTP请求方法

根据HTTP标准，HTTP请求可以使用多种请求方法。

HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。

HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

![HTTP请求方法](http://p9myzkds7.bkt.clouddn.com/HTTP%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95.png)

## HTTP 响应头信息

HTTP请求头提供了关于请求，响应或者其他的发送实体的信息。

在本章节中我们将具体来介绍HTTP响应头信息。

![HTTP 响应头信息](http://p9myzkds7.bkt.clouddn.com/HTTP/HTTP%20%E5%93%8D%E5%BA%94%E5%A4%B4%E4%BF%A1%E6%81%AF.png)

## HTTP状态码

当浏览者访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。

HTTP状态码的英文为HTTP Status Code。

下面是常见的HTTP状态码：

- 200 - 请求成功
- 301 - 资源（网页等）被永久转移到其它URL
- 404 - 请求的资源（网页等）不存在
- 500 - 内部服务器错误

### HTTP状态码分类

HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。HTTP状态码共分为5种类型：

HTTP状态码分类:

![HTTP状态码分类](http://p9myzkds7.bkt.clouddn.com/HTTP/HTTP%E7%8A%B6%E6%80%81%E7%A0%81%E5%88%86%E7%B1%BB.png)

HTTP状态码列表:

[HTTP状态码列表](http://www.runoob.com/http/http-status-codes.html)

## HTTP content-type

Content-Type，内容类型，一般是指网页中存在的Content-Type，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，这就是经常看到一些Asp网页点击的结果却是下载到的一个文件或一张图片的原因。

HTTP content-type 对照表

[HTTP content-type 对照表](http://www.runoob.com/http/http-content-type.html)