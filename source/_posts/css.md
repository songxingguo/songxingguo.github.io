title: CSS 教程
author: songxingguo
tags:
  - CSS
categories:
  - 读书笔记
date: 2018-09-16 21:58:00
---
# CSS 教程

通过使用 CSS 我们可以大大提升网页开发的工作效率！

在我们的 CSS 教程中，您会学到如何使用 CSS 同时控制多重网页的样式和布局。

## CSS 简介

#### 什么是 CSS?

- CSS 指层叠样式表 (Cascading Style Sheets)
- 样式定义如何显示 HTML 元素
- 样式通常存储在样式表中
- 把样式添加到 HTML 4.0 中，是为了解决内容与表现分离的问题
- 外部样式表可以极大提高工作效率
- 外部样式表通常存储在 CSS 文件中
- 多个样式定义可层叠为一

### 样式解决了一个很大的问题

HTML 标签原本被设计为用于定义文档内容，如下实例：

```html
<h1>这是一个标题</h1>
<p>这是一个段落。</p>
```
样式表定义如何显示 HTML 元素，就像 HTML 3.2 的字体标签和颜色属性所起的作用那样。样式通常保存在外部的 .css 文件中。通过仅仅编辑一个简单的 CSS 文档，外部样式表使你有能力同时改变站点中所有页面的布局和外观。

为了解决这个问题，万维网联盟（W3C），这个非营利的标准化联盟，肩负起了 HTML 标准化的使命，并在 HTML 4.0 之外创造出样式（Style）。

当代浏览器都支持 CSS .

### CSS 样式表极大地提高了工作效率

样式表定义如何显示 HTML 元素

样式表定义如何显示 HTML 元素，就像 HTML 3.2 的字体标签和颜色属性所起的作用那样。样式通常保存在外部的 .css 文件中。通过仅仅编辑一个简单的 CSS 文档，外部样式表使你有能力同时改变站点中所有页面的布局和外观。

## CSS 语法

### CSS 实例

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明:

![CSS 实例](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/CSS%20%E5%AE%9E%E4%BE%8B.png)

选择器通常是您需要改变样式的 HTML 元素。

每条声明由一个属性和一个值组成。

属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。

### CSS 实例

CSS声明总是以分号(;)结束，声明组以大括号({})括起来:

```css
p {color:red;text-align:center;}
```
为了让CSS可读性更强，你可以每行只描述一个属性:

```css
p
{
color:red;
text-align:center;
}
```
### CSS 注释

注释是用来解释你的代码，并且可以随意编辑它，浏览器会忽略它。

CSS注释以 "`/*`" 开始, 以 "`*/`" 结束, 实例如下:

```css
/*这是个注释*/
p
{
text-align:center;
/*这是另一个注释*/
color:black;
font-family:arial;
}
```
## CSS Id 和 Class

### id 和 class 选择器

如果你要在HTML元素中设置CSS样式，你需要在元素中设置"id" 和 "class"选择器。

### id 选择器

id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。

HTML元素以id属性来设置id选择器,CSS 中 id 选择器以 "#" 来定义。

以下的样式规则应用于元素属性 id="para1":

```css
#para1
{
    text-align:center;
    color:red;
}
```
> ID属性不要以数字开头，数字开头的ID在 Mozilla/Firefox 浏览器中不起作用。

### class 选择器

class 选择器用于描述一组元素的样式，class 选择器有别于id选择器，class可以在多个元素中使用。

class 选择器在HTML中以class属性表示, 在 CSS 中，类选择器以一个点"."号显示：

在以下的例子中，所有拥有 center 类的 HTML 元素均为居中。

```css
.center {text-align:center;}
```
你也可以指定特定的HTML元素使用class。

在以下实例中, 所有的 p 元素使用 class="center" 让该元素的文本居中:

```css
p.center {text-align:center;}
```
> 类名的第一个字符不能使用数字！它无法在 Mozilla 或 Firefox 中起作用。

## CSS 创建

当读到一个样式表时，浏览器会根据它来格式化 HTML 文档。

### 如何插入样式表

插入样式表的方法有三种:

外部样式表(External style sheet)
内部样式表(Internal style sheet)
内联样式(Inline style)

- #### 外部样式表

  当样式需要应用于很多页面时，外部样式表将是理想的选择。在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。每个页面使用 <link> 标签链接到样式表。 <link> 标签在（文档的）头部：

  ```html
  <head>
  <link rel="stylesheet" type="text/css" href="mystyle.css">
  </head>
  ```
  浏览器会从文件 mystyle.css 中读到样式声明，并根据它来格式文档。

  外部样式表可以在任何文本编辑器中进行编辑。文件不能包含任何的 html 标签。样式表应该以 .css 扩展名进行保存。下面是一个样式表文件的例子：

  ```css
  hr {color:sienna;}
  p {margin-left:20px;}
  body {background-image:url("/images/back40.gif");}
  ```
  > 不要在属性值与单位之间留有空格（如："margin-left: 20 px" ），正确的写法是 "margin-left: 20px" 。
  
- #### 内部样式表

  当单个文档需要特殊的样式时，就应该使用内部样式表。你可以使用 `<style>` 标签在文档头部定义内部样式表，就像这样:

  ```html
  <head>
  <style>
  hr {color:sienna;}
  p {margin-left:20px;}
  body {background-image:url("images/back40.gif");}
  </style>
  </head>
  ```
- #### 内联样式

  由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在一个元素上应用一次时。

  要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：

  ```html
  <p style="color:sienna;margin-left:20px">这是一个段落。</p>
  ```
### 多重样式

如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来。 

例如，外部样式表拥有针对 h3 选择器的三个属性：

```css
h3
{
    color:red;
    text-align:left;
    font-size:8pt;
}
```
而内部样式表拥有针对 h3 选择器的两个属性：

```css
h3
{
    text-align:right;
    font-size:20pt;
}
```
假如拥有内部样式表的这个页面同时与外部样式表链接，那么 h3 得到的样式是：

```css
color:red;
text-align:right;
font-size:20pt;
```
即颜色属性将被继承于外部样式表，而文字排列（text-alignment）和字体尺寸（font-size）会被内部样式表中的规则取代。

### 多重样式优先级

样式表允许以多种方式规定样式信息。样式可以规定在单个的 HTML 元素中，在 HTML 页的头元素中，或在一个外部的 CSS 文件中。甚至可以在同一个 HTML 文档内部引用多个外部样式表。

一般情况下，优先级如下：

