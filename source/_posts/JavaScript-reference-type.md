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

- #### eval() 方法

  > 整个 ECMAScript 语言中 **最强大的一个方法** ： **eval()** 。
 
  eval() 方法就像一个完整的 **ECMAScript 解析器** ，它只接受一个参数，即要 **执行的 ECMAScript（或 JavaScript）字符串** 。看下面的例子：
 
   ```js
   eval("alert('hi')");
   ```
   这行代码的作用等价于下面这行代码：

   ```js
   alert("hi");
   ```
   <!-- more -->

   当解析器发现代码中调用 eval() 方法时，它会 **将传入的参数当作实际的 ECMAScript 语句来解析，然后把执行结果插入到原位置** 。 **通过 eval() 执行的代码被认为是包含该次调用的执行环境的一部分，因此被执行的代码具有与该执行环境相同的作用域链** 。这意味着 **通过 eval() 执行代码可以引用在包含环境中定义的变量** ，举个例子：

   ```js
   var msg = "hello world";
   eval("alert(msg)"); //"hello world"
   ```
   可见，变量 msg 是在 eval() 调用环境之外定义的，但其调用的 alert() 仍然能够显示 "hello world"。 这是因为上面的第二行代码最终被替换成了一行真正的代码。同样地，我们 **可以在 eval() 调用中定义一个函数，然后在调用的外部代码中引用这个函数** ：

   ```js
   eval("function sayHIi() { alert ('hi'); }");
   sayHi();
   ```
   显然，函数 sayHi() 是在 eval() 内部定义的。但由于对 eval() 的调用最终会被替换成定义的实际代码，因此可以在下一行调用 sayHi()。**对于变量也一样** ：

   ```js
   eval("var msg =  'hello world'");
   alert(msg);
   ```
   在 eval() 中创建的 **变量或函数都不会被提升** ，因为在解析代码的时候，他们被包含在一个字符串中；它们 **只在 eval() 执行的时候创建** 。

  **严格模式** 下，**在外部访问不到 eval() 中创建的任何变量或函数** ，因此前面两个例子都会导致错误。同样，**严格模式** 下，**为 eval 赋值也会导致错误** ：
  
  ```js
  "use strict"
  evval = "hi"; //causes error
  ```
 > **能够解释代码字符串的能力非常强大，但也非常危险** 。因此在使用 eval() 时必须极为谨慎，特别是在用它执行用户输入数据的情况下。否则，可能会有恶意用户输入威胁你的站点或应用程序的安全的代码（即所谓的 **代码注入** ）。
 
- #### Global 对象属性

  下表列出了 Global 对象的所有属性。
  
  ![Global 对象的所有属性](http://p9myzkds7.bkt.clouddn.com/JavaScript-reference-typeGlobal%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%89%80%E6%9C%89%E5%B1%9E%E6%80%A7.png)
  
  <!-- | 属性           | 说明                    |
  | -------------- | ----------------------- |
  | undefined      | 特殊值 undefined        |
  | NaN            | 特殊值 NaN              |
  | Infinity       | 特殊值 Infinity         |
  | Object         | 构造函数 Object         |
  | Array          | 构造函数 Array          |
  | Function       | 构造函数 Function       |
  | Boolean        | 构造函数 Boolean        |
  | String         | 构造函数 String         |
  | Number         | 构造函数 Number         |
  | Date           | 构造函数 Date           |
  | RegExp         | 构造函数 RegExp         |
  | Error          | 构造函数 Error          |
  | EvalError      | 构造函数 EvalError      |
  | RangeError     | 构造函数 RangeError     |
  | ReferenceError | 构造函数 ReferenceError |
  | SyntaxError    | 构造函数 SyntaxError    |
  | Type Error     | 构造函数 TypeError      |
  | URIError       | 构造函数 URIError       | -->
  
  ECMAScript 5 明确禁止给 undefined 、NaN 和 Infinity 赋值，这样做即使在非严格模式下也会导致错误。
 
- #### window 对象

  ECMAaScript 虽然没有指出如何直接访问 Global 对象，但 Web 浏览器都是 **将这个全局对象作为 window 对象的一部分加以实现的** 。因此，在全局作用域中声明的所有变量和函数，就都 **成为了 window 对象的属性** 。来看一个例子。
  
  ```js
  var color = "red";
   
  function sayColor() {
    alert(window.color);
  }
  
  window.sayColor(); // "red"
  ```
  这里定义了一个名为 color 的全局变量和一个名为 sayColor() 的全局函数。在 sayColor() 内部我们通过 window.color 来访问 color 变量，以说明全局变量是 window 对象的属性。然后，又使用 window.sayColor() 来直接通过 window 对象调用这个函数，结果显示在了警告框中。
  
  > JavaScript 中的 window 对象除了扮演 ECMAScript 规定的 Global 对象的角色外，还承担的了很多别的任务。
  
  另一种 **取得 Global 对象的方法** 是使用以下代码：
  
  ```js
  var global = function() {
    return this;
  }
  ```
  以上代码创建了一个立即调用的函数表达式，返回 this 的值。如前所述，**在没有给函数明确指定 this 值的情况下** （无论是通过 **将函数添加为对象的方法** ，还是通过 **call()** 或 **apply()** ）, **this 值等于 Global 对象** 。而像这样通过简单的返回 this 来取得 Global 对象，**在任何执行环境下都是可行的** 。
  
 
 
 
 
 