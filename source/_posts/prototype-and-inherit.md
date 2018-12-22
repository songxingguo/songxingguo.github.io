title: JavaScript 原型链和继承
author: songxingguo
tags:
  - JavaScript
categories:
  - 前端技术
date: 2018-12-22 11:01:00
---
## 原型链

构造函数、原型对象和实例三者之间的关系：

![关系图](https://graphbed.qiniu.songxingguo.com/prototype-and-inherit/1544147270338.png)

<!-- more -->

三个概念：

- 构造函数：被 new 操作符操作的普通函数。

- 实例：

- 原型对象：

三个属性： 

- __proto__

- prototype

- constuctor

对象间的关系：

![三者关系](https://graphbed.qiniu.songxingguo.com/prototype-and-inherit/%E4%B8%89%E8%80%85%E7%9A%84%E5%85%B3%E7%B3%BB.jpg)


> 抓住 new 运算符，然后向上梳理。

### new 运算符

[new 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)

![new 运算符](https://graphbed.qiniu.songxingguo.com/prototype-and-inherit/new%E6%93%8D%E4%BD%9C%E7%AC%A6)

重点：

- 如果构造函数没有返回对象，才会返回创建的新对象。

## 继承