**(内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**

```html
<head>
    <!-- 外部样式 style.css -->
    <link rel="stylesheet" type="text/css" href="style.css"/>
    <!-- 设置：h3{color:blue;} -->
    <style type="text/css">
      /* 内部样式 */
      h3{color:green;}
    </style>
</head>
<body>
    <h3>测试！</h3>
</body>
```
> 注意：如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。


## CSS 背景

CSS 背景属性用于定义HTML元素的背景。

CSS 属性定义背景效果:

- background-color
- background-image
- background-repeat
- background-attachment
- background-position

### 背景颜色

background-color 属性定义了元素的背景颜色.

页面的背景颜色使用在body的选择器中:

```css
body {background-color:#b0c4de;}
```
CSS中，颜色值通常以以下方式定义:

- 十六进制 - 如："#ff0000"
- RGB - 如："rgb(255,0,0)"
- 颜色名称 - 如："red"

以下实例中, h1, p, 和 div 元素拥有不同的背景颜色:

```css
h1 {background-color:#6495ed;}
p {background-color:#e0ffff;}
div {background-color:#b0c4de;}
```

### 背景图像

background-image 属性描述了元素的背景图像.

默认情况下，背景图像进行平铺重复显示，以覆盖整个元素实体.

页面背景图片设置实例:

```css
body {background-image:url('paper.gif');}
```
下面是一个例子是一个糟糕的文字和背景图像组合。文本可读性差:

```css
body {background-image:url('bgdesert.jpg');}
```
- #### 背景图像 - 水平或垂直平铺

  默认情况下 background-image 属性会在页面的水平或者垂直方向平铺。

  一些图像如果在水平方向与垂直方向平铺，这样看起来很不协调，如下所示: 

  ```css
  body {
    background-image:url('gradient2.png');
  }
  ```
  如果图像只在水平方向平铺 (repeat-x), 页面背景会更好些:

  ```css
  body {
    background-image:url('gradient2.png');
    background-repeat:repeat-x;
  }
  ```
- #### 背景图像- 设置定位与不平铺

  Remark 让背景图像不影响文本的排版

  如果你不想让图像平铺，你可以使用 background-repeat 属性:

  ```css
  body {
    background-image:url('img_tree.png');
    background-repeat:no-repeat;
  }
  ```
  以上实例中，背景图像与文本显示在同一个位置，为了让页面排版更加合理，不影响文本的阅读，我们可以改变图像的位置。

  可以利用 background-position 属性改变图像在背景中的位置:

  ```css
  body {
    background-image:url('img_tree.png');
    background-repeat:no-repeat;
    background-position:right top;
  }
  ```
### 背景- 简写属性

在以上实例中我们可以看到页面的背景颜色通过了很多的属性来控制。

为了简化这些属性的代码，我们可以将这些属性合并在同一个属性中.

背景颜色的简写属性为 "background":

```css
body {background:#ffffff url('img_tree.png') no-repeat right top;}
```
当使用简写属性时，属性值的顺序为：:

- background-color
- background-image
- background-repeat
- background-attachment
- background-position

以上属性无需全部使用，你可以按照页面的实际需要使用.

### CSS 背景属性

![CSS 背景属性](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/CSS%20%E8%83%8C%E6%99%AF%E5%B1%9E%E6%80%A7.png)

## CSS 文本格式


### 文本颜色

颜色属性被用来设置文字的颜色。

颜色是通过CSS最经常的指定：

- 十六进制值 - 如: ＃FF0000
- 一个RGB值 - 如: RGB(255,0,0)
- 颜色的名称 - 如: red

一个网页的背景颜色是指在主体内的选择：

```
body {color:red;}
h1 {color:#00ff00;}
h2 {color:rgb(255,0,0);}
```

> 对于W3C标准的CSS：如果你定义了颜色属性，你还必须定义背景色属性。

### 文本的对齐方式

文本排列属性是用来设置文本的水平对齐方式。

文本可居中或对齐到左或右,两端对齐.

当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。

```
h1 {text-align:center;}
p.date {text-align:right;}
p.main {text-align:justify;}
```
### 文本修饰

text-decoration 属性用来设置或删除文本的装饰。

从设计的角度看 text-decoration属性主要是用来删除链接的下划线：

```js
a {text-decoration:none;}
```
也可以这样装饰文字：

```css
h1 {text-decoration:overline;}
h2 {text-decoration:line-through;}
h3 {text-decoration:underline;}
```
> 我们不建议强调指出不是链接的文本，因为这常常混淆用户。

### 文本转换

文本转换属性是用来指定在一个文本中的大写和小写字母。

可用于所有字句变成大写或小写字母，或每个单词的首字母大写。

```css
p.uppercase {text-transform:uppercase;}
p.lowercase {text-transform:lowercase;}
p.capitalize {text-transform:capitalize;}
```

### 文本缩进

文本缩进属性是用来指定文本的第一行的缩进。

```css
p {text-indent:50px;}
```

### 所有CSS文本属性。

![所有CSS文本属性](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%89%80%E6%9C%89CSS%E6%96%87%E6%9C%AC%E5%B1%9E%E6%80%A7.png)

## CSS 字体

CSS字体属性定义字体，加粗，大小，文字样式。

serif和sans-serif字体之间的区别

![serif和sans-serif字体之间的区别](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/serif%E5%92%8Csans-serif%E5%AD%97%E4%BD%93%E4%B9%8B%E9%97%B4%E7%9A%84%E5%8C%BA%E5%88%AB.png)

>  在计算机屏幕上，sans-serif字体被认为是比serif字体容易阅读

### CSS字型

在CSS中，有两种类型的字体系列名称：

- 通用字体系列 - 拥有相似外观的字体系统组合（如 "Serif" 或 "Monospace"）
- 特定字体系列 - 一个特定的字体系列（如 "Times" 或 "Courier"）

![CSS字型](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/CSS%E5%AD%97%E5%9E%8B.png)

### 字体系列

font-family 属性设置文本的字体系列。

font-family 属性应该设置几个字体名称作为一种"后备"机制，如果浏览器不支持第一种字体，他将尝试下一种字体。

注意: 如果字体系列的名称超过一个字，它必须用引号，如Font Family："宋体"。

多个字体系列是用一个逗号分隔指明：

```css
p{font-family:"Times New Roman", Times, serif;}
```
对于较常用的字体组合，看看我们的 [Web安全字体组合](http://www.runoob.com/cssref/css-websafe-fonts.html)。

### 字体样式

主要是用于指定斜体文字的字体样式属性。

这个属性有三个值：

- 正常 - 正常显示文本
- 斜体 - 以斜体字显示的文字
- 倾斜的文字 - 文字向一边倾斜（和斜体非常类似，但不太支持）

```css
p.normal {font-style:normal;}
p.italic {font-style:italic;}
p.oblique {font-style:oblique;}
```
### 字体大小

font-size 属性设置文本的大小。

能否管理文字的大小，在网页设计中是非常重要的。但是，你不能通过调整字体大小使段落看上去像标题，或者使标题看上去像段落。

请务必使用正确的HTML标签，就`<h1> - <h6>`表示标题和`<p>`表示段落：

字体大小的值可以是绝对或相对的大小。

绝对大小：

- 设置一个指定大小的文本
- 不允许用户在所有浏览器中改变文本大小
- 确定了输出的物理尺寸时绝对大小很有用

相对大小：

- 相对于周围的元素来设置大小
- 允许用户在浏览器中改变文字大小

> 如果你不指定一个字体的大小，默认大小和普通文本段落一样，是16像素（16px=1em）。

- #### 设置字体大小像素

  设置文字的大小与像素，让您完全控制文字大小：

  ```css
  h1 {font-size:40px;}
  h2 {font-size:30px;}
  p {font-size:14px;}
  ```

  上面的例子可以在 Internet Explorer 9, Firefox, Chrome, Opera, 和 Safari 中通过缩放浏览器调整文本大小。

  虽然可以通过浏览器的缩放工具调整文本大小，但是，这种调整是整个页面，而不仅仅是文本

- #### 用em来设置字体大小

  为了避免Internet Explorer 中无法调整文本的问题，许多开发者使用 em 单位代替像素。

  em的尺寸单位由W3C建议。

  1em和当前字体大小相等。在浏览器中默认的文字大小是16px。

  因此，1em的默认大小是16px。可以通过下面这个公式将像素转换为em：px/16=em

  ```css
  h1 {font-size:2.5em;} /* 40px/16=2.5em */
  h2 {font-size:1.875em;} /* 30px/16=1.875em */
  p {font-size:0.875em;} /* 14px/16=0.875em */
  ```
  在上面的例子，em的文字大小是与前面的例子中像素一样。不过，如果使用 em 单位，则可以在所有浏览器中调整文本大小。

  不幸的是，仍然是IE浏览器的问题。调整文本的大小时，会比正常的尺寸更大或更小。

- #### 使用百分比和EM组合

  在所有浏览器的解决方案中，设置 `<body>`元素的默认字体大小的是百分比：

  ```css
  body {font-size:100%;}
  h1 {font-size:2.5em;}
  h2 {font-size:1.875em;}
  p {font-size:0.875em;}
  ```
  我们的代码非常有效。在所有浏览器中，可以显示相同的文本大小，并允许所有浏览器缩放文本的大小。
  
### 所有CSS字体属性
 
 ![所有CSS字体属性](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%89%80%E6%9C%89CSS%E5%AD%97%E4%BD%93%E5%B1%9E%E6%80%A7.png)
  
  
## CSS 链接

不同的链接可以有不同的样式。

### 链接样式

链接的样式，可以用任何CSS属性（如颜色，字体，背景等）。

特别的链接，可以有不同的样式，这取决于他们是什么状态。

这四个链接状态是：

- a:link - 正常，未访问过的链接
- a:visited - 用户已访问过的链接
- a:hover - 当用户鼠标放在链接上时
- a:active - 链接被点击的那一刻

```
a:link {color:#000000;}      /* 未访问链接*/
a:visited {color:#00FF00;}  /* 已访问链接 */
a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
a:active {color:#0000FF;}  /* 鼠标点击时 */
```

当设置为若干链路状态的样式，也有一些顺序规则：

- a:hover 必须跟在 a:link 和 a:visited后面
- a:active 必须跟在 a:hover后面

### 常见的链接样式

根据上述链接的颜色变化的例子，看它是在什么状态。

让我们通过一些其他常见的方式转到链接样式：

### 文本修饰

text-decoration 属性主要用于删除链接中的下划线：

```css
a:link {text-decoration:none;}
a:visited {text-decoration:none;}
a:hover {text-decoration:underline;}
a:active {text-decoration:underline;}
```
### 背景颜色

背景颜色属性指定链接背景色：

```css
a:link {background-color:#B2FF99;}
a:visited {background-color:#FFFF85;}
a:hover {background-color:#FF704D;}
a:active {background-color:#FF704D;}
```

## CSS 列表

CSS列表属性作用如下：

- 设置不同的列表项标记为有序列表
- 设置不同的列表项标记为无序列表
- 设置列表项标记为图像

### 列表

在HTML中，有两种类型的列表：

- 无序列表 - 列表项标记用特殊图形（如小黑点、小方框等）
- 有序列表 - 列表项的标记有数字或字母

使用CSS，可以列出进一步的样式，并可用图像作列表项标记。

### 不同的列表项标记

list-style-type属性指定列表项标记的类型是：

```css
ul.a {list-style-type: circle;}
ul.b {list-style-type: square;}
 
ol.c {list-style-type: upper-roman;}
ol.d {list-style-type: lower-alpha;}
```
一些值是无序列表，以及有些是有序列表。

### 作为列表项标记的图像

要指定列表项标记的图像，使用列表样式图像属性：

```css
ul
{
    list-style-image: url('sqpurple.gif');
}
```
上面的例子在所有浏览器中显示并不相同，IE和Opera显示图像标记比火狐，Chrome和Safari更高一点点。

如果你想在所有的浏览器放置同样的形象标志，就应使用浏览器兼容性解决方案，过程如下

浏览器兼容性解决方案
同样在所有的浏览器，下面的例子会显示的图像标记：

```css
ul
{
    list-style-type: none;
    padding: 0px;
    margin: 0px;
}
ul li
{
    background-image: url(sqpurple.gif);
    background-repeat: no-repeat;
    background-position: 0px 5px; 
    padding-left: 14px; 
}
```

例子解释：

- ul:

  - 设置列表样式类型为没有删除列表项标记
  - 设置填充和边距0px（浏览器兼容性）

-  ul中所有li:

  - 设置图像的URL，并设置它只显示一次（无重复）
  - 您需要的定位图像位置（左0px和上下5px）
  - 用padding-left属性把文本置于列表中

### 列表 -简写属性

在单个属性中可以指定所有的列表属性。这就是所谓的简写属性。

为列表使用简写属性，列表样式属性设置如下：

```css
ul
{
    list-style: square url("sqpurple.gif");
}
```
可以按顺序设置如下属性：

```
list-style-type
list-style-position (有关说明，请参见下面的CSS属性表)
list-style-image
```
如果上述值丢失一个，其余仍在指定的顺序，就没关系。

### 所有的CSS列表属性

![所有的CSS列表属性](http://p9myzkds7.bkt.clouddn.com/sorting-algorithm/%E6%89%80%E6%9C%89%E7%9A%84CSS%E5%88%97%E8%A1%A8%E5%B1%9E%E6%80%A7.png)

## CSS 表格

使用 CSS 可以使 HTML 表格更美观。

### 表格边框

指定CSS表格边框，使用border属性。

下面的例子指定了一个表格的Th和TD元素的黑色边框：

```css
table, th, td {
  border: 1px solid black;
}
```
请注意，在上面的例子中的表格有双边框。这是因为表和th/ td元素有独立的边界。

为了显示一个表的单个边框，使用 border-collapse属性。

### 折叠边框

border-collapse 属性设置表格的边框是否被折叠成一个单一的边框或隔开：

```
table {
  border-collapse:collapse;
}
table,th, td {
  border: 1px solid black;
}
```
### 表格宽度和高度

Width和height属性定义表格的宽度和高度。

下面的例子是设置100％的宽度，50像素的th元素的高度的表格：

```css
table {
  width:100%;
}
th {
  height:50px;
}
```
### 表格文字对齐

表格中的文本对齐和垂直对齐属性。

text-align属性设置水平对齐方式，像左，右，或中心：

```css
td {
  text-align:right;
}
```
垂直对齐属性设置垂直对齐，比如顶部，底部或中间：

```css
td {
  height:50px;
  vertical-align:bottom;
}
```
### 表格填充

如果在表的内容中控制空格之间的边框，应使用td和th元素的填充属性：

```css
td {
  padding:15px;
}
```
### 表格颜色

下面的例子指定边框的颜色，和th元素的文本和背景颜色：

```css
table, td, th {
  border:1px solid green;
}
th {
  background-color:green;
  color:white;
}
```
## CSS 盒子模型

### CSS 盒子模型(Box Model)

所有HTML元素可以看作盒子，在CSS中，"box model"这一术语是用来设计和布局时使用。

CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。

盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

下面的图片说明了盒子模型(Box Model)：

![CSS 盒子模型](http://p9myzkds7.bkt.clouddn.com/CSS/CSS%E7%9B%92%E6%A8%A1%E5%9E%8B.png)

不同部分的说明：

- Margin(外边距) - 清除边框外的区域，外边距是透明的。
- Border(边框) - 围绕在内边距和内容外的边框。
- Padding(内边距) - 清除内容周围的区域，内边距是透明的。
- Content(内容) - 盒子的内容，显示文本和图像。

为了正确设置元素在所有浏览器中的宽度和高度，你需要知道的盒模型是如何工作的。

### 元素的宽度和高度

> 重要: 当您指定一个CSS元素的宽度和高度属性时，你只是设置内容区域的宽度和高度。要知道，完全大小的元素，你还必须添加填充，边框和边距。.

下面的例子中的元素的总宽度为300px：

```css
div {
    width: 300px;
    border: 25px solid green;
    padding: 25px;
    margin: 25px;
}
```
让我们自己算算：

300px (宽)
+ 50px (左 + 右填充)
+ 50px (左 + 右边框)
+ 50px (左 + 右边距)
= 450px

试想一下，你只有250像素的空间。让我们设置总宽度为250像素的元素:

```css
div {
    width: 220px;
    padding: 10px;
    border: 5px solid gray;
    margin: 0; 
}
```
最终元素的总宽度计算公式是这样的：

总元素的宽度=宽度+左填充+右填充+左边框+右边框+左边距+右边距

元素的总高度最终计算公式是这样的：

总元素的高度=高度+顶部填充+底部填充+上边框+下边框+上边距+下边距

### 浏览器的兼容性问题

一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。根据 W3C 的规范，元素内容占据的空间是由 width 属性设置的，而内容周围的 padding 和 border 值是另外计算的。不幸的是，IE5.X 和 6 在怪异模式中使用自己的非标准模型。这些浏览器的 width 属性不是内容的宽度，而是内容、内边距和边框的宽度的总和。

虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。也就是，不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。

IE8 及更早IE版本不支持设置填充的宽度和边框的宽度属性。

解决IE8及更早版本不兼容问题可以在HTML页面声明 `<!DOCTYPE html>` 即可。

## CSS 边框

### CSS 边框属性

CSS边框属性允许你指定一个元素边框的样式和颜色。

![CSS 边框属性](http://p9myzkds7.bkt.clouddn.com/CSS/css%E8%BE%B9%E6%A1%86%E5%B1%9E%E6%80%A7.png)

### 边框样式

边框样式属性指定要显示什么样的边界。

> border-style属性用来定义边框的样式

border-style 值:

![border-style 值](http://p9myzkds7.bkt.clouddn.com/CSS/border-style%20%E5%80%BC.png)

### 边框宽度

您可以通过 border-width 属性为边框指定宽度。

为边框指定宽度有两种方法：可以指定长度值，比如 2px 或 0.1em(单位为 px, pt, cm, em 等)，或者使用 3 个关键字之一，它们分别是 thick 、medium（默认值） 和 thin。

注意：CSS 没有定义 3 个关键字的具体宽度，所以一个用户可能把 thick 、medium 和 thin 分别设置为等于 5px、3px 和 2px，而另一个用户则分别设置为 3px、2px 和 1px。

```css
p.one
{
    border-style:solid;
    border-width:5px;
}
p.two
{
    border-style:solid;
    border-width:medium;
}
```
### 边框颜色

border-color属性用于设置边框的颜色。可以设置的颜色：

- name - 指定颜色的名称，如 "red"
- RGB - 指定 RGB 值, 如 "rgb(255,0,0)"
- Hex - 指定16进制值, 如 "#ff0000"

您还可以设置边框的颜色为"transparent"。

注意： border-color单独使用是不起作用的，必须得先使用border-style来设置边框样式。

```css
p.one
{
    border-style:solid;
    border-color:red;
}
p.two
{
    border-style:solid;
    border-color:#98bf21;
}
```
### 边框-单独设置各边

在CSS中，可以指定不同的侧面不同的边框：

```css
p
{
    border-top-style:dotted;
    border-right-style:solid;
    border-bottom-style:dotted;
    border-left-style:solid;
}
```
上面的例子也可以设置一个单一属性：

```css
border-style:dotted solid;
```
border-style属性可以有1-4个值：

- border-style:dotted solid double dashed;

  - 上边框是 dotted
  - 右边框是 solid
  - 底边框是 double
  - 左边框是 dashed

- border-style:dotted solid double;

  - 上边框是 dotted
  - 左、右边框是 solid
  - 底边框是 double

- border-style:dotted solid;

  - 上、底边框是 dotted
  - 右、左边框是 solid

- border-style:dotted;

  - 四面边框是 dotted

上面的例子用了border-style。然而，它也可以和border-width 、 border-color一起使用。

#### 边框-简写属性

上面的例子用了很多属性来设置边框。

你也可以在一个属性中设置边框。

你可以在"border"属性中设置：

- border-width
- border-style (required)
- border-color

```
border:5px solid red;
```
![CSS 边框属性](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%89%80%E6%9C%89CSS%E8%BE%B9%E6%A1%86%E5%B1%9E%E6%80%A7.png)

## CSS 轮廓（outline）

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定元素轮廓的样式、颜色和宽度。

### CSS 轮廓（outline）

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

![CSS 轮廓](http://p9myzkds7.bkt.clouddn.com/CSS/CSS%20%E8%BD%AE%E5%BB%93.png)

### 所有CSS 轮廓（outline）属性

"CSS" 列中的数字表示哪个CSS版本定义了该属性(CSS1 或者CSS2)。

CSS outline 属性规定元素轮廓的样式、颜色和宽度。

![所有CSS 轮廓（outline）属性](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%89%80%E6%9C%89CSS%20%E8%BD%AE%E5%BB%93%EF%BC%88outline%EF%BC%89%E5%B1%9E%E6%80%A7.png)

## CSS margin(外边距)

CSS margin(外边距)属性定义元素周围的空间。

### margin

margin 清除周围的（外边框）元素区域。margin 没有背景颜色，是完全透明的。

margin 可以单独改变元素的上，下，左，右边距，也可以一次改变所有的属性。

![margin](http://p9myzkds7.bkt.clouddn.com/CSS/margin.png)

可能的值

![可能的值](http://p9myzkds7.bkt.clouddn.com/CSS/%E5%8F%AF%E8%83%BD%E7%9A%84%E5%80%BC.png)

> Margin可以使用负值，重叠的内容。


### Margin - 单边外边距属性

在CSS中，它可以指定不同的侧面不同的边距：

```css
margin-top:100px;
margin-bottom:100px;
margin-right:50px;
margin-left:50px;
```
### Margin - 简写属性

为了缩短代码，有可能使用一个属性中margin指定的所有边距属性。这就是所谓的简写属性。

所有边距属性的简写属性是 margin :

```
margin:100px 50px;
```
margin属性可以有一到四个值。/p>

- margin:25px 50px 75px 100px;

  - 上边距为25px
  - 右边距为50px
  - 下边距为75px
  - 左边距为100px

- margin:25px 50px 75px;

  - 上边距为25px
  - 左右边距为50px
  - 下边距为75px

- margin:25px 50px;

  - 上下边距为25px
  - 左右边距为50px

- margin:25px;

  - 所有的4个边距都是25px

### 所有的CSS边距属性

![所有的CSS边距属性](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%89%80%E6%9C%89%E7%9A%84CSS%E8%BE%B9%E8%B7%9D%E5%B1%9E%E6%80%A7.png)

## CSS padding（填充）

CSS padding（填充）是一个简写属性，定义元素边框与元素内容之间的空间，即上下左右的内边距。

### padding（填充）

当元素的 padding（填充）内边距被清除时，所释放的区域将会受到元素背景颜色的填充。

单独使用 padding 属性可以改变上下左右的填充。

![padding](http://p9myzkds7.bkt.clouddn.com/CSS/padding.png)

### 可能的值

![padding可能的值](http://p9myzkds7.bkt.clouddn.com/CSS/padding%E5%8F%AF%E8%83%BD%E7%9A%84%E5%80%BC.png)


### 填充- 单边内边距属性

在CSS中，它可以指定不同的侧面不同的填充：

```css
padding-top:25px;
padding-bottom:25px;
padding-right:50px;
padding-left:50px;
```
上内边距是 25px
右内边距是 50px
下内边距是 25px
左内边距是 50px

### 填充 - 简写属性

为了缩短代码，它可以在一个属性中指定的所有填充属性。

这就是所谓的简写属性。所有的填充属性的简写属性是 padding :

```css
padding:25px 50px;
```
Padding属性，可以有一到四个值。

- padding:25px 50px 75px 100px;

  - 上填充为25px
  - 右填充为50px
  - 下填充为75px
  - 左填充为100px
  
- padding:25px 50px 75px;

  - 上填充为25px
  - 左右填充为50px
  - 下填充为75px

- padding:25px 50px;

  - 上下填充为25px
  - 左右填充为50px

- padding:25px;

  - 所有的填充都是25px

### 所有的CSS填充属性

![所有的CSS填充属性](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%89%80%E6%9C%89%E7%9A%84CSS%E5%A1%AB%E5%85%85%E5%B1%9E%E6%80%A7.png)

## CSS 分组 和 嵌套 选择器

### 分组选择器

在样式表中有很多具有相同样式的元素。

```css
h1
{
    color:green;
}
h2
{
    color:green;
}
p
{
    color:green;
}
```
为了尽量减少代码，你可以使用分组选择器。

每个选择器用逗号分隔。

在下面的例子中，我们对以上代码使用分组选择器：

```css
h1,h2,p
{
    color:green;
}
```
### 嵌套选择器

它可能适用于选择器内部的选择器的样式。

在下面的例子设置了三个样式：

- p{ }: 为所有 p 元素指定一个样式。
- .marked{ }: 为所有 class="marked" 的元素指定一个样式。
- .marked p{ }: 为所有 class="marked" 元素内的 p 元素指定一个样式。
- p.marked{ }: 为所有 class="marked" 的 p 元素指定一个样式。

```css
p
{
    color:blue;
    text-align:center;
}
.marked
{
    background-color:red;
}
.marked p
{
    color:white;
}
p.marked{
    text-decoration:underline;
}
```
## CSS 尺寸 (Dimension)

CSS 尺寸 (Dimension) 属性允许你控制元素的高度和宽度。同样，它允许你增加行间距。

所有CSS 尺寸 (Dimension)属性

![所有CSS 尺寸 (Dimension)属性](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%89%80%E6%9C%89CSS%20%E5%B0%BA%E5%AF%B8%20%28Dimension%29%E5%B1%9E%E6%80%A7.png)

## CSS Display(显示) 与 Visibility（可见性）

display属性设置一个元素应如何显示，visibility属性指定一个元素应可见还是隐藏。

### 隐藏元素 - display:none或visibility:hidden

隐藏一个元素可以通过把display属性设置为"none"，或把visibility属性设置为"hidden"。但是请注意，这两种方法会产生不同的结果。

**visibility:hidden** 可以隐藏某个元素，但 **隐藏的元素仍需占用与未隐藏之前一样的空间** 。也就是说，该 **元素虽然被隐藏了** ，但 **仍然会影响布局** 。

```
h1.hidden {visibility:hidden;}
```
**display:none** 可以隐藏某个元素，且 **隐藏的元素不会占用任何空间** 。也就是说，该元素不但被隐藏了，而且 **该元素原本占用的空间也会从页面布局中消失** 。

```
h1.hidden {display:none;}
```
### CSS Display - 块和内联元素

块元素是一个元素，占用了全部宽度，在前后都是换行符。

块元素的例子：


- `<h1>`
- `<p>`
- `<div>`

内联元素只需要必要的宽度，不强制换行。

内联元素的例子：

- `<span>`
- `<a>`

### 如何改变一个元素显示

可以更改内联元素和块元素，反之亦然，可以使页面看起来是以一种特定的方式组合，并仍然遵循web标准。

下面的示例把列表项显示为内联元素：

```css
li {display:inline;}
```
下面的示例把span元素作为块元素：

```
span {display:block;}
```
注意：变更元素的显示类型看该元素是如何显示，它是什么样的元素。例如：一个内联元素设置为display:block是不允许有它内部的嵌套块元素。

## CSS Position(定位)

position 属性指定了元素的定位类型。

position 属性的五个值：

- static
- relative
- fixed
- absolute
- sticky

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定position属性。他们也有不同的工作方式，这取决于定位方法。

### static 定位

HTML元素的默认值，即没有定位，元素出现在正常的流中。

静态定位的元素不会受到 top, bottom, left, right影响。

```
div.static {
    position: static;
    border: 3px solid #73AD21;
}
```
### fixed 定位

元素的位置相对于浏览器窗口是固定位置。

即使窗口是滚动的它也不会移动：

```css
p.pos_fixed
{
    position:fixed;
    top:30px;
    right:5px;
}
```
注意： Fixed 定位在 IE7 和 IE8 下需要描述 !DOCTYPE 才能支持。

Fixed定位使元素的位置与文档流无关，因此不占据空间。

Fixed定位的元素和其他元素重叠。

### relative 定位

相对定位元素的定位是相对其正常位置。

```css
h2.pos_left
{
    position:relative;
    left:-20px;
}
h2.pos_right
{
    position:relative;
    left:20px;
}
```
移动相对定位元素，但它原本所占的空间不会改变。

```css
h2.pos_top
{
    position:relative;
    top:-50px;
}
```
**相对定位元素经常被用来作为绝对定位元素的容器块。**

### absolute 定位

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于`<html>`:

```css
h2
{
    position:absolute;
    left:100px;
    top:150px;
}
```
absolute 定位使元素的位置与文档流无关，因此不占据空间。

absolute 定位的元素和其他元素重叠。

### sticky 定位

sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。

position: sticky; 基于用户的滚动位置来定位。

粘性定位的元素是依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换。

**它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。**

元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

注意: Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。

```
div.sticky {
    position: -webkit-sticky; /* Safari */
    position: sticky;
    top: 0;
    background-color: green;
    border: 2px solid #4CAF50;
}
```
### 重叠的元素

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素

z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）

一个元素可以有正数或负数的堆叠顺序：

```css
img
{
    position:absolute;
    left:0px;
    top:0px;
    z-index:-1;
}
```
具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面。

注意： 如果两个定位元素重叠，**没有指定z - index** ，**最后定位在HTML代码中的元素将被显示在最前面** 。

### 所有的CSS定位属性

"CSS" 列中的数字表示哪个CSS(CSS1 或者CSS2)版本定义了该属性。

[所有的CSS定位属性](http://www.runoob.com/css/css-positioning.html)

## CSS 布局 - Overflow

CSS overflow 属性用于控制内容溢出元素框时显示的方式。

CSS Overflow
CSS overflow 属性可以控制内容溢出元素框时在对应的元素区间内添加滚动条。

overflow属性有以下值：

![overflow属性的值](http://p9myzkds7.bkt.clouddn.com/CSS/overflow%E5%B1%9E%E6%80%A7%E7%9A%84%E5%80%BC.png)

**注意:overflow 属性只工作于指定高度的块元素上。**

注意: 在 OS X Lion ( Mac 系统) 系统上，滚动条默认是隐藏的，使用的时候才会显示 (设置 "overflow:scroll" 也是一样的)。

### overflow: visible

默认情况下，overflow 的值为 visible， 意思是内容溢出元素框：

这里的文本内容会溢出元素框。

这里的文本内容会溢出元素框。

这里的文本内容会溢出元素框。

这里的文本内容会溢出元素框。

这里的文本内容会溢出元素框。

这里的文本内容会溢出元素框。

这里的文本内容会溢出元素框。

```css
div {
    width: 200px;
    height: 50px;
    background-color: #eee;
    overflow: visible;
}
```

## CSS Float(浮动)

### 什么是 CSS Float（浮动）？

CSS 的 Float（浮动），会使元素向左或向右移动，其周围的元素也会重新排列。

Float（浮动），往往是用于图像，但它在布局时一样非常有用。

### 元素怎样浮动

元素的水平方向浮动，意味着 **元素只能左右移动而不能上下移动** 。

一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

**浮动元素之后的元素将围绕它。**

**浮动元素之前的元素将不会受到影响。**

如果图像是右浮动，下面的文本流将环绕在它左边：

```css
img
{
    float:right;
}
```

### 彼此相邻的浮动元素

如果你把几个浮动的元素放到一起，如果有空间的话，它们将彼此相邻。

在这里，我们对图片廊使用 float 属性：

```
.thumbnail 
{
    float:left;
    width:110px;
    height:90px;
    margin:5px;
}
```

### 清除浮动 - 使用 clear

元素浮动之后，周围的元素会重新排列，为了避免这种情况，使用 clear 属性。

clear 属性指定元素两侧不能出现浮动元素。

使用 clear 属性往文本中添加图片廊：

```css
.text_line
{
    clear:both;
}
```

### CSS 中所有的浮动属性

"CSS" 列中的数字表示不同的 CSS 版本（CSS1 或 CSS2）定义了该属性。

![CSS 中所有的浮动属性](http://p9myzkds7.bkt.clouddn.com/CSS/CSS%20%E4%B8%AD%E6%89%80%E6%9C%89%E7%9A%84%E6%B5%AE%E5%8A%A8%E5%B1%9E%E6%80%A7.png)

## CSS 布局 - 水平 & 垂直对齐

### 元素居中对齐

要水平居中对齐一个元素(如 `<div>`), 可以使用 margin: auto;。

设置到元素的宽度将防止它溢出到容器的边缘。

元素通过指定宽度，并将两边的空外边距平均分配：

![元素居中对齐](http://p9myzkds7.bkt.clouddn.com/CSS/%E5%85%83%E7%B4%A0%E5%B1%85%E4%B8%AD.png)

```css
.center {
    margin: auto;
    width: 50%;
    border: 3px solid green;
    padding: 10px;
}
```
**注意: 如果没有设置 width 属性(或者设置 100%)，居中对齐将不起作用。**

### 文本居中对齐

如果仅仅是为了文本在元素内居中对齐，可以使用 text-align: center;

![文本居中对齐](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%96%87%E6%9C%AC%E5%B1%85%E4%B8%AD%E5%AF%B9%E9%BD%90.png)

```cs
.center {
    text-align: center;
    border: 3px solid green;
}
```

### 图片居中对齐

要让图片居中对齐, 可以使用 margin: auto; 并将它放到 **块** 元素中:

![图片居中对齐](http://p9myzkds7.bkt.clouddn.com/CSS/%E5%9B%BE%E7%89%87%E5%B1%85%E4%B8%AD.png)

```css
img {
    display: block;
    margin: auto;
    width: 40%;
}
```
### 左右对齐 - 使用定位方式

我们可以使用 position: absolute; 属性来对齐元素:

![使用定位方式](http://p9myzkds7.bkt.clouddn.com/CSS/%E5%B7%A6%E5%8F%B3%E5%AF%B9%E9%BD%90.png)

```css
.right {
    position: absolute;
    right: 0px;
    width: 300px;
    border: 3px solid #73AD21;
    padding: 10px;
}
```
注释：绝对定位元素会被从正常流中删除，并且能够交叠元素。

提示: 当使用 position 来对齐元素时, 通常 `<body>` 元素会设置 margin 和 padding 。 这样可以避免在不同的浏览器中出现可见的差异。

当使用 position 属性时，IE8 以及更早的版本存在一个问题。如果容器元素（在我们的案例中是 `<div class="container">`）设置了指定的宽度，并且省略了 !DOCTYPE 声明，那么 IE8 以及更早的版本会在右侧增加 17px 的外边距。这似乎是为滚动条预留的空间。当使用 position 属性时，请始终设置 !DOCTYPE 声明：

```
body {
    margin: 0;
    padding: 0;
}
 
.container {
    position: relative;
    width: 100%;
}
 
.right {
    position: absolute;
    right: 0px;
    width: 300px;
    background-color: #b0e0e6;
}
```

### 左右对齐 - 使用 float 方式

我们也可以使用 float 属性来对齐元素:

```css
.right {
    float: right;
    width: 300px;
    border: 3px solid #73AD21;
    padding: 10px;
}
```
当像这样对齐元素时，对 `<body>` 元素的外边距和内边距进行预定义是一个好主意。这样可以避免在不同的浏览器中出现可见的差异。

注意：如果子元素的高度大于父元素，且子元素设置了浮动，那么子元素将溢出，这时候你可以使用 "clearfix(清除浮动)" 来解决该问题。

我们可以在父元素上添加 overflow: auto; 来解决子元素溢出的问题:

```css
.clearfix {
    overflow: auto;
}
```
当使用 float 属性时，IE8 以及更早的版本存在一个问题。如果省略 !DOCTYPE 声明，那么 IE8 以及更早的版本会在右侧增加 17px 的外边距。这似乎是为滚动条预留的空间。当使用 float 属性时，请始终设置 !DOCTYPE 声明：

```css
body {
    margin: 0;
    padding: 0;
}
 
.right {
    float: right;
    width: 300px;
    background-color: #b0e0e6;
}
```

### 垂直居中对齐 - 使用 padding

CSS 中有很多方式可以实现垂直居中对齐。 一个简单的方式就是头部顶部使用 padding:

![使用 padding](http://p9myzkds7.bkt.clouddn.com/CSS/padding%E5%AE%9E%E7%8E%B0.png)

```css
.center {
    padding: 70px 0;
    border: 3px solid green;
}
```
如果要水平和垂直都居中，可以使用 padding 和 text-align: center:

![水平和垂直都居中](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%B0%B4%E5%B9%B3%E5%92%8C%E5%9E%82%E7%9B%B4%E9%83%BD%E5%B1%85.png)

```css
.center {
    padding: 70px 0;
    border: 3px solid green;
    text-align: center;
}
```
### 垂直居中 - 使用 line-height

![使用 line-height](http://p9myzkds7.bkt.clouddn.com/CSS/%E4%BD%BF%E7%94%A8%20line-height.png)

```css
.center {
    line-height: 200px;
    height: 200px;
    border: 3px solid green;
    text-align: center;
}
 
/* 如果文本有多行，添加以下代码: */
.center p {
    line-height: 1.5;
    display: inline-block;
    vertical-align: middle;
}
```
### 垂直居中 - 使用 position 和 transform

除了使用 padding 和 line-height 属性外,我们还可以使用 transform 属性来设置垂直居中:

```css
.center { 
    height: 200px;
    position: relative;
    border: 3px solid green; 
}
 
.center p {
    margin: 0;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```
提示: 更多 transform 属性内容可以参阅 [2D 翻转章节](http://www.runoob.com/css3/css3-2dtransforms.html)。

## CSS 组合选择符

### CSS 组合选择符

> 组合选择符说明了两个选择器直接的关系。

CSS组合选择符包括各种简单选择符的组合方式。

在 CSS3 中包含了四种组合方式:

- 后代选择器(以空格分隔)
- 子元素选择器(以大于号分隔）
- 相邻兄弟选择器（以加号分隔）
- 普通兄弟选择器（以破折号分隔）

### 后代选择器

后代选择器用于选取某元素的后代元素。

以下实例选取所有 `<p>` 元素插入到 `<div>` 元素中: 

```css
div p
{
  background-color:yellow;
}
```
### 子元素选择器

与后代选择器相比，子元素选择器（Child selectors）只能选择作为某元素子元素的元素。

以下实例选择了`<div>`元素中所有直接子元素 `<p>` ：

```css
div>p
{
  background-color:yellow;
}
```
### 相邻兄弟选择器

相邻兄弟选择器（Adjacent sibling selector）可选择紧接在另一元素后的元素，且二者有相同父元素。

如果需要选择紧接在另一个元素后的元素，而且二者有相同的父元素，可以使用相邻兄弟选择器（Adjacent sibling selector）。

以下实例选取了所有位于 <div> 元素后的第一个 <p> 元素:

```css
div+p
{
  background-color:yellow;
}
```

### 后续兄弟选择器

后续兄弟选择器选取所有指定元素之后的相邻兄弟元素。

以下实例选取了所有 `<div>` 元素之后的所有相邻兄弟元素 `<p>` : 

```css
div~p
{
  background-color:yellow;
}
```

## CSS 伪类(Pseudo-classes)

CSS伪类是用来添加一些选择器的特殊效果。

### 语法

伪类的语法：

```
selector:pseudo-class {property:value;}
```
CSS类也可以使用伪类：

```
selector.class:pseudo-class {property:value;}
```
### anchor伪类

在支持 CSS 的浏览器中，链接的不同状态都可以以不同的方式显示

```css
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */
```
注意： 在CSS定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

注意： 在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

注意：伪类的名称不区分大小写。

### 伪类和CSS类

伪类可以与 CSS 类配合使用：

a.red:visited {color:#FF0000;}
 
```html
<a class="red" href="css-syntax.html">CSS 语法</a>
```
如果在上面的例子的链接已被访问，它会显示为红色。

### CSS :first-child 伪类

您可以使用 :first-child 伪类来选择父元素的第一个子元素。

注意：在IE8的之前版本必须声明<!DOCTYPE> ，这样 :first-child 才能生效。

匹配第一个 `<p>` 元素
在下面的例子中，选择器匹配作为任何元素的第一个子元素的 `<p>` 元素：

```
p:first-child
{
    color:blue;
}
```
匹配所有`<p>` 元素中的第一个 `<i>` 元素
在下面的例子中，选择相匹配的所有`<p>`元素的第一个 `<i>` 元素：

```css
p > i:first-child
{
    color:blue;
}
```
匹配所有作为第一个子元素的 `<p>` 元素中的所有 `<i>` 元素
在下面的例子中，选择器匹配所有作为元素的第一个子元素的 `<p>` 元素中的所有 `<i>` 元素：

```css
p:first-child i
{
    color:blue;
}
```
### CSS - :lang 伪类

:lang 伪类使你有能力为不同的语言定义特殊的规则

注意：IE8必须声明<!DOCTYPE>才能支持;lang伪类。

在下面的例子中，:lang 类为属性值为 no 的q元素定义引号的类型：

```css
q:lang(no) {quotes: "~" "~";}
```

### 所有CSS伪类/元素

[所有CSS伪类/元素](http://www.runoob.com/css/css-pseudo-classes.html)

## CSS 伪元素

CSS伪元素是用来添加一些选择器的特殊效果。

### 语法

伪元素的语法：

```
selector:pseudo-element {property:value;}
```
CSS类也可以使用伪元素：

```
selector.class:pseudo-element {property:value;}
```

### :first-line 伪元素

"first-line" 伪元素用于向文本的首行设置特殊样式。

在下面的例子中，浏览器会根据 "first-line" 伪元素中的样式对 p 元素的第一行文本进行格式化：

```css
p:first-line 
{
    color:#ff0000;
    font-variant:small-caps;
}
```
注意："first-line" 伪元素只能用于块级元素。

注意： 下面的属性可应用于 "first-line" 伪元素：

- font properties
- color properties 
- background properties
- word-spacing
- letter-spacing
- text-decoration
- vertical-align
- text-transform
- line-height
- clear

### :first-letter 伪元素

"first-letter" 伪元素用于向文本的首字母设置特殊样式：

```css
p:first-letter 
{
    color:#ff0000;
    font-size:xx-large;
}
```
注意： "first-letter" 伪元素只能用于块级元素。

注意： 下面的属性可应用于 "first-letter" 伪元素： 

- font properties
- color properties 
- background properties
- margin properties
- padding properties
- border properties
- text-decoration
- vertical-align (only if "float" is "none")
- text-transform
- line-height
- float
- clear

### 伪元素和CSS类

伪元素可以结合CSS类： 

```css
p.article:first-letter {color:#ff0000;}
```

```html
<p class="article">文章段落</p>
```
上面的例子会使所有 class 为 article 的段落的首字母变为红色。

### 多个伪元素

可以结合多个伪元素来使用。

在下面的例子中，段落的第一个字母将显示为红色，其字体大小为 xx-large。第一行中的其余文本将为蓝色，并以小型大写字母显示。

段落中的其余文本将以默认字体大小和颜色来显示：

```css
p:first-letter
{
    color:#ff0000;
    font-size:xx-large;
}
p:first-line 
{
    color:#0000ff;
    font-variant:small-caps;
}
```
### CSS - :before 伪元素

":before" 伪元素可以在元素的内容前面插入新内容。

下面的例子在每个 <h1>元素前面插入一幅图片：

```css
h1:before 
{
    content:url(smiley.gif);
}
```
### CSS - :after 伪元素

":after" 伪元素可以在元素的内容之后插入新内容。

下面的例子在每个 `<h1>` 元素后面插入一幅图片：

```css
h1:after
{
    content:url(smiley.gif);
}
```

### 所有CSS伪类/元素

![所有CSS伪类/元素](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%89%80%E6%9C%89CSS%E4%BC%AA%E7%B1%BB%E5%85%83%E7%B4%A0.png)


## CSS 导航栏

**垂直**

![垂直](http://p9myzkds7.bkt.clouddn.com/CSS/%E5%9E%82%E7%9B%B4.png)

**水平**

![水平](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%B0%B4%E5%B9%B3.png)

### 导航栏

熟练使用导航栏，对于任何网站都非常重要。

使用CSS你可以转换成好看的导航栏而不是枯燥的HTML菜单。

### 导航栏=链接列表

作为标准的HTML基础一个导航栏是必须的。在我们的例子中我们将建立一个标准的HTML列表导航栏。

导航条基本上是一个链接列表，所以使用 `<ul>` 和 `<li>`元素非常有意义：

```css
<ul>
  <li><a href="#home">主页</a></li>
  <li><a href="#news">新闻</a></li>
  <li><a href="#contact">联系</a></li>
  <li><a href="#about">关于</a></li>
</ul>
```
现在，让我们从列表中删除边距和填充：

```css
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
}
```
例子解析：

- list-style-type:none - 移除列表前小标志。一个导航栏并不需要列表标记
- 移除浏览器的默认设置将边距和填充设置为0

上面的例子中的代码是垂直和水平导航栏使用的标准代码。

### 垂直导航栏

上面的代码，我们只需要 `<a>`元素的样式，建立一个垂直的导航栏：

```css
a
{
    display:block;
    width:60px;
}
```
示例说明：

- display:block - 显示块元素的链接，让整体变为可点击链接区域（不只是文本），它允许我们指定宽度
- width:60px - 块元素默认情况下是最大宽度。我们要指定一个60像素的宽度

提示：查看 [完整样式的垂直导航栏的示例](http://www.runoob.com/try/try.php?filename=trycss_navbar_vertical_advanced)。

注意： 请务必指定 `<a>`元素在垂直导航栏的的宽度。如果省略宽度，IE6可能产生意想不到的效果。

### 垂直导航条实例

创建一个简单的垂直导航条实例，在鼠标移动到选项时，修改背景颜色：

![垂直导航条实例](http://p9myzkds7.bkt.clouddn.com/CSS/%E5%9E%82%E7%9B%B4%E5%AF%BC%E8%88%AA%E6%9D%A1%E5%AE%9E%E4%BE%8B.png)

```css
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    width: 200px;
    background-color: #f1f1f1;
}
 
li a {
    display: block;
    color: #000;
    padding: 8px 16px;
    text-decoration: none;
}
 
/* 鼠标移动到选项上修改背景颜色 */
li a:hover {
    background-color: #555;
    color: white;
}
```
### 激活/当前导航条实例

在点击了选项后，我们可以添加 "active" 类来标准哪个选项被选中：

![激活/当前导航条实例](http://p9myzkds7.bkt.clouddn.com/CSS/%E6%BF%80%E6%B4%BB%E3%80%81%E5%BD%93%E5%89%8D%E5%AF%BC%E8%88%AA%E6%9D%A1%E5%AE%9E%E4%BE%8B.png)

```css
.active {
    background-color: #4CAF50;
    color: white;
}
```
### 创建链接并添加边框

可以在 `<li>` or `<a>` 上添加text-align:center 样式来让链接居中。

可以在 border `<ul>` 上添加 border 属性来让导航栏有边框。如果要在每个选项上添加边框，可以在每个 `<li>` 元素上添加border-bottom :

```css
ul {
    border: 1px solid #555;
}
 
li {
    text-align: center;
    border-bottom: 1px solid #555;
}
 
li:last-child {
    border-bottom: none;
}
```
### 全屏高度的固定导航条

接下来我们创建一个左边是全屏高度的固定导航条，右边是可滚动的内容。

```css
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    width: 25%;
    background-color: #f1f1f1;
    height: 100%; /* 全屏高度 */
    position: fixed; 
    overflow: auto; /* 如果导航栏选项多，允许滚动 */
}
```
注意: 该实例可以在移动设备上使用。

### 水平导航栏

有两种方法创建横向导航栏。使用内联(inline)或浮动(float)的列表项。

这两种方法都很好，但如果你想链接到具有相同的大小，你必须使用浮动的方法。

### 内联列表项

建立一个横向导航栏的方法之一是指定元素， 上述代码是标准的内联:

```css
li
{
    display:inline;
}
```
实例解析：

- display:inline; -默认情况下，`<li>`元素是块元素。在这里，我们删除换行符之前和之后每个列表项，以显示一行。

提示: 查看 [完整样式的水平导航栏的示例](http://www.runoob.com/try/try.php?filename=trycss_navbar_horizontal_advanced)。

### 浮动列表项

在上面的例子中链接有不同的宽度。

对于所有的链接宽度相等，浮动 `<li>`元素，并指定为 `<a>`元素的宽度：

```css
li
{
    float:left;
}
a
{
    display:block;
    width:60px;
}
```
实例解析：

- float:left - 使用浮动块元素的幻灯片彼此相邻
- display:block - 显示块元素的链接，让整体变为可点击链接区域（不只是文本），它允许我们指定宽度
- width:60px - 块元素默认情况下是最大宽度。我们要指定一个60像素的宽度

提示:查看 完全样式的横向导航栏的示例。

### 水平导航条实例

创建一个水平导航条，在鼠标移动到选项后修改背景颜色。

```css
ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
    background-color: #333;
}
 
li {
    float: left;
}
 
li a {
    display: block;
    color: white;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none;
}
 
/*鼠标移动到选项上修改背景颜色 */
li a:hover {
    background-color: #111;
}
```
- #### 激活/当前导航条实例

  在点击了选项后，我们可以添加 "active" 类来标准哪个选项被选中：

  ```css
  .active {
      background-color: #4CAF50;
  }
  ```
- #### 链接右对齐

  将导航条最右边的选项设置右对齐 (float:right;)：

  ```css
  <ul>
    <li><a href="#home">主页</a></li>
    <li><a href="#news">新闻</a></li>
    <li><a href="#contact">联系</a></li>
    <li style="float:right"><a class="active" href="#about">关于</a></li>
  </ul>
  ```
- #### 添加分割线

  `<li>` 通过 border-right 样式来添加分割线:

  ```css
  /* 除了最后一个选项(last-child) 其他的都添加分割线 */
  li {
      border-right: 1px solid #bbb;
  }

  li:last-child {
      border-right: none;
  }
  ```
- #### 固定导航条

  可以设置页面的导航条固定在头部或者底部：

  **固定在头部**

  ```css
  ul {
      position: fixed;
      top: 0;
      width: 100%;
  }
  ```
  **固定在底部**

  ```css
  ul {
      position: fixed;
      bottom: 0;
      width: 100%;
  }
  ```
  注意: 该实例可以在移动设备上使用。

- #### 灰色水平导航条

  **固定在底部**

  ```css
  ul {
      border: 1px solid #e7e7e7;
      background-color: #f3f3f3;
  }

  li a {
      color: #666;
  }
  ```