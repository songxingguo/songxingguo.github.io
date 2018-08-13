title: 《JavaScript 高级程序设计》学习笔记-引用类型
author: songxingguo
tags:
  - JavaScript
categories:
  - 前端技术
  - 读书笔记
date: 2018-08-13 15:23:20
---
## 单体内置对象

### Global 对象

- eval() 方法

 > 整个 ECMAScript 语言中 **最强大的一个方法** ： **eval()** 。
 
 eval() 方法就像一个完整的 **ECMAScript 解析器** ，它只接受一个参数，即要 **执行的 ECMAScript（或 JavaScript）字符串** 。看下面的例子：
 
 ```
 eval("alert('hi')");
 ```
 这行代码的作用等价于下面这行代码：
 
 ```
 alert("hi");
 ```
 当解析器发现代码中调用 eval() 方法时，它会将传入的参数当作实际的 ECMAScript 语句来解析，然后把执行结果插入到原位置。通过 eval() 执行的代码被认为是包含该次调用的执行环境的一部分，因此被执行的代码具有与该执行环境相同的作用域链。这意味着通过 eval() 执行代码可以引用在包含环境中定义的变量，举个例子：
 
 ```
 var msg = "hello world";
 eval("alert(msg)"); //"hello world"
 ```