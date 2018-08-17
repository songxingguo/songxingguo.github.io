title: 《JavaScript 高级程序设计》学习笔记-函数表达式
author: songxingguo
tags:
  - JavaScript
categories:
  - 前端技术
  - 读书笔记
date: 2018-08-17 16:24:00
---
# 函数表达式

函数表达式是 JavaScript 中的一个既强又容易令人困惑的特性。定义函数的方式有两种：一种是函数声明，另一种就是函数表达式。函数声明的语法是这样的。

```
function functionName(arg0, arg1, arg2) {
  // 函数体
}
```
首先是 function 关键字，然后是函数的名字，这就是指定函数名的方式。Firefox、Safari、Chrome 和 Opera 都给函数定义了一个非标准的 name 属性，通过这个属性可以访问到给函数指定的名字。这个属性的值永远等于跟在 function 关键字后面的标识符。

```
// 只是 Firefox、Safari、Chrome 和 Opera 有效
alert(functionName.name); // "fcuntionName"
```
<!-- more -->