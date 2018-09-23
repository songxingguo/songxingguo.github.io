title: HTML5  教程
author: songxingguo
tags:
  - HTML5
categories:
  - 读书笔记
date: 2018-09-23 16:34:00
---
## HTML5 简介

HTML5是HTML最新的修订版本，2014年10月由万维网联盟（W3C）完成标准制定。

HTML5的设计目的是为了在移动设备上支持多媒体。

HTML5 简单易学。

### 什么是 HTML5?

HTML5 是下一代 HTML 标准。

HTML , HTML 4.01的上一个版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变。

HTML5 仍处于完善之中。然而，大部分现代浏览器已经具备了某些 HTML5 支持。

### HTML5 是如何起步的？

HTML5 是 W3C 与 WHATWG 合作的结果,WHATWG 指 Web Hypertext Application Technology Working Group。

WHATWG 致力于 web 表单和应用程序，而 W3C 专注于 XHTML 2.0。在 2006 年，双方决定进行合作，来创建一个新版本的 HTML。

HTML5 中的一些有趣的新特性：

- 用于绘画的 canvas 元素
- 用于媒介回放的 video 和 audio 元素
- 对本地离线存储的更好的支持
- 新的特殊内容元素，比如 article、footer、header、nav、section
- 新的表单控件，比如 calendar、date、time、email、url、search

### HTML5 <!DOCTYPE>

<!doctype> 声明必须位于 HTML5 文档中的第一行,使用非常简单:

```html
<!DOCTYPE html>
```
### 最小的HTML5文档

下面是一个简单的HTML5文档：

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
> 注意：对于中文网页需要使用 `<meta charset="utf-8">` 声明编码，否则会出现乱码。

### HTML5 的改进

- 新元素
- 新属性
- 完全支持 CSS3
- Video 和 Audio
- 2D/3D 制图
- 本地存储
- 本地 SQL 数据
- Web 应用

### HTML5 多媒体

使用 HTML5 你可以简单的在网页中播放 视频(video)与音频 (audio) 。

HTML5 `<video>`
HTML5 `<audio>`

### HTML5 应用

使用 HTML5 你可以简单地开发应用

- 本地数据存储
- 访问本地文件
- 本地 SQL 数据
- 缓存引用
- Javascript 工作者
- XHTMLHttpRequest 2

### HTML5 图形

使用 HTML5 你可以简单的绘制图形:

- 使用 `<canvas>` 元素。
- 使用内联 SVG。
- 使用 CSS3 2D 转换、CSS3 3D 转换。

### HTML5 使用 CSS3

- 新选择器
- 新属性
- 动画
- 2D/3D 转换
- 圆角
- 阴影效果
- 可下载的字体

