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

## HTML 标题

### HTML 标题

标题（Heading）是通过 `<h1>` - `<h6>` 标签进行定义的.

`<h1>` 定义最大的标题。 `<h6>` 定义最小的标题。

```html
<h1>这是一个标题。</h1>
<h2>这是一个标题。</h2>
<h3>这是一个标题。</h3>
```
> 注释: 浏览器会自动地在标题的前后添加空行。

### 标题很重要

请确保将 HTML 标题 标签只用于标题。不要仅仅是为了生成粗体或大号的文本而使用标题。

搜索引擎使用标题为您的网页的结构和内容编制索引。

因为用户可以通过标题来快速浏览您的网页，所以用标题来呈现文档结构是很重要的。

应该将 h1 用作主标题（最重要的），其后是 h2（次重要的），再其次是 h3，以此类推。

### HTML 水平线

`<hr>` 标签在 HTML 页面中创建水平线。

hr 元素可用于分隔内容。

```html
<p>这是一个段落。</p>
<hr>
<p>这是一个段落。</p>
<hr>
<p>这是一个段落。</p>
```
### HTML 注释

可以将注释插入 HTML 代码中，这样可以提高其可读性，使代码更易被人理解。浏览器会忽略注释，也不会显示它们。

注释写法如下:

```html
<!-- 这是一个注释 -->
```
> 注释: 开始括号之后（左边的括号）需要紧跟一个叹号，结束括号之前（右边的括号）不需要，合理地使用注释可以对未来的代码编辑工作产生帮助。。

### HTML 提示 - 如何查看源代码

你是否看过一些网页然后惊叹它是如何实现的的。

如果您想找到其中的奥秘，只需要单击右键，然后选择"查看源文件"（IE）或"查看页面源代码"（Firefox），其他浏览器的做法也是类似的。这么做会打开一个包含页面 HTML 代码的窗口。

## HTML 段落

HTML 可以将文档分割为若干段落。

### HTML 段落

段落是通过 `<p>` 标签定义的。

```html
<p>这是一个段落 </p>
<p>这是另一个段落</p>
```
> 注意：浏览器会自动地在段落的前后添加空行。（`</p>` 是块级元素）

### 不要忘记结束标签

即使忘了使用结束标签，大多数浏览器也会正确地将 HTML 显示出来：

```html
<p>这是一个段落
<p>这是另一个段落
```
上面的例子在大多数浏览器中都没问题，但不要依赖这种做法。忘记使用结束标签会产生意想不到的结果和错误。

> 注释: 在未来的 HTML 版本中，不允许省略结束标签。

### HTML 折行

如果您希望在不产生一个新段落的情况下进行换行（新行），请使用 `<br />` 标签：

`<p>` 这个 `<br>` 段落 `<br>` 演示了分行的效果 `</p>`

`<br />` 元素是一个空的 HTML 元素。由于关闭标签没有任何意义，因此它没有结束标签。

### HTML 输出- 使用提醒

我们无法确定 HTML 被显示的确切效果。屏幕的大小，以及对窗口的调整都可能导致不同的结果。

对于 HTML，您无法通过在 HTML 代码中添加额外的空格或换行来改变输出的效果。

当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格或空行都会被算作一个空格。需要注意的是，HTML 代码中的所有连续的空行（换行）也被显示为一个空格。

## HTML 文本格式化

### HTML 文本格式化

```
加粗文本

斜体文本

电脑自动输出

这是 下标 和 上标
```
### HTML 格式化标签

HTML 使用标签 `<b>`("bold") 与 `<i>`("italic") 对输出的文本进行格式, 如：粗体 or 斜体

这些HTML标签被称为格式化标签（请查看底部完整标签参考手册）。

> 通常标签 `<strong>` 替换加粗标签 `<b>` 来使用, `<em>` 替换 `<i>` 标签使用。
然而，这些标签的含义是不同的：
`<b>` 与 `<i>` 定义粗体或斜体文本。
`<strong>` 或者 `<em>` 意味着你要呈现的文本是重要的，所以要突出显示。现今所有主要浏览器都能渲染各种效果的字体。不过，未来浏览器可能会支持更好的渲染效果。

### HTML 文本格式化标签

![HTML 文本格式化标签](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%20%E6%96%87%E6%9C%AC%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%A0%87%E7%AD%BE.png)

### HTML "计算机输出" 标签

![HTML "计算机输出" 标签](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%BE%93%E5%87%BA%E6%A0%87%E7%AD%BE.png)

### HTML 引文, 引用, 及标签定义

![HTML 引文, 引用, 及标签定义](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%20%E5%BC%95%E6%96%87,%20%E5%BC%95%E7%94%A8,%20%E5%8F%8A%E6%A0%87%E7%AD%BE%E5%AE%9A%E4%B9%89.png)

## HTML 链接

HTML 使用超级链接与网络上的另一个文档相连。几乎可以在所有的网页中找到链接。点击链接可以从一张页面跳转到另一张页面。

