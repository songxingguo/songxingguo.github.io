title: HTML教程
author: songxingguo
tags:
  - HTML
categories:
  - 读书笔记
date: 2018-09-10 14:19:00
---
# HTML 教程- (HTML5 标准)

**超文本标记语言**（英语：HyperText Markup Language，简称：HTML）是一种用于 **创建网页的标准标记语言** 。

您可以使用 HTML 来建立自己的 WEB 站点，HTML 运行在浏览器上，由浏览器来解析。

在本教程中，您将学习如何使用 HTML 来创建站点。

HTML 很容易学习！相信您能很快学会它！

<!-- more -->

## HTML 实例

本教程包含了数百个 HTML 实例。

使用本站的编辑器，您可以轻松实现在线修改 HTML，并查看实例运行结果。

注意：对于中文网页需要使用 <meta charset="utf-8"> 声明编码，否则会出现乱码。有些浏览器(如 360 浏览器)会设置 **GBK** 为 **默认编码** ，则你需要设置为 `<meta charset="gbk">` 。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>菜鸟教程(runoob.com)</title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
</body>
</html>
```

## HTML文档的后缀名

- .html
- .htm

以上两种后缀名没有区别，都可以使用。

## HTML 简介

### HTML 实例

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>菜鸟教程(runoob.com)</title>
</head>
<body>
 
<h1>我的第一个标题</h1>
 
<p>我的第一个段落。</p>
 
</body>
</html>
```
实例解析

- `<!DOCTYPE html>` 声明为 HTML5 文档
- `<html>` 元素是 HTML 页面的根元素
- `<head>` 元素包含了文档的元（meta）数据，如 <meta charset="utf-8"> 定义网页编码格式为 utf-8。
- `<title>` 元素描述了文档的标题
- `<body>` 元素包含了可见的页面内容
- `<h1>` 元素定义一个大标题
- `<p>` 元素定义一个段落

### 什么是HTML?

HTML 是用来描述网页的一种语言。

- HTML 指的是超文本标记语言: HyperText Markup Language
- HTML 不是一种编程语言，而是一种标记语言
- 标记语言是一套标记标签 (markup tag)
- HTML 使用标记标签来描述网页
- HTML 文档包含了HTML 标签及文本内容
- HTML文档也叫做 web 页面

### HTML 标签

HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

- HTML 标签是由尖括号包围的关键词，比如 `<html>`
- HTML 标签通常是成对出现的，比如 `<b>` 和 `</b>`
- 标签对中的第一个标签是开始标签，第二个标签是结束标签
- 开始和结束标签也被称为开放标签和闭合标签

```
<标签>内容</标签>
```
### HTML 元素

"HTML 标签" 和 "HTML 元素" 通常都是描述同样的意思.

但是严格来讲, 一个 HTML 元素包含了开始标签与结束标签，如下实例:

HTML 元素:

```
<p>这是一个段落。</p>
```

### Web 浏览器

Web浏览器（如谷歌浏览器，Internet Explorer，Firefox，Safari）是用于读取HTML文件，并将其作为网页显示。

浏览器并不是直接显示的HTML标签，但可以使用标签来决定如何展现HTML页面的内容给用户：

