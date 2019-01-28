title: CSS3 选择器
author: songxingguo
tags:
  - CSS3
categories:
  - 前端技术
date: 2019-01-24 15:55:00
---
## CSS3 选择器分类

> 基本选择器、层次选择器、伪类选择器、伪元素和属性选择器。

![CSS3 选择器分类](https://graphbed.qiniu.songxingguo.com/CSS3-selector/CSS3%E9%80%89%E6%8B%A9%E5%99%A8%E5%88%86%E7%B1%BB.png)

<!-- more -->

## 基本选择器

### 基本选择器语法

![基本选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E5%9F%BA%E6%9C%AC%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

### 通配符选择器

通配符选择器（*） 用来选择所有元素，也可以选择某个元素下的所有元素。

如：

```css
* {margin: 0; padding: 0;}
```
```css
.demo * {background: orange}
```
### 元素选择器

元素选择器（E）是 CSS 选择器中最常见、最基本的选择器。文档元素包括 html、body、p、div 等。

如：

```css
ul {background:grey}
```
### ID 选择器

ID 选择器（#id）具有唯一性，在一个页面中不会同时出现 id 相同的属性值。在使用 id 选择器时，需要在 id 属性值的前面加上 "#" 号。

如：

```css
first{background: lime; color: }
```
### 类选择器

类选择器（.class）是以独立于文档元素的方式来指定元素样式。"类选择器在一个页面中可以有多个相同的类名，而 ID 选择器其 ID 值在整个页面中是唯一的一个"

如：

```css
.item {background: green;color: #fff;font-weight: bold}
```
#### 多类选择器

通过多个类名进行匹配，其中任何一个类名不存在都无法匹配。

如：

```css
.item.important {background:red;}
```
#### 带标签的类名选择器

如：

```css
ul.block{background: #ccc}
```
### 群组选择器

群组选择器（selector1, selectoN）是将具有相同样式的元素分组在一起，每个选择器之间用逗号（,）隔开，例如“selector1,selector2, ...,selectorN”。

如：

```css
html,body {padding: 0}
```
## 层次选择器

层次选择器通过 HTML 的 DOM 元素间的层次关系获取元素，其主要的层次关系包括后代、父子、相邻兄弟和通用兄弟几种关系。

### 层次选择器语法

![层次选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E5%B1%82%E6%AC%A1%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

### 后代选择器

后代选择器（E F）也称为包含选择器，作用就是可以选择某元素的后代元素。**这里的 F 元素不管是 E 元素的子元素、孙辈元素或者更深层次的关系，都将被选中。**

如：

```css
div div {background: orange}
```
![使用后代选择器的效果](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E4%BD%BF%E7%94%A8%E5%90%8E%E4%BB%A3%E9%80%89%E6%8B%A9%E5%99%A8%E7%9A%84%E6%95%88%E6%9E%9C.png)

### 子选择器

子选择器（E>F）只能选择某元素的子元素，其中 E 为父元素，而 F 为子元素，其中 E>F 表示选择了 E 元素下所有的子元素 F。

如：

```css
body > div {background: green}
```
![使用子选择器的效果](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E4%BD%BF%E7%94%A8%E5%AD%90%E9%80%89%E6%8B%A9%E5%99%A8%E7%9A%84%E6%95%88%E6%9E%9C.png)

### 相邻兄弟选择器

相邻兄弟选择器（E + F）可以选择紧接在另一个元素后的元素，它们具有相同的父元素。换句话说：E 和 F 是同辈元素， F 元素在 E 元素后面，并且相邻。

如：

```
.active + div {background: lime}
```
![使用相邻兄弟选择器效果](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E4%BD%BF%E7%94%A8%E7%9B%B8%E9%82%BB%E5%85%84%E5%BC%9F%E9%80%89%E6%8B%A9%E5%99%A8%E6%95%88%E6%9E%9C.png)

### 通用兄弟选择器

通用兄弟选择器（E ~ F）是 CSS3 新增加的，用于 **选择某元素后面的所有兄弟元素** ，它们和相邻兄弟选择器类似，需要在同一个父元素之中。

如：

```css
.active ~ div {background: red}
```
![使用通用兄弟选择效果](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E4%BD%BF%E7%94%A8%E9%80%9A%E7%94%A8%E5%85%84%E5%BC%9F%E9%80%89%E6%8B%A9%E5%99%A8%E6%95%88%E6%9E%9C.png)

## 伪类选择器

### 动态伪类选择器

#### 动态伪类选择器语法

动态伪类包含两种，第一种是在链接中常看到的锚点伪类，另一种为用户行为伪类。

![动态伪类选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E5%8A%A8%E6%80%81%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

> 锚点伪类的设置必须遵循一个“爱恨原则”LoVe/HAte,也就是“link-visited-hover-active”。

### 目标伪类选择器

目标伪类选择器 “:target”用来匹配文档（页面）的 URI 中的目标元素。例如“#contact”“:target”就是用来匹配 ID 为 “contact” 的元素。

#### 目标伪类选择器语法

![目标伪类选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E7%9B%AE%E6%A0%87%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

如：

[菜鸟实例](http://www.runoob.com/try/try.php?filename=trycss3_target)

![“target”应用场景](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E2%80%9Ctarget%E2%80%9D%20%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF.png)

### 语言伪类选择器

语言伪类选择器是根据元素的语言编码匹配元素。E:lang(language) 表示选择匹配 E 的所有元素，且匹配元素指定了 lang 属性，而且其值为 language 。

如：

```css
p:lang(it)
{ 
  background:yellow;
}
```
```html
<p lang="it">Ciao bella!</p>
```
[菜鸟示例](http://www.runoob.com/try/try.php?filename=trycss_sel_lang)

### UI 元素状态伪类选择器

UI 元素状态伪类选择器主要用于 from 表单元素上，以提高网页的人机交互、操作逻辑以及页面的整体美观。

#### UI 元素状态伪类选择器语法

UI 元素的状态包括：启用、禁用、选中、未选中、获得焦点、失去焦点、锁定和待机等。

![UI 元素状态伪类选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/UI%20%E5%85%83%E7%B4%A0%E7%8A%B6%E6%80%81%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

如：

```html
<input type="text" value="Mouse" /><br>
<input type="text" disabled="disabled" value="Disneyland" />
```
```css
input[type="text"]:enabled
{
  background:#ffff00;
}
input[type="text"]:disabled
{
  background:#dddddd;
}
```
[菜鸟实例](http://www.runoob.com/try/try.php?filename=trycss3_enabled_disabled)

### 结构伪类选择器

结构伪类选择器可以根据元素在文档树中的某些特性（如相对位置）定位到它们。

#### 结构伪类选择器语法

![结构伪类选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E7%BB%93%E6%9E%84%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

![结构伪类选择器语法（续）](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E7%BB%93%E6%9E%84%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95%EF%BC%88%E7%BB%AD%EF%BC%89.png)

CSS3 结构伪类选择器

![CSS3 结构伪类选择器](https://graphbed.qiniu.songxingguo.com/CSS3-selector/CSS3%20%E7%BB%93%E6%9E%84%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8.png)

### 结构伪类选择器中的 n 是什么

4 个伪类选择器接受参数 n ：

- :nth-child(n)
- :nth-last-child(n)
- :nth-of-type(n)
- :nth-last-of-type(n)

参数按常用情况划分为 7 种情形：

1. **参数 n 为具体的数值**

   这个数值可以是任何大于 0 的正整数，例如 “:nth-child(3)”。

2. **参数 n 为表达式 “n*length”**

   选择 n 的倍数，其中 n 从 0 开始计算，而 length 为大于 0 的整数，例如 “:nth-child(2n)”。
  
3. **参数 n 为表达式 “n+length”**

   选择大于或等于 length 的元素，例如 “:nth-child(n+3)”。
   
4. **参数 n 为表达式 “-n+length”**

   选择小于或等于 length 的元素，例如 “:nth-child(-n+3)”。
   
5. **参数 n 为表达式 “n*length+b”**

   其中 b 是偏移值，其表示隔 length 个元素选中第 “n*length+b” 个元素，例如“:nth-child(2n+1)” 。
   
6. **参数 n 为关键词 “odd”**

   选择系类中的奇数（1、3、5、7）元素，其效果等同于 “:nth-child(2n+1)”。
   
7. **参数 n 为关键词 “even”**

   选择系类中的奇数（2、4、6、8）元素，其效果等同于 “:nth-child(2n)”。

结构伪类表达式的计算列表

![结构伪类表达式的计算列表](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E7%BB%93%E6%9E%84%E4%BC%AA%E7%B1%BB%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%E8%AE%A1%E7%AE%97%E5%88%97%E8%A1%A8.png)

> **:nth-child(n) 和 :nth-of-type(n) 的区别：**
> :nth-child 选择的是某父元素的子元素，这个子元素并没有指定确切的类型，而 :nth-of-type 选择的是某父元素的子元素，而且这个子元素是指定类型。

### 否定伪类选择器

否定选择器 “:not()”主要用于定位不匹配该选择器的元素。

#### 否定伪类选择器语法

![否定伪类选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E5%90%A6%E5%AE%9A%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

如：

```css
:not(footer){}
```
## 伪元素

伪元素可以用于定位文档中包含的文本，但无法在文档树中定位。

> CSS3 将伪元素调整为 双冒号。

### 伪元素 ::first-letter

“:first-letter” 用于选择文本块的第一个字母，除非在同一行中包含一些其他元素。
例如下沉字母或首字母。

### 伪元素 ::first-line

“::first-line” 用来匹配元素的第一行文本，匹配 block、inline-block、tablee-caption、table-cell 等级别元素的第一行。

### 伪元素 ::before 和 ::after

“::before” 和 “::after”不是指存在于标记中的内容，而是可以插入额外内容的位置。尽管生成的内容不会成为 DOM 的一部分，但它同样可以设置样式。

> 要为伪元素生成内容，还需要配合 content 属性。

如：

在页面上所有外部链接之后的括号中附加它们指向的 URL。

```css
a[herf^=http]::after {
  content:"("attr(herf)")"
}
```
### 伪元素 ::selection

“::selection” 是用来匹配突出显示的文本。

[菜鸟案例](http://www.runoob.com/try/try.php?filename=trycss3_selection)

## 属性选择器

属性选择器可以基于元素的属性来匹配元素，支持基于模式匹配来定位元素。

### 属性选择器语法

![属性选择器语法](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E5%B1%9E%E6%80%A7%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95.png)

![属性选择器语法（续）](https://graphbed.qiniu.songxingguo.com/CSS3-selector/%E5%B1%9E%E6%80%A7%E9%80%89%E6%8B%A9%E5%99%A8%E8%AF%AD%E6%B3%95%EF%BC%88%E7%BB%AD%EF%BC%89.png)

CSS3 中常见的通配符

![CSS3 中常见的通配符](https://graphbed.qiniu.songxingguo.com/CSS3-selector/CSS3%20%E4%B8%AD%E5%B8%B8%E8%A7%81%E7%9A%84%E9%80%9A%E9%85%8D%E7%AC%A6.png)

如：

多属性选择元素

```css
a[id][title]{background-color: red;}
```
> E[attr=val] 选择器中，属性和属性值必须完全匹配。例如只有a[class="links item"]{...} 才能匹配。

