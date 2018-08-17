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

函数表达式是 JavaScript 中的一个既强又容易令人困惑的特性。定义函数的方式有两种：一种是 **函数声明** ，另一种就是 **函数表达式** 。**函数声明** 的语法是这样的。

```js
function functionName(arg0, arg1, arg2) {
  // 函数体
}
```
首先是 function 关键字，然后是函数的名字，这就是指定函数名的方式。Firefox、Safari、Chrome 和 Opera 都给函数定义了一个非标准的 name 属性，通过这个属性可以访问到给函数指定的名字。这个属性的值永远等于跟在 function 关键字后面的标识符。

```js
// 只是 Firefox、Safari、Chrome 和 Opera 有效
alert(functionName.name); // "fcuntionName"
```
<!-- more -->

关于函数声明，它的一个重复特征就是 **函数提升**（function declaration hoisting），意思是在 **执行代码之前会读取函数声明** 。**这意味着可以把函数声明放在调用它的语句后面** 。

```js
sayHi();
function sayHi() {
  alert("Hi!");
}
```
这个例子不会抛出错误，因为代码执行之前会先读取函数声明。
第二种创建函数的方式是使用 **函数表达式** 。函数表达式有几种不同的形式。下面是最常见的一个形式。

```js
var functionName = function(arg0, arg1, arg2) {
  // 函数体
}
```
这种形式看起来好像是常规的变量赋值语句，即创建一个函数并将它赋值给变量 functionName 。这种情况下创的函数叫做 **匿名函数**（anonymous function）,因为 **function 关键字后面没有标识符** 。（匿名函数有时候也叫拉姆达函数。）匿名函数的 name 属性是空字符串。
**函数表达式与其他表达式一样** ，**在使用前必须先赋值** 。以下代码会导致错误。

```js
sayHi();  //错误；函数还不存在
var sayHi = function() {
  alert("Hi!");
}
```
理解函数提升的关键，就是 **理解函数声明与函数表达式之间的区别** 。例如，执行以下代码的结果可能会让人意想不到。

```js
// 不要这样做！
if(condition){
  function sayHi() {
    alert("Hi!");
  }
} else {
  function sayHi() {
    alert("Yo!");
  }
}
```
表面上看，以上代码表示 conditionn 为 true 时，使用一个 sayHi() 的定义；否则，就使用另一个定义。实际上，这 **在 ECMAScript 中属于无效语法** ，**JavaScript引擎会尝试修正错误，将其转换为合理的状态** 。但问题是 **浏览器尝试修正错误的做法并不一致** 。大多数浏览器会返回第二个声明，忽略 condition ; Firefox 会在 condition 为 ture 时返回第一个声明。因此这种使用方式很危险，不应该出现在你的代码中。不过，**如果是使用函数表达式，那就没有什么问题了** 。

```js
// 可以这样做
var sayHi;

if (condition) {
  sayHi = function() {
    alert("Hi!");
  }
} else {
   sayHi = function() {
     alert("Yo!");
   }
}
```
这个例子不会有什么意外，不同函数会根据 condition 被赋值 sayHi。

能够创建函数再赋值给变量，也就能够把函数作为其他函数的值返回。下面是 createComparisonFunction() 函数：

```js
function createCompariseFunction(propertyName) {
  return function(object1, object2) {
    var value1 = object1[propertyName];
    var value2 = object2[propertyName];
    
    if (value1 < value2) {
       return -1;
    } else i f (value1 > value2) {
      return 1;
    } else {
      return 0;
    }
  };
}
```
createComparisonFunction() 就返回了一个匿名函数。返回的函数可能会被赋值给一个变量，或者以其他方式被调用；不过，**在 createComparisonFunction() 函数内部，它是匿名的** 。在把函数当成值来使用的情况下，都可以使用匿名函数。不过，这并不是匿名函数唯一的用途。

## 递归

递归函数是在一个函数通过名字调用自身的情况下构成的，如下所示。

```js
function factorial(num) {
   if (num <= 1) {
     return 1;
   } else {
     return num * factorial(num -1);
   }
}
```
这是一个经典的递归阶乘函数。虽然这个函数表面看来没什么问题，但下面的代码却可能导致它出错。

```js
var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4)); //出错！
```
以上代码先把 factorial() 函数保存在变量 anotherFactorial 中，然后将 factorial 变量设置为 null, 结果  **指向原始函数的引用只剩下一个** 。但在接下来调用 anotherFactorial() 时，由于  **必须执行 factorial()** ，而 **factorial 已经不再是函数**，所以就会导致错误。在这种情况下，使用 arguments.callee 可以解决这个问题。

我们知道，**arguments.callee 是一个指向正在执行的函数的指针** ，，因此可以用它来实现对函数的递归调用，例如：

```js
function factorial(num) {
  if (num <= 1) {
    return 1;
  } else {
    return num  * arguments.callee(num -1);
  }
}
```
加粗的代码显示，通过使用 arguments.callee 代替函数名，可以确保无论怎样调用函数都不会出问题。因此，在编写递归函数时，使用 arguments.callee 总比使用函数名更保险。

但在 **严格模式** 下，**不能通过脚本访问 arguments.callee** ，访问这个属性会导致错误。不过，**可以使用命名函数表达式达成相同的结果** 。例如：

```js
var factorial (function f(num) {
  if (num <= 1) {
    return 1;
  } else {
    return num * f(num -1);
  }
});
```
以上代码创建了一个 **名为 f() 的命名函数表达式** ，然后 **将它赋值给变量 factorial** 。即使把函数赋值给了另一个变量，函数的名字 f 仍然有效，所以递归调用照样能正确完成。这种方式在严格模式和非严格模式下都行得通。

## 闭包

有不少开发人员总是搞不清 **匿名函数** 和 **闭包** 这两个概念。因此经常混用。**闭包** 是指 **有权访问另一个函数作用域中变量的函数** 。**创建闭包的常见方式** ，就是 **在一个函数内部创建另一个函数** ，仍以前面的 createComparisonFuncton() 函数为例，请注意加粗的代码。

```js
functioon createComparisonFunction(propertyName) {
  return function(object1, object2) {
    var value1 = object1[propertyName];
    var value2 = object2[propertyName];
    
    if (value1 < value2) {
       return -1;
    } else i f (value1 > value2) {
      return 1;
    } else {
      return 0;
    }
  };
}
```
在这个例子中，突出的那两行代码是 **内部函数（一个匿名函数）中的代码** ，**这两行代码访问了外部函数中的变量 propertyName** 。即使这个内部函数被返回了，而且是在其他地方被调用了 ，但它仍然可以访问变量 propertyName。**之所以还能够访问这个变量** ，是因为 **内部函数的作用域链中包含 createComparisonFunction() 的作用域** 。要彻底搞清楚其中的细节，必须从理解函数被调用的时候都会发生什么入手。

前面作用域链的概念。而 **有关如何创建作用域链以及作用域链有什么作用的细** 节，对彻底理解闭包至关重要。当 **某个函数被调用** 时，**会创建** 一个 **执行环境**（execution contect）及 **相应的作用域链** 。然后，**使用 arguments 和其他命名参数的值来初始化函数的活动对象** （ activation object）。但 **在作用域链中** ，**外部函数的活动对象始终处于第二位**，**外部函数的外部函数的活动对象处于第三位** ，......**直至作为作用域链终点的全局执行环境** 。







