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

#### 目标伪类选择器