![第一个页面](http://p9myzkds7.bkt.clouddn.com/HTML/html-first.png)


### HTML 网页结构

下面是一个可视化的HTML页面结构：

![HTML页面结构](http://p9myzkds7.bkt.clouddn.com/HTML/html%E7%BB%93%E6%9E%84.png)

> 只有 `<body>` -区域 (-白色部分) -才会在浏览器中显示。

### HTML版本

从初期的网络诞生后，已经出现了许多HTML版本:

![HTML版本](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%E7%89%88%E6%9C%AC.png)

### <!DOCTYPE> 声明


<!DOCTYPE>声明有助于浏览器中正确显示网页。

网络上有很多不同的文件，如果能够正确声明HTML的版本，浏览器就能正确显示网页内容。

doctype 声明是不区分大小写的，以下方式均可：

```html
<!DOCTYPE html> 

<!DOCTYPE HTML> 

<!doctype html> 

<!Doctype Html>
```
### 通用声明

HTML5

```html
<!DOCTYPE html>
```
HTML 4.01

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
```
XHTML 1.0

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```
查看完整网页声明类型 [DOCTYPE 参考手册](http://www.runoob.com/tags/tag-doctype.html)。

### 中文编码

目前在大部分浏览器中，直接输出中文会出现中文乱码的情况，这时候我们就需要在头部将字符声明为 UTF-8。

HTML 实例

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>页面标题</title>
</head>
<body>
 
<h1>我的第一个标题</h1>
 
<p>我的第一个段落。</p>
 
</body>
</html>
```
## HTML 基础

### HTML 标题

HTML 标题（Heading）是通过`<h1>` - `<h6>` 标签来定义的.

```html
<h1>这是一个标题</h1>
<h2>这是一个标题</h2>
<h3>这是一个标题</h3>
```
### HTML 段落

HTML 段落是通过标签 `<p>` 来定义的.

```html
<p>这是一个段落。</p>
<p>这是另外一个段落。</p>
```
### HTML 链接

HTML 链接是通过标签 `<a>` 来定义的.

```html
<a href="http://www.runoob.com">这是一个链接 `</a>`
```
> 提示:在 href 属性中指定链接的地址。

### HTML 图像

HTML 图像是通过标签 `<img>` 来定义的.

```html
<img src="/images/logo.png" width="258" height="39" />
```
> 注意： 图像的名称和尺寸是以属性的形式提供的。

## HTML 元素

HTML 文档由 HTML 元素定义。

### HTML 元素

![HTML元素](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%E5%85%83%E7%B4%A0.png)

*开始标签常被称为起始标签（opening tag），结束标签常称为闭合标签（closing tag）。

### HTML 元素语法

- HTML 元素以开始标签起始
- HTML 元素以结束标签终止
- 元素的内容是开始标签与结束标签之间的内容
- 某些 HTML 元素具有空内容（empty content）
- 空元素在开始标签中进行关闭（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有属性

> 注释: 您将在本教程的下一章中学习更多有关属性的内容。

### 嵌套的 HTML 元素

HTML 文档由嵌套的 HTML 元素构成。

- #### HTML 文档实例

  ```html
  <!DOCTYPE html>
  <html>

  <body>
  <p>这是第一个段落。</p>
  </body>

  </html>
  ```
  以上实例包含了三个 HTML 元素。

- #### HTML 实例解析

  `<p>` 元素:

  ```html
  <p>这是第一个段落。</p>
  ```
  这个 `<p>` 元素定义了 HTML 文档中的一个段落。
  这个元素拥有一个开始标签 `<p>` 以及一个结束标签 `</p>`.
  元素内容是: 这是第一个段落。

  `<body>` 元素:

  ```html
  <body>
  <p>这是第一个段落。</p>
  </body>
  ```
  `<body>` 元素定义了 HTML 文档的主体。
  这个元素拥有一个开始标签 `<body>` 以及一个结束标签 `</body>`。
  元素内容是另一个 HTML 元素（p 元素）。

  `<html>` 元素：

  ```html
  <html>

  <body>
  <p>这是第一个段落。</p>
  </body>

  </html>
  ```
  `<html>` 元素定义了整个 HTML 文档。
  这个元素拥有一个开始标签 `<html>` ，以及一个结束标签 `</html>`.
  元素内容是另一个 HTML 元素（body 元素）。

### 不要忘记结束标签

即使您忘记了使用结束标签，大多数浏览器也会正确地显示 HTML：

```html
<p>这是一个段落
<p>这是一个段落
```
以上实例在浏览器中也能正常显示，因为关闭标签是可选的。

但不要依赖这种做法。忘记使用结束标签会产生不可预料的结果或错误。

### HTML 空元素

没有内容的 HTML 元素被称为空元素。空元素是在开始标签中关闭的。

`<br>` 就是没有关闭标签的空元素（`<br>` 标签定义换行）。

在 XHTML、XML 以及未来版本的 HTML 中，所有元素都必须被关闭。

在开始标签中添加斜杠，比如 `<br />`，是关闭空元素的正确方法，HTML、XHTML 和 XML 都接受这种方式。

即使 `<br>` 在所有浏览器中都是有效的，但使用 `<br />` 其实是更长远的保障。

### HTML 提示：使用小写标签

HTML 标签对大小写不敏感：`<P>` 等同于 `<p>`。许多网站都使用大写的 HTML 标签。

菜鸟教程使用的是小写标签，因为万维网联盟（W3C）在 HTML 4 中推荐使用小写，而在未来 (X)HTML 版本中强制使用小写。

## HTML 属性

属性是 HTML 元素提供的附加信息。

### HTML 属性

- HTML 元素可以设置属性
- 属性可以在元素中添加附加信息
- 属性一般描述于开始标签
- 属性总是以名称/值对的形式出现，比如：name="value"。

### 属性实例

HTML 链接由 `<a>` 标签定义。链接的地址在 href 属性中指定：

```html
<a href="http://www.runoob.com">这是一个链接</a>
```
### HTML 属性常用引用属性值

属性值应该始终被包括在引号内。

双引号是最常用的，不过使用单引号也没有问题。

Remark提示: 在某些个别的情况下，比如属性值本身就含有双引号，那么您必须使用单引号，例如：name='John "ShotGun" Nelson'

### HTML 提示：使用小写属性

属性和属性值对大小写不敏感。

不过，万维网联盟在其 HTML 4 推荐标准中推荐小写的属性/属性值。

而新版本的 (X)HTML 要求使用小写属性。

### HTML 属性参考手册

查看完整的HTML属性列表: [HTML 标签参考手册](http://www.runoob.com/tags/html-reference.html)。

下面列出了适用于大多数 HTML 元素的属性：

![HTML属性](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%E5%B1%9E%E6%80%A7.png)