了解更多CSS3知识请查看本站的 [CSS3 教程](http://www.runoob.com/css3/css3-tutorial.html)。

### 语义元素

HTML5 添加了很多语义元素如下所示：

![语义元素](http://p9myzkds7.bkt.clouddn.com/HTML5/%E8%AF%AD%E4%B9%89%E5%85%83%E7%B4%A0.png)

### HTML5 表单

新表单元素, 新属性，新输入类型，自动验证。

### 已移除元素

以下的 HTML 4.01 元素在HTML5中已经被删除:

- `<acronym>`
- `<applet>`
- `<basefont>`
- `<big>`
- `<center>`
- `<dir>`
- `<font>`
- `<frame>`
- `<frameset>`
- `<noframes>`
- `<strike>`
  	
### 每一章中的实例

通过我们的 HTML 编辑器，您能够编辑 HTML，然后点击按钮来查看结果。

```html
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
 
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  你的浏览器不支持 video 标签。
</video>
 
</body>
</html>
```
### HTML5 浏览器支持

最新版本的 Safari、Chrome、Firefox 以及 Opera 支持某些 HTML5 特性。Internet Explorer 9 将支持某些 HTML5 特性。

![HTML5 浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/browsers_say.png)

IE9 以下版本浏览器兼容HTML5的方法，使用本站的静态资源的html5shiv包：

```html
<!--[if lt IE 9]>
    <script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
<![endif]-->
```
载入后，初始化新标签的CSS：

```
/*html5*/
article,aside,dialog,footer,header,section,nav,figure,menu{display:block}
```
### HTML5 参考手册

在本站中你可以找到关于HTML5 的标签及属性描述，详细请点击 [HTML5参考手册](http://www.runoob.com/tags/html-reference.html)。

## HTML5 浏览器支持

你可以让一些较早的浏览器（不支持HTML5）支持 HTML5。

### HTML5 浏览器支持

现代的浏览器都支持 HTML5。

此外，所有浏览器，包括旧的和最新的，对无法识别的元素会作为内联元素自动处理。

正因为如此，你可以 "教会" 浏览器处理 "未知" 的 HTML 元素。

> 甚至你可以教会 IE6 (Windows XP 2001) 浏览器处理未知的 HTML 元素。

### 将 HTML5 元素定义为块元素

HTML5 定了 8 个新的 HTML 语义（semantic） 元素。所有这些元素都是 块级 元素。

为了能让旧版本的浏览器正确显示这些元素，你可以设置 CSS 的 display 属性值为 block:

```css
header, section, footer, aside, nav, main, article, figure {
    display: block; 
}
```
### 为 HTML 添加新元素

你可以为 HTML 添加新的元素。

该实例向 HTML 添加的新的元素，并为该元素定义样式，元素名为 `<myHero>` ：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>为 HTML 添加新元素</title>
<script>
document.createElement("myHero")
</script>
<style>
myHero {
    display: block;
    background-color: #ddd;
    padding: 50px;
    font-size: 30px;
}
</style> 
</head>
 
<body>
 
<h1>我的第一个标题</h1>
 
<p>我的第一个段落。</p>
 
<myHero>我的第一个新元素</myHero>
 
</body>
</html>
```
JavaScript 语句 document.createElement("myHero") 是为 IE 浏览器添加新的元素。

### Internet Explorer 浏览器问题

你可以使用以上的方法来为 IE 浏览器添加 HTML5 元素，但是：

> Internet Explorer 8 及更早 IE 版本的浏览器不支持以上的方式。

我们可以使用 Sjoerd Visscher 创建的 "HTML5 Enabling JavaScript", " shiv" 来解决该问题:

```html
<!--[if lt IE 9]>
  <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```
以上代码是一个注释，作用是在 IE 浏览器的版本小于 IE9 时将读取 html5.js 文件，并解析它。

注意：国内用户请使用本站静态资源库（Google 资源库在国内不稳定）：

```html
<!--[if lt IE 9]>
  <script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
<![endif]-->
```
针对IE浏览器html5shiv 是比较好的解决方案。html5shiv主要解决HTML5提出的新的元素不被IE6-8识别，这些新元素不能作为父节点包裹子元素，并且不能应用CSS样式。

### 完美的 Shiv 解决方案

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>渲染 HTML5</title>
  <!--[if lt IE 9]>
  <script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script>
  <![endif]-->
</head>
 
<body>
 
<h1>我的第一篇文章</h1>
 
<article>
菜鸟教程 —— 学的不仅是技术，更是梦想！！！
</article>
 
</body>
</html>
```
html5shiv.js 引用代码必须放在 <head> 元素中，因为 IE 浏览器在解析 HTML5 新元素时需要先加载该文件。

## HTML5 新元素

### HTML5 新元素

自1999年以后HTML 4.01 已经改变了很多,今天，在HTML 4.01中的几个已经被废弃，这些元素在HTML5中已经被删除或重新定义。

为了更好地处理今天的互联网应用，HTML5添加了很多新元素及功能，比如: 图形的绘制，多媒体内容，更好的页面结构，更好的形式 处理，和几个api拖放元素，定位，包括网页 应用程序缓存，存储，网络工作者，等。

### `<canvas>` 新元素

![<canvas> 新元素](http://p9myzkds7.bkt.clouddn.com/HTML5/canvas%20%E6%96%B0%E5%85%83%E7%B4%A0.png)
  
### 新多媒体元素

![新多媒体元素](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%96%B0%E5%A4%9A%E5%AA%92%E4%BD%93%E5%85%83%E7%B4%A0.png)

### 新表单元素

![新表单元素](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%96%B0%E8%A1%A8%E5%8D%95%E5%85%83%E7%B4%A0.png)

### 新的语义和结构元素

HTML5提供了新的元素来创建更好的页面结构：

![新的语义和结构元素](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%96%B0%E7%9A%84%E8%AF%AD%E4%B9%89%E5%92%8C%E7%BB%93%E6%9E%84%E5%85%83%E7%B4%A0.png)

### 已移除的元素

以下的 HTML 4.01 元素在HTML5中已经被删除:

- `<acronym>`
- `<applet>`
- `<basefont>`
- `<big>`
- `<center>`
- `<dir>`
- `<font>`
- `<frame>`
- `<frameset>`
- `<noframes>`
- `<strike>`
- `<tt>`

### HTML5 Canvas

`<canvas>` 标签定义图形，比如图表和其他图像，您必须使用脚本来绘制图形。

在画布上（Canvas）画一个红色矩形，渐变矩形，彩色矩形，和一些彩色的文字。

![Canvas](http://p9myzkds7.bkt.clouddn.com/HTML5/%E5%BD%A9%E8%89%B2%E6%96%87%E5%AD%97.png)

### 什么是 canvas?

HTML5 `<canvas>` 元素用于图形的绘制，通过脚本 (通常是JavaScript)来完成.

`<canvas>` 标签只是图形容器，您必须使用脚本来绘制图形。

你可以通过多种方法使用 canvas 绘制路径,盒、圆、字符以及添加图像。

浏览器支持
表格中的数字表示支持 `<canvas>` 元素的第一个浏览器版本号。
  
![浏览器版本号](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E7%89%88%E6%9C%AC%E5%8F%B7.png)

### 创建一个画布（Canvas）

一个画布在网页中是一个矩形框，通过 `<canvas>` 元素来绘制.

注意: 默认情况下 `<canvas>` 元素没有边框和内容。

`<canvas>`简单实例如下:

```html
<canvas id="myCanvas" width="200" height="100"></canvas>
```
注意: 标签通常需要指定一个id属性 (脚本中经常引用), width 和 height 属性定义的画布的大小.

提示:你可以在HTML页面中使用多个 `<canvas>` 元素.

使用 style 属性来添加边框:

```html
<canvas id="myCanvas" width="200" height="100"
style="border:1px solid #000000;">
</canvas>
```
### 使用 JavaScript 来绘制图像

canvas 元素本身是没有绘图能力的。所有的绘制工作必须在 JavaScript 内部完成：

```html
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
ctx.fillStyle="#FF0000";
ctx.fillRect(0,0,150,75);
```
实例解析:

首先，找到 `<canvas>` 元素:

```js
var c=document.getElementById("myCanvas");
```
然后，创建 context 对象：

```js
var ctx=c.getContext("2d");
```
getContext("2d") 对象是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。

下面的两行代码绘制一个红色的矩形：

```html
ctx.fillStyle="#FF0000";
ctx.fillRect(0,0,150,75);
```
设置fillStyle属性可以是CSS颜色，渐变，或图案。fillStyle 默认设置是#000000（黑色）。

fillRect(x,y,width,height) 方法定义了矩形当前的填充方式。

### Canvas 坐标

canvas 是一个二维网格。

canvas 的左上角坐标为 (0,0)

上面的 fillRect 方法拥有参数 (0,0,150,75)。

意思是：在画布上绘制 150x75 的矩形，从左上角开始 (0,0)。

坐标实例

如下图所示，画布的 X 和 Y 坐标用于在画布上对绘画进行定位。鼠标移动的矩形框上，显示定位坐标。

![定位坐标](http://p9myzkds7.bkt.clouddn.com/HTML5/%E5%AE%9A%E4%BD%8D%E5%9D%90%E6%A0%87.png)

### Canvas - 路径

在Canvas上画线，我们将使用以下两种方法：

- moveTo(x,y) 定义线条开始坐标
- lineTo(x,y) 定义线条结束坐标

绘制线条我们必须使用到 "ink" 的方法，就像stroke().

实例
定义开始坐标(0,0), 和结束坐标 (200,100)。然后使用 stroke() 方法来绘制线条:

![绘制线条](http://p9myzkds7.bkt.clouddn.com/HTML5/%E7%BB%98%E5%88%B6%E7%BA%BF%E6%9D%A1.png)

JavaScript:

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
ctx.moveTo(0,0);
ctx.lineTo(200,100);
ctx.stroke();
```

在canvas中绘制圆形, 我们将使用以下方法:

```js
arc(x,y,r,start,stop)
```
实际上我们在绘制圆形时使用了 "ink" 的方法, 比如 stroke() 或者 fill().


使用 arc() 方法 绘制一个圆:

![绘制一个圆](http://p9myzkds7.bkt.clouddn.com/HTML5/%E7%BB%98%E5%88%B6%E5%9C%86%E5%BD%A2.png)

JavaScript:

```
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
ctx.beginPath();
ctx.arc(95,50,40,0,2*Math.PI);
ctx.stroke();
```
### Canvas - 文本

使用 canvas 绘制文本，重要的属性和方法如下：

- font - 定义字体
- fillText(text,x,y) - 在 canvas 上绘制实心的文本
- strokeText(text,x,y) - 在 canvas 上绘制空心的文本

- #### 使用 fillText():

  使用 "Arial" 字体在画布上绘制一个高 30px 的文字（实心）：

  ![高 30px 的文字](http://p9myzkds7.bkt.clouddn.com/HTML5/%E9%AB%98%2030px%20%E7%9A%84%E6%96%87%E5%AD%97%EF%BC%88%E5%AE%9E%E5%BF%83%EF%BC%89.png)

  JavaScript:

  ```js
  var c=document.getElementById("myCanvas");
  var ctx=c.getContext("2d");
  ctx.font="30px Arial";
  ctx.fillText("Hello World",10,50);
  ```
- #### 使用 strokeText():

  使用 "Arial" 字体在画布上绘制一个高 30px 的文字（空心）：

  ![高 30px 的文字（空心）](http://p9myzkds7.bkt.clouddn.com/HTML5/%E9%AB%98%2030px%20%E7%9A%84%E6%96%87%E5%AD%97%EF%BC%88%E7%A9%BA%E5%BF%83%EF%BC%89.png)

  JavaScript:

  ```js
  var c=document.getElementById("myCanvas");
  var ctx=c.getContext("2d");
  ctx.font="30px Arial";
  ctx.strokeText("Hello World",10,50);
  ```
## Canvas - 渐变

渐变可以填充在矩形, 圆形, 线条, 文本等等, 各种形状可以自己定义不同的颜色。

以下有两种不同的方式来设置Canvas渐变：

```js
createLinearGradient(x,y,x1,y1) - 创建线条渐变
createRadialGradient(x,y,r,x1,y1,r1) - 创建一个径向/圆渐变
```
当我们使用渐变对象，必须使用两种或两种以上的停止颜色。

addColorStop()方法指定颜色停止，参数使用坐标来描述，可以是0至1.

使用渐变，设置fillStyle或strokeStyle的值为 渐变，然后绘制形状，如矩形，文本，或一条线。

使用 createLinearGradient():

创建一个线性渐变。使用渐变填充矩形:

![渐变填充矩形](http://p9myzkds7.bkt.clouddn.com/HTML5/%E4%BD%BF%E7%94%A8%E6%B8%90%E5%8F%98%E5%A1%AB%E5%85%85%E7%9F%A9%E5%BD%A2.png)

JavaScript:

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
 
// 创建渐变
var grd=ctx.createLinearGradient(0,0,200,0);
grd.addColorStop(0,"red");
grd.addColorStop(1,"white");
 
// 填充渐变
ctx.fillStyle=grd;
ctx.fillRect(10,10,150,80);
```
使用 createRadialGradient():

实例
创建一个径向/圆渐变。使用渐变填充矩形：

![使用渐变填充矩形](http://p9myzkds7.bkt.clouddn.com/HTML5/%E5%BE%84%E5%90%91%E3%80%81%E5%9C%86%E6%B8%90%E5%8F%98.png)

JavaScript:

```
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
 
// 创建渐变
var grd=ctx.createRadialGradient(75,50,5,90,60,100);
grd.addColorStop(0,"red");
grd.addColorStop(1,"white");
 
// 填充渐变
ctx.fillStyle=grd;
ctx.fillRect(10,10,150,80);
```
## Canvas - 图像

把一幅图像放置到画布上, 使用以下方法:

- drawImage(image,x,y)

使用图像:

![使用图像](http://p9myzkds7.bkt.clouddn.com/HTML5/%E4%BD%BF%E7%94%A8%E5%9B%BE%E5%83%8F.jpg)

把一幅图像放置到画布上:

![把一幅图像放置到画布上](http://p9myzkds7.bkt.clouddn.com/HTML5/%E5%9B%BE%E5%83%8F%E6%94%BE%E7%BD%AE%E5%88%B0%E7%94%BB%E5%B8%83%E4%B8%8A.png)

JavaScript:

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
var img=document.getElementById("scream");
ctx.drawImage(img,10,10);
```
### HTML Canvas 参考手册

标签的完整属性可以参考 [Canvas 参考手册](http://www.runoob.com/tags/ref-canvas.html).

### HTML `<canvas>` 标签

![<canvas> 标签](http://p9myzkds7.bkt.clouddn.com/HTML5/HTMLcanvas%E6%A0%87%E7%AD%BE.png)
  
## HTML5 内联 SVG

HTML5 支持内联 SVG。

![内联 SVG](http://p9myzkds7.bkt.clouddn.com/HTML5/%E5%86%85%E8%81%94%20SVG.png)

### 什么是SVG？

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用于定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG 是万维网联盟的标准

### SVG优势

与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：

- SVG 图像可通过文本编辑器来创建和修改
- SVG 图像可被搜索、索引、脚本化或压缩
- SVG 是可伸缩的
- SVG 图像可在任何的分辨率下被高质量地打印
- SVG 可在图像质量不下降的情况下被放大

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81.gif)

Internet Explorer 9+, Firefox, Opera, Chrome, 和 Safari 支持内联SVG。

### 把 SVG 直接嵌入 HTML 页面

在 HTML5 中，您能够将 SVG 元素直接嵌入 HTML 页面中：

```html
<!DOCTYPE html>
<html>
<body>
 
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;">
</svg>
 
</body>
</html>
```
结果：

抱歉, 你的浏览器不支持内联SVG.
学习更多关于 SVG 教程, 请访问 [SVG 教程](http://www.runoob.com/svg/svg-tutorial.html).

### SVG 与 Canvas两者间的区别

SVG 是一种使用 XML 描述 2D 图形的语言。

Canvas 通过 JavaScript 来绘制 2D 图形。

SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。

在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

### Canvas 与 SVG 的比较

下表列出了 canvas 与 SVG 之间的一些不同之处。

![不同之处](http://p9myzkds7.bkt.clouddn.com/HTML5/Canvas%20%E4%B8%8E%20SVG%20%E7%9A%84%E6%AF%94%E8%BE%83.png)

## HTML5 MathML

HTML5 可以在文档中使用 MathML 元素，对应的标签是 `<math>`...`</math>` 。

MathML 是数学标记语言，是一种基于XML（标准通用标记语言的子集）的标准，用来在互联网上书写数学符号和公式的置标语言。

> 注意：大部分浏览器都支持 MathML 标签，如果你的浏览器不支持该标签，可以使用最新版的 Firefox 或 Safari 浏览器查看。

### MathML 实例

以下是一个简单的 MathML 实例：

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
      <title>菜鸟教程(runoob.com)</title>
   </head>
    
   <body>
    
      <math xmlns="http://www.w3.org/1998/Math/MathML">
        
         <mrow>
            <msup><mi>a</mi><mn>2</mn></msup>
            <mo>+</mo>
                
            <msup><mi>b</mi><mn>2</mn></msup>
            <mo>=</mo>
                
            <msup><mi>c</mi><mn>2</mn></msup>
         </mrow>
            
      </math>
        
   </body>
</html> 
```
运行结果图，如下所示：

![运行结果图](http://p9myzkds7.bkt.clouddn.com/HTML5/mathml1.jpg)

以下实例添加了一些运算符：

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
      <title>菜鸟教程(runoob.com)</title>
   </head>
    
   <body>
    
      <math xmlns="http://www.w3.org/1998/Math/MathML">
        
         <mrow>            
            <mrow>
                
               <msup>
                  <mi>x</mi>
                  <mn>2</mn>
               </msup>
                    
               <mo>+</mo>
                    
               <mrow>
                  <mn>4</mn>
                  <mo>⁢</mo>
                  <mi>x</mi>
               </mrow>
                    
               <mo>+</mo>
               <mn>4</mn>
                    
            </mrow>
                
            <mo>=</mo>
            <mn>0</mn>
                 
         </mrow>
      </math>
        
   </body>
</html> 
```
运行结果图，如下所示：

![运行结果](http://p9myzkds7.bkt.clouddn.com/HTML5/mathml2-1.jpg)

以下实例是一个 2×2 矩阵，可以在 Firefox 3.5 以上版本查看到效果：

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
      <title>菜鸟教程(runoob.com)</title>
   </head>
    
   <body>
      <math xmlns="http://www.w3.org/1998/Math/MathML">
        
         <mrow>
            <mi>A</mi>
            <mo>=</mo>
            
            <mfenced open="[" close="]">
            
               <mtable>
                  <mtr>
                     <mtd><mi>x</mi></mtd>
                     <mtd><mi>y</mi></mtd>
                  </mtr>
                    
                  <mtr>
                     <mtd><mi>z</mi></mtd>
                     <mtd><mi>w</mi></mtd>
                  </mtr>
               </mtable>
               
            </mfenced>
         </mrow>
      </math>
      
   </body>
</html> 
```
运行结果图，如下所示：

![运行结果图](http://p9myzkds7.bkt.clouddn.com/HTML5/mathml3.jpg)

## HTML5 拖放（Drag 和 Drop）

拖放（Drag 和 drop）是 HTML5 标准的组成部分。

![拖放](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%8B%96%E6%94%BE.png)

将 RUNOOB.COM 图标拖动到矩形框中。

### 拖放

拖放是一种常见的特性，即抓取对象以后拖到另一个位置。

在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 9+, Firefox, Opera, Chrome, 和 Safari 支持拖动。

注意:Safari 5.1.2不支持拖动.

### HTML5 拖放实例

下面的例子是一个简单的拖放实例：

```html
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title>
<style type="text/css">
#div1 {width:350px;height:70px;padding:10px;border:1px solid #aaaaaa;}
</style>
<script>
function allowDrop(ev)
{
    ev.preventDefault();
}
 
function drag(ev)
{
    ev.dataTransfer.setData("Text",ev.target.id);
}
 
function drop(ev)
{
    ev.preventDefault();
    var data=ev.dataTransfer.getData("Text");
    ev.target.appendChild(document.getElementById(data));
}
</script>
</head>
<body>
 
<p>拖动 RUNOOB.COM 图片到矩形框中:</p>
 
<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
<br>
<img id="drag1" src="/images/logo.png" draggable="true" ondragstart="drag(event)" width="336" height="69">
 
</body>
</html>
```
它看上去也许有些复杂，不过我们可以分别研究拖放事件的不同部分。

### 设置元素为可拖放

首先，为了使元素可拖动，把 draggable 属性设置为 true ：

```html
<img draggable="true">
```
### 拖动什么 - ondragstart 和 setData()

然后，规定当元素被拖动时，会发生什么。

在上面的例子中，ondragstart 属性调用了一个函数，drag(event)，它规定了被拖动的数据。

dataTransfer.setData() 方法设置被拖数据的数据类型和值：

```js
function drag(ev)
{
    ev.dataTransfer.setData("Text",ev.target.id);
}
```
在这个例子中，数据类型是 "Text"，值是可拖动元素的 id ("drag1")。

### 放到何处 - ondragover

ondragover 事件规定在何处放置被拖动的数据。

默认地，无法将数据/元素放置到其他元素中。如果需要设置允许放置，我们必须阻止对元素的默认处理方式。

这要通过调用 ondragover 事件的 event.preventDefault() 方法：

```js
event.preventDefault()
```
### 进行放置 - ondrop

当放置被拖数据时，会发生 drop 事件。

在上面的例子中，ondrop 属性调用了一个函数，drop(event)：

```js
function drop(ev)
{
    ev.preventDefault();
    var data=ev.dataTransfer.getData("Text");
    ev.target.appendChild(document.getElementById(data));
}
```
代码解释：

- 调用 preventDefault() 来避免浏览器对数据的默认处理（drop 事件的默认行为是以链接形式打开）
- 通过 dataTransfer.getData("Text") 方法获得被拖的数据。该方法将返回在 setData() 方法中设置为相同类型的任何数据。
- 被拖数据是被拖元素的 id ("drag1")
- 把被拖元素追加到放置元素（目标元素）中

### HTML5 Geolocation（地理定位）

HTML5 Geolocation（地理定位）用于定位用户的位置。

### 定位用户的位置

HTML5 Geolocation API 用于获得用户的地理位置。

鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 9+, Firefox, Chrome, Safari 和 Opera 支持Geolocation（地理定位）.

注意: Geolocation（地理定位）对于拥有 GPS 的设备，比如 iPhone，地理定位更加精确。

### HTML5 - 使用地理定位

请使用 getCurrentPosition() 方法来获得用户的位置。

下例是一个简单的地理定位实例，可返回用户位置的经度和纬度:

```js
var x=document.getElementById("demo");
function getLocation()
{
    if (navigator.geolocation)
    {
        navigator.geolocation.getCurrentPosition(showPosition);
    }
    else
    {
        x.innerHTML="该浏览器不支持获取地理位置。";
    }
}
 
function showPosition(position)
{
    x.innerHTML="纬度: " + position.coords.latitude + 
    "<br>经度: " + position.coords.longitude;    
}
```
实例解析:

- 检测是否支持地理定位
- 如果支持，则运行 getCurrentPosition() 方法。如果不支持，则向用户显示一段消息。
- 如果getCurrentPosition()运行成功，则向参数showPosition中规定的函数返回一个coordinates对象
- showPosition() 函数获得并显示经度和纬度

上面的例子是一个非常基础的地理定位脚本，不含错误处理。

### 处理错误和拒绝

getCurrentPosition() 方法的第二个参数用于处理错误。它规定当获取用户位置失败时运行的函数：

```js
function showError(error)
{
    switch(error.code) 
    {
        case error.PERMISSION_DENIED:
            x.innerHTML="用户拒绝对获取地理位置的请求。"
            break;
        case error.POSITION_UNAVAILABLE:
            x.innerHTML="位置信息是不可用的。"
            break;
        case error.TIMEOUT:
            x.innerHTML="请求用户地理位置超时。"
            break;
        case error.UNKNOWN_ERROR:
            x.innerHTML="未知错误。"
            break;
    }
}
```
错误代码：

- Permission denied - 用户不允许地理定位
- Position unavailable - 无法获取当前位置
- Timeout - 操作超时

### 在地图中显示结果

如需在地图中显示结果，您需要访问可使用经纬度的地图服务，比如谷歌地图或百度地图：

```
function showPosition(position)
{
    var latlon=position.coords.latitude+","+position.coords.longitude;
 
    var img_url="http://maps.googleapis.com/maps/api/staticmap?center="
    +latlon+"&zoom=14&size=400x300&sensor=false";
    document.getElementById("mapholder").innerHTML="<img src='"+img_url+"'>";
}
```
在上例中，我们使用返回的经纬度数据在谷歌地图中显示位置（使用静态图像）。

### Google地图脚本

上面的链接向您演示如何使用脚本来显示带有标记、缩放和拖曳选项的交互式地图。

### 给定位置的信息

本页演示的是如何在地图上显示用户的位置。不过，地理定位对于给定位置的信息同样很有用处。

实例:

- 更新本地信息
- 显示用户周围的兴趣点
- 交互式车载导航系统 (GPS)

### getCurrentPosition() 方法 - 返回数据

若成功，则 getCurrentPosition() 方法返回对象。始终会返回 latitude、longitude 以及 accuracy 属性。如果可用，则会返回其他下面的属性。

![返回的属性](http://p9myzkds7.bkt.clouddn.com/HTML5/%E8%BF%94%E5%9B%9E%E6%95%B0%E6%8D%AE.png)

### Geolocation 对象 - 其他有趣的方法

watchPosition() - 返回用户的当前位置，并继续返回用户移动时的更新位置（就像汽车上的 GPS）。

clearWatch() - 停止 watchPosition() 方法

下面的例子展示 watchPosition() 方法。您需要一台精确的 GPS 设备来测试该例（比如 iPhone）：

```js
var x=document.getElementById("demo");
function getLocation()
{
    if (navigator.geolocation)
    {
        navigator.geolocation.watchPosition(showPosition);
    }
    else
    {
        x.innerHTML="该浏览器不支持获取地理位置。";
    }
}
function showPosition(position)
{
    x.innerHTML="纬度: " + position.coords.latitude + 
    "<br>经度: " + position.coords.longitude; 
}
```
## HTML5 Video(视频)

很多站点都会使用到视频. HTML5 提供了展示视频的标准。

### Web站点上的视频

直到现在，仍然不存在一项旨在网页上显示视频的标准。

今天，大多数视频是通过插件（比如 Flash）来显示的。然而，并非所有浏览器都拥有同样的插件。

HTML5 规定了一种通过 video 元素来包含视频的标准方法。

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 9+, Firefox, Opera, Chrome, 和 Safari 支持 `<video>` 元素.

注意: Internet Explorer 8 或者更早的IE版本不支持 `<video>` 元素。
 
### HTML5 (视频)- 如何工作

如需在 HTML5 中显示视频，您所有需要的是：

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
您的浏览器不支持Video标签。
</video>
```
`<video>` 元素提供了 播放、暂停和音量控件来控制视频。

同时 `<video>` 元素也提供了 width 和 height 属性控制视频的尺寸.如果设置的高度和宽度，所需的视频空间会在页面加载时保留。如果没有设置这些属性，浏览器不知道大小的视频，浏览器就不能再加载时保留特定的空间，页面就会根据原始视频的大小而改变。

`<video>` 与`</video>` 标签之间插入的内容是提供给不支持 video 元素的浏览器显示的。

`<video>` 元素支持多个 `<source>` 元素. `<source>` 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式：

视频格式与浏览器的支持
当前， `<video>` 元素支持三种视频格式： MP4, WebM, 和 Ogg:

![视频格式](http://p9myzkds7.bkt.clouddn.com/HTML5/%E8%A7%86%E9%A2%91%E6%A0%BC%E5%BC%8F%E4%B8%8E%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E6%94%AF%E6%8C%81.png)

- MP4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
- WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件
- Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件

### 视频格式

![视频格式](http://p9myzkds7.bkt.clouddn.com/HTML5/%E8%A7%86%E9%A2%91%E6%A0%BC%E5%BC%8F.png)

### HTML5 `<video>` - 使用 DOM 进行控制

HTML5 `<video>` 和 `<audio>` 元素同样拥有方法、属性和事件。

`<video>` 和 `<audio>`元素的方法、属性和事件可以使用JavaScript进行控制.

其中的方法用于播放、暂停以及加载等。其中的属性（比如时长、音量等）可以被读取或设置。其中的 DOM 事件能够通知您，比方说，`<video>` 元素开始播放、已暂停，已停止，等等。

例中简单的方法，向我们演示了如何使用 `<video>` 元素，读取并设置属性，以及如何调用方法。

为视频创建简单的播放/暂停以及调整尺寸控件：

[简单的播放/暂停](http://www.runoob.com/try/try.php?filename=tryhtml5_video_js_prop)

上面的例子调用了两个方法：play() 和 pause()。它同时使用了两个属性：paused 和 width。

更多参考请查看 [HTML5 Audio/Video DOM 参考手册](http://www.runoob.com/tags/ref-av-dom.html)。

### HTML5 Video 标签

![HTML5 Video 标签](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%20Video%20%E6%A0%87%E7%AD%BE.png)

## HTML5 Audio(音频)

HTML5 提供了播放音频文件的标准。

### 互联网上的音频

直到现在，仍然不存在一项旨在网页上播放音频的标准。

今天，大多数音频是通过插件（比如 Flash）来播放的。然而，并非所有浏览器都拥有同样的插件。

HTML5 规定了在网页上嵌入音频元素的标准，即使用 `<audio>` 元素。

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 9+, Firefox, Opera, Chrome, 和 Safari 都支持 `<audio>` 元素.

注意: Internet Explorer 8 及更早IE版本不支持 `<audio>` 元素.

### HTML5 Audio - 如何工作

如需在 HTML5 中播放音频，你需要使用以下代码：

```html
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
您的浏览器不支持 audio 元素。
</audio>
```
control 属性供添加播放、暂停和音量控件。

在`<audio>` 与 `</audio>` 之间你需要插入浏览器不支持的`<audio>`元素的提示文本 。

`<audio>` 元素允许使用多个 `<source>` 元素. `<source>` 元素可以链接不同的音频文件，浏览器将使用第一个支持的音频文件

### 音频格式及浏览器支持

目前, `<audio>`元素支持三种音频格式文件: MP3, Wav, 和 Ogg:

![音频格式及浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E9%9F%B3%E9%A2%91%E6%A0%BC%E5%BC%8F%E5%8F%8A%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81.png)

### 音频格式的MIME类型

![音频格式的MIME类型](http://p9myzkds7.bkt.clouddn.com/HTML5/%E9%9F%B3%E9%A2%91%E6%A0%BC%E5%BC%8F%E7%9A%84MIME%E7%B1%BB%E5%9E%8B.png)

### HTML5 Audio 标签

![HTML5 Audio 标签](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%20Audio%20%E6%A0%87%E7%AD%BE.png)

## HTML5 新的 Input 类型

HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。

本章全面介绍这些新的输入类型：

- color
- date
- datetime
- datetime-local
- email
- month
- number
- range
- search
- tel
- time
- url
- week

注意:并不是所有的主流浏览器都支持新的input类型，不过您已经可以在所有主流的浏览器中使用它们了。即使不被支持，仍然可以显示为常规的文本域。

### Input 类型: color

color 类型用在input字段主要用于选取颜色，如下所示：

从拾色器中选择一个颜色:

```html
选择你喜欢的颜色: <input type="color" name="favcolor">
```

### Input 类型: date

date 类型允许你从一个日期选择器选择一个日期。

定义一个时间控制器:

```html
生日: <input type="date" name="bday">
```
### Input 类型: datetime

datetime 类型允许你选择一个日期（UTC 时间）。

定义一个日期和时间控制器（本地时间）:

```html
生日 (日期和时间): <input type="datetime" name="bdaytime">
```
### Input 类型: datetime-local

datetime-local 类型允许你选择一个日期和时间 (无时区).

定义一个日期和时间 (无时区):

```html
生日 (日期和时间): <input type="datetime-local" name="bdaytime">
```
### Input 类型: email

email 类型用于应该包含 e-mail 地址的输入域。

在提交表单时，会自动验证 email 域的值是否合法有效:

```html
E-mail: <input type="email" name="email">
```
提示: iPhone 中的 Safari 浏览器支持 email 输入类型，并通过改变触摸屏键盘来配合它（添加 @ 和 .com 选项）。

### Input 类型: month

month 类型允许你选择一个月份。

定义月与年 (无时区):

```html
生日 (月和年): <input type="month" name="bdaymonth">
```
### Input 类型: number

number 类型用于应该包含数值的输入域。

您还能够设定对所接受的数字的限定：

定义一个数值输入域(限定):

```html
数量 ( 1 到 5 之间 ): <input type="number" name="quantity" min="1" max="5">
```
使用下面的属性来规定对数字类型的限定：

![数字类型的限定](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%95%B0%E5%AD%97%E7%B1%BB%E5%9E%8B%E7%9A%84%E9%99%90%E5%AE%9A.png)

尝试一下带有所有限定属性的例子 [尝试一下](http://www.runoob.com/try/try.php?filename=tryhtml5_form_number_adv)

### Input 类型: range

range 类型用于应该包含一定范围内数字值的输入域。

range 类型显示为滑动条。

定义一个不需要非常精确的数值（类似于滑块控制）:

```html
<input type="range" name="points" min="1" max="10">
```
请使用下面的属性来规定对数字类型的限定：

- max - 规定允许的最大值
- min - 规定允许的最小值
- step - 规定合法的数字间隔
- value - 规定默认值
- Input 类型: search
- search 类型用于搜索域，比如站点搜索或 Google 搜索。

定义一个搜索字段 (类似站点搜索或者Google搜索):

```html
Search Google: <input type="search" name="googlesearch">
```
### Input 类型: tel

定义输入电话号码字段:

```html
电话号码: <input type="tel" name="usrtel">
```
### Input 类型: time

time 类型允许你选择一个时间。

定义可输入时间控制器（无时区）：

```html
选择时间: <input type="time" name="usr_time">
```
### Input 类型: url

url 类型用于应该包含 URL 地址的输入域。

在提交表单时，会自动验证 url 域的值。

定义输入URL字段:

```html
添加您的主页: <input type="url" name="homepage">
```
提示: iPhone 中的 Safari 浏览器支持 url 输入类型，并通过改变触摸屏键盘来配合它（添加 .com 选项）。

### Input 类型: week

week 类型允许你选择周和年。

定义周和年 (无时区):

```html
选择周: <input type="week" name="week_year">
```

### HTML5 `<input>` 标签

![HTML5 <input> 标签](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%20input%20%E6%A0%87%E7%AD%BE.png)

## HTML5 表单元素

### HTML5 新的表单元素

HTML5 有以下新的表单元素:

- `<datalist>`
- `<keygen>`
- `<output>`
  
注意:不是所有的浏览器都支持HTML5 新的表单元素，但是你可以在使用它们，即使浏览器不支持表单属性，仍然可以显示为常规的表单元素。

### HTML5 `<datalist>` 元素

`<datalist>` 元素规定输入域的选项列表。

`<datalist>` 属性规定 form 或 input 域应该拥有自动完成功能。当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项：

使用 `<input>` 元素的列表属性与 `<datalist>` 元素绑定.

`<input>` 元素使用`<datalist>`预定义值:

```html
<input list="browsers">
 
<datalist id="browsers">
  <option value="Internet Explorer">
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
```
### HTML5 `<keygen>` 元素

`<keygen>` 元素的作用是提供一种验证用户的可靠方法。

`<keygen>`标签规定用于表单的密钥对生成器字段。

当提交表单时，会生成两个键，一个是私钥，一个公钥。

私钥（private key）存储于客户端，公钥（public key）则被发送到服务器。公钥可用于之后验证用户的客户端证书（client certificate）。

带有keygen字段的表单:

```html
<form action="demo_keygen.asp" method="get">
用户名: <input type="text" name="usr_name">
加密: <keygen name="security">
<input type="submit">
</form>
```
### HTML5 `<output>` 元素

`<output>` 元素用于不同类型的输出，比如计算或脚本输出：

将计算结果显示在 `<output>` 元素:

```html
<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">0
<input type="range" id="a" value="50">100 +
<input type="number" id="b" value="50">=
<output name="x" for="a b"></output>
</form>
```
### HTML5 新表单元素

![HTML5 新表单元素](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%20%E6%96%B0%E8%A1%A8%E5%8D%95%E5%85%83%E7%B4%A0.png)

## HTML5 表单属性

### HTML5 新的表单属性

HTML5 的 `<form>` 和 `<input>`标签添加了几个新属性.

`<form>`新属性：

- autocomplete
- novalidate

`<input>` 新属性：

- autocomplete
- autofocus
- form
- formaction
- formenctype
- formmethod
- formnovalidate
- formtarget	
- height 与 width
- list
- min 与 max
- multiple
- pattern (regexp)
- placeholder
- required
- step

### `<form>` / `<input>` autocomplete 属性

autocomplete 属性规定 form 或 input 域应该拥有自动完成功能。

当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项。

提示: autocomplete 属性有可能在 form元素中是开启的，而在input元素中是关闭的。

注意: autocomplete 适用于 `<form>` 标签，以及以下类型的 `<input>` 标签：text, search, url, telephone, email, password, datepickers, range 以及 color。

HTML form 中开启 autocomplete (一个 input 字段关闭 autocomplete ):

```html
<form action="demo-form.php" autocomplete="on">
  First name:<input type="text" name="fname"><br>
  Last name: <input type="text" name="lname"><br>
  E-mail: <input type="email" name="email" autocomplete="off"><br>
  <input type="submit">
</form>
```
提示:某些浏览器中，您可能需要启用自动完成功能，以使该属性生效。

### `<form>` novalidate 属性

novalidate 属性是一个 boolean(布尔) 属性.

novalidate 属性规定在提交表单时不应该验证 form 或 input 域。

无需验证提交的表单数据

```html
<form action="demo-form.php" novalidate>
  E-mail: <input type="email" name="user_email">
  <input type="submit">
</form>
```
### `<input>` autofocus 属性

autofocus 属性是一个 boolean 属性.

autofocus 属性规定在页面加载时，域自动地获得焦点。

让 "First name" input 输入域在页面载入时自动聚焦：

```html
First name:<input type="text" name="fname" autofocus>
```
### `<input>` form 属性

form 属性规定输入域所属的一个或多个表单。

提示:如需引用一个以上的表单，请使用空格分隔的列表。

位于form表单外的 input 字段引用了 HTML form (该 input 表单仍然属于form表单的一部分):

```html
<form action="demo-form.php" id="form1">
  First name: <input type="text" name="fname"><br>
  <input type="submit" value="提交">
</form>
 
Last name: <input type="text" name="lname" form="form1">
```

### `<input>` formaction 属性

The formaction 属性用于描述表单提交的URL地址.

The formaction 属性会覆盖`<form>` 元素中的action属性.

注意: The formaction 属性用于 type="submit" 和 type="image".

以下HTMLform表单包含了两个不同地址的提交按钮：

```html
<form action="demo-form.php">
  First name: <input type="text" name="fname"><br>
  Last name: <input type="text" name="lname"><br>
  <input type="submit" value="提交"><br>
  <input type="submit" formaction="demo-admin.php"
  value="提交">
</form>
```

### `<input>` formenctype 属性

formenctype 属性描述了表单提交到服务器的数据编码 (只对form表单中 method="post" 表单)

formenctype 属性覆盖 form 元素的 enctype 属性。

主要: 该属性与 type="submit" 和 type="image" 配合使用。

第一个提交按钮已默认编码发送表单数据，第二个提交按钮以 "multipart/form-data" 编码格式发送表单数据:

```html
<form action="demo-post_enctype.php" method="post">
  First name: <input type="text" name="fname"><br>
  <input type="submit" value="提交">
  <input type="submit" formenctype="multipart/form-data"
  value="以 Multipart/form-data 提交">
</form>
```
### `<input>` formmethod 属性

formmethod 属性定义了表单提交的方式。

formmethod 属性覆盖了 `<form>` 元素的 method 属性。

注意: 该属性可以与 type="submit" 和 type="image" 配合使用。

重新定义表单提交方式实例:

```html
<form action="demo-form.php" method="get">
  First name: <input type="text" name="fname"><br>
  Last name: <input type="text" name="lname"><br>
  <input type="submit" value="提交">
  <input type="submit" formmethod="post" formaction="demo-post.php"
  value="使用 POST 提交">
</form>
```
### `<input>` formnovalidate 属性

novalidate 属性是一个 boolean 属性.

novalidate属性描述了 `<input>` 元素在表单提交时无需被验证。

formnovalidate 属性会覆盖 `<form>` 元素的novalidate属性.

注意: formnovalidate 属性与type="submit一起使用

两个提交按钮的表单(使用与不适用验证 ):

```html
<form action="demo-form.php">
  E-mail: <input type="email" name="userid"><br>
  <input type="submit" value="提交"><br>
  <input type="submit" formnovalidate value="不验证提交">
</form>
```
### `<input>` formtarget 属性

formtarget 属性指定一个名称或一个关键字来指明表单提交数据接收后的展示。

The formtarget 属性覆盖 `<form>` 元素的target属性.

注意: formtarget 属性与type="submit" 和 type="image"配合使用.

两个提交按钮的表单, 在不同窗口中显示:

```html
<form action="demo-form.php">
  First name: <input type="text" name="fname"><br>
  Last name: <input type="text" name="lname"><br>
  <input type="submit" value="正常提交">
  <input type="submit" formtarget="_blank"
  value="提交到一个新的页面上">
</form>
```
### `<input>` height 和 width 属性

height 和 width 属性规定用于 image 类型的 `<input>` 标签的图像高度和宽度。

> 注意: height 和 width 属性只适用于 image 类型的`<input>` 标签。

提示:图像通常会同时指定高度和宽度属性。如果图像设置高度和宽度，图像所需的空间 在加载页时会被保留。如果没有这些属性， 浏览器不知道图像的大小，并不能预留 适当的空间。图片在加载过程中会使页面布局效果改变 （尽管图片已加载）。

定义了一个图像提交按钮, 使用了 height 和 width 属性:

```html
<input type="image" src="img_submit.gif" alt="Submit" width="48" height="48">
```
### `<input>` list 属性

list 属性规定输入域的 datalist。datalist 是输入域的选项列表。

在 `<datalist>`中预定义 `<input>` 值:

```html
<input list="browsers">

<datalist id="browsers">
  <option value="Internet Explorer">
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
```
### `<input>` min 和 max 属性

min、max 和 step 属性用于为包含数字或日期的 input 类型规定限定（约束）。

注意: min、max 和 step 属性适用于以下类型的 `<input>` 标签：date pickers、number 以及 range。

### `<input>` 元素最小值与最大值设置:

```html
Enter a date before 1980-01-01:
<input type="date" name="bday" max="1979-12-31">

Enter a date after 2000-01-01:
<input type="date" name="bday" min="2000-01-02">

Quantity (between 1 and 5):
<input type="number" name="quantity" min="1" max="5">
```
### `<input>` multiple 属性

multiple 属性是一个 boolean 属性.

multiple 属性规定`<input>` 元素中可选择多个值。

注意: multiple 属性适用于以下类型的 `<input>` 标签：email 和 file:

上传多个文件:

```html
Select images: <input type="file" name="img" multiple>
```

### `<input>` pattern 属性

pattern 属性描述了一个正则表达式用于验证 `<input>` 元素的值。

注意:pattern 属性适用于以下类型的 `<input>` 标签: text, search, url, tel, email, 和 password.

提示： 是用来全局 title 属性描述了模式.

提示： 您可以在我们的 JavaScript 教程中学习到有关正则表达式的内容

下面的例子显示了一个只能包含三个字母的文本域（不含数字及特殊字符）：

```html
Country code: <input type="text" name="country_code" pattern="[A-Za-z]{3}" title="Three letter country code">
```
### `<input>` placeholder 属性

placeholder 属性提供一种提示（hint），描述输入域所期待的值。

简短的提示在用户输入值前会显示在输入域上。

注意: placeholder 属性适用于以下类型的 `<input>` 标签：text, search, url, telephone, email 以及 password。

input 字段提示文本t:

```html
<input type="text" name="fname" placeholder="First name">
```
### `<input>` required 属性

required 属性是一个 boolean 属性.

required 属性规定必须在提交之前填写输入域（不能为空）。

注意:required 属性适用于以下类型的 `<input>` 标签：text, search, url, telephone, email, password, date pickers, number, checkbox, radio 以及 file。

不能为空的input字段:

```html
Username: <input type="text" name="usrname" required>
```
### `<input>` step 属性

step 属性为输入域规定合法的数字间隔。

如果 step="3"，则合法的数是 -3,0,3,6 等

提示： step 属性可以与 max 和 min 属性创建一个区域值.

注意: step 属性与以下type类型一起使用: number, range, date, datetime, datetime-local, month, time 和 week.

规定input step步长为3:

```html
<input type="number" name="points" step="3">
```

### HTML5 `<input>` 标签

![HTML5 <input> 标签](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%20input%20%E6%A0%87%E7%AD%BE-1.png)

## HTML5 语义元素


语义= 意义

语义元素 = 有意义的元素

什么是语义元素?
一个语义元素能够清楚的描述其意义给浏览器和开发者。

无语义 元素实例: `<div>` 和 `<span>` - 无需考虑内容.

语义元素实例: `<form>`, `<table>`, and `<img>` - 清楚的定义了它的内容.

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 9+, Firefox, Chrome, Safari 和 Opera 支持语义元素。

注意: Internet Explorer 8 及更早版本不支持该元素。 但是文章底部提供了兼容的解决方法.

### HTML5中新的语义元素

许多现有网站都包含以下HTML代码： `<div id="nav">`, `<div class="header">`, 或者 `<div id="footer">`, 来指明导航链接, 头部, 以及尾部.

HTML5 提供了新的语义元素来明确一个Web页面的不同部分:

- `<header>`
- `<nav>`
- `<section>`
- `<article>`
- `<aside>`
- `<figcaption>`
- `<figure>`
- `<footer>`

![Web页面的不同部分](http://p9myzkds7.bkt.clouddn.com/HTML5/img_sem_elements.gif)

### HTML5 `<section>` 元素

`<section>` 标签定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。

根据W3C HTML5文档: section 包含了一组内容及其标题。

```html
<section>
  <h1>WWF</h1>
  <p>The World Wide Fund for Nature (WWF) is....</p>
</section>
```

### HTML5 `<article>` 元素

`<article>` 标签定义独立的内容。.

`<article>` 元素使用实例:

- Forum post
- Blog post
- News story
- Comment

```html
<article>
  <h1>Internet Explorer 9</h1>
  <p>Windows Internet Explorer 9(缩写为 IE9 )在2011年3月14日21:00 发布。</p>
</article>
```
### HTML5 `<nav>` 元素

`<nav>` 标签定义导航链接的部分。

`<nav>` 元素用于定义页面的导航链接部分区域，但是，不是所有的链接都需要包含在 `<nav>` 元素中!

```html
<nav>
    <a href="/html/">HTML</a> |
    <a href="/css/">CSS</a> |
    <a href="/js/">JavaScript</a> |
    <a href="/jquery/">jQuery</a>
</nav>
```

### HTML5 `<aside>` 元素

`<aside>` 标签定义页面主区域内容之外的内容（比如侧边栏）。

aside 标签的内容应与主区域的内容相关.

```html
<p>My family and I visited The Epcot center this summer.</p>
 
<aside>
  <h4>Epcot Center</h4>
  <p>The Epcot Center is a theme park in Disney World, Florida.</p>
</aside>
```
### HTML5 `<header>` 元素

`<header>`元素描述了文档的头部区域

`<header>`元素主要用于定义内容的介绍展示区域.

在页面中你可以使用多个`<header>` 元素.

以下实例定义了文章的头部:

```html
<article>
  <header>
    <h1>Internet Explorer 9</h1>
    <p><time pubdate datetime="2011-03-15"></time></p>
  </header>
  <p>Windows Internet Explorer 9(缩写为 IE9 )是在2011年3月14日21:00发布的</p>
</article>
```
### HTML5 `<footer>` 元素

`<footer>` 元素描述了文档的底部区域.

`<footer>` 元素应该包含它的包含元素

一个页脚通常包含文档的作者，著作权信息，链接的使用条款，联系信息等

文档中你可以使用多个 `<footer>`元素.

```html
<footer>
  <p>Posted by: Hege Refsnes</p>
  <p><time pubdate datetime="2012-03-01"></time></p>
</footer>
```

### HTML5 `<figure>` 和 `<figcaption>` 元素

`<figure>`标签规定独立的流内容（图像、图表、照片、代码等等）。

`<figure>` 元素的内容应该与主内容相关，但如果被删除，则不应对文档流产生影响。

`<figcaption>` 标签定义 `<figure>` 元素的标题.

`<figcaption>`元素应该被置于 "figure" 元素的第一个或最后一个子元素的位置。

```html
<figure>
  <img src="img_pulpit.jpg" alt="The Pulpit Rock" width="304" height="228">
  <figcaption>Fig1. - The Pulpit Pock, Norway.</figcaption>
</figure>
```
我们可以开始使用这些语义元素吗?
以上的元素都是块元素(除了`<figcaption>`).

为了让这些块及元素在所有版本的浏览器中生效，你需要在样式表文件中设置一下属性 (以下样式代码可以让旧版本浏览器支持本章介绍的块级元素):

```css
header, section, footer, aside, nav, article, figure
{ 
    display: block; 
}
```
### Internet Explorer 8 及更早IE版本中的问题

IE8 及更早IE版本无法在这些元素中渲染CSS效果，以至于你不能使用 `<header>`, `<section>`, `<footer>`, `<aside>`, `<nav>`, `<article>`, `<figure>`, 或者其他的HTML5 elements.

解决办法: 你可以使用HTML5 Shiv Javascript脚本来解决IE的兼容问题。HTML5 Shiv下载地址：http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js

下载后，将以下代码放入到网页中：

```html
<!--[if lt IE 9]>
<script src="html5shiv.js"></script>
<![endif]-->
```
以上代码在浏览器小于IE9版本时会加载html5shiv.js文件. 你必须将其放置于`<head>` 元素中，因为 IE浏览器需要在头部加载后渲染这些HTML5的新元素

## HTML5 Web 存储


HTML5 web 存储,一个比cookie更好的本地存储方式。

### b什么是 HTML5 Web 存储?

使用HTML5可以在本地存储用户的浏览数据。

早些时候,本地存储使用的是 cookie。但是Web 存储需要更加的安全与快速. 这些数据不会被保存在服务器上，但是这些数据只用于用户请求网站数据上.它也可以存储大量的数据，而不影响网站的性能.

数据以 键/值 对存在, web网页的数据只允许该网页访问使用。

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 8+, Firefox, Opera, Chrome, 和 Safari支持Web 存储。

注意: Internet Explorer 7 及更早IE版本不支持web 存储.

### localStorage 和 sessionStorage 

客户端存储数据的两个对象为：

- localStorage - 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除。
- sessionStorage - 用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

在使用 web 存储前,应检查浏览器是否支持 localStorage 和sessionStorage:

```js
if(typeof(Storage)!=="undefined")
{
    // 是的! 支持 localStorage  sessionStorage 对象!
    // 一些代码.....
} else {
    // 抱歉! 不支持 web 存储。
}
```
### localStorage 对象

localStorage 对象存储的数据没有时间限制。第二天、第二周或下一年之后，数据依然可用。

```js
localStorage.sitename="菜鸟教程";
document.getElementById("result").innerHTML="网站名：" + localStorage.sitename;
```
实例解析：

- 使用 key="sitename" 和 value="菜鸟教程" 创建一个 localStorage 键/值对。
- 检索键值为"sitename" 的值然后将数据插入 id="result"的元素中。

以上实例也可以这么写：

```js
// 存储
localStorage.sitename = "菜鸟教程";
// 查找
document.getElementById("result").innerHTML = localStorage.sitename;
```

移除 localStorage 中的 ""sitename" :

```js
localStorage.removeItem(""sitename");
```
不管是 localStorage，还是 sessionStorage，可使用的API都相同，常用的有如下几个（以localStorage为例）：

- 保存数据：localStorage.setItem(key,value);
- 读取数据：localStorage.getItem(key);
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 得到某个索引的key：localStorage.key(index);

提示: 键/值对通常以字符串存储，你可以按自己的需要转换该格式。

下面的实例展示了用户点击按钮的次数。

代码中的字符串值转换为数字类型:

```html
if (localStorage.clickcount)
{
    localStorage.clickcount=Number(localStorage.clickcount)+1;
}
else
{
    localStorage.clickcount=1;
}
document.getElementById("result").innerHTML=" 你已经点击了按钮 " + localStorage.clickcount + " 次 ";
```
### sessionStorage 对象

sessionStorage 方法针对一个 session 进行数据存储。当用户关闭浏览器窗口后，数据会被删除。

如何创建并访问一个 sessionStorage：

```
if (sessionStorage.clickcount)
{
    sessionStorage.clickcount=Number(sessionStorage.clickcount)+1;
}
else
{
    sessionStorage.clickcount=1;
}
document.getElementById("result").innerHTML="在这个会话中你已经点击了该按钮 " + sessionStorage.clickcount + " 次 ";
```
### Web Storage 开发一个简单的网站列表程序

网站列表程序实现以下功能：

- 可以输入网站名，网址，以网站名作为key存入localStorage；
- 根据网站名，查找网址；
- 列出当前已保存的所有网站；

以下代码用于保存于查找数据：

save() 与 find() 方法

```js
//保存数据  
function save(){  
    var siteurl = document.getElementById("siteurl").value;  
    var sitename = document.getElementById("sitename").value;  
    localStorage.setItem(sitename, siteurl);
    alert("添加成功");
}
//查找数据  
function find(){  
    var search_site = document.getElementById("search_site").value;  
    var sitename = localStorage.getItem(search_site);  
    var find_result = document.getElementById("find_result");  
    find_result.innerHTML = search_site + "的网址是：" + sitename;  
}
```
完整实例演示如下：

```html
<div style="border: 2px dashed #ccc;width:320px;text-align:center;">     
    <label for="sitename">网站名(key)：</label>  
    <input type="text" id="sitename" name="sitename" class="text"/>  
    <br/>  
    <label for="siteurl">网 址(value)：</label>  
    <input type="text" id="siteurl" name="siteurl"/>  
    <br/>  
    <input type="button" onclick="save()" value="新增记录"/>  
    <hr/>  
    <label for="search_site">输入网站名：</label>  
    <input type="text" id="search_site" name="search_site"/>  
    <input type="button" onclick="find()" value="查找网站"/>  
    <p id="find_result"><br/></p>  
</div>
```
实现效果截图：

![实现效果截图](http://p9myzkds7.bkt.clouddn.com/HTML5/local11.png)

以上实例只是演示了简单的 localStorage 存储与查找，更多情况下我们存储的数据会更复杂。

接下来我们将使用 JSON.stringify 来存储对象数据，JSON.stringify 可以将对象转换为字符串。

```js
var site = new Object;
...
var str = JSON.stringify(site); // 将对象转换为字符串
```

之后我们使用 JSON.parse 方法将字符串转换为 JSON 对象：

```js
var site = JSON.parse(str);
```
JavaScript 实现代码：

save() 与 find() 方法

```js
//保存数据  
function save(){  
    var site = new Object;
    site.keyname = document.getElementById("keyname").value;
    site.sitename = document.getElementById("sitename").value;  
    site.siteurl = document.getElementById("siteurl").value;
    var str = JSON.stringify(site); // 将对象转换为字符串
    localStorage.setItem(site.keyname,str);  
    alert("保存成功");
}  
//查找数据  
function find(){  
    var search_site = document.getElementById("search_site").value;  
    var str = localStorage.getItem(search_site);  
    var find_result = document.getElementById("find_result");
    var site = JSON.parse(str);  
    find_result.innerHTML = search_site + "的网站名是：" + site.sitename + "，网址是：" + site.siteurl;  
}
```
完整实例如下：

```html
<div style="border: 2px dashed #ccc;width:320px;text-align:center;">
    <label for="keyname">别名(key):</label>  
    <input type="text" id="keyname" name="keyname" class="text"/>  
    <br/>  
    <label for="sitename">网站名：</label>  
    <input type="text" id="sitename" name="sitename" class="text"/>  
    <br/>  
    <label for="siteurl">网 址：</label>  
    <input type="text" id="siteurl" name="siteurl"/>  
    <br/>  
    <input type="button" onclick="save()" value="新增记录"/>  
    <hr/>  
    <label for="search_site">输入别名(key)：</label>  
    <input type="text" id="search_site" name="search_site"/>  
    <input type="button" onclick="find()" value="查找网站"/>  
    <p id="find_result"><br/></p>  
</div>
```
实例中的 loadAll 输出了所有存储的数据，你需要确保 localStorage 存储的数据都为 JSON 格式，否则 JSON.parse(str) 将会报错。

输出结果演示：

![输出结果演示](http://p9myzkds7.bkt.clouddn.com/HTML5/08572F9A-A2E7-4752-BE1B-D66E2C3B36C9.jpg)

## HTML5 Web SQL 数据库

Web SQL 数据库 API 并不是 HTML5 规范的一部分，但是它是一个独立的规范，引入了一组使用 SQL 操作客户端数据库的 APIs。

如果你是一个 Web 后端程序员，应该很容易理解 SQL 的操作。

你也可以参考我们的 SQL 教程，了解更多数据库操作知识。

Web SQL 数据库可以在最新版的 Safari, Chrome 和 Opera 浏览器中工作。

### 核心方法

以下是规范中定义的三个核心方法：

- openDatabase：这个方法使用现有的数据库或者新建的数据库创建一个数据库对象。
- transaction：这个方法让我们能够控制一个事务，以及基于这种情况执行提交或者回滚。
- executeSql：这个方法用于执行实际的 SQL 查询。

### 打开数据库

我们可以使用 openDatabase() 方法来打开已存在的数据库，如果数据库不存在，则会创建一个新的数据库，使用代码如下：

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
openDatabase() 方法对应的五个参数说明：
```
- 数据库名称
- 版本号
- 描述文本
- 数据库大小
- 创建回调

第五个参数，创建回调会在创建数据库后被调用。

### 执行查询操作

执行操作使用 database.transaction() 函数：

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (tx) {  
   tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
});
```
上面的语句执行后会在 'mydb' 数据库中创建一个名为 LOGS 的表。

### 插入数据

在执行上面的创建表语句后，我们可以插入一些数据：

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (tx) {
   tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
   tx.executeSql('INSERT INTO LOGS (id, log) VALUES (1, "菜鸟教程")');
   tx.executeSql('INSERT INTO LOGS (id, log) VALUES (2, "www.runoob.com")');
});
```
我们也可以使用动态值来插入数据：

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (tx) {  
  tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
  tx.executeSql('INSERT INTO LOGS (id,log) VALUES (?, ?)', [e_id, e_log]);
});
```
实例中的 e_id 和 e_log 是外部变量，executeSql 会映射数组参数中的每个条目给 "?"。

### 读取数据

以下实例演示了如何读取数据库中已经存在的数据：

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
 
db.transaction(function (tx) {
   tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
   tx.executeSql('INSERT INTO LOGS (id, log) VALUES (1, "菜鸟教程")');
   tx.executeSql('INSERT INTO LOGS (id, log) VALUES (2, "www.runoob.com")');
});
 
db.transaction(function (tx) {
   tx.executeSql('SELECT * FROM LOGS', [], function (tx, results) {
      var len = results.rows.length, i;
      msg = "<p>查询记录条数: " + len + "</p>";
      document.querySelector('#status').innerHTML +=  msg;
    
      for (i = 0; i < len; i++){
         alert(results.rows.item(i).log );
      }
    
   }, null);
});
```
完整实例

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
var msg;
 
db.transaction(function (tx) {
    tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (1, "菜鸟教程")');
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (2, "www.runoob.com")');
    msg = '<p>数据表已创建，且插入了两条数据。</p>';
    document.querySelector('#status').innerHTML =  msg;
});
 
db.transaction(function (tx) {
tx.executeSql('SELECT * FROM LOGS', [], function (tx, results) {
    var len = results.rows.length, i;
    msg = "<p>查询记录条数: " + len + "</p>";
    document.querySelector('#status').innerHTML +=  msg;
 
    for (i = 0; i < len; i++){
        msg = "<p><b>" + results.rows.item(i).log + "</b></p>";
        document.querySelector('#status').innerHTML +=  msg;
    }
}, null);
});
```
以上实例运行结果如下图所示：

![以上实例运行结果](http://p9myzkds7.bkt.clouddn.com/HTML5/websql.jpg)

### 删除记录

删除记录使用的格式如下：

```js
db.transaction(function (tx) {
    tx.executeSql('DELETE FROM LOGS  WHERE id=1');
});
```
删除指定的数据id也可以是动态的：

```js
db.transaction(function(tx) {
    tx.executeSql('DELETE FROM LOGS WHERE id=?', [id]);
});
```
### 更新记录

更新记录使用的格式如下：

```js
db.transaction(function (tx) {
    tx.executeSql('UPDATE LOGS SET log=\'www.w3cschool.cc\' WHERE id=2');
});
```
更新指定的数据id也可以是动态的：

```js
db.transaction(function(tx) {
    tx.executeSql('UPDATE LOGS SET log=\'www.w3cschool.cc\' WHERE id=?', [id]);
});
```
完整实例

```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
var msg;
 
 db.transaction(function (tx) {
    tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (1, "菜鸟教程")');
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (2, "www.runoob.com")');
    msg = '<p>数据表已创建，且插入了两条数据。</p>';
    document.querySelector('#status').innerHTML =  msg;
 });
 
 db.transaction(function (tx) {
      tx.executeSql('DELETE FROM LOGS  WHERE id=1');
      msg = '<p>删除 id 为 1 的记录。</p>';
      document.querySelector('#status').innerHTML =  msg;
 });
 
 db.transaction(function (tx) {
     tx.executeSql('UPDATE LOGS SET log=\'www.w3cschool.cc\' WHERE id=2');
      msg = '<p>更新 id 为 2 的记录。</p>';
      document.querySelector('#status').innerHTML =  msg;
 });
 
 db.transaction(function (tx) {
    tx.executeSql('SELECT * FROM LOGS', [], function (tx, results) {
       var len = results.rows.length, i;
       msg = "<p>查询记录条数: " + len + "</p>";
       document.querySelector('#status').innerHTML +=  msg;
       
       for (i = 0; i < len; i++){
          msg = "<p><b>" + results.rows.item(i).log + "</b></p>";
          document.querySelector('#status').innerHTML +=  msg;
       }
    }, null);
 });
```
以上实例运行结果如下图所示：

![实例运行结果](http://p9myzkds7.bkt.clouddn.com/HTML5/8E8CF48D-A590-4577-B338-715C0034D029.png)

## HTML5 应用程序缓存

使用 HTML5，通过创建 cache manifest 文件，可以轻松地创建 web 应用的离线版本。

### 什么是应用程序缓存（Application Cache）？

HTML5 引入了应用程序缓存，这意味着 web 应用可进行缓存，并可在没有因特网连接时进行访问。

应用程序缓存为应用带来三个优势：

1. 离线浏览 - 用户可在应用离线时使用它们
2. 速度 - 已缓存资源加载得更快
3. 减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源。

### 浏览器支持

![浏览器支持](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81-%E6%8B%96%E6%94%BE.png)

Internet Explorer 10, Firefox, Chrome, Safari 和 Opera 支持应用程序缓存.

### HTML5 Cache Manifest 实例

下面的例子展示了带有 cache manifest 的 HTML 文档（供离线浏览）：

```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">

<body>
文档内容......
</body>

</html>
```

### Cache Manifest 基础

如需启用应用程序缓存，请在文档的`<html>` 标签中包含 manifest 属性：

```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">
...
</html>
````
每个指定了 manifest 的页面在用户对其访问时都会被缓存。如果未指定 manifest 属性，则页面不会被缓存（除非在 manifest 文件中直接指定了该页面）。

manifest 文件的建议的文件扩展名是：".appcache"。

> 请注意，manifest 文件需要配置正确的 MIME-type，即 "text/cache-manifest"。必须在 web 服务器上进行配置。

### Manifest 文件

manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。

manifest 文件可分为三个部分：

- CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
- NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）

###  CACHE MANIFEST

  第一行，CACHE MANIFEST，是必需的：

  ```
  CACHE MANIFEST
  /theme.css
  /logo.gif
  /main.js
  ```
  上面的 manifest 文件列出了三个资源：一个 CSS 文件，一个 GIF 图像，以及一个 JavaScript 文件。当 manifest 文件加载后，浏览器会从网站的根目录下载这三个文件。然后，无论用户何时与因特网断开连接，这些资源依然是可用的。

### NETWORK

  下面的 NETWORK 小节规定文件 "login.php" 永远不会被缓存，且离线时是不可用的：

  ```
  NETWORK:
  login.php
  ```
  可以使用星号来指示所有其他资源/文件都需要因特网连接：

  ```
  NETWORK:
  *
  ```
### FALLBACK

  下面的 FALLBACK 小节规定如果无法建立因特网连接，则用 "offline.html" 替代 /html5/ 目录中的所有文件：

  ```
  FALLBACK:
  /html/ /offline.html
  ```
  注意: 第一个 URI 是资源，第二个是替补。

### 更新缓存

一旦应用被缓存，它就会保持缓存直到发生下列情况：

- 用户清空浏览器缓存
- manifest 文件被修改（参阅下面的提示）
- 由程序来更新应用缓存

### 实例 - 完整的 Manifest 文件

```
CACHE MANIFEST
# 2012-02-21 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.php

FALLBACK:
/html/ /offline.html
```
> 提示:以 "#" 开头的是注释行，但也可满足其他用途。应用的缓存会在其 manifest 文件更改时被更新。如果您编辑了一幅图片，或者修改了一个 JavaScript 函数，这些改变都不会被重新缓存。更新注释行中的日期和版本号是一种使浏览器重新缓存文件的办法。

### 关于应用程序缓存的说明

请留心缓存的内容。

一旦文件被缓存，则浏览器会继续展示已缓存的版本，即使您修改了服务器上的文件。为了确保浏览器更新缓存，您需要更新 manifest 文件。

注意: 浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。

## HTML5 Web Workers


