title: 《你不知道的 JavaScript-上卷》
author: songxingguo
tags: []
categories:
  - 读书笔记
date: 2018-10-23 11:17:00
---
## 作用域和闭包

### 作用域是什么

#### 理解作用域

![理解作用域](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E4%BD%9C%E7%94%A8%E5%9F%9F1.png)

<!-- more -->

![理解作用域](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E7%90%86%E8%A7%A3%E4%BD%9C%E7%94%A8%E5%9F%9F.png)

#### 小结

作用域是一套规则，用于确定 **在何处以及如何查找变量**（标识符）。如果查找的目的是 **对变量进行赋值** ，那么就会使用 **LHS查询** ；如果目的是 **获取变量的值** ，就会使用 **RHS查询** 。

### 词法作用域

#### 词法阶段

![作用域气泡](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%B0%94%E6%B3%A1.png)

作用域查找会在找到第一个匹配的标识符时停止。

遮蔽效应：在多层嵌套作用域中可以定义同名的标识符，内部的标识符“屏蔽”了外部的标识符。

> 全局变量会自动成为全局对象（比如浏览器中的window对象）的属性。因此通过这种技术可以访问那些被同名变量所屏蔽的全局变量。但非全局变量如果被屏蔽了，无论如何都无法被访问。

![词法阶段](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E8%AF%8D%E6%B3%95%E4%BD%9C%E7%94%A8%E5%9F%9F.png)

#### 欺骗词法

![欺骗词法](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E6%AC%BA%E9%AA%97%E8%AF%8D%E6%B3%95.png)