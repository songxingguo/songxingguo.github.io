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
 <!-- more -->
 
 当解析器发现代码中调用 eval() 方法时，它会 **将传入的参数当作实际的 ECMAScript 语句来解析，然后把执行结果插入到原位置** 。 **通过 eval() 执行的代码被认为是包含该次调用的执行环境的一部分，因此被执行的代码具有与该执行环境相同的作用域链** 。这意味着 **通过 eval() 执行代码可以引用在包含环境中定义的变量** ，举个例子：
 
 ```
 var msg = "hello world";
 eval("alert(msg)"); //"hello world"
 ```
 可见，变量 msg 是在 eval() 调用环境之外定义的，但其调用的 alert() 仍然能够显示 "hello world"。 这是因为上面的第二行代码最终被替换成了一行真正的代码。同样地，我们 **可以在 eval() 调用中定义一个函数，然后在调用的外部代码中引用这个函数** ：
 
 ```
 eval("function sayHIi() { alert ('hi'); }");
 sayHi();
 ```
 显然，函数 sayHi() 是在 eval() 内部定义的。但由于对 eval() 的调用最终会被替换成定义的实际代码，因此可以在下一行调用 sayHi()。**对于变量也一样** ：
 
 ```
 eval("var msg =  'hello world'");
 alert(msg);
 ```
 

 