### HTML 超链接（链接）

HTML使用标签 `<a>`来设置超文本链接。

超链接可以是一个字，一个词，或者一组词，也可以是一幅图像，您可以点击这些内容来跳转到新的文档或者当前文档中的某个部分。

当您把鼠标指针移动到网页中的某个链接上时，箭头会变为一只小手。

在标签 `<a>` 中使用了href属性来描述链接的地址。

默认情况下，链接将以以下形式出现在浏览器中：

- 一个未访问过的链接显示为蓝色字体并带有下划线。
- 访问过的链接显示为紫色并带有下划线。
- 点击链接时，链接显示为红色并带有下划线。

> 注意：如果为这些超链接设置了 CSS 样式，展示样式会根据 CSS 的设定而显示。

### HTML 链接语法

链接的 HTML 代码很简单。它类似这样：

```html
<a href="url">链接文本</a>
```
href 属性描述了链接的目标。.

```html
<a href="http://www.runoob.com/">访问菜鸟教程</a>
```
上面这行代码显示为：[访问菜鸟教程](http://www.runoob.com/)

点击这个超链接会把用户带到菜鸟教程的首页。

提示: "链接文本" 不必一定是文本。图片或其他 HTML 元素都可以成为链接。

### HTML 链接 - target 属性

使用 target 属性，你可以定义被链接的文档在何处显示。

下面的这行会在新窗口打开文档：

```html
<a href="http://www.runoob.com/" target="_blank">访问菜鸟教程!</a>
```
### HTML 链接- id 属性

id属性可用于创建在一个HTML文档书签标记。

提示: 书签是不以任何特殊的方式显示，在HTML文档中是不显示的，所以对于读者来说是隐藏的。

在HTML文档中插入ID:

```html
<a id="tips">有用的提示部分</a>
```
在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：

```html
<a href="#tips">访问有用的提示部分</a>
```
或者，从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：

```html
<a href="http://www.runoob.com/html/html-links.html#tips">
访问有用的提示部分 </a>
```
### 基本的注意事项 - 有用的提示

注释： 请始终将正斜杠添加到子文件夹。假如这样书写链接：href="http://www.runoob.com/html "，就会向服务器产生两次 HTTP 请求。这是因为服务器会添加正斜杠到这个地址，然后创建一个新的请求，就像这样：href="http://www.runoob.com/html/ "。

### HTML 链接标签

![HTML 链接标签](http://p9myzkds7.bkt.clouddn.com/HTML/HTML%20%E9%93%BE%E6%8E%A5%E6%A0%87%E7%AD%BE.png)

## HTML `<head>`
  
### HTML `<head>` 元素

<head> 元素包含了所有的头部标签元素。在 `<head>` 元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

可以添加在头部区域的元素标签为: `<title>`, `<style>`, `<meta>`, `<link>`, `<script>`, `<noscript>`, and `<base>`.

### HTML `<title>` 元素

`<title>` 标签定义了不同文档的标题。

`<title>` 在 HTML/XHTML 文档中是必须的。

`<title>` 元素:

- 定义了浏览器工具栏的标题
-  当网页添加到收藏夹时，显示在收藏夹中的标题
- 显示在搜索引擎结果页面的标题

一个简单的 HTML 文档:

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>文档标题</title>
</head>
 
<body>
文档内容......
</body>
 
</html>
```
### HTML `<base>` 元素

`<base>` 标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接:

```html
<head>
<base href="http://www.runoob.com/images/" target="_blank">
</head>
```
### HTML `<link>` 元素

`<link>` 标签定义了文档与外部资源之间的关系。

`<link>` 标签通常用于链接到样式表:

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```
### HTML `<style>` 元素

`<style>` 标签定义了HTML文档的样式文件引用地址.

在 `<style>` 元素中你也可以直接添加样式来渲染 HTML 文档:

```html
<head>
<style type="text/css">
body {background-color:yellow}
p {color:blue}
</style>
</head>
```
### HTML `<meta>` 元素

meta标签描述了一些基本的元数据。

`<meta>` 标签提供了元数据.元数据也不显示在页面上，但会被浏览器解析。

META 元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。

元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。

`<meta>` 一般放置于 `<head>` 区域

### `<meta>` 标签- 使用实例

为搜索引擎定义关键词:

```html
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
```
为网页定义描述内容:

```html
<meta name="description" content="免费 Web & 编程 教程">
```
定义网页作者:

```html
<meta name="author" content="Runoob">
```
每30秒钟刷新当前页面:

```html
<meta http-equiv="refresh" content="30">
```
### HTML `<script>` 元素

`<script>` 标签用于加载脚本文件，如： JavaScript。

`<script>` 元素在以后的章节中会详细描述。

### HTML head 元素

![head 元素](http://p9myzkds7.bkt.clouddn.com/HTML/head%20%E5%85%83%E7%B4%A0.png)