title: 《你不知道的 JavaScript-中卷》
author: songxingguo
tags:
  - JavaScript
categories:
  - 读书笔记
date: 2018-11-20 18:15:00
---
## 类型和语法

### 类型

![类型](https://graphbed.qiniu.songxingguo.com/deep-javascript-2/%E7%B1%BB%E5%9E%8B.png)

大多数开发者认为，像 JavaScript 这样的动态语言是没有类型（type）的。让我们来看看ES5.1 规范（ http://www.ecma-international.org/ecma-262/5.1/ ）对此是如何界定的：

> ECMAScript 语言中所有的值都有一个对应的语言类型。ECMAScript 语言类型包括 Undefined、Null、Boolean、String、Number 和 Object。

<!-- more -->

#### 内置类型

JavaScript 有七种内置类型：

- 空值（null）
- 未定义（undefined）
- 布尔值（ boolean）
- 数字（number）
- 字符串（string）
- 对象（object）
- 符号（symbol，ES6 中新增）

> 除对象之外，其他统称为“基本类型”。

我们可以用 typeof 运算符来查看值的类型，它返回的是类型的字符串值。有意思的是，这七种类型和它们的字符串值并不一一对应：

```js
typeof undefined === "undefined"; // true
typeof true === "boolean"; // true
typeof 42 === "number"; // true
typeof "42" === "string"; // true
typeof { life: 42 } === "object"; // true
// ES6中新加入的类型
typeof Symbol() === "symbol"; // true
```
以上 **六种类型均有同名的字符串值与之对应** 。

你可能注意到 null 类型不在此列。它比较特殊，typeof 对它的处理有问题：

```js
typeof null === "object"; // true
```

**正确的返回结果应该是 "null"，但这个 bug 由来已久** ，在 JavaScript 中已经存在了将近二十年，也许永远也不会修复，因为这牵涉到太多的 Web 系统，“修复”它会产生更多的bug，令许多系统无法正常工作。

我们需要使用 **复合条件来检测 null 值的类型** ：

```js
var a = null;
(!a && typeof a === "object"); // true
```
null 是基本类型中唯一的一个“假值”（falsy 或者 false-like，参见第 4 章）类型，typeof 对它的返回值为 "object"。

还有一种情况：

```js
typeof function a(){ /* .. */ } === "function"; // true
```
这样看来，**function**（函数）也是 **JavaScript 的一个内置类型** 。然而查阅规范就会知道，它实际上是 **object 的一个“子类型”** 。具体来说，**函数是“可调用对象”**，它有一个 **内部属性 [[Call]]** ，**该属性使其可以被调用** 。

函数不仅是 **对象** ，还可以 **拥有属性** 。例如：

```js
function a(b,c) {
 /* .. */
}
```
函数对象的 **length 属性** 是其 **声明的参数的个数** ：

```js
a.length; // 2
```
因为该函数声明了两个命名参数，b 和 c，所以其 length 值为 2。

再来看看数组。JavaScript 支持数组，那么它是否也是一个特殊类型？

```js
typeof [1,2,3] === "object"; // true
```
不，数组也是对象。确切地说，它也是 **object 的一个“子类型”**（参见第 3 章），数组的元素按数字顺序来进行索引（而非普通像对象那样通过字符串键值），其 **length 属性** 是 **元素的个数** 。

#### 值和类型

JavaScript 中的 **变量是没有类型的** ，**只有值才有** 。变量可以随时持有任何类型的值。

换个角度来理解就是，JavaScript 不做“类型强制”；也就是说，语言引擎不要求变量总是持有与其初始值同类型的值。一个变量可以现在被赋值为字符串类型值，随后又被赋值为数字类型值。

42 的类型为 number，并且无法更改。而 "42" 的类型为 string。数字 42 可以通过强制类型
转换（coercion）为字符串 "42"（参见第 4 章）。

在对变量执行 typeof 操作时，得到的结果并不是该变量的类型，而是该变量持有的值的类型，因为 JavaScript 中的变量没有类型。

```js
var a = 42;
typeof a; // "number"
a = true;
typeof a; // "boolean"
typeof 运算符总是会返回一个字符串：
typeof typeof 42; // "string"
typeof 42 首先返回字符串 "number"，然后 typeof "number" 返回 "string"。
```

##### undefined 和 undeclared

变量在未持有值的时候为 undefined。此时 typeof 返回 "undefined"：

```js
var a;
typeof a; // "undefined"
var b = 42;
var c;
// later
b = c;
typeof b; // "undefined"
typeof c; // "undefined"
```
大多数开发者倾向于将 undefined 等同于 undeclared（未声明），但在 JavaScript 中它们完全是两回事。

**已在作用域中声明** 但还 **没有赋值的变量** ，是 **undefined** 的。相反，还 **没有在作用域中声明过的变量** ，是 **undeclared** 的。

例如：

```js
var a;
a; // undefined
b; // ReferenceError: b is not defined
```
浏览器对这类情况的处理很让人抓狂。上例中，“b is not defined”容易让人误以为是“b is undefined”。这里再强调一遍，**“undefined”和“is not defined”是两码事** 。此时如果浏览器报错成“b is not found”或者“b is not declared”会更准确。

更让人抓狂的是 typeof 处理 undeclared 变量的方式。例如：

```js
var a;
typeof a; // "undefined"
typeof b; // "undefined"
```
对于 **undeclared（或者 not defined）变量，typeof 照样返回 "undefined"** 。请注意虽然 b 是一个 undeclared 变量，但 typeof b 并没有报错。这是因为 **typeof 有一个特殊的安全防范机制** 。

此时 typeof 如果能返回 undeclared（而非 undefined）的话，情况会好很多。

##### typeof Undeclared

该安全防范机制对在浏览器中运行的 JavaScript 代码来说还是很有帮助的，因为多个脚本文件会在共享的全局命名空间中加载变量。

>很多开发人员认为全局命名空间中不应该有变量存在，所有东西都应该被封装到模块和私有 / 独立的命名空间中。理论上这样没错，却不切实际。然而这仍不失为一个值得为之努力奋斗的目标。好在 ES6 中加入了对模块的支持，这使我们又向目标迈近了一步。

举个简单的例子，在程序中使用全局变量 DEBUG 作为“调试模式”的开关。在输出调试信息到控制台之前，我们会检查 DEBUG 变量是否已被声明。顶层的全局变量声明 var DEBUG = true 只在 debug.js 文件中才有，而该文件只在开发和测试时才被加载到浏览器，在生产环境中不予加载。

问题是如何在程序中检查全局变量 DEBUG 才不会出现 ReferenceError 错误。这时 **typeof 的安全防范机制就成了我们的好帮手** ：

```js
// 这样会抛出错误
if (DEBUG) {
 console.log( "Debugging is starting" );
}
// 这样是安全的
if (typeof DEBUG !== "undefined") {
 console.log( "Debugging is starting" );
}
```
这不仅对用户定义的变量（比如 DEBUG）有用，对内建的 API 也有帮助：

```js
if (typeof atob === "undefined") {
 atob = function() { /*..*/ };
}
```
>如果要为某个缺失的功能写 polyfill（即衬垫代码或者补充代码，用来补充当前运行环境中缺失的功能），一般不会用 var atob 来声明变量 atob。如果在 if 语句中使用 var atob，声明会被提升（hoisted，参见《你不知道的 JavaScript（上卷）》1 中的“作用域和闭包”部分）到作用域（即当前脚本或函数的作用域）的最顶层，即使 if 条件不成立也是如此（因为 atob 全局变量已经存在）。在有些浏览器中，对于一些特殊的内建全局变量（通常称为“宿主对象”，host object），这样的重复声明会报错。去掉 var 则可以防止声明被提升。

还有一种不用通过 typeof 的安全防范机制的方法，就是 **检查所有全局变量是否是全局对象的属性** ，浏览器中的全局对象是 window。所以前面的例子也可以这样来实现：

```js
if (window.DEBUG) {
 // ..
}
if (!window.atob) {
 // ..
}
```
与 undeclared 变量不同，访问不存在的对象属性（甚至是在全局对象 window 上）不会产生ReferenceError 错误。

一些开发人员不喜欢通过 window 来访问全局对象，尤其当代码需要运行在多种 JavaScript 环境中时（不仅仅是浏览器，还有服务器端，如 node.js 等），因为此时全局对象并非总是 window。

从技术角度来说，**typeof 的安全防范机制对于非全局变量也很管用** ，虽然这种情况并不多见，也有一些开发人员不大愿意这样做。如果想让别人在他们的程序或模块中复制粘贴你的代码，就需要检查你用到的变量是否已经在宿主程序中定义过：

```js
function doSomethingCool() {
  var helper = (typeof FeatureXYZ !== "undefined") ? FeatureXYZ: function() {
    /*.. default feature ..*/
  };
  var val = helper();
  // ..
}
```
其他模块和程序引入 doSomethingCool() 时，doSomethingCool() 会检查 FeatureXYZ 变量是
否已经在宿主程序中定义过；如果是，就用现成的，否则就自己定义：

```js
// 一个立即执行函数表达式（IIFE，参见《你不知道的JavaScript（上卷）》“作用域和闭包”
// 部分的3.3.2节）
(function() {
  function FeatureXYZ() {
    /*.. my XYZ feature ..*/
  }
  // 包含doSomethingCool(..)
  function doSomethingCool() {
    var helper = (typeof FeatureXYZ !== "undefined") ? FeatureXYZ: function() {
      /*.. default feature ..*/
    };
    var val = helper();
    // ..
  }
  doSomethingCool();
})();
```
这里，FeatureXYZ 并不是一个全局变量，但我们还是可以使用 typeof 的安全防范机制来做
检查，因为这里没有全局对象可用（像前面提到的 window.___）。

还有一些人喜欢使用“ **依赖注入** ”（dependency injection）设计模式，就是将依赖通过参数显式地传递到函数中，如：

```js
function doSomethingCool(FeatureXYZ) {
  var helper = FeatureXYZ ||
  function() {
    /*.. default feature ..*/
  };
  var val = helper();
  // ..
}
```
上述种种选择和方法各有利弊。好在 typeof 的安全防范机制为我们提供了更多选择。

#### 小结

JavaScript 有 七 种 内 置 类 型：null、undefined、boolean、number、string、object 和 symbol，可以使用 typeof 运算符来查看。

变量没有类型，但它们持有的值有类型。类型定义了值的行为特征。

很多开发人员将 undefined 和 undeclared 混为一谈，但在 JavaScript 中它们是两码事。

undefined 是值的一种。undeclared 则表示变量还没有被声明过。

遗憾的是，JavaScript 却将它们混为一谈，在我们试图访问 "undeclared" 变量时这样报错：ReferenceError: a is not defined，并且 typeof 对 undefined 和 undeclared 变量都返回 "undefined"。

然而，通过 typeof 的安全防范机制（阻止报错）来检查 undeclared 变量，有时是个不错的办法。

### 值

![值](https://graphbed.qiniu.songxingguo.com/deep-javascript-2/%E5%80%BC.png)

数组（array）、字符串（string）和数字（number）是一个程序最基本的组成部分，但在JavaScript 中，它们可谓让人喜忧掺半。

#### 数组

和其他强类型语言不同，在 JavaScript 中，数组可以容纳任何类型的值，可以是字符串、数字、对象（object），甚至是其他数组（多维数组就是通过这种方式来实现的）：

```js
var a = [ 1, "2", [3] ];

a.length; // 3
a[0] === 1; // true
a[2][0] === 3; // true
```
对数组声明后即 **可向其中加入值** ，**不需要预先设定大小**（参见 3.4.1 节）：

```js
var a = [ ];

a.length; // 0

a[0] = 1;
a[1] = "2";
a[2] = [ 3 ];

a.length; // 3
```
> 使用 delete 运算符可以将单元从数组中删除，但是请注意，单元删除后，数组的 length 属性并不会发生变化。第 5 章将详细介绍 delete 运算符。

在创建“稀疏”数组（sparse array，即含有空白或空缺单元的数组）时要特别注意：

```js
var a = [ ];

a[0] = 1;
// 此处没有设置a[1]单元
a[2] = [ 3 ];

a[1]; // undefined
a.length; // 3
```
上面的代码可以正常运行，但其中的“空白单元”（empty slot）可能会导致出人意料的结果。a[1] 的值为 undefined，但这与将其显式赋值为 undefined（a[1] = undefined）还是有所区别。

数组通过数字进行索引，但有趣的是它们也是对象，所以也 **可以包含字符串键值和属性**（但这些并 **不计算在数组长度内** ）：

```js
var a = [ ];
a[0] = 1;
a["foobar"] = 2;
a.length; // 1
a["foobar"]; // 2
a.foobar; // 2
```
这里有个问题需要特别注意，如果 **字符串键值能够被强制类型转换为十进制数字** 的话，它就**会被当作数字索引来处理** 。

```js
var a = [ ];
a["13"] = 42;
a.length; // 14
```
在数组中加入字符串键值 / 属性并不是一个好主意。建议使用 **对象来存放键值 / 属性值** ，用 **数组来存放数字索引值** 。

##### 类数组

有时需要将类数组（一组通过数字索引的值）转换为真正的数组，这一般通过数组工具函数（如 indexOf(..)、concat(..)、forEach(..) 等）来实现。

例如，一些 DOM 查询操作会返回 DOM 元素列表，它们并非真正意义上的数组，但十分类似。另一个例子是 **通过 arguments 对象（类数组）将函数的参数当作列表来访问**（从ES6 开始已废止）。

工具函数 **slice(..)** 经常被用于这类转换：

```js
function foo() {
  var arr = Array.prototype.slice.call( arguments );
  arr.push( "bam" );
  console.log( arr );
}

foo( "bar", "baz" ); // ["bar","baz","bam"]
```
如上所示，**slice() 返回参数列表（上例中是一个类数组）的一个数组复本** 。用 ES6 中的内置工具函数 **Array.from(..)** 也能实现同样的功能：

```js
var arr = Array.from( arguments );
```
#### 字符串

字符串经常被当成字符数组。字符串的内部实现究竟有没有使用数组并不好说，但 **JavaScript 中的字符串和字符数组并不是一回事** ，最多只是看上去相似而已。

例如下面两个值：

```js
var a = "foo";
var b = ["f","o","o"];
```
字符串和数组的确很相似，它们都是类数组，都有 length 属性以及 indexOf(..)（从 ES5开始数组支持此方法）和 concat(..) 方法：

```js
[source,js]

a.length; // 3
b.length; // 3

a.indexOf( "o" ); // 1
b.indexOf( "o" ); // 1

var c = a.concat( "bar" ); // "foobar"
var d = b.concat( ["b","a","r"] ); // ["f","o","o","b","a","r"]

a === c; // false
b === d; // false

a; // "foo"
b; // ["f","o","o"]
```
但这并不意味着它们都是“字符数组”，比如：

```js
a[1] = "O";
b[1] = "O";

a; // "foo"
b; // ["f","O","o"]
```
JavaScript 中 **字符串是不可变的** ，而数组是可变的。并且 a[1] 在 JavaScript 中并非总是合法语法，在老版本的 IE 中就不被允许（现在可以了）。**正确的方法应该是 a.charAt(1)** 。

**字符串不可变** 是指 **字符串的成员函数不会改变其原始值** ，而是 **创建并返回一个新的字符串** 。而数组的成员函数都是在其原始值上进行操作。

```js
c = a.toUpperCase();
a === c; // false
a; // "foo"
c; // "FOO"

b.push( "!" );
b; // ["f","O","o","!"]
```
许多数组函数用来处理字符串很方便。虽然字符串没有这些函数，但可以通过“借用”数组的非变更方法来处理字符串：

```js
a.join; // undefined
a.map; // undefined

var c = Array.prototype.join.call( a, "-" );
var d = Array.prototype.map.call( a, function(v){
  return v.toUpperCase() + ".";
} ).join( "" );

c; // "f-o-o"
d; // "F.O.O."
```
另一个不同点在于字符串反转（JavaScript 面试常见问题）。**数组有一个字符串没有的可变更成员函数 reverse()** ：

```js
a.reverse; // undefined
b.reverse(); // ["!","o","O","f"]
b; // ["f","O","o","!"]
```
可惜我们无法“借用”数组的可变更成员函数，因为字符串是不可变的：

```
Array.prototype.reverse.call( a );
// 返回值仍然是字符串"foo"的一个封装对象（参见第3章）:(
```
一个变通（破解）的办法是 **先将字符串转换为数组，待处理完后再将结果转换回字符串** ：

```js
var c = a
 // 将a的值转换为字符数组
 .split( "" )
 // 将数组中的字符进行倒转
 .reverse()
 // 将数组中的字符拼接回字符串
 .join( "" );
 
c; // "oof"
```
这种方法的确简单粗暴，但对简单的字符串却完全适用。

> 请注意！上述方法 **对于包含复杂字符（Unicode，如星号、多字节字符等）的字符串并不适用** 。这时则需要功能更加完备、能够处理 Unicode 的工具库。可以参考 Mathias Bynen 的 Esrever（https://github.com/mathiasbynents/esrever）。

如果需要经常以字符数组的方式来处理字符串的话，倒不如 **直接使用数组** 。这样就不用在字符串和数组之间来回折腾。可以 **在需要时使用 join("") 将字符数组转换为字符串** 。

#### 数字

JavaScript 只有一种数值类型：number（数字），包括“整数”和带小数的十进制数。此处“整数”之所以加引号是因为和其他语言不同，**JavaScript 没有真正意义上的整数** ，这也是它一直以来为人诟病的地方。这种情况在将来或许会有所改观，但目前只有数字类型。

JavaScript 中的 **“整数”** 就是 **没有小数的十进制数** 。所以 **42.0 即等同于“整数”42** 。

与大部分现代编程语言（包括几乎所有的脚本语言）一样，JavaScript 中的数字类型是基于 **IEEE 754** 标准来实现的，该标准通常也被称为“**浮点数**”。**JavaScript 使用的是“双精度”格式**（即 64 位二进制）。

网上的很多文章详细介绍了二进制浮点数在内存中的存储方式，以及不同方式各自的考量。要想正确使用 JavaScript 中的数字类型，并非一定要了解数位（bit）在内存中的存储方式，所以本书对此不多作介绍，有兴趣的读者可以参见 IEEE 754 的相关细节。

##### 数字的语法

JavaScript 中的数字常量一般用十进制表示。例如：

```js
var a = 42;
var b = 42.3;
```
数字前面的 0 可以省略：

```js
var a = 0.42;
var b = .42;
```
小数点后小数部分最后面的 0 也可以省略：

```js
var a = 42.0;
var b = 42.;
```
>  42. 这种写法没问题，只是不常见，但从代码的可读性考虑，不建议这样写。

默认情况下大部分数字都以十进制显示，小数部分最后面的 0 被省略，如：

```js
var a = 42.300;
var b = 42.0;

a; // 42.3
b; // 42
```
**特别大和特别小的数字默认用指数格式显示** ，与 toExponential() 函数的输出结果相同。

例如：

```js
var a = 5E10;
a; // 50000000000
a.toExponential(); // "5e+10"

var b = a * a;
b; // 2.5e+21

var c = 1 / a;
c; // 2e-11
```
由于数字值可以使用 Number 对象进行封装（参见第 3 章），因此数字值可以调用 **Number.prototype** 中的方法（参见第 3 章）。例如，tofixed(..) 方法可指定小数部分的显示位数：

```js
var a = 42.59;

a.toFixed( 0 ); // "43"
a.toFixed( 1 ); // "42.6"
a.toFixed( 2 ); // "42.59"
a.toFixed( 3 ); // "42.590"
a.toFixed( 4 ); // "42.5900"
```
请注意，上例中的输出结果实际上是 **给定数字的字符串形式** ，**如果指定的小数部分的显示位数多于实际位数就用 0 补齐** 。

toPrecision(..) 方法用来 **指定有效数位的显示位数** ：

```js
var a = 42.59;

a.toPrecision( 1 ); // "4e+1"
a.toPrecision( 2 ); // "43"
a.toPrecision( 3 ); // "42.6"
a.toPrecision( 4 ); // "42.59"
a.toPrecision( 5 ); // "42.590"
a.toPrecision( 6 ); // "42.5900"
```
上面的方法不仅适用于数字变量，也适用于数字常量。不过对于 . 运算符需要给予特别注意，因为它是一个有效的数字字符，会被优先识别为数字常量的一部分，然后才是对象属性访问运算符。

```js
// 无效语法：
42.toFixed( 3 ); // SyntaxError

// 下面的语法都有效：
(42).toFixed( 3 ); // "42.000"
0.42.toFixed( 3 ); // "0.420"
42..toFixed( 3 ); // "42.000"
```
42.tofixed(3) 是无效语法，因为 . 被视为常量 42. 的一部分（如前所述），所以没有 . 属
性访问运算符来调用 tofixed 方法。

42..tofixed(3) 则没有问题，因为第一个 . 被视为 number 的一部分，第二个 . 是属性访问运算符。只是这样看着奇怪，实际情况中也很少见。在基本类型值上直接调用的方法并不多见，不过这并不代表不好或不对。

> 一些工具库扩展了 Number.prototype 的内置方法（参见第 3 章）以提供更多的数值操作，比如用 10..makeItRain() 方法来实现十秒钟金钱雨动画等效果。

下面的语法也是有效的（请注意其中的空格）：

```js
42 .toFixed(3); // "42.000"
```
然而对数字常量而言，这样的语法很容易引起误会，不建议使用。

我们还可以 **用指数形式来表示较大的数字** ，如：

```js
var onethousand = 1E3; // 即 1 * 10^3
var onemilliononehundredthousand = 1.1E6; // 即 1.1 * 10^6
```
**数字常量还可以用其他格式来表示** ，如二进制、八进制和十六进制。

当前的 JavaScript 版本都支持这些格式：

```
0xf3; // 243的十六进制
0Xf3; // 同上

0363; // 243的八进制
```
> 从 ES6 开始，严格模式（strict mode）不再支持 0363 八进制格式（新格式如下）。0363 格式在非严格模式（non-strict mode）中仍然受支持，但是考虑到将来的兼容性，最好不要再使用（我们现在使用的应该是严格模式）。

ES6 支持以下新格式：

```
0o363; // 243的八进制
0O363; // 同上

0b11110011; // 243的二进制
0B11110011; // 同上
```
考虑到代码的易读性，不推荐使用 0O363 格式，因为 0 和大写字母 O 在一起容易混淆。**建议尽量使用小写的 0x、0b 和 0o** 。

##### 较小的数值

二进制浮点数最大的问题（不仅 JavaScript，所有遵循 IEEE 754 规范的语言都是如此），是会出现如下情况：

```js
0.1 + 0.2 === 0.3; // false
```
从数学角度来说，上面的条件判断应该为 true，可结果为什么是 false 呢？

简单来说，**二进制浮点数中的 0.1 和 0.2 并不是十分精确** ，它们相加的结果并非刚好等于 0.3，而是 **一个比较接近的数字 0.30000000000000004** ，所以条件判断结果为 false。

>有人认为，JavaScript 应该采用一种可以精确呈现数字的实现方式。一直以来出现过很多替代方案，只是都没能成为标准，以后大概也不会。这个问题看似简单，实则不然，否则早就解决了。

问题是，如果一些数字无法做到完全精确，是否意味着数字类型毫无用处呢？答案当然是否定的。

在处理带有小数的数字时需要特别注意。很多（也许是绝大多数）程序只需要处理整数，最大不超过百万或者万亿，此时使用 JavaScript 的数字类型是绝对安全的。

那么 **应该怎样来判断 0.1 + 0.2 和 0.3 是否相等呢？**

最常见的方法是 **设置一个误差范围值** ，通常称为“**机器精度**”（machine epsilon），对 JavaScript 的数字来说，这个值通常是 **2^-52 (2.220446049250313e-16)** 。

从 ES6 开始，该值定义在 **Number.EPSILON** 中，我们可以直接拿来用，也可以为 **ES6 之前的版本写 polyfill**：

```js
if (!Number.EPSILON) {
  Number.EPSILON = Math.pow(2,-52);
}
```
可以使用 Number.EPSILON 来比较两个数字是否相等（在指定的误差范围内）：

```js
function numbersCloseEnoughToEqual(n1,n2) {
  return Math.abs( n1 - n2 ) < Number.EPSILON;
}

var a = 0.1 + 0.2;
var b = 0.3;

numbersCloseEnoughToEqual( a, b ); // true
numbersCloseEnoughToEqual( 0.0000001, 0.0000002 ); // false
```
能够呈现的 **最大浮点数大约是 1.798e+308**（这是一个相当大的数字），它定义在 **Number.MAX_VALUE** 中。**最小浮点数** 定义在 **Number.MIN_VALUE** 中，大约是 **5e-324** ，它不是负数，但 **无限接近于 0** ！

##### 整数的安全范围

数字的呈现方式决定了“整数”的安全值范围远远小于 Number.MAX_VALUE。

能够被“安全”呈现的 **最大整数是 2^53 - 1** ，即 **9007199254740991** ，在 ES6 中被定义为 **Number.MAX_SAFE_INTEGER** 。**最小整数是 -9007199254740991** ，在 ES6 中被定义为  **Number.MIN_SAFE_INTEGER** 。

有时 JavaScript 程序需要处理一些比较大的数字，如数据库中的 64 位 ID 等。由于 **JavaScript 的数字类型无法精确呈现 64 位数值** ，所以 **必须将它们保存（转换）为字符串** 。

好在大数值操作并不常见（它们的比较操作可以通过字符串来实现）。如果确实需要对大数值进行数学运算，目前还是需要借助相关的工具库。将来 JavaScript 也许会加入对大数值的支持。

##### 整数检测

要 **检测一个值是否是整数** ，可以使用 ES6 中的 **Number.isInteger(..) 方法** ：

```js
Number.isInteger( 42 ); // true
Number.isInteger( 42.000 ); // true
Number.isInteger( 42.3 ); // false
```
也可以为 ES6 之前的版本 **polyfill Number.isInteger(..) 方法** ：

```js
if (!Number.isInteger) {
  Number.isInteger = function(num) {
    return typeof num == "number" && num % 1 == 0;
  };
}
```
要 **检测一个值是否是安全的整数**，可以使用 ES6 中的 **Number.isSafeInteger(..) 方法** ：

```js
Number.isSafeInteger( Number.MAX_SAFE_INTEGER ); // true
Number.isSafeInteger( Math.pow( 2, 53 ) ); // false
Number.isSafeInteger( Math.pow( 2, 53 ) - 1 ); // true
```
可以为 ES6 之前的版本 **polyfill Number.isSafeInteger(..) 方法** ：

```js
if (!Number.isSafeInteger) {
  Number.isSafeInteger = function(num) {
    return Number.isInteger(num) && Math.abs(num) <= Number.MAX_SAFE_INTEGER;
  };
}
```
##### 32 位有符号整数

虽然整数最大能够达到 53 位，但是有些数字操作（如数位操作）只适用于 32 位数字，所以这些操作中数字的安全范围就要小很多，变成从 Math.pow(-2,31)（-2147483648，约－21 亿）到 Math.pow(2,31) - 1（2147483647，约 21 亿）。

a | 0 可以将变量 a 中的数值转换为 32 位有符号整数，因为数位运算符 | 只适用于 32 位整数（它只关心 32 位以内的值，其他的数位将被忽略）。因此与 0 进行操作即可截取 a 中的 32 位数位。

> 某些特殊的值并不是 32 位安全范围的，如 NaN 和 Infinity（下节将作相关介绍），此时会对它们执行虚拟操作（abstract operation）ToInt32（参见第 4 章），以便转换为符合数位运算符要求的 +0 值。

#### 特殊数值

JavaScript 数据类型中有几个特殊的值需要开发人员特别注意和小心使用。

##### 不是值的值

undefined 类型只有一个值，即 undefined。null 类型也只有一个值，即 null。它们的名称既是类型也是值。

undefined 和 null 常被用来表示“空的”值或“不是值”的值。二者之间有一些细微的差别。例如：

- null 指空值（empty value）
- undefined 指没有值（missing value）

或者：

- undefined 指从未赋值
- null 指曾赋过值，但是目前没有值

**null** 是一个 **特殊关键字** ，**不是标识符** ，我们 **不能将其当作变量来使用和赋值**  。然而 **undefined 却是一个标识符，可以被当作变量来使用和赋值** 。

##### undefined

在非严格模式下，我们可以为全局标识符 undefined 赋值（这样的设计实在是欠考虑！）：

```js
function foo() {
  undefined = 2; // 非常糟糕的做法！
}

foo();

function foo() {
  "use strict";
  undefined = 2; // TypeError!
}

foo();
```

在非严格和严格两种模式下，我们可以声明一个名为 undefined 的局部变量。再次强调最好不要这样做！

```js
function foo() {
  "use strict";
  var undefined = 2;
  console.log(undefined); // 2
}
foo();
```
**永远不要重新定义 undefined。**

##### void 运算符

undefined 是一个内置标识符（除非被重新定义，见前面的介绍），它的值为 undefined，通过 void 运算符即可得到该值。

表达式 void ___ 没有返回值，因此返回结果是 undefined。void 并不改变表达式的结果，只是让表达式不返回值：

```js
var a = 42;
console.log( void a, a ); // undefined 42
```
按惯例我们用 void 0 来获得 undefined（这主要源自 C 语言，当然使用 void true 或其他 void 表达式也是可以的）。void 0、void 1 和 undefined 之间并没有实质上的区别。

void 运算符在其他地方也能派上用场，比如不让表达式返回任何结果（即使其有副作用）。例如：

```js
function doSomething() {
  // 注： APP.ready 由程序自己定义
  if (!APP.ready) {
    // 稍后再试
    return void setTimeout(doSomething, 100);
  }
  
  var result;
  
  // 其他
  return result;
}

// 现在可以了吗？
if (doSomething()) {
  // 立即执行下一个任务
}
```
这里 setTimeout(..) 函数返回一个数值（计时器间隔的唯一标识符，用来取消计时），但是为了确保 if 语句不产生误报（false positive），我们要 void 掉它。很多开发人员喜欢分开操作，效果都一样，只是没有使用 void 运算符：

```js
if (!APP.ready) {
  // 稍后再试
  setTimeout( doSomething,100 );
  return;
}
```
总之，如果要 **将代码中的值（如表达式的返回值）设为 undefinedv ，就可以 **使用 void** 。这种做法并不多见，但在某些情况下却很有用。

##### 特殊的数字

数字类型中有几个特殊的值，下面将详细介绍。

###### 不是数字的数字

如果数学运算的操作数不是数字类型（或者无法解析为常规的十进制或十六进制数字），就无法返回一个有效的数字，这种情况下返回值为 NaN。

NaN 意指“不是一个数字”（not a number），这个名字容易引起误会，后面将会提到。将它理解为“无效数值”“失败数值”或者“坏数值”可能更准确些。

例如：

```js
var a = 2 / "foo"; // NaN
typeof a === "number"; // true
```
换句话说，“不是数字的数字”仍然是数字类型。这种说法可能有点绕。

**NaN** 是一个“**警戒值**”（sentinel value，有特殊用途的常规值），用于 **指出数字类型中的错误情况** ，即“ **执行数学运算没有成功，这是失败后返回的结果** ”。

有人也许认为如果要检查变量的值是否为 NaN，可以直接和 NaN 进行比较，就像比较 null和 undefined 那样。实则不然。

```js
var a = 2 / "foo";
a == NaN; // false
a === NaN; // false
```
NaN 是一个特殊值，它和自身不相等，是 **唯一一个非自反**（自反，reflexive，即 x === x 不成立）的值。而 NaN != NaN 为 true，很奇怪吧？

既然我们无法对 NaN 进行比较（结果永远为 false），那应该怎样来判断它呢？

```js
var a = 2 / "foo";
isNaN( a ); // true
```
很简单，可以使用内建的全局工具函数 isNaN(..) 来判断一个值是否是 NaN。

然而操作起来并非这么容易。isNaN(..) 有一个严重的缺陷，它的检查方式过于死板，就
是“**检查参数是否不是 NaN，也不是数字** ”。但是 **这样做的结果并不太准确** ：

```js
var a = 2 / "foo";
var b = "foo";

a; // NaN
b; "foo"

window.isNaN( a ); // true
window.isNaN( b ); // true——晕！
```
很明显 "foo" 不是一个数字，但是它也不是 NaN。这个 bug 自 JavaScript 问世以来就一直存在，至今已超过 19 年。

从 ES6 开始我们可以使用工具函数 **Number.isNaN(..)** 。**ES6 之前的浏览器的 polyfill** 如下：

```js
if (!Number.isNaN) {
  Number.isNaN = function(n) {
    return (typeof n === "number" && window.isNaN(n));
  };
}

var a = 2 / "foo";
var b = "foo";

Number.isNaN(a); // true
Number.isNaN(b); // false——好！

```
实际上还有一个更简单的方法，即利用 NaN 不等于自身这个特点。**NaN 是 JavaScript 中唯一一个不等于自身的值** 。

于是我们可以这样：

```js
if (!Number.isNaN) {
  Number.isNaN = function(n) {
    return n !== n;
  };
}
```
很多 JavaScript 程序都可能存在 NaN 方面的问题，所以我们应该尽量使用 Number.isNaN(..) 这样可靠的方法，无论是系统内置还是 polyfill。

如果你仍在代码中使用 isNaN(..)，那么你的程序迟早会出现 bug。

###### 无穷数

熟悉传统编译型语言（如 C）的开发人员可能都遇到过编译错误（compiler error）或者运行时错误（runtime exception），例如“除以 0”：

```js
var a = 1 / 0;
```
然而在 JavaScript 中上例的结果为 Infinity（即 Number.POSITIVE_INfiNITY）。同样：

```js
var a = 1 / 0; // Infinity
var b = -1 / 0; // -Infinity
```
如果除法运算中的一个操作数为负数， 则结果为 -Infinity（ 即Number.NEGATIVE_INfiNITY）。

JavaScript 使用有限数字表示法（finite numeric representation，即之前介绍过的 IEEE 754 浮点数），所以和纯粹的数学运算不同，**JavaScript 的运算结果有可能溢出** ，**此时结果为 Infinity 或者 -Infinity** 。

例如：

```js
var a = Number.MAX_VALUE; // 1.7976931348623157e+308
a + a; // Infinity
a + Math.pow( 2, 970 ); // Infinity
a + Math.pow( 2, 969 ); // 1.7976931348623157e+308
```
规范规定，如果数学运算（如加法）的结果超出处理范围，则由 IEEE 754 规范中的“就近取整”（round-to-nearest）模式来决定最后的结果。例如，相对于 Infinity，Number.MAX_VALUE + Math.pow(2, 969) 与 Number.MAX_VALUE 更为接近，因此它被“向下取整”（round down）；而 Number.MAX_VALUE + Math.pow(2, 970) 与 Infinity 更为接近，所以它被“向上取整”（round up）。

这个问题想多了容易头疼，还是就此打住吧。

计算结果一旦溢出为无穷数（infinity）就无法再得到有穷数。换句话说，就是你可以从有穷走向无穷，但无法从无穷回到有穷。

有人也许会问：“那么无穷除以无穷会得到什么结果呢？”我们的第一反应可能会是“1”或者“无穷”，可惜都不是。因为从数学运算和 JavaScript 语言的角度来说，Infinity/Infinity 是一个未定义操作，结果为 NaN。

那么有穷正数除以 Infinity 呢？很简单，结果是 0。有穷负数除以 Infinity 呢？这里留个悬念，后面将作介绍。

###### 零值

这部分内容对于习惯数学思维的读者可能会带来困惑，JavaScript 有一个常规的 0（也叫作+0）和一个 -0。在解释为什么会有 -0 之前，我们先来看看 JavaScript 是如何来处理它的。

-0 除了可以用作常量以外，也可以是某些数学运算的返回值。例如：

```js
var a = 0 / -3; // -0
var b = 0 * -3; // -0
```
加法和减法运算不会得到负零（negative zero）。

负零在开发调试控制台中通常显示为 -0，但在一些老版本的浏览器中仍然会显示为 0。
根据规范，对负零进行字符串化会返回 "0"：

```js
var a = 0 / -3;

// 至少在某些浏览器的控制台中显示是正确的
a; // -0

// 但是规范定义的返回结果是这样！
a.toString(); // "0"
a + ""; // "0"
String( a ); // "0"

// JSON也如此，很奇怪
JSON.stringify( a ); // "0"
````
有意思的是，如果反过来将其从字符串转换为数字，得到的结果是准确的：

```js
+"-0"; // -0
Number( "-0" ); // -0
JSON.parse( "-0" ); // -0
```
> JSON.stringify(-0) 返回 "0"，而 JSON.parse("-0") 返回 -0。

负零转换为字符串的结果令人费解，它的比较操作也是如此：

```js
var a = 0;
var b = 0 / -3;

a == b; // true
-0 == 0; // true

a === b; // true
-0 === 0; // true

0 > -0; // false
a > b; // false
```
要区分 -0 和 0，不能仅仅依赖开发调试窗口的显示结果，还需要做一些特殊处理：

```js
function isNegZero(n) {
  n = Number( n );
  return (n === 0) && (1 / n === -Infinity);
}

isNegZero( -0 ); // true
isNegZero( 0 / -3 ); // true
isNegZero( 0 ); // false
```
抛开学术上的繁枝褥节不论，我们为什么需要负零呢？

有些应用程序中的数据需要以级数形式来表示（比如动画帧的移动速度），数字的符号位（sign）用来代表其他信息（比如移动的方向）。**此时如果一个值为 0 的变量失去了它的符号位，它的方向信息就会丢失** 。所以 **保留 0 值的符号位可以防止这类情况发生** 。

##### 特殊等式

如前所述，NaN 和 -0 在相等比较时的表现有些特别。由于 NaN 和自身不相等，所以必须使用 ES6 中的 Number.isNaN(..)（或者 polyfill）。而 -0 等于 0（对于 === 也是如此，参见第4 章），因此我们必须使用 isNegZero(..) 这样的工具函数。

ES6 中新加入了一个工具方法 **Object.is(..) 来判断两个值是否绝对相等** ，可以用来处理上述所有的特殊情况：

```js
var a = 2 / "foo";
var b = -3 * 0;

Object.is( a, NaN ); // true
Object.is( b, -0 ); // true

Object.is( b, 0 ); // false
```
对于 ES6 之前的版本，Object.is(..) 有一个简单的 polyfill：

```js
if (!Object.is) {
  Object.is = function(v1, v2) {
    // 判断是否是-0
    if (v1 === 0 && v2 === 0) {
      return 1 / v1 === 1 / v2;
    }
    // 判断是否是NaN
    if (v1 !== v1) {
      return v2 !== v2;
    }
    // 其他情况
    return v1 === v2;
  };
}
```
**能使用 == 和 ===（参见第 4 章）时就尽量不要使用 Object.is(..)** ，因为 **前者效率更高、更为通用** 。Object.is(..) 主要用来处理那些特殊的相等比较。

#### 值和引用

在许多编程语言中，赋值和参数传递可以通过 **值复制**（value-copy）或者 **引用复制**（reference-copy）来完成，这取决于我们使用什么语法。

例如，在 C++ 中如果要向函数传递一个数字并在函数中更改它的值，就可以这样来声明参数 int& myNum，即如果传递的变量是 x，myNum 就是指向 x 的引用。引用就像一种特殊的指针，是来指向变量的指针（别名）。如果参数不声明为引用的话，参数值总是通过值复制的方式传递，即便对复杂的对象值也是如此。

JavaScript 中没有指针，引用的工作机制也不尽相同。在 JavaScript 中变量不可能成为指向另一个变量的引用。

JavaScript 引用指向的是值。如果一个值有 10 个引用，这些引用指向的都是同一个值，它们相互之间没有引用 / 指向关系。

JavaScript 对值和引用的赋值 / 传递在语法上没有区别，完全根据值的类型来决定。

下面来看一个例子：

```js
var a = 2;
var b = a; // b是a的值的一个副本
b++;
a; // 2
b; // 3

var c = [1,2,3];
var d = c; // d是[1,2,3]的一个引用
d.push( 4 );
c; // [1,2,3,4]
d; // [1,2,3,4]
```
简单值（即标量基本类型值，scalar primitive）总是通过值复制的方式来赋值 / 传递，包括null、undefined、字符串、数字、布尔和 ES6 中的 symbol。

复合值（compound value）——对象（包括数组和封装对象，参见第 3 章）和函数，则总是通过引用复制的方式来赋值 / 传递。

上例中 2 是一个标量基本类型值，所以变量 a 持有该值的一个复本，b 持有它的另一个复本。b 更改时，a 的值保持不变。

c 和 d 则分别指向同一个复合值 [1,2,3] 的两个不同引用。请注意，c 和 d 仅仅是指向值[1,2,3]，并非持有。所以它们更改的是同一个值（如调用 .push(4)），随后它们都指向更改后的新值 [1,2,3,4]。

由于引用指向的是值本身而非变量，所以一个引用无法更改另一个引用的指向。

```js
var a = [1,2,3];
var b = a;
a; // [1,2,3]
b; // [1,2,3]

// 然后
b = [4,5,6];
a; // [1,2,3]
b; // [4,5,6]
```
b=[4,5,6] 并不影响 a 指向值 [1,2,3]，除非 b 不是指向数组的引用，而是指向 a 的指针，但在 JavaScript 中不存在这种情况！

函数参数就经常让人产生这样的困惑：

```js
function foo(x) {
 x.push( 4 );
 x; // [1,2,3,4]
 
 // 然后
 x = [4,5,6];
 x.push( 7 );
 x; // [4,5,6,7]
}

var a = [1,2,3];
foo( a );
a; // 是[1,2,3,4]，不是[4,5,6,7]
```
我们向函数传递 a 的时候，实际是将引用 a 的一个复本赋值给 x，而 a 仍然指向 [1,2,3]。在函数中我们可以通过引用 x 来更改数组的值（push(4) 之后变为 [1,2,3,4]）。但 x = [4,5,6] 并不影响 a 的指向，所以 a 仍然指向 [1,2,3,4]。

我们不能通过引用 x 来更改引用 a 的指向，只能更改 a 和 x 共同指向的值。

如果要将 a 的值变为 [4,5,6,7]，必须更改 x 指向的数组，而不是为 x 赋值一个新的数组。

```js
function foo(x) {
  x.push(4);
  x; // [1,2,3,4]
  // 然后
  x.length = 0; // 清空数组
  x.push(4, 5, 6, 7);
  x; // [4,5,6,7]
}

var a = [1,2,3];

foo( a );

a; // 是[4,5,6,7]，不是[1,2,3,4]
```
从上例可以看出，x.length = 0 和 x.push(4,5,6,7) 并没有创建一个新的数组，而是更改了当前的数组。于是 a 指向的值变成了 [4,5,6,7]。

请记住：我们无法自行决定使用值复制还是引用复制，一切由值的类型来决定。

如果通过值复制的方式来传递复合值（如数组），就需要为其创建一个复本，这样传递的就不再是原始值。例如：

```js
foo( a.slice() );
```
slice(..) 不带参数会返回当前数组的一个浅复本（shallow copy）。由于传递给函数的是指向该复本的引用，所以 foo(..) 中的操作不会影响 a 指向的数组。

相反，如果要将标量基本类型值传递到函数内并进行更改，就需要将该值封装到一个复合值（对象、数组等）中，然后通过引用复制的方式传递。

```js
function foo(wrapper) {
 wrapper.a = 42;
}

var obj = {
 a: 2
};

foo( obj );
obj.a; // 42
```
这里 obj 是一个封装了标量基本类型值 a 的封装对象。obj 引用的一个复本作为参数 wrapper 被传递到 foo(..) 中。这样我们就可以通过 wrapper 来访问该对象并更改它的属性。函数执行结束后 obj.a 将变成 42。


这样看来，如果需要传递指向标量基本类型值（比如 2）的引用，就可以将其封装到对应的数字封装对象中（参见第 3 章）。

与预期不同的是，虽然传递的是指向数字对象的引用复本，但我们并不能通过它来更改其中的基本类型值：

```js
function foo(x) {
  x = x + 1;
  x; // 3
}

var a = 2;
var b = new Number(a); // Object(a)也一样

foo(b);
console.log(b); // 是2，不是3

```
原因是标量基本类型值是不可更改的（字符串和布尔也是如此）。如果一个数字对象的标量基本类型值是 2，那么该值就不能更改，除非创建一个包含新值的数字对象。

x = x + 1 中，x 中的标量基本类型值 2 从数字对象中拆封（或者提取）出来后，x 就神不知鬼不觉地从引用变成了数字对象，它的值为 2 + 1 等于 3。然而函数外的 b 仍然指向原来那个值为 2 的数字对象。

我们还可以为数字对象添加属性（只要不更改其内部的基本类型值即可），通过它们间接地进行数据交换。

不过这种做法不太常见，大多数开发人员可能都觉得这不是一个好办法。

相对而言，前面用 obj 作为封装对象的办法可能更好一些。这并不是说数字等封装对象没有什么用，只是多数情况下我们应该优先考虑使用标量基本类型。

引用的功能很强大，但有时也难免成为阻碍。赋值 / 参数传递是通过引用还是值复制完全由值的类型来决定，所以使用哪种类型也间接决定了赋值 / 参数传递的方式。

#### 小结

JavaScript 中的数组是通过数字索引的一组任意类型的值。字符串和数组类似，但是它们的行为特征不同，在将字符作为数组来处理时需要特别小心。JavaScript 中的数字包括“整数”和“浮点型”。

基本类型中定义了几个特殊的值。

null 类型只有一个值 null，undefined 类型也只有一个值 undefined。所有变量在赋值之前默认值都是 undefined。void 运算符返回 undefined。

数 字 类 型 有 几 个 特 殊 值， 包 括 NaN（ 意 指“not a number”， 更 确 切 地 说是“invalid number”）、+Infinity、-Infinity 和 -0。

简单标量基本类型值（字符串和数字等）通过值复制来赋值 / 传递，而复合值（对象等）通过引用复制来赋值 / 传递。JavaScript 中的引用和其他语言中的引用 / 指针不同，它们不能指向别的变量 / 引用，只能指向值。

### 原生函数

![原生函数](https://graphbed.qiniu.songxingguo.com/deep-javascript-2/%E5%8E%9F%E7%94%9F%E5%87%BD%E6%95%B0.png)

JavaScript 的内建函数（built-in function），也叫原生函数（native function），如 String 和 Number。

常用的原生函数有：

- String()
- Number()
- Boolean()
- Array()
- Object()
- Function()
- RegExp()
- Date()
- Error()
- Symbol()——ES6 中新加入的！

实际上，它们就是内建函数。

熟 悉 Java 语 言 的人会发现，JavaScript 中 的 String() 和 Java 中的字符串构造函数String(..) 非常相似，可以这样来用：

```js
var s = new String( "Hello World!" );
console.log( s.toString() ); // "Hello World!"
```
原生函数可以被当作构造函数来使用，但其构造出来的对象可能会和我们设想的有所出入：

```js
var a = new String( "abc" );

typeof a; // 是"object"，不是"String"

a instanceof String; // true

Object.prototype.toString.call( a ); // "[object String]"
```
通过构造函数（如 new String("abc")）创建出来的是封装了基本类型值（如 "abc"）的封装对象。

请注意：typeof 在这里返回的是对象类型的子类型。

可以这样来查看封装对象：

```js
console.log( a );
```
由于不同浏览器在开发控制台中显示对象的方式不同（对象序列化 , object serialization），所以上面的输出结果也不尽相同。

> 在本书写作期间，Chrome 的最新版本是这样显示的：String {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}，而老版本这样显示：String {0: "a", 1: "b", 2: "c"}。最新版本的 Firefox 这样显示：String ["a","b","c"]；老版本这样显示："abc"，并且可以点击打开对象查看器。这些输出结果随着浏览器的演进不断变化，也带给人们不同的体验。

再次强调，**new String("abc") 创建的是字符串 "abc" 的封装对象** ，而 **非基本类型值 "abc"** 。

#### 内部属性 [[Class]]

所有 typeof 返回值为 "object" 的对象（如数组）都包含一个 **内部属性 [[Class]]**（我们可以把它看作一个内部的分类，而非传统的面向对象意义上的类）。这个属性无法直接访问，一般通过 **Object.prototype.toString(..)** 来查看。例如：

```js
Object.prototype.toString.call( [1,2,3] );
// "[object Array]"

Object.prototype.toString.call( /regex-literal/i );
// "[object RegExp]"
```
上例中，数组的内部 [[Class]] 属性值是 "Array"，正则表达式的值是 "RegExp"。多数情况下，**对象的内部 [[Class]] 属性和创建该对象的内建原生构造函数相对应**（如下），但 **并非总是如此** 。

那么基本类型值呢？下面先来看看 null 和 undefined：

```js
Object.prototype.toString.call( null );
// "[object Null]"

Object.prototype.toString.call( undefined );
// "[object Undefined]"
```
虽然 Null() 和 Undefined() 这样的原生构造函数并不存在，但是内部 [[Class]] 属性值仍然是 "Null" 和 "Undefined"。

其他基本类型值（如字符串、数字和布尔）的情况有所不同，通常称为“包装”（boxing，参见 3.2 节）：

```js
Object.prototype.toString.call( "abc" );
// "[object String]"

Object.prototype.toString.call( 42 );
// "[object Number]"

Object.prototype.toString.call( true );
// "[object Boolean]"
```
上例中基本类型值被各自的封装对象自动包装，所以它们的内部 [[Class]] 属性值分别为
"String"、"Number" 和 "Boolean"。

#### 封装对象包装

封装对象（object wrapper）扮演着十分重要的角色。由于基本类型值没有.length 和 .toString() 这样的属性和方法，需要通过封装对象才能访问，此时 **JavaScript 会自动为基本类型值包装（box 或者 wrap）一个封装对象** ：

```js
var a = "abc";

a.length; // 3
a.toUpperCase(); // "ABC"
```
如果需要经常用到这些字符串属性和方法，比如在 for 循环中使用 i < a.length，那么从一开始就创建一个封装对象也许更为方便，这样 JavaScript 引擎就不用每次都自动创建了。

但实际证明这并不是一个好办法，因为浏览器已经为 .length 这样的常见情况做了性能优化，直接使用封装对象来“提前优化”代码反而会降低执行效率。

一般情况下，我们不需要直接使用封装对象。最好的办法是让 JavaScript 引擎自己决定什么时候应该使用封装对象。换句话说，就是 **应该优先考虑使用 "abc" 和 42 这样的基本类型值，而非 new String("abc") 和 new Number(42)** 。

##### 封装对象释疑

使用封装对象时有些地方需要特别注意。

比如 Boolean：

```js
var a = new Boolean( false );

if (!a) {
  console.log( "Oops" ); // 执行不到这里
}
```
我们为 false 创建了一个封装对象，然而 **该对象是真值**（“truthy”，即总是返回 true，参见第 4 章），所以 **这里使用封装对象得到的结果和使用 false 截然相反** 。

**如果想要自行封装基本类型值，可以使用 Object(..) 函数**（不带 new 关键字）：

```js
var a = "abc";
var b = new String( a );
var c = Object( a );

typeof a; // "string"
typeof b; // "object"
typeof c; // "object"

b instanceof String; // true
c instanceof String; // true

Object.prototype.toString.call( b ); // "[object String]"
Object.prototype.toString.call( c ); // "[object String]"
```
再次强调，一般不推荐直接使用封装对象（如上例中的 b 和 c），但它们偶尔也会派上用场。

#### 拆封

如果想要得到封装对象中的基本类型值，可以使用 **valueOf() 函数** ：

```js
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );

a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true
```
**在需要用到封装对象中的基本类型值的地方会发生隐式拆封** 。具体过程（即强制类型转换）将在第 4 章详细介绍。

```js
var a = new String( "abc" );
var b = a + ""; // b的值为"abc"

typeof a; // "object"
typeof b; // "string"
```
#### 原生函数作为构造函数

关于数组（array）、对象（object）、函数（function）和正则表达式，我们通常喜欢以常量的形式来创建它们。实际上，使用常量和使用构造函数的效果是一样的（创建的值都是通过封装对象来包装）。

如前所述，应该尽量避免使用构造函数，除非十分必要，因为它们经常会产生意想不到的结果。

##### Array(..)

```js
var a = new Array( 1, 2, 3 );
a; // [1, 2, 3]

var b = [1, 2, 3];
b; // [1, 2, 3]
```
> 构造函数 Array(..) 不要求必须带 new 关键字。不带时，它会被自动补上。

因此 Array(1,2,3) 和 new Array(1,2,3) 的效果是一样的。

Array 构造函数 **只带一个数字参数** 的时候，**该参数会被作为数组的预设长度**（length），而非只充当数组中的一个元素。

这实非明智之举：一是容易忘记，二是容易出错。

更为关键的是，**数组并没有预设长度这个概念** 。这样 **创建出来的只是一个空数组** ，只不过它的 length 属性被设置成了指定的值。

如若一个数组没有任何单元，但它的 length 属性中却显示有单元数量，这样奇特的数据结构会导致一些怪异的行为。而这一切都归咎于已被废止的旧特性（类似 arguments 这样的类数组）。

>我们将包含至少一个“空单元”的数组称为“稀疏数组”。

对此，不同浏览器的开发控制台显示的结果也不尽相同，这让问题变得更加复杂。

例如：

```js
var a = new Array( 3 );

a.length; // 3
a;
```
a 在 Chrome 中显示为 [ undefined x 3 ]（目前为止），这意味着它有三个值为 undefined 的单元，但实际上单元并不存在（“空单元”这个叫法也同样不准确）。

从下面代码的结果可以看出它们的差别：

```js
var a = new Array( 3 );
var b = [ undefined, undefined, undefined ];
var c = [];

c.length = 3;
a;
b;
c;
```
>我们可以创建包含空单元的数组，如上例中的 c。只要将 length 属性设置为超过实际单元数的值，就能隐式地制造出空单元。另外还可以通过 delete b[1] 在数组 b 中制造出一个空单元。


b 在当前版本的 Chrome 中显示为 [ undefined, undefined, undefined ]，而 a 和 c 则显示为 [ undefined x 3 ]。是不是感到很困惑？

更令人费解的是在当前版本的 Firefox 中 a 和 c 显示为 [ , , , ]。仔细看来，这其中有三个逗号，代表四个空单元，而不是三个。

Firefox 在输出结果后面多添了一个 ,，原因是从 ES5 规范开始就允许在列表（数组值、属性列表等）末尾多加一个逗号（在实际处理中会被忽略不计）。所以如果你在代码或者调试控制台中输入 [ , , , ]，实际得到的是 [ , , ]（包含三个空单元的数组）。这样做虽然在控制台中看似令人费解，实则是为了让复制粘贴结果更为准确。

> 针对这种情况，Firefox 将 [ , , , ] 改为显示 Array [<3 empty slots>]，这无疑是个很大的提升。

更糟糕的是，上例中 a 和 b 的行为有时相同，有时又大相径庭：

```js
a.join( "-" ); // "--"
b.join( "-" ); // "--"

a.map(function(v,i){ return i; }); // [ undefined x 3 ]
b.map(function(v,i){ return i; }); // [ 0, 1, 2 ]
```
a.map(..) 之所以执行失败，是因为数组中并不存在任何单元，所以 **map(..) 无从遍历** 。而join(..) 却不一样，它的具体实现可参考下面的代码：

```js
function fakeJoin(arr, connector) {
  var str = "";
  for (var i = 0; i < arr.length; i++) {
    if (i > 0) {
      str += connector;
    }
    if (arr[i] !== undefined) {
      str += arr[i];
    }
  }
  return str;
}

var a = new Array(3);
fakeJoin(a, "-"); // "--"

```
从中可以看出，**join(..) 首先假定数组不为空** ，然后 **通过 length 属性值来遍历其中的元素** 。而 **map(..) 并不做这样的假定** ，因此结果也往往在预期之外，并可能导致失败。

我们可以通过下述方式来 **创建包含 undefined 单元（而非“空单元”）的数组** ：

```js
var a = Array.apply( null, { length: 3 } );
a; // [ undefined, undefined, undefined ]
```
上述代码或许会引起困惑，下面大致解释一下。

apply(..) 是一个工具函数，适用于所有函数对象，它会以一种特殊的方式来调用传递给它的函数。

第一个参数是 this 对象（《你不知道的 JavaScript（上卷）》的“this 和对象原型”部分中有相关介绍），这里不用太过费心，暂将它设为 null。第二个参数则必须是一个数组（或者类似数组的值，也叫作类数组对象，array-like object），其中的值被用作函数的参数。

于是 Array.apply(..) 调用 Array(..) 函数，并且将 { length: 3 } 作为函数的参数。

我们可以设想 apply(..) 内部有一个 for 循环（与上述 join(..) 类似），从 0 开始循环到length（即循环到 2，不包括 3）。

假设在 apply(..) 内部该数组参数名为 arr，for 循环就会这样来遍历数组：arr[0]、arr[1]、arr[2]。 然 而， 由 于 { length: 3 } 中 并 不 存 在 这 些 属 性， 所 以 返 回 值 为undefined。

换句话说，我们执行的实际上是 **Array(undefined, undefined, undefined)** ，所以 **结果是单元值为 undefined 的数组** ，而 **非空单元数组** 。

虽然 Array.apply( null, { length: 3 } ) 在创建 undefined 值的数组时有些奇怪和繁琐，但是其结果远比 Array(3) 更准确可靠。

总之，**永远不要创建和使用空单元数组** 。


##### Object(..)、Function(..) 和 RegExp(..)

同样，除非万不得已，否则尽量不要使用 Object(..)/Function(..)/RegExp(..)：

```js
var c = new Object();
c.foo = "bar";
c; // { foo: "bar" }

var d = { foo: "bar" };
d; // { foo: "bar" }

var e = new Function( "a", "return a * 2;" );
var f = function(a) { return a * 2; }
function g(a) { return a * 2; }

var h = new RegExp( "^a*b+", "g" );
var i = /^a*b+/g;
```
在实际情况中 **没有必要使用 new Object() 来创建对象** ，因为 **这样就无法像常量形式那样一次设定多个属性，而必须逐一设定** 。

构造函数 **Function** 只在极少数情况下很有用，比如 **动态定义函数参数和函数体** 的时候。不要把 Function(..) 当作 eval(..) 的替代品，你基本上不会通过这种方式来定义函数。

强烈建议 **使用常量形式（如 /^a*b+/g）来定义正则表达式** ，这样不仅语法简单，执行效率也更高，因为 JavaScript 引擎在代码执行前会对它们进行预编译和缓存。与前面的构造函数不同，**RegExp(..)** 有时还是很有用的，比如 **动态定义正则表达式** 时：

```js
var name = "Kyle";
var namePattern = new RegExp( "\\b(?:" + name + ")+\\b", "ig" );

var matches = someText.match( namePattern );
```
上述情况在 JavaScript 编程中时有发生，这时 new RegExp("pattern","flags") 就能派上用场。

##### Date(..) 和 Error(..)

相较于其他原生构造函数，Date(..) 和 Error(..) 的用处要大很多，因为没有对应的常量形式来作为它们的替代。

创建日期对象必须使用 new Date()。Date(..) 可以带参数，用来指定日期和时间，而不带参数的话则使用当前的日期和时间。

Date(..) 主要用来获得当前的 Unix 时间戳（从 1970 年 1 月 1 日开始计算，以秒为单位）。

该值可以通过日期对象中的 getTime() 来获得。

从 ES5 开始引入了一个更简单的方法，即 **静态函数 Date.now()** 。对 **ES5 之前的版本我们可以使用下面的 polyfill** ：

```js
if (!Date.now) {
  Date.now = function(){
    return (new Date()).getTime();
  };
}
```
>如果调用 Date() 时不带 new 关键字，则会得到当前日期的字符串值。其具体格式规范没有规定，浏览器使用 "Fri Jul 18 2014 00:31:02 GMT-0500 (CDT)"这样的格式来显示。

构造函数 Error(..)（与前面的 Array() 类似）带不带 new 关键字都可。

创建错误对象（error object）主要是为了获得当前运行栈的上下文（大部分 JavaScript 引擎通过只读属性 .stack 来访问）。栈上下文信息包括函数调用栈信息和产生错误的代码行号，以便于调试（debug）。

错误对象通常与 throw 一起使用：

```js
function foo(x) {
  if (!x) {
    throw new Error("x wasn’t provided");
  }
  // ..
}
```
通常错误对象至少包含一个 message 属性，有时也不乏其他属性（必须作为只读属性访问），如 type。除了访问 stack 属性以外，最好的办法是调用（显式调用或者通过强制类型转换隐式调用，参见第 4 章）toString() 来获得经过格式化的便于阅读的错误信息。

> 除 Error(..) 之外，还有一些针对特定错误类型的原生构造函数，如 EvalError(..)、 RangeError(..)、ReferenceError(..)、SyntaxError(..)、TypeError(..) 和 URIError(..)。这些构造函数很少被直接使用，它们在程序发生异常（比如试图使用未声明的变量产生 ReferenceError 错误）时会被自动调用。

##### Symbol(..)

ES6 中新加入了一个基本数据类型 ——符号（Symbol）。符号是具有唯一性的特殊值（并非绝对），用它来命名对象属性不容易导致重名。该类型的引入主要源于 ES6 的一些特殊构造，此外符号也可以自行定义。

符号可以用作属性名，但无论是在代码还是开发控制台中都无法查看和访问它的值，只会显示为诸如 Symbol(Symbol.create) 这样的值。

ES6 中有一些预定义符号，以 Symbol 的静态属性形式出现，如 Symbol.create、Symbol.iterator 等，可以这样来使用：

```js
obj[Symbol.iterator] = function(){ /*..*/ };
```
我们可以使用 Symbol(..) 原生构造函数来自定义符号。但它比较特殊，不能带 new 关键字，否则会出错：

```js
var mysym = Symbol( "my own symbol" );
mysym; // Symbol(my own symbol)
mysym.toString(); // "Symbol(my own symbol)"
typeof mysym; // "symbol"

var a = { };
a[mysym] = "foobar";

Object.getOwnPropertySymbols( a );
// [ Symbol(my own symbol) ]
```
虽然符号实际上并非私有属性（通过 Object.getOwnPropertySymbols(..) 便可以公开获得对象中的所有符号），但它却主要用于私有或特殊属性。很多开发人员喜欢用它来替代有下划线（_）前缀的属性，而下划线前缀通常用于命名私有或特殊属性。

> 符号并非对象，而是 **一种简单标量基本类型** 。

##### 原生原型

原生构造函数有自己的 .prototype 对象，如 Array.prototype、String.prototype 等。**这些对象包含其对应子类型所特有的行为特征** 。

例如，**将字符串值封装为字符串对象之后，就能访问 String.prototype 中定义的方法**。根据文档 约定，我们将 String.prototype.XYZ 简写为 String#XYZ，对其他 .prototypes 也同样如此。

- String#indexOf(..)
  在字符串中找到指定子字符串的位置。

- String#charAt(..)
  获得字符串指定位置上的字符。

- String#substr(..)、String#substring(..) 和 String#slice(..)
  获得字符串的指定部分。

- String#toUpperCase() 和 String#toLowerCase()
  将字符串转换为大写或小写。

- String#trim()
  去掉字符串前后的空格，返回新的字符串。

以上方法并不改变原字符串的值，而是返回一个新字符串。

借助原型代理（prototype delegation，参见《你不知道的 JavaScript（上卷）》的“this 和对象原型”部分），所有字符串都可以访问这些方法：

```js
var a = " abc ";

a.indexOf( "c" ); // 3
a.toUpperCase(); // " ABC "
a.trim(); // "abc"
```
其他构造函数的原型包含它们各自类型所特有的行为特征，比如 Number#tofixed(..)（将数字转换为指定长度的整数字符串）和 Array#concat(..)（合并数组）。所有的函数都可以调用 Function.prototype 中的 apply(..)、call(..) 和 bind(..)。


然而，有些原生原型（native prototype）并非普通对象那么简单：

```js
typeof Function.prototype; // "function"
Function.prototype(); // 空函数！

RegExp.prototype.toString(); // "/(?:)/"——空正则表达式
"abc".match( RegExp.prototype ); // [""]
```
更糟糕的是，我们甚至可以修改它们（而不仅仅是添加属性）：

```js
Array.isArray( Array.prototype ); // true
Array.prototype.push( 1, 2, 3 ); // 3
Array.prototype; // [1,2,3]

// 需要将Array.prototype设置回空，否则会导致问题！
Array.prototype.length = 0;
```
这里，Function.prototype 是一个函数，RegExp.prototype 是一个正则表达式，而 Array.prototype 是一个数组。是不是很有意思？

###### 将原型作为默认值

**Function.prototype 是一个空函数** ，**RegExp.prototype 是一个“空”的正则表达式**（无任何匹配），而 **Array.prototype 是一个空数组** 。对未赋值的变量来说，它们是很好的 **默认值**。

例如：

```js
function isThisCool(vals,fn,rx) {
  vals = vals || Array.prototype;
  fn = fn || Function.prototype;
  rx = rx || RegExp.prototype;
 
 return rx.test(
  vals.map( fn ).join( "" )
 );
}

isThisCool(); // true

isThisCool(
  ["a","b","c"],
  function(v){ return v.toUpperCase(); },
  /D/
); // false
```
>从 ES6 开始，我们不再需要使用 vals = vals || .. 这样的方式来设置默认值（参见第 4 章），因为默认值可以通过函数声明中的内置语法来设置（参见第 5 章）。

这种方法的一个好处是 .prototypes 已被创建并且仅创建一次。相反，如果将 []、function(){} 和 /(?:)/ 作为默认值，则每次调用 isThisCool(..) 时它们都会被创建一次（具体创建与否取决于 JavaScript 引擎，稍后它们可能会被垃圾回收），这样无疑会造成内存和 CPU 资源的浪费。

另外需要注意的一点是，如果默认值随后会被更改，那就不要使用 Array.prototype。上例中的 vals 是作为只读变量来使用，更改 vals 实际上就是更改 Array.prototype，而这样会导致前面提到过的一系列问题！

>以上我们介绍了原生原型及其用途，使用它们时要十分小心，特别是要对它们进行更改时。

#### 小结

JavaScript 为基本数据类型值提供了封装对象，称为原生函数（如 String、Number、Boolean等）。它们为基本数据类型值提供了该子类型所特有的方法和属性（如：String#trim() 和Array#concat(..)）。

对于简单标量基本类型值，比如 "abc"，如果要访问它的 length 属性或 String.prototype方法，JavaScript 引擎会自动对该值进行封装（即用相应类型的封装对象来包装它）来实现对这些属性和方法的访问。

### 强制类型转换

![强制类型转换](https://graphbed.qiniu.songxingguo.com/deep-javascript-2/%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2.png)

#### 值类型转换

**将值从一种类型转换为另一种类型** 通常称为 **类型转换**（type casting），这是 **显式的情况** ；**隐式的情况** 称为 **强制类型转换**（coercion）。

>JavaScript 中的强制类型转换总是返回标量基本类型值（参见第 2 章），如字符串、数字和布尔值，不会返回对象和函数。在第 3 章中，我们介绍过“封装”，就是为标量基本类型值封装一个相应类型的对象，但这并非严格意义上的强制类型转换。

也可以这样来区分：**类型转换发生在静态类型语言的编译阶段** ，而 **强制类型转换则发生在动态类型语言的运行时**（runtime）。

然而在 JavaScript 中通常将它们统称为 **强制类型转换** ，我个人则倾向于用“隐式强制类型转换”（implicit coercion）和“显式强制类型转换”（explicit coercion）来区分。

二者的区别显而易见：我们 **能够从代码中看出哪些地方是显式强制类型转换** ，而 **隐式强制类型转换则不那么明显** ，通常是某些操作产生的副作用。

例如：

```js
var a = 42;
var b = a + ""; // 隐式强制类型转换
var c = String( a ); // 显式强制类型转换
```
对变量 b 而言，强制类型转换是隐式的；由于 + 运算符的其中一个操作数是字符串，所以是字符串拼接操作，结果是数字 42 被强制类型转换为相应的字符串 "42"。

而 String(..) 则是将 a 显式强制类型转换为字符串。

两者都是将数字 42 转换为字符串 "42"。然而它们各自不同的处理方式成为了争论的焦点。

这里的“显式”和“隐式”以及“明显的副作用”和“隐藏的副作用”，都是相对而言的。

要是你明白 a + "" 是怎么回事，它对你来说就是“显式”的。相反，如果你不知道String(..) 可以用来做字符串强制类型转换，它对你来说可能就是“隐式”的。

我们在这里以普遍通行的标准来讨论“显式”和“隐式”，而非 JavaScript 专家和规范的标准。如果你的理解与此有出入，请参照我们的标准。

要知道我们编写的代码大都是给别人看的。即便是 JavaScript 高手也需要顾及其他不同水平的开发人员，要考虑他们是否能读懂自己的代码，以及他们 **对于“显式”和“隐式”的理解是否和自己一致** 。

#### 抽象值操作

介绍显式和隐式强制类型转换之前，我们需要掌握字符串、数字和布尔值之间类型转换的基本规则。ES5 规范第 9 节中定义了一些“抽象操作”（即“仅供内部使用的操作”）和转换规则。这里我们着重介绍 ToString、ToNumber 和 ToBoolean，附带讲一讲 ToPrimitive。

##### ToString

规范的 9.8 节中定义了抽象操作 ToString，它负责处理 **非字符串到字符串的强制类型转换** 。基本类型值的字符串化规则为：**null 转换为 "null"，undefined 转换为 "undefined"，true转换为 "true"** 。数字的字符串化则遵循通用规则，不过第 2 章中讲过的那些极小和极大的数字使用指数形式：

```js
// 1.07 连续乘以七个 1000
var a = 1.07 * 1000 * 1000 * 1000 * 1000 * 1000 * 1000 * 1000;

// 七个1000一共21位数字
a.toString(); // "1.07e21"
```
对普通对象来说，除非自行定义，否则 **toString()（Object.prototype.toString()）返回内部属性 [[Class]] 的值**（参见第 3 章），如 "[object Object]"。

然而前面我们介绍过，如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。

数组的默认 toString() 方法经过了重新定义，将所有单元字符串化以后再用 "," 连接起来：

```js
var a = [1,2,3];
a.toString(); // "1,2,3"
```
**toString() 可以被显式调用，或者在需要字符串化时自动调用。**

###### JSON 字符串化

工具函数 **JSON.stringify(..)** 在将 JSON 对象序列化为字符串时也用到了 **ToString** 。

请注意，**JSON 字符串化并非严格意义上的强制类型转换** ，因为其中也涉及 ToString 的相关规则，所以这里顺带介绍一下。

对大多数简单值来说，JSON 字符串化和 toString() 的效果基本相同，只不过序列化的结果总是字符串：

```js
JSON.stringify( 42 ); // "42"
JSON.stringify( "42" ); // ""42"" （含有双引号的字符串）
JSON.stringify( null ); // "null"
JSON.stringify( true ); // "true"
```
所有安全的 JSON 值（JSON-safe）都可以使用 JSON.stringify(..) 字符串化。安全的 JSON 值是指能够呈现为有效 JSON 格式的值。

为了简单起见，我们来看看什么是不安全的 JSON 值。**undefined、function、symbol（ES6+）和包含循环引用（对象之间相互引用，形成一个无限循环）的对象都不符合 JSON结构标准** ，支持 JSON 的语言无法处理它们。

JSON.stringify(..) 在对象中 **遇到 undefined、function 和 symbol 时会自动将其忽略** ，**在数组中则会返回 null**（以保证单元位置不变）。

例如：

```js
JSON.stringify(undefined); // undefined
JSON.stringify(function() {}); // undefined
JSON.stringify(
  [1, undefined,function() {},4]
); // "[1,null,null,4]"
JSON.stringify({
  a: 2,
  b: function() {}
}); // "{"a":2}"
```
对包含循环引用的对象执行 JSON.stringify(..) 会出错。

如果对象中定义了 **toJSON() 方法** ，JSON 字符串化时会首先调用该方法，然后用它的返回值来进行序列化。

如果要对含有非法 JSON 值的对象做字符串化，或者对象中的某些值无法被序列化时，就 **需要定义 toJSON() 方法来返回一个安全的 JSON 值** 。

例如：

```js
var o = {};

var a = {
  b: 42,
  c: o,
  d: function() {}
};

// 在a中创建一个循环引用
o.e = a;
// 循环引用在这里会产生错误
// JSON.stringify( a );

// 自定义的JSON序列化
a.toJSON = function() {
  // 序列化仅包含b
  return {
    b: this.b
  };
};

JSON.stringify(a); // "{"b":42}"
```
很多人误以为 toJSON() 返回的是 JSON 字符串化后的值，其实不然，除非我们确实想要对字符串进行字符串化（通常不会！）。**toJSON() 返回的应该是一个适当的值，可以是任何类型，然后再由 JSON.stringify(..) 对其进行字符串化** 。

也就是说，toJSON() 应该“返回一个能够被字符串化的安全的 JSON 值”，而不是“返回一个 JSON 字符串”。

例如：

```js
var a = {
  val: [1, 2, 3],
  
  // 可能是我们想要的结果！
  toJSON: function() {
    return this.val.slice(1);
  }
};

var b = {
  val: [1, 2, 3],
  
  // 可能不是我们想要的结果！
  toJSON: function() {
    return "[" + this.val.slice(1).join() + "]";
  }
};

JSON.stringify(a); // "[2,3]"
JSON.stringify(b); // ""[2,3]""
```
这里第二个函数是对 toJSON 返回的字符串做字符串化，而非数组本身。

我们可以向 JSON.stringify(..) 传递一个 **可选参数 replacer** ，它可以是 **数组或者函数** ，**用来指定对象序列化过程中哪些属性应该被处理，哪些应该被排除，和 toJSON() 很像** 。

如果 replacer 是一个数组，那么它必须是一个字符串数组，其中包含序列化要处理的对象的属性名称，除此之外其他的属性则被忽略。

如果 replacer 是一个函数，它会对对象本身调用一次，然后对对象中的每个属性各调用一次，每次传递两个参数，键和值。如果要忽略某个键就返回 undefined，否则返回指定的值。

```js
var a = {
  b: 42,
  c: "42",
  d: [1, 2, 3]
};

JSON.stringify(a, ["b", "c"]); // "{"b":42,"c":"42"}"

JSON.stringify(a, function(k, v) {
  if (k !== "c") return v;
});
// "{"b":42,"d":[1,2,3]}"
```
**如果 replacer 是函数，它的参数 k 在第一次调用时为 undefined（就是对对象本身调用的那次）**。if 语句将属性 "c" 排除掉。由于字符串化是递归的，因此数组 [1,2,3] 中的每个元素都会通过参数 v 传递给 replacer，即 1、2 和 3，参数 k 是它们的索引值，即 0、1 和 2。

JSON.string 还有一个 **可选参数 space** ，**用来指定输出的缩进格式** 。**space 为正整数时是指定每一级缩进的字符数** ，它还可以是 **字符串** ，此时最前面的十个字符被用于每一级的缩进：

```js
var a = {
  b: 42,
  c: "42",
  d: [1, 2, 3]
};

JSON.stringify(a, null, 3);
// "{
// "b": 42,
// "c": "42",
// "d": [
// 1,
// 2,
// 3
// ]
// }"
JSON.stringify(a, null, "-----");
// "{
// -----"b": 42,
// -----"c": "42",
// -----"d": [
// ----------1,
// ----------2,
// ----------3
// -----]
// }"
```
请记住，JSON.stringify(..) 并不是强制类型转换。在这里介绍是因为它涉及 ToString 强制类型转换，具体表现在以下两点。

(1) 字符串、数字、布尔值和 null 的 JSON.stringify(..) 规则与 ToString 基本相同。
(2) 如果传递给 JSON.stringify(..) 的对象中定义了 toJSON() 方法，那么该方法会在字符串化前调用，以便将对象转换为安全的 JSON 值。

##### ToNumber

有时我们需要 **将非数字值当作数字来使用** ，比如数学运算。为此 ES5 规范在 9.3 节定义了抽象操作 ToNumber。

其中 **true 转换为 1，false 转换为 0**  。**undefined 转换为 NaN，null 转换为 0** 。

**ToNumber 对字符串的处理基本遵循数字常量的相关规则 / 语法（参见第 3 章）。处理失败时返回 NaN**（处理数字常量失败时会产生语法错误）。不同之处是 **ToNumber 对以 0 开头的十六进制数并不按十六进制处理**（而是按十进制，参见第 2 章）。

数字常量的语法规则与 ToNumber 处理字符串所遵循的规则之间差别不大，这里不做进一步介绍，可参考 ES5 规范的 9.3.1 节。

对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

为了将值转换为相应的基本类型值，抽象操作 ToPrimitive（参见 ES5 规范 9.1 节）会首先（通过内部操作 DefaultValue，参见 ES5 规范 8.12.8 节）检查该值是否有 **valueOf() 方法** 。

**如果有并且返回基本类型值，就使用该值进行强制类型转换** 。如果没有就使用 **toString()** 的返回值（如果存在）来进行强制类型转换。

**如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。**

从 ES5 开始，使用 Object.create(null) 创建的对象 [[Prototype]] 属性为 null，并且没有 valueOf() 和 toString() 方法，因此无法进行强制类型转换。详情请参考本系列的《你不知道的 JavaScript（上卷）》“this 和对象原型”部分中 [[Prototype]] 相关部分。

我们稍后将详细介绍数字的强制类型转换，在下面的示例代码中我们假定 Number(..) 已经实现了此功能。

例如：

```js
var a = {
  valueOf: function() {
    return "42";
  }
};

var b = {
  toString: function() {
    return "42";
  }
};

var c = [4, 2];
c.toString = function() {
  return this.join(""); // "42"
};

Number(a); // 42
Number(b); // 42
Number(c); // 42
Number(""); // 0
Number([]); // 0
Number(["abc"]); // NaN
```
##### ToBoolean

下面介绍布尔值，关于这个主题存在许多误解和困惑，需要我们特别注意。

首先也是最重要的一点是，JavaScript 中有两个关键词 true 和 false，分别代表布尔类型中的真和假。我们常误以为数值 1 和 0 分别等同于 true 和 false。在有些语言中可能是这样，但在 JavaScript 中布尔值和数字是不一样的。**虽然我们可以将 1 强制类型转换为 true，将 0 强制类型转换为 false，反之亦然，但它们并不是一回事。**

###### 假值（falsy value）

我们再来看看其他值是如何被强制类型转换为布尔值的。

JavaScript 中的值可以分为以下两类：

(1) 可以被强制类型转换为 false 的值
(2) 其他（被强制类型转换为 true 的值）

JavaScript 规范具体定义了一小撮可以被强制类型转换为 false 的值。

ES5 规范 9.2 节中定义了抽象操作 ToBoolean，列举了布尔强制类型转换所有可能出现的结果。

以下这些是假值：


- undefined
- null
- false
- +0、-0 和 NaN
- ""

假值的布尔强制类型转换结果为 false。

从逻辑上说，**假值列表以外的都应该是真值**（truthy）。但 JavaScript 规范对此并没有明确定义，只是给出了一些示例，例如规定所有的对象都是真值，我们可以理解为假值列表以外的值都是真值。

###### 假值对象（falsy object）

这个标题似乎有点自相矛盾。前面讲过规范规定所有的对象都是真值，怎么还会有假值对象呢？

有人可能会以为假值对象就是包装了假值的封装对象（如 ""、0 和 false，参见第 3 章），实际不然。

例如：

```js
var a = new Boolean( false );
var b = new Number( 0 );
var c = new String( "" );
```
它们都是封装了假值的对象（参见第 3 章）。那它们究竟是 true 还是 false 呢？答案很简单：

```js
var d = Boolean( a && b && c );
d; // true
```
d 为 true，说明 a、b、c 都为 true。

请注意，这里 Boolean(..) 对 a && b && c 进行了封装，有人可能会问为什么。我们暂且记下，稍后会作说明。你可以试试不用 Boolean(..) 的话 d = a && b && c 会产生什么结果。

如果假值对象并非封装了假值的对象，那它究竟是什么？

值得注意的是，虽然 JavaScript 代码中会出现假值对象，但它实际上并不属于 JavaScript 语言的范畴。

浏览器在某些特定情况下，在常规 JavaScript 语法基础上自己创建了一些外来（exotic）值，这些就是“假值对象”。

假值对象看起来和普通对象并无二致（都有属性，等等），但将它们强制类型转换为布尔值时结果为 false。

最常见的例子是 **document.all** ，它是一个类数组对象，包含了页面上的所有元素，**由 DOM（而不是 JavaScript 引擎）提供给 JavaScript 程序使用** 。它以前曾是一个真正意义上的对象，布尔强制类型转换结果为 true，不过现在它是一个假值对象。

**document.all 并不是一个标准用法，早就被废止了。**

有人也许会问：“既然这样的话，浏览器能否将它彻底去掉？”这个想法是好的，只不过仍然有很多 JavaScript 程序在使用它。

那为什么它要是假值呢？因为我们经常 **通过将 document.all 强制类型转换为布尔值（比如在 if 语句中）来判断浏览器是否是老版本的 IE** 。IE 自诞生之日起就始终遵循浏览器标准，较其他浏览器更为有力地推动了 Web 的发展。

`if(document.all) { /* it’s IE */ }` 依然存在于许多程序中，也许会一直存在下去，这对
IE 的用户体验来说不是一件好事。

虽然我们无法彻底摆脱 document.all，但为了让新版本更符合标准，IE 并不打算继续支持
if (document.all) { .. }。

“那我们应该怎么办？”

“也许可以修改 JavaScript 的类型机制，将 document.all 作为假值来处理！”

这并不是一个好办法。大多数 JavaScript 开发人员对这个坑了解得不多，不过更糟糕的还是对其置若罔闻的态度。

###### 真值（truthy value）

**真值就是假值列表之外的值。**

例如：

```js
var a = "false";
var b = "0";
var c = "''";

var d = Boolean( a && b && c );

d;
```
这里 d 应该是 true 还是 false 呢？

答案是 true。上例的字符串看似假值，但所有字符串都是真值。不过 "" 除外，因为它是假值列表中唯一的字符串。

再如：

```js
var a = []; // 空数组——是真值还是假值？
var b = {}; // 空对象——是真值还是假值？
var c = function(){}; // 空函数——是真值还是假值？

var d = Boolean( a && b && c );

d;
```
d 依然是 true。还是同样的道理，**[]、{} 和 function(){}** 都不在假值列表中，因此 **它们都是真值** 。

也就是说真值列表可以无限长，无法一一列举，所以我们只能用假值列表作为参考。

你可以花五分钟时间将假值列表写出来贴在显示器上，或者记在脑子里，在需要判断真 / 假值的时候就可以派上用场。

掌握真 / 假值的重点在于理解布尔强制类型转换（显式和隐式），在此基础上我们就能对强制类型转换示例进行深入介绍。

#### 显式强制类型转换

**显式强制类型转换是那些显而易见的类型转换** ，很多类型转换都属于此列。

我们在编码时应尽可能地将类型转换表达清楚，以免给别人留坑。类型转换越清晰，代码可读性越高，更容易理解。

对显式强制类型转换几乎不存在非议，它类似于静态语言中的类型转换，已被广泛接受，不会有什么坑。我们后面会再讨论这个话题。

##### 字符串和数字之间的显式转换

我们从最常见的字符串和数字之间的强制类型转换开始。

字符串和数字之间的转换是通过 **String(..)** 和 **Number(..)** 这两个内建函数（原生构造函数，参见第 3 章）来实现的，请注意 **它们前面没有 new 关键字** ，并 **不创建封装对象** 。

下面是两者之间的显式强制类型转换：

```js
var a = 42;
var b = String( a );

var c = "3.14";
var d = Number( c );

b; // "42"
d; // 3.14
```
String(..) 遵循前面讲过的 ToString 规则，将值转换为字符串基本类型。Number(..) 遵循前面讲过的 ToNumber 规则，将值转换为数字基本类型。

它们和静态语言中的类型转换很像，一目了然，所以我们将它们归为显式强制类型转换。

例如，在 C/C++ 中可以使用 (int)x 或 int(x) 将 x 转换为整数。大部分人倾向于后者，因为它看起来更像函数调用。JavaScript 中的 Number(x) 与此十分类似，至于它是否真是一个函数并不重要。

除了 String(..) 和 Number(..) 以外，还有其他方法可以实现字符串和数字之间的显式转换：

```js
var a = 42;
var b = a.toString();


var c = "3.14";
var d = +c;

b; // "42"
d; // 3.14
```
**a.toString()** 是显式的（“toString”意为“to a string”），不过其中涉及隐式转换。因为toString() 对 42 这样的基本类型值不适用，所以 **JavaScript 引擎会自动为 42 创建一个封装对象**（参见第 3 章），然后 **对该对象调用 toString()** 。这里 **显式转换中含有隐式转换** 。

上例中 +c 是 + 运算符的一元（unary）形式（即只有一个操作数）。+ 运算符显式地将 c 转换为数字，而非数字加法运算（也不是字符串拼接，见下）。

+c 是显式还是隐式，取决于你自己的理解和经验。如果你已然知道 **一元运算符 + 会将操作数显式强制类型转换为数字** ，那它就是显式的。如果不明就里的话，它就是隐式强制类型转换，让你摸不着头脑。

>在 JavaScript 开源社区中，一元运算 + 被普遍认为是显式强制类型转换。

不过这样有时候也容易产生误会。例如：

```js
var c = "3.14";
var d = 5+ +c;

d; // 8.14
```
一元运算符 - 和 + 一样，并且它还会反转数字的符号位。由于 -- 会被当作递减运算符来处理，所以我们不能使用 -- 来撤销反转，而应该像 - -"3.14" 这样，**在中间加一个空格，才能得到正确结果 3.14** 。

运算符的一元和二元形式的组合你也许能够想到很多种情况，下面是一个疯狂的例子：

```js
1 + - + + + - + 1; // 2
```
**尽量不要把一元运算符 +（还有 -）和其他运算符放在一起使用。** 上面的代码可以运行，但非常糟糕。此外 d = +c（还有 d =+ c）也容易和 d += c 搞混，两者天壤之别。

> 一元运算符 + 紧挨着 ++ 和 -- 也很容易引起混淆。例如 a +++b、a + ++b 和 a + + +b。关于 ++，请参见 5.1.2 节。

我们的目的是让代码更清晰、更易懂，而非适得其反。

###### 日期显式转换为数字

一元运算符 + 的另一个常见用途是 **将日期（Date）对象强制类型转换为数字**  ，返回结果为 Unix 时间戳，以微秒为单位（从 1970 年 1 月 1 日 00:00:00 UTC 到当前时间）：

```js
var d = new Date( "Mon, 18 Aug 2014 08:53:06 CDT" );
+d; // 1408369986000
```
我们常用下面的方法来获得当前的时间戳，例如：

```js
var timestamp = +new Date();
```
JavaScript 有一处奇特的语法，即 **构造函数没有参数时可以不用带 ()** 。于是我们可能会碰到 var timestamp = +new Date; 这样的写法。这样能否提高代码可读性还存在争议，因为这仅用于 new fn()，对一般的函数调用 fn() 并不适用。

将日期对象转换为时间戳并非只有强制类型转换这一种方法，或许 **使用更显式的方法会更好一些** ：

```js
var timestamp = new Date().getTime();
// var timestamp = (new Date()).getTime();
// var timestamp = (new Date).getTime();
```
不过最好还是使用 ES5 中新加入的静态方法 **Date.now()** ：

```js
var timestamp = Date.now();
```
为老版本浏览器提供 **Date.now() 的 polyfill** 也很简单：

```js
if (!Date.now) {
  Date.now = function() {
    return +new Date();
  };
}
```
我们 **不建议对日期类型使用强制类型转换** ，**应该使用 Date.now() 来获得当前的时间戳，使用 new Date(..).getTime() 来获得指定时间的时间戳** 。

###### 奇特的 ~ 运算符

一个常被人忽视的地方是 ~ 运算符（即字位操作“非”）相关的强制类型转换，它很让人费解，以至于了解它的开发人员也常常对其敬而远之。秉承本书的一贯宗旨，我们在此深入探讨一下 ~ 有哪些用处。

在 2.3.5 节中，我们讲过字位运算符只适用于 32 位整数，运算符会强制操作数使用 32 位格式。这是通过抽象操作 **ToInt32** 来实现的（ES5 规范 9.5 节）。

ToInt32 首先执行 ToNumber 强制类型转换，比如 "123" 会先被转换为 123，然后再执行ToInt32。

虽然严格说来并非强制类型转换（因为返回值类型并没有发生变化），但 **字位运算符（如 |和 ~）和某些特殊数字一起使用时会产生类似强制类型转换的效果** ，**返回另外一个数字** 。

例如 | 运算符（字位操作“或”）的空操作（no-op）0 | x，它仅执行 ToInt32 转换（第 2 章中介绍过）：

```
0 | -0; // 0
0 | NaN; // 0
0 | Infinity; // 0
0 | -Infinity; // 0
```
以上 **这些特殊数字无法以 32 位格式呈现**（因为它们来自 64 位 IEEE 754 标准，参见第 2章），因此 **ToInt32 返回 0** 。

关于 0 | ___ 是显式还是隐式仍存在争议。从规范的角度来说它无疑是显式的，但如果对字位运算符没有这样深入的理解，它可能就是隐式的。为了前后保持一致，我们这里将其视为显式。

再回到 ~。它首先 **将值强制类型转换为 32 位数字** ，然后 **执行字位操作“非”**（对每一个字位进行反转）。

>这与 ! 很相像，不仅 **将值强制类型转换为布尔值 <，还对其做字位反转**（参见 4.3.3 节）。

字位反转是个很晦涩的主题，JavaScript 开发人员一般很少需要关心到字位级别。

对 ~ 还可以有另外一种诠释，源自早期的计算机科学和离散数学：~ 返回 2 的补码。这样一来问题就清楚多了！

~x 大致等同于 -(x+1)。很奇怪，但相对更容易说明问题：

```
~42; // -(42+1) ==> -43
```
也许你还是没有完全弄明白 ~ 到底是什么玩意？为什么把它放在强制类型转换一章中介绍？稍安勿躁。

在 -(x+1) 中唯一能够得到 0（或者严格说是 -0）的 x 值是 -1。也就是说 **如果 x 为 -1 时，~和一些数字值在一起会返回假值 0，其他情况则返回真值。**

然而这与我们讨论的内容有什么关系呢？

**-1 是一个“哨位值”** ，哨位值是那些在各个类型中（这里是数字）被赋予了特殊含义的值。

在 C 语言中我们用 -1 来代表函数执行失败，用大于等于 0 的值来代表函数执行成功。

JavaScript 中字符串的 indexOf(..) 方法也遵循这一惯例，该方法在字符串中搜索指定的子字符串，如果找到就返回子字符串所在的位置（从 0 开始），否则返回 -1。

indexOf(..) 不仅能够得到子字符串的位置，还可以用来检查字符串中是否包含指定的子字符串，相当于一个条件判断。例如：

```js
var a = "Hello World";

if (a.indexOf( "lo" ) >= 0) { // true
 // 找到匹配！
}

if (a.indexOf( "lo" ) != -1) { // true
 // 找到匹配！
}

if (a.indexOf( "ol" ) < 0) { // true
 // 没有找到匹配！
}

if (a.indexOf( "ol" ) == -1) { // true
 // 没有找到匹配！
}
```
`>=` 0 和 `== -1` 这样的写法不是很好，称为“抽象渗漏”，意思是在代码中暴露了底层的实现细节，这里是指用 -1 作为失败时的返回值，这些细节应该被屏蔽掉。

现在我们终于明白 ~ 有什么用处了！ **~ 和 indexOf() 一起可以将结果强制类型转换**（实际上仅仅是转换）为真 / 假值：

```js
var a = "Hello World";~a.indexOf("lo"); // -4 <-- 真值!

if (~a.indexOf("lo")) { // true
  // 找到匹配！
}

~a.indexOf("ol"); // 0 <-- 假值!
! ~a.indexOf("ol"); // true

if (!~a.indexOf("ol")) { // true
  // 没有找到匹配！
}
```

如果 indexOf(..) 返回 -1，~ 将其转换为假值 0，其他情况一律转换为真值。

>由 -(x+1) 推断 ~-1 的结果应该是 -0，然而实际上结果是 0，因为它是字位操作而非数学运算。

从技术角度来说，if (~a.indexOf(..)) 仍然是对 indexOf(..) 的返回结果进行隐式强制类型转换，0 转换为 false，其他情况转换为 true。但我觉得 ~ 更像显式强制类型转换，前提是我对它有充分的理解。

个人认为 ~ 比 >= 0 和 == -1 更简洁。

######  字位截除

一些开发人员使用 ~~ 来截除数字值的小数部分，以为这和 Math.floor(..) 的效果一样，实际上并非如此。

**~~** 中的 **第一个 ~ 执行 ToInt32 并反转字位** ，然后 **第二个 ~ 再进行一次字位反转** ，即 **将所有字位反转回原值** ，最后 **得到的仍然是 ToInt32 的结果** 。

>~~ 和 !! 很相似，我们将在 4.3.3 节中介绍。

对 ~~ 我们要多加注意。首先它 **只适用于 32 位数字** ，更重要的是 **它对负数的处理与 Math.floor(..) 不同** 。

```js
Math.floor( -49.6 ); // -50
~~-49.6; // -49
```
**~~x 能将值截除为一个 32 位整数，x | 0 也可以，而且看起来还更简洁。**

出于对运算符优先级（详见第 5 章）的考虑，我们可能更倾向于使用 ~~x：

```js
~~1E20 / 10; // 166199296
1E20 | 0 / 10; // 1661992960
(1E20 | 0) / 10; // 166199296
```
我们在使用 ~ 和 ~~ 进行此类转换时需要确保其他人也能够看得懂。

##### 显式解析数字字符串

**解析字符串中的数字**  和 **将字符串强制类型转换为数字** 的返回结果都是 **数字** 。但解析和转换两者之间还是有明显的差别。

例如：

```js
var a = "42";
var b = "42px";

Number( a ); // 42
parseInt( a ); // 42

Number( b ); // NaN
parseInt( b ); // 42
```
**解析允许字符串中含有非数字字符** ，**解析按从左到右的顺序** ，如果 **遇到非数字字符就停止** 。而 **转换不允许出现非数字字符** ，**否则会失败并返回 NaN** 。

解析和转换之间不是相互替代的关系。它们虽然类似，但各有各的用途。如果字符串右边的非数字字符不影响结果，就可以使用解析。而转换要求字符串中所有的字符都是数字，像 "42px" 这样的字符串就不行。

**解析字符串中的浮点数** 可以使用 **parseFloat(..) 函数** 。

不要忘了 **parseInt(..) 针对的是字符串值** 。向 parseInt(..) 传递数字和其他类型的参数是没有用的，比如 true、function(){...} 和 [1,2,3]。

**非字符串参数会首先被强制类型转换为字符串** （参见 4.2.1 节），依赖这样的隐式强制类型转换并非上策，应该避免向 parseInt(..) 传递非字符串参数。

ES5 之前的 parseInt(..) 有一个坑导致了很多 bug。即如果 **没有第二个参数来指定转换的基数**（又称为 radix），**parseInt(..) 会根据字符串的第一个字符来自行决定基数** 。

如果 **第一个字符是 x 或 X** ，则 **转换为十六进制数字** 。如果是 **0**，则 **转换为八进制数字** 。

以 x 和 X 开头的十六进制相对来说还不太容易搞错，而八进制则不然。例如：

```js
var hour = parseInt( selectedHour.value );
var minute = parseInt( selectedMinute.value );

console.log(
  "The time you selected was: " + hour + ":" + minute
);
```
上面的代码看似没有问题，但是 **当小时为 08、分钟为 09 时，结果是 0:0，因为 8 和 9 都不是有效的八进制数** 。

**将第二个参数设置为 10，即可避免这个问题** ：

```js
var hour = parseInt( selectedHour.value, 10 );
var minute = parseInt( selectedMiniute.value, 10 );
```
从 ES5 开始 parseInt(..) 默认转换为十进制数，除非另外指定。如果你的代码需要在 ES5之前的环境运行，请记得将第二个参数设置为 10。

###### 解析非字符串

曾经有人发帖吐槽过 parseInt(..) 的一个坑：

```
parseInt( 1/0, 19 ); // 18
```
很多人想当然地以为（实际上大错特错）“如果第一个参数值为 Infinity，解析结果也应该是 Infinity”，返回 18 也太无厘头了。

尽管这个例子纯属虚构，我们还是来看看 JavaScript 是否真的这样无厘头。

其中第一个错误是向 parseInt(..) 传递非字符串，这完全是在自找麻烦。此时 **JavaScript会将参数强制类型转换为它能够处理的字符串** 。

有人可能会觉得这不合理，parseInt(..) 应该拒绝接受非字符串参数。但如果这样的话，它是否应该抛出一个错误？这是 Java 的做法。一想到 JavaScript 代码中到处是抛出的错误，要在每个地方加上 try..catch，我整个人都不好了。

那是不是应该返回 NaN ？也许吧，但是下面的情况是否应该运行失败？

```js
parseInt( new String( "42") );
```
因为它的参数也是一个非字符串。如果你认为此时应该将 String 封装对象拆封（unbox）为 "42"，那么将 42 先转换为 "42" 再解析回 42 不也合情合理吗？

这种 **半显式、半隐式的强制类型转换很多时候非常有用** 。例如：

```js
var a = {
  num: 21,
  toString: function() {
    return String(this.num * 2);
  }
};

parseInt(a); // 42
```
**parseInt(..) 先将参数强制类型转换为字符串再进行解析** ，这样做没有任何问题。因为传递错误的参数而得到错误的结果，并不能归咎于函数本身。

怎么来处理 Infinity（1/0 的结果）最合理呢？有两个选择："Infinity" 和 "∞"，**JavaScript 选择的是 "Infinity"** 。

**JavaScript 中所有的值都有一个默认的字符串形式** ，这很不错，能够方便我们调试。

再回到基数 19，这显然是个玩笑话，在实际的 JavaScript 代码中不会用到基数 19。它的 **有效数字字符范围是 0-9 和 a-i（区分大小写）**。

parseInt(1/0, 19) 实际上是 parseInt("Infinity", 19)。第一个字符是 "I"，以 19 为基数时值为 18。第二个字符 "n" 不是一个有效的数字字符，解析到此为止，和 "42px" 中的 "p" 一样。

最后的结果是 18，而非 Infinity 或者报错。所以理解其中的工作原理对于我们学习
JavaScript 是非常重要的。

此外还有一些看起来奇怪但实际上解释得通的例子：

```js
parseInt( 0.000008 ); // 0 ("0" 来自于 "0.000008")
parseInt( 0.0000008 ); // 8 ("8" 来自于 "8e-7")
parseInt( false, 16 ); // 250 ("fa" 来自于 "false")
parseInt( parseInt, 16 ); // 15 ("f" 来自于 "function..")
parseInt( "0x10" ); // 16
parseInt( "103", 2 ); // 2
```
其实 parseInt(..) 函数是十分靠谱的，只要使用得当就不会有问题。因为使用不当而导致一些莫名其妙的结果，并不能归咎于 JavaScript 本身。

##### 显式转换为布尔值

现在我们来看看 **从非布尔值强制类型转换为布尔值的情况** 。

与前面的 String(..) 和 Number(..) 一样，**Boolean(..)（不带 new）** 是显式的 ToBoolean 强制类型转换：

```js
var a = "0";
var b = [];
var c = {};

var d = "";
var e = 0;
var f = null;
var g;

Boolean( a ); // true
Boolean( b ); // true
Boolean( c ); // true

Boolean( d ); // false
Boolean( e ); // false
Boolean( f ); // false
Boolean( g ); // false
```
虽然 Boolean(..) 是显式的，但并不常用。

和前面讲过的 + 类似，一元运算符 ! **显式地将值强制类型转换为布尔值** 。但是它 **同时还将真值反转为假值**（或者将假值反转为真值）。所以 **显式强制类型转换为布尔值** 最常用的方法是 **!!**，因为 **第二个 ! 会将结果反转回原值** ：

```js
var a = "0";
var b = [];
var c = {};

var d = "";
var e = 0;
var f = null;
var g;

!!a; // true
!!b; // true
!!c; // true

!!d; // false
!!e; // false
!!f; // false
!!g; // false
```
在  **if(..).. 这样的布尔值上下文** 中，如果 **没有使用 Boolean(..) 和 !** ，就会 **自动隐式地进行 ToBoolean 转换** 。建议使用 Boolean(..) 和 !! 来进行显式转换以便让代码更清晰易读。

显式 ToBoolean 的另外一个用处，是 **在 JSON 序列化过程中将值强制类型转换为 true 或 false** ：

```js
var a = [
1,
function() {
  /*..*/
},
2,
function() {
  /*..*/
}];

JSON.stringify(a); // "[1,null,2,null]"

JSON.stringify(a,function(key, val) {
  if (typeof val == "function") {
    // 函数的ToBoolean强制类型转换
    return !! val;
  } else {
    return val;
  }
});
// "[1,true,2,true]"
```
下面的语法对于熟悉 Java 的人并不陌生：

```js
var a = 42;
var b = a ? true : false;
```
三元运算符 ? : 判断 a 是否为真，如果是则将变量 b 赋值为 true，否则赋值为 false。

表面上这是一个显式的 ToBoolean 强制类型转换，因为返回结果是 true 或者 false。

然而这里涉及隐式强制类型转换，因为 a 要首先 **被强制类型转换为布尔值才能进行条件判断** 。这种情况称为“显式的隐式”，有百害而无一益，我们应彻底杜绝。

**建议使用 Boolean(a) 和 !!a 来进行显式强制类型转换。**

#### 隐式强制类型转换

隐式强制类型转换指的是 **那些隐蔽的强制类型转换** ，**副作用也不是很明显** 。换句话说，你自己觉得不够明显的强制类型转换都可以算作隐式强制类型转换。

显式强制类型转换旨在让代码更加清晰易读，而隐式强制类型转换看起来就像是它的对立面，会让代码变得晦涩难懂。

对强制类型转换的诟病大多是针对隐式强制类型转换。

《JavaScript 语言精粹》的作者 Douglas Crockford 在许多场合和文章中都主张不要使用强制类型转换，认为其非常糟糕。然而他的代码中也大量使用了隐式和显式强制类型转换。实际上他的吐槽大部分是针对 == 运算符，但读完本章你会发现这只是强制类型转换的冰山一角。

问题是，隐式强制类型转换真是如此不堪吗？它是不是 JavaScript 语言的设计缺陷？我们是否应该对其退避三舍？

估计大多数读者会回答“是的”。其实不然，请容我细细道来。

让我们从另一个角度来看待隐式强制类型转换，看看它究竟为何物、该如何使用，不要简单地把它当作“显式强制类型转换的对立面”，因为这样理解过于狭隘，忽略了它们之间一个细微却十分重要的区别。

隐式强制类型转换的作用是减少冗余，让代码更简洁。

##### 隐式地简化

我们先来看一个例子，它不是 JavaScript 代码，而是强类型语言的伪代码：

```js
SomeType x = SomeType( AnotherType( y ) )
```
其中变量 y 的值被转换为 SomeType 类型。问题是语言本身不允许直接将 y 转换为 SomeType 类型。于是我们需要一个中间步骤，先将 y 转换为 AnotherType 类型，然后再从 AnotherType 转换为 SomeType。

如果能够这样：

```js
SomeType x = SomeType( y )
```
省去了中间步骤以后，类型转换变得更简洁了。这些无关紧要的中间步骤可以也应该被隐藏。

也许有些情况下这些中间步骤还是必要的，但是我觉得通过语言机制或定制方法来简化代码，抽象和隐藏那些细枝末节，有助于提高代码的可读性。

当然这些中间步骤仍然会发生在某处。通过隐藏这些细节，我们就可以专注于问题本身，这里是将变量 y 转换为 SomeType 类型。

虽然这并非是个十分恰当的隐式强制类型转换的例子，但我想说明的问题是，隐式强制类型转换同样可以用来提高代码可读性。

然而隐式强制类型转换也会带来一些负面影响，有时甚至是弊大于利。因此我们更应该学习怎样去其糟粕，取其精华。

很多开发人员认为如果某个机制有优点 A 但同时又有缺点 Z，为了保险起见不如全部弃之不用。

我不赞同这种“因噎废食”的做法。不要因为只看到了隐式强制类型转换的缺点就想当然地认为它一无是处。它也有好的方面，希望越来越多的开发人员能加以发现和运用。

##### 字符串和数字之间的隐式强制类型转换

前面我们讲了字符串和数字之间的显式强制类型转换，现在介绍它们之间的隐式强制类型转换。先来看一些会产生隐式强制类型转换的操作。

通过重载，+ 运算符即能 **用于数字加法**，也能 **用于字符串拼接**v 。JavaScript 怎样来判断我们要执行的是哪个操作？例如：

```js
var a = "42";
var b = "0";

var c = 42;
var d = 0;

a + b; // "420"
c + d; // 42
```
这里为什么会得到 "420" 和 42 两个不同的结果呢？通常的理解是，因为某一个或者两个操作数都是字符串，所以 + 执行的是字符串拼接操作。这样解释只对了一半，实际情况要复杂得多。

例如：

```js
var a = [1,2];
var b = [3,4];

a + b; // "1,23,4"
```
a 和 b 都不是字符串，但是它们都被强制转换为字符串然后进行拼接。原因何在？

下面两段内容与规范有关，如果太难理解可以跳过。

根据 ES5 规范 11.6.1 节，如果某个操作数是字符串或者能够通过以下步骤转换为字符串的话，+ 将进行拼接操作。如果其中一个操作数是对象（包括数组），则首先 **对其调用 ToPrimitive 抽象操作**（规范 9.1 节），**该抽象操作再调用 [[DefaultValue]]**（规范 8.12.8节），**以数字作为上下文** 。

你或许注意到这与 ToNumber 抽象操作处理对象的方式一样（参见 4.2.2 节）。因为数组的**valueOf() 操作无法得到简单基本类型值** ，于是它转而调用 **toString()** 。因此上例中的两个数组变成了 "1,2" 和 "3,4"。+ 将它们拼接后返回 "1,23,4"。

简单来说就是，如果 **+ 的其中一个操作数是字符串（或者通过以上步骤可以得到字符串），则执行字符串拼接；否则执行数字加法。**

有一个坑常常被提到，即 **[] + {} 和 {} + []，它们返回不同的结果，分别是 "[object Object]" 和 0** 。我们将在 5.1.3 节详细介绍。

对隐式强制类型转换来说，这意味着什么？

我们可以将数字和空字符串 "" 相 + 来将其转换为字符串：

```js
var a = 42;
var b = a + "";

b; // "42"
```
`+` 作为数字加法操作是可互换的，即 2 + 3 等同于 3 + 2。作为字符串拼接操作则不行，但 **对空字符串 "" 来说，a + "" 和 "" + a 结果一样** 。

a + "" 这样的隐式转换十分常见，一些对隐式强制类型转换持批评态度的人也不能免俗。

这本身就很能说明问题，无论怎样被人诟病，隐式强制类型转换仍然有其用武之地。

a + ""（隐式）和前面的 String(a)（显式）之间有一个细微的差别需要注意。根据 ToPrimitive 抽象操作规则，**a + "" 会对 a 调用 valueOf() 方法** ，然后 **通过 ToString 抽象操作将返回值转换为字符串** 。而 **String(a) 则是直接调用 ToString()** 。

它们最后返回的都是字符串，但如果 a 是对象而非数字结果可能会不一样！

例如：

```js
var a = {
 valueOf: function() { return 42; },
 toString: function() { return 4; }
};

a + ""; // "42"

String( a ); // "4"
```
你一般不太可能会遇到这个问题，除非你的代码中真的有这些匪夷所思的数据结构和操作。在定制 valueOf() 和 toString() 方法时需要特别小心，因为这会影响强制类型转换的结果。

再来看看从字符串强制类型转换为数字的情况。

```js
var a = "3.14";
var b = a - 0;

b; // 3.14
```
`-` 是数字减法运算符，因此 a - 0 会将 a 强制类型转换为数字。也可以使用 a * 1 和 a / 1，因为这两个运算符也只适用于数字，只不过这样的用法不太常见。

对象的 - 操作与 + 类似：

```js
var a = [3];
var b = [1];

a - b; // 2
```
为了执行减法运算，a 和 b 都需要被转换为数字，它们首先 **被转换为字符串**（通过 toString()），然后 **再转换为数字** 。

字符串和数字之间的隐式强制类型转换真如人们所说的那样糟糕吗？我个人不这么看。

b = String(a)（显式）和 b = a + ""（隐式）各有优点，b = a + "" 更常见一些。虽然饱受诟病，但隐式强制类型转换仍然有它的用处。

##### 布尔值到数字的隐式强制类型转换

在 **将某些复杂的布尔逻辑转换为数字加法** 的时候，隐式强制类型转换能派上大用场。当然这种情况并不多见，属于特殊情况特殊处理。

例如：

```js
function onlyOne(a,b,c) {
 return !!((a && !b && !c) ||
 (!a && b && !c) || (!a && !b && c));
}

var a = true;
var b = false;

onlyOne( a, b, b ); // true
onlyOne( b, a, b ); // true

onlyOne( a, b, a ); // false
```
如果其中有且仅有一个参数为 true，则 onlyOne(..) 返回 true。其在条件判断中使用了隐式强制类型转换，其他地方则是显式的，包括最后的返回值。

但如果有多个参数时（4 个、5 个，甚至 20 个），用上面的代码就很难处理了。这时就可以使用从布尔值到数字（0 或 1）的强制类型转换：

```js
function onlyOne() {
  var sum = 0;
  for (var i = 0; i < arguments.length; i++) {
    // 跳过假值，和处理0一样，但是避免了NaN
    if (arguments[i]) {
      sum += arguments[i];
    }
  }
  return sum == 1;
}

var a = true;
var b = false;

onlyOne(b, a); // true
onlyOne(b, a, b, b, b); // true
onlyOne(b, b); // false
onlyOne(b, a, b, b, b, a); // false
```
在 onlyOne(..) 中除了使用 for 循环，还可以使用 ES5 规范中的 reduce(..)函数。

通过 sum += arguments[i] 中的隐式强制类型转换，**将真值（true/truthy）转换为 1 并进行累加** 。如果有且仅有一个参数为 true，则结果为 1；否则不等于 1，sum == 1 条件不成立。

同样的功能也可以 **通过显式强制类型转换来实现** ：

```js
function onlyOne() {
  var sum = 0;
  for (var i = 0; i < arguments.length; i++) {
    sum += Number( !! arguments[i]);
  }
  return sum === 1;
}
```
**!!arguments[i] 首先将参数转换为 true 或 false** 。因此非布尔值参数在这里也是可以的，比如：onlyOne("42", 0)（否则的话，字符串会执行拼接操作，这样结果就不对了）。

**转换为布尔值** 以后，再 **通过 Number(..) 显式强制类型转换为 0 或 1** 。

这里使用显式强制类型转换会不会更好一些？注释里说这样的确能够避免 NaN 带来的问题，不过最终是看我们自己的需要。我个人觉得前者，即隐式强制类型转换，更为简洁（前提是不会传递 undefined 和 NaN 这样的值），而显式强制类型转换则会带来一些代码冗余。

总之如本书一贯强调的那样，一切都取决于我们自己的判断和权衡。

无论使用隐式还是显式，我们都能通过修改 onlyTwo(..) 或者 onlyFive(..)来处理更复杂的情况，只需要将最后的条件判断从 1 改为 2 或 5。这比加入一大堆 && 和 || 表达式简洁得多。所以强制类型转换在这里还是很有用的。

##### 隐式强制类型转换为布尔值

现在我们来看看到布尔值的隐式强制类型转换，它最为常见也最容易搞错。

相对布尔值，数字和字符串操作中的隐式强制类型转换还算比较明显。下面的情况会发生布尔值隐式强制类型转换。

(1) if (..) 语句中的条件判断表达式。
(2) for ( .. ; .. ; .. ) 语句中的条件判断表达式（第二个）。
(3) while (..) 和 do..while(..) 循环中的条件判断表达式。
(4) ? : 中的条件判断表达式。
(5) 逻辑运算符 ||（逻辑或）和 &&（逻辑与）左边的操作数（作为条件判断表达式）。

以上情况中，**非布尔值会被隐式强制类型转换为布尔值** ，遵循前面介绍过的 ToBoolean 抽象操作规则。

例如：

```js
var a = 42;
var b = "abc";
var c;
var d = null;

if (a) {
  console.log("yep"); // yep
}

while (c) {
  console.log("nope, never runs");
}

c = d ? a: b;
c; // "abc"

if ((a && d) || c) {
  console.log("yep"); // yep
}
```
上例中的非布尔值会被隐式强制类型转换为布尔值以便执行条件判断。

##### || 和 &&

逻辑运算符 ||（或）和 &&（与）应该并不陌生，也许正因为如此有人觉得它们在 JavaScript 中的表现也和在其他语言中一样。

这里面有一些非常重要但却不太为人所知的细微差别。

我其实不太赞同将它们称为“逻辑运算符”，因为这不太准确。称它们为“ **选择器运算符** ”（selector operators）或者“ **操作数选择器运算符** ”（operand selector operators）更恰当些。

为什么？因为和其他语言不同，**在 JavaScript 中它们返回的并不是布尔值。**

它们的返回值是两个操作数中的一个（且仅一个）。即 **选择两个操作数中的一个，然后返回它的值。**

引述 ES5 规范 11.11 节：

**&& 和 || 运算符的返回值并不一定是布尔类型，而是两个操作数其中一个的值。**

例如：

```js
var a = 42;
var b = "abc";
var c = null;

a || b; // 42
a && b; // "abc"

c || b; // "abc"
c && b; // null
```
在 C 和 PHP 中，上例的结果是 true 或 false，在 JavaScript（以及 Python 和 Ruby）中却是某个操作数的值。

|| 和 && 首先会 **对第一个操作数（a 和 c）执行条件判断** ，如果其 **不是布尔值（如上例）就先进行 ToBoolean 强制类型转换** ，然后 **再执行条件判断** 。

对于 **||** 来说，如果 **条件判断结果为 true 就返回第一个操作数**（a 和 c）的值，如果 **为 false 就返回第二个操作数（b）的值** 。

**&&** 则相反，如果 **条件判断结果为 true 就返回第二个操作数（b）的值** ，如果 **为 false 就返回第一个操作数（a 和 c）的值** 。

|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果（其中可能涉及强制类型转换）。c && b 中 c 为 null，是一个假值，因此 **&& 表达式的结果是 null（即 c 的值），而非条件判断的结果 false。**

现在明白我为什么把它们叫作“操作数选择器”了吧？

换一个角度来理解：

```js
a || b;
// 大致相当于(roughly equivalent to):
a ? a : b;

a && b;
// 大致相当于(roughly equivalent to):
a ? b : a;
```
之所以说大致相当，是因为它们返回结果虽然相同但是却有一个细微的差别。在 a ? a : b 中，如果 a 是一个复杂一些的表达式（比如有副作用的函数调用等），它有可能被执行两次（如果第一次结果为真）。而在 a || b 中 a 只执行一次，其结果用于条件判断和返回结果（如果适用的话）。a b 和 a ? b : a 也是如此。

下面是一个十分常见的 || 的用法，也许你已经用过但并未完全理解：

```js
function foo(a,b) {
  a = a || "hello";
  b = b || "world";
 
  console.log( a + " " + b );
}

foo(); // "hello world"
foo( "yeah", "yeah!" ); // "yeah yeah!"
```
a = a || "hello"（又称为 C# 的“空值合并运算符”的 JavaScript 版本）检查变量 a，如果还未赋值（或者为假值），就赋予它一个默认值（"hello"）。

这里需要注意！

```js
foo( "That’s it!", "" ); // "That’s it! world" <-- 晕!
```
第二个参数 "" 是一个假值（falsy value，参见 4.2.3 节），因此 b = b || "world" 条件不成立，返回默认值 "world"。

这种用法很常见，但是其中不能有假值，除非加上更明确的条件判断，或者转而使用 ? :三元表达式。

通过这种方式来设置默认值很方便，甚至那些公开诟病 JavaScript 强制类型转换的人也经常使用。

再来看看 &&。

有一种用法对开发人员不常见，然而 JavaScript 代码压缩工具常用。就是如果第一个操作数为真值，则 && 运算符“选择”第二个操作数作为返回值，这也叫作“ **守护运算符** ”（guard operator，参见 5.2.1 节），即 **前面的表达式为后面的表达式“把关”**：

```js
function foo() {
  console.log( a );
}

var a = 42;

a && foo(); // 42
```
**foo() 只有在条件判断 a 通过时才会被调用** 。如果 **条件判断未通过** ，**a && foo() 就会悄然终止**（也叫作“ **短路** ”，short circuiting），foo() 不会被调用。

这样的用法对开发人员不太常见，开发人员通常使用 if (a) { foo(); }。但 JavaScript代码压缩工具用的是 a && foo()，因为更简洁。以后再碰到这样的代码你就知道是怎么回事了。

|| 和 && 各自有它们的用武之地，前提是我们理解并且愿意在代码中运用隐式强制类型转换。

a = b || "something" 和 a && b() 用到了“短路”机制，我们将在 5.2.1 节详细介绍。

你大概会有疑问：既然返回的不是 true 和 false，为什么 a && (b || c) 这样的表达式在 if 和 for 中没出过问题？

这或许并不是代码的问题，问题在于你可能不知道这些条件判断表达式最后还会执行布尔值的隐式强制类型转换。

例如：

```js
var a = 42;
var b = null;
var c = "foo";

if (a && (b || c)) {
  console.log( "yep" );
}
```
这里 **a && (b || c) 的结果实际上是 "foo" 而非 true** ，然后 **再由 if 将 foo 强制类型转换为布尔值** ，所以 **最后结果为 true** 。

现在明白了吧，这里发生了 **隐式强制类型转换** 。如果要 **避免隐式强制类型转换** 就得这样：

```js
if (!!a && (!!b || !!c)) {
  console.log( "yep" );
}
```
##### 符号的强制类型转换

目前我们介绍的显式和隐式强制类型转换结果是一样的，它们之间的差异仅仅体现在代码可读性方面。

但 ES6 中引入了符号类型，它的强制类型转换有一个坑，在这里有必要提一下。ES6 允许从符号到字符串的显式强制类型转换，然而隐式强制类型转换会产生错误，具体的原因不在本书讨论范围之内。

例如：

```js
var s1 = Symbol( "cool" );
String( s1 ); // "Symbol(cool)"

var s2 = Symbol( "not cool" );
s2 + ""; // TypeError
```
**符号不能够被强制类型转换为数字**（显式和隐式都会产生错误），但 **可以被强制类型转换为布尔值**（ **显式和隐式结果都是 true** ）。

由于规则缺乏一致性，我们要对 ES6 中符号的强制类型转换多加小心。

好在鉴于符号的特殊用途（参见第 3 章），我们不会经常用到它的强制类型转换。

#### 宽松相等和严格相等

**宽松相等**（loose equals）== 和**严格相等**（strict equals）=== 都用来判断两个值是否“相等”，但是它们之间有一个很重要的区别，特别是在判断条件上。

常见的误区是“== 检查值是否相等，=== 检查值和类型是否相等”。听起来蛮有道理，然而还不够准确。很多 JavaScript 的书籍和博客也是这样来解释的，但是很遗憾他们都错了。

正确的解释是：“**== 允许在相等比较中进行强制类型转换，而 === 不允许。**”

##### 相等比较操作的性能

我们来看一看两种解释的区别。

根据第一种解释（不准确的版本），=== 似乎比 == 做的事情更多，因为它还要检查值的类型。第二种解释中 == 的工作量更大一些，因为如果值的类型不同还需要进行强制类型转换。

有人觉得 == 会比 === 慢，实际上虽然强制类型转换确实要多花点时间，但仅仅是微秒级（百万分之一秒）的差别而已。

如果进行比较的两个值类型相同，则 == 和 === 使用相同的算法，所以除了 JavaScript 引擎实现上的细微差别之外，它们之间并没有什么不同。

如果两个值的类型不同，我们就 **需要考虑有没有强制类型转换的必要** ，**有就用 ==，没有就用 ===，不用在乎性能** 。

== 和 === 都会检查操作数的类型。区别在于操作数类型不同时它们的处理方式不同。

##### 抽象相等

ES5 规范 11.9.3 节的“抽象相等比较算法”定义了 == 运算符的行为。该算法简单而又全面，涵盖了所有可能出现的类型组合，以及它们进行强制类型转换的方式。

“抽象相等”（abstract equality）的这些规则正是隐式强制类型转换被诟病的原因。开发人员觉得它们太晦涩，很难掌握和运用，弊（导致 bug）大于利（提高代码可读性）。这种观点我不敢苟同，因为本书的读者都是优秀的开发人员，整天与算法和代码打交道，“抽象相等”对各位来说只是小菜一碟。建议大家看一看 ES5 规范 11.9.3 节，你会发现这些规则其实非常简单明了。

其中第一段（11.9.3.1）规定如果两个值的类型相同，就仅比较它们是否相等。例如，42 等于 42，"abc" 等于 "abc"。

有几个非常规的情况需要注意。

- **NaN 不等于 NaN**（参见第 2 章）。
- **+0 等于 -0**（参见第 2 章）。

11.9.3.1 的最后定义了对象（包括函数和数组）的宽松相等 ==。两个对象指向同一个值时
即视为相等，不发生强制类型转换。

>=== 的定义和 11.9.3.1 一样，包括对象的情况。实际上在比较两个对象的时候，== 和 === 的工作原理是一样的。

11.9.3 节中还规定，**== 在比较两个不同类型的值时会发生隐式强制类型转换** ，**会将其中之一或两者都转换为相同的类型后再进行比较** 。

###### 字符串和数字之间的相等比较

我们沿用本章前面字符串和数字的例子来解释 == 中的强制类型转换：

```js
var a = 42;
var b = "42";
a === b; // false
a == b; // true
```
因为没有强制类型转换，所以 a === b 为 false，42 和 "42" 不相等。

而 a == b 是宽松相等，即如果两个值的类型不同，则对其中之一或两者都进行强制类型转换。

具体怎么转换？是 a 从 42 转换为字符串，还是 b 从 "42" 转换为数字？

ES5 规范 11.9.3.4-5 这样定义：

(1) **如果 Type(x) 是数字，Type(y) 是字符串，则返回 x == ToNumber(y) 的结果。**
(2) **如果 Type(x) 是字符串，Type(y) 是数字，则返回 ToNumber(x) == y 的结果。**

规范使用 Number 和 String 来代表数字和字符串类型，而本书使用的是数字（number）和字符串（string）。切勿将规范中的 Number 和原生函数 Number()混为一谈。本书中类型名的首字符大写和小写是一回事。

根据规范，"42" 应该被强制类型转换为数字以便进行相等比较。相关规则，特别是 ToNumber 抽象操作的规则前面已经介绍过。本例中两个值相等，均为 42。

###### 其他类型和布尔类型之间的相等比较

== 最容易出错的一个地方是 true 和 false 与其他类型之间的相等比较。

例如：

```js
var a = "42";
var b = true;
a == b; // false
```
我们都知道 "42" 是一个真值（见本章前面部分），为什么 == 的结果不是 true 呢？原因既简单又复杂，让人很容易掉坑里，很多 JavaScript 开发人员对这个地方并未引起足够的重视。

规范 11.9.3.6-7 是这样说的：

(1) **如果 Type(x) 是布尔类型，则返回 ToNumber(x) == y 的结果；**
(2) **如果 Type(y) 是布尔类型，则返回 x == ToNumber(y) 的结果。**

仔细分析例子，首先：

```js
var x = true;
var y = "42";
x == y; // false
```
Type(x) 是布尔值，所以 ToNumber(x) 将 true 强制类型转换为 1，变成 1 == "42"，二者的类型仍然不同，"42" 根据规则被强制类型转换为 42，最后变成 1 == 42，结果为 false。

反过来也一样：

```js
var x = "42";
var y = false;
x == y; // false
```
Type(y) 是布尔值，所以 ToNumber(y) 将 false 强制类型转换为 0，然后 "42" == 0 再变成 42 == 0，结果为 false。

也就是说，字符串 "42" 既不等于 true，也不等于 false。一个值怎么可以既非真值也非假值，这也太奇怪了吧？

这个问题本身就是错误的，我们被自己的大脑欺骗了。

"42" 是一个真值没错，但 "42" == true 中并没有发生布尔值的比较和强制类型转换。这里不是 "42" 转换为布尔值（true），而是 true 转换为 1，"42" 转换为 42。

这里并不涉及 ToBoolean，所以 "42" 是真值还是假值与 == 本身没有关系！

重点是我们要搞清楚 == 对不同的类型组合怎样处理。== 两边的布尔值会被强制类型转换为数字。

很奇怪吧？我个人建议无论什么情况下都不要使用 == true 和 == false。

请注意，这里说的只是 ==，=== true 和 === false 不允许强制类型转换，所以并不涉及ToNumber。

例如：

```js
var a = "42"
// 不要这样用，条件判断不成立：
if (a == true) {
 // ..
}
// 也不要这样用，条件判断不成立：
if (a === true) {
 // ..
}
// 这样的显式用法没问题：
if (a) {
 // ..
}
// 这样的显式用法更好：
if (!!a) {
 // ..
}
// 这样的显式用法也很好：
if (Boolean( a )) {
 // ..
}
```
避免了 == true 和 == false（也叫作布尔值的宽松相等）之后我们就不用担心这些坑了。

###### null 和 undefined 之间的相等比较

null 和 undefined 之间的 == 也涉及隐式强制类型转换。ES5 规范 11.9.3.2-3 规定：

(1) **如果 x 为 null，y 为 undefined，则结果为 true。**
(2) **如果 x 为 undefined，y 为 null，则结果为 true。**

**在 == 中 null 和 undefined 相等**（它们也与其自身相等），除此之外其他值都不存在这种情况。

这也就是说 **在 == 中 null 和 undefined 是一回事** ，**可以相互进行隐式强制类型转换** ：

```js
var a = null;
var b;

a == b; // true
a == null; // true
b == null; // true

a == false; // false
b == false; // false
a == ""; // false
b == ""; // false
a == 0; // false
b == 0; // false
```
**null 和 undefined 之间的强制类型转换是安全可靠的** ，上例中除 null 和 undefined 以外的其他值均无法得到假阳（false positive）结果。**个人认为通过这种方式将 null 和 undefined 作为等价值来处理比较好。**

例如：

```js
var a = doSomething();

if (a == null) {
  // ..
}
```
条件判断 **a == null** 仅在 doSomething() 返回 **非 null 和 undefined 时才成立** ，**除此之外其他值都不成立** ，**包括 0、false 和 "" 这样的假值** 。

下面是 **显式的做法** ，其中 **不涉及强制类型转换，个人感觉更繁琐一些**（大概执行效率也会更低）：

```js
var a = doSomething();

if (a === undefined || a === null) {
  // ..
}
```
我认为 **a == null 这样的隐式强制类型转换在保证安全性的同时还能提高代码可读性** 。

###### 对象和非对象之间的相等比较

关于对象（对象 / 函数 / 数组）和标量基本类型（字符串 / 数字 / 布尔值）之间的相等比较，ES5 规范 11.9.3.8-9 做如下规定：

(1) **如果 Type(x) 是字符串或数字，Type(y) 是对象，则返回 x == ToPrimitive(y) 的结果；**
(2) **如果 Type(x) 是对象，Type(y) 是字符串或数字，则返回 ToPromitive(x) == y 的结果。**

**这里只提到了字符串和数字，没有布尔值。** 原因是我们之前介绍过 11.9.3.6-7 中规定了布尔值会先被强制类型转换为数字。

例如：

```js
var a = 42;
var b = [ 42 ];

a == b; // true
```
[ 42 ] 首先调用 ToPromitive 抽象操作（参见 4.2 节），返回 "42"，变成 "42" == 42，然后又变成 42 == 42，最后二者相等。

之前介绍过的 **ToPromitive 抽象操作** 的所有特性（如 **toString()** 、**valueOf()** ）在这里都适用。如果 **我们需要自定义 valueOf() 以便从复杂的数据结构返回一个简单值进行相等比较，这些特性会很有帮助** 。

在第 3 章中，我们介绍过“拆封”，即 **“打开”封装对象**（如 new String("abc")），返回其中的 **基本数据类型值**（"abc"）。**== 中的 ToPromitive 强制类型转换也会发生这样的情况** ：

```js
var a = "abc";
var b = Object( a ); // 和new String( a )一样

a === b; // false
a == b; // true
```
a == b 结果为 true，因为 **b 通过 ToPromitive 进行强制类型转换**（也称为“拆封”，英文为 unboxed 或者 unwrapped），并 **返回标量基本类型值 "abc"** ，与 a 相等。

但有一些值不这样，原因是 == 算法中其他优先级更高的规则。例如：

```js
var a = null;
var b = Object( a ); // 和Object()一样
a == b; // false

var c = undefined;
var d = Object( c ); // 和Object()一样
c == d; // false

var e = NaN;
var f = Object( e ); // 和new Number( e )一样
e == f; // false
```
因为没有对应的封装对象，所以 null 和 undefined 不能够被封装（boxed），Object(null) 和 Object() 均返回一个常规对象。

**NaN 能够被封装为数字封装对象，但拆封之后 NaN == NaN 返回 false，因为 NaN 不等于 NaN**（参见第 2 章）。

##### 比较少见的情况

我们已经全面介绍了 == 中的隐式强制类型转换（常规和非常规的情况），现在来看一下那些需要特别注意和避免的比较少见的情况。

首先来看看更改内置原生原型会导致哪些奇怪的结果。

###### 返回其他数字

```js
Number.prototype.valueOf = function() {
  return 3;
};

new Number( 2 ) == 3; // true
```
2 == 3 不会有这种问题，因为 2 和 3 都是数字基本类型值，不会调用**Number.prototype.valueOf()** 方法。而 **Number(2) 涉及 ToPrimitive 强制类型转换** ，因此会调用 **valueOf()** 。

真是让人头大。这也是强制类型转换和 == 被诟病的原因之一。但问题并非出自 JavaScript，而是我们自己。不要有这样的想法，觉得“编程语言应该阻止我们犯错误”。

还有更奇怪的情况：

```js
if (a == 2 && a == 3) {
   // ..
}
```
你也许觉得这不可能，因为 a 不会同时等于 2 和 3。但 **“同时”一词并不准确** ，因为 a == 2 在 a == 3 之前执行。

如果让 **a.valueOf() 每次调用都产生副作用** ，比如 **第一次返回 2，第二次返回 3，就会出现这样的情况** 。这实现起来很简单：

```js
var i = 2;

Number.prototype.valueOf = function() {
  return i++;
};

var a = new Number( 42 );
if (a == 2 && a == 3) {
  console.log( "Yep, this happened." );
}
```
再次强调，千万不要这样，也不要因此而抱怨强制类型转换。对一种机制的滥用并不能成为诟病它的借口。我们应该正确合理地运用强制类型转换，避免这些极端的情况。

###### 假值的相等比较

**== 中的隐式强制类型转换** 最为人诟病的地方是假值的相等比较。下面分别列出了常规和非常规的情况：

```js
"0" == null; // false
"0" == undefined; // false
"0" == false; // true -- 晕！
"0" == NaN; // false
"0" == 0; // true
"0" == ""; // false

false == null; // false
false == undefined; // false
false == NaN; // false
false == 0; // true -- 晕！
false == ""; // true -- 晕！
false == []; // true -- 晕！
false == {}; // false

"" == null; // false
"" == undefined; // false
"" == NaN; // false
"" == 0; // true -- 晕！
"" == []; // true -- 晕！
"" == {}; // false

0 == null; // false
0 == undefined; // false
0 == NaN; // false
0 == []; // true -- 晕！
0 == {}; // false
```
以上 24 种情况中有 17 种比较好理解。比如我们都知道 "" 和 NaN 不相等，"0" 和 0 相等。

然而有 7 种我们注释了“晕！”，因为 **它们属于假阳**（false positive）的情况，里面坑很多。

"" 和 0 明显是两个不同的值，它们之间的强制类型转换很容易搞错。**请注意这里不存在假阴**（false negative）的情况。

###### 极端情况

这还不算完，还有更极端的例子：

```js
[] == ![] // true
```
事情变得越来越疯狂了。看起来这似乎是真值和假值的相等比较，结果不应该是 true，因为一个值不可能同时既是真值也是假值！

事实并非如此。让我们看看 ! 运算符都做了些什么？根据 ToBoolean 规则，**它会进行布尔值的显式强制类型转换**（同时反转奇偶校验位）。所以 **[] == ![] 变成了 [] == false** 。前面我们讲过 **false == []** ，最后的结果就顺理成章了。

再来看看其他情况：

```js
2 == [2]; // true
"" == [null]; // true
```
介绍 ToNumber 时我们讲过，**== 右边的值 [2] 和 [null] 会进行 ToPrimitive 强制类型转换** ，以便能够和左边的基本类型值（2 和 ""）进行比较。因为 **数组的 valueOf() 返回数组本身** ，所以 **强制类型转换过程中数组会进行字符串化。**

第一行中的 **[2] 会转换为 "2"** ，然后 **通过 ToNumber 转换为 2** 。第二行中的 **[null] 会直接转换为 ""** 。

所以最后的结果就是 2 == 2 和 "" == ""。

如果还是觉得头大，那么你的困惑可能并非来自强制类型转换，而是 **ToPrimitive 将数组转换为字符串这一过程** 。也许你认为 [2].toString() 返回的不是 "2"，[null].toString()返回的也不是 ""。

但是如果不这样处理的话又能怎样呢？我实在想不出其他更好的办法。或许应该将 [2] 转换为 "[2]"，但这样的话在别的地方又显得很奇怪。

有人也许会觉得既然 String(null) 返回 "null"，所以 **String([null]) 也应该返回 "null"** 。确实有道理，但这就是问题所在。

隐式强制类型转换本身不是问题的根源，因为 **[null] 在显式强制类型转换中也是转换为 ""** 。问题在于将数组转换为字符串是否合理，具体该如何处理。所以实际上这是 **String([..]) 规则的问题** 。又或者根本就不应该将数组转换为字符串？但这样一来又会导致很多其他问题。

还有一个坑常常被提到：

```js
0 == "\n"; // true
```
前面介绍过，**""、"\n"（或者 " "** 等其他空格组合）等 **空字符串被 ToNumber 强制类型转换为 0** 。这样处理总没有问题了吧，不然你要怎么办？

或许可以将空字符串和空格转换为 NaN，这样 " " == NaN 就为 false 了，然而这并没有从根本上解决问题。

0 == "\n" 导致程序出错的几率小之又小，很容易避免。

类型转换总会出现一些特殊情况，并非只有强制类型转换，任何编程语言都是如此。问题出在我们的臆断（有时或许碰巧猜对了？！），但这并不能成为诟病强制类型转换机制的理由。

上述 7 种情况基本涵盖了所有我们可能遇到的坑（除修改 valueOf() 和 toStrign() 的情况以外）。

与前面 24 种情况列表相对应的是下面这个列表：

```js
42 == "43"; // false
"foo" == 42; // false
"true" == true; // false

42 == "42"; // true
"foo" == [ "foo" ]; // true
```
这些是非假值的常规情况（实际上还可以加上无穷大数字的相等比较），其中涉及的强制类型转换是安全的，也比较好理解。

###### 完整性检查

我们深入介绍了隐式强制类型转换中的一些特殊情况。也难怪大多数开发人员都觉得这太晦涩，唯恐避之不及。

现在回过头来做一下完整性检查（sanity check）。

前面列举了相等比较中的强制类型转换的 7 个坑，不过另外还有至少 17 种情况是绝对安全和容易理解的。

因为 7 棵歪脖树而放弃整片森林似乎有点因噎废食了，所以明智的做法是扬其长避其短。

再来看看那些“短”的地方：

```js
"0" == false; // true -- 晕！
false == 0; // true -- 晕！
false == ""; // true -- 晕！
false == []; // true -- 晕！
"" == 0; // true -- 晕！
"" == []; // true -- 晕！
0 == []; // true -- 晕！
```
其中有 4 种情况涉及 == false，之前我们说过应该避免，应该不难掌握。现在剩下 3 种：

```js
"" == 0; // true -- 晕！
"" == []; // true -- 晕！
0 == []; // true -- 晕！
```
正常情况下我们应该不会这样来写代码。我们应该不太可能会用 == [] 来做条件判断，而是用 == "" 或者 == 0，如：

```js
function doSomething(a) {
 if (a == "") {
 // ..
 }
}
```
如果不小心碰到 doSomething(0) 和 doSomething([]) 这样的情况，结果会让你大吃一惊。

又如：

```js
function doSomething(a, b) {
  if (a == b) {
    // ..
  }
}
```
doSomething("",0) 和 doSomething([],"") 也会如此。

这些特殊情况会导致各种问题，我们要多加小心，好在它们并不十分常见。

###### 安全运用隐式强制类型转换

我们要对 == 两边的值认真推敲，以下两个原则可以让我们有效地避免出错。

- **如果两边的值中有 true 或者 false，千万不要使用 ==。**
- **如果两边的值中有 []、"" 或者 0，尽量不要使用 ==。**

这时最好用 === 来避免不经意的强制类型转换。这两个原则可以让我们避开几乎所有强制类型转换的坑。

这种情况下强制类型转换越显式越好，能省去很多麻烦。

所以 **== 和 === 选择哪一个取决于是否允许在相等比较中发生强制类型转换。**

强制类型转换在很多地方非常有用，能够让相等比较更简洁（比如 null 和 undefined）。

**隐式强制类型转换在部分情况下确实很危险，这时为了安全起见就要使用 ===。**

有一种情况下强制类型转换是绝对安全的，那就是 typeof 操作。typeof 总是返回七个字符串之一（参见第 1 章），其中没有空字符串。所以在类型检查过程中不会发生隐式强制类型转换。typeof x == "function" 是 100% 安全的，和 typeof x === "function" 一样。事实上两者在规范中是一回事。所以既不要盲目听命于代码工具每一处都用 ===，更不要对这个问题置若罔闻。

我们要对自己的代码负责。

隐式强制类型转换真的那么不堪吗？某些情况下是，但总的来说并非如此。

作为一个成熟负责的开发人员，我们应该学会安全有效地运用强制类型转换（显式和隐式），并对周围的同行言传身教。

Alex Dorey（GitHub 用户名 @dorey）在 GitHub 上制作了一张图表，列出了各种相等比较的情况，如图 4-1 所示。

![JavaScript 中的相等比较](https://graphbed.qiniu.songxingguo.com/deep-javascript-2/JavaScript%20%E4%B8%AD%E7%9A%84%E7%9B%B8%E7%AD%89%E6%AF%94%E8%BE%83.png)

#### 抽象关系比较

a < b 中涉及的隐式强制类型转换不太引人注意，不过还是很有必要深入了解一下。

ES5 规范 11.8.5 节定义了“抽象关系比较”（abstract relational comparison），分为两个部
分：比较双方都是字符串（后半部分）和其他情况（前半部分）。

该算法仅针对 a < b，a=""> b 会被处理为 b <> 比较双方首先 **调用 ToPrimitive** ，如果 **结果出现非字符串** ，就 **根据 ToNumber 规则将双方强制类型转换为数字来进行行比较** 。

例如：

```js
var a = [ 42 ];
var b = [ "43" ];

a < b; // true
b < a; // false
```
前面介绍过的 -0 和 NaN 的相关规则在这里也适用。

如果比较双方都是字符串，则按字母顺序来进行比较：

```js
var a = [ "42" ];
var b = [ "043" ];

a < b; // false
```
**a 和 b 并没有被转换为数字** ，因为 **ToPrimitive 返回的是字符串** ，所以 这里比较的是 "42" 和 "043" 两个字符串** ，它们分别以 "4" 和 "0" 开头。因为 "0" 在字母顺序上小于 "4"，所以最后结果为 false。

同理：

```js
var a = [ 4, 2 ];
var b = [ 0, 4, 3 ];

a < b; // false
```
a 转换为 "4, 2"，b 转换为 "0, 4, 3"，同样是按字母顺序进行比较。

再比如：

```js
var a = { b: 42 };
var b = { b: 43 };

a < b; // ??
```
结果还是 false，因为 a 是 [object Object]，b 也是 [object Object]，所以按照字母顺序 a < b 并不成立。

下面的例子就有些奇怪了：

```js
var a = { b: 42 };
var b = { b: 43 };

a < b; // false
a == b; // false
a > b; // false
a <= b; // true
a >= b; // true
```
为什么 a == b 的结果不是 true ？它们的字符串值相同（同为 "[object Object]"），按道理应该相等才对？实际上不是这样，你可以回忆一下前面讲过的对象的相等比较。

但是如果 a < b 和 a == b 结果为 false，为什么 a <= b 和 a >= b 的结果会是 true 呢？因为根据规范 a <= b 被处理为 b < a，然后将结果反转。因为 b < a 的结果是 false，所以 a <= b 的结果是 true。

这可能与我们设想的大相径庭，即 **<= 应该是“小于或者等于”**。实际上 **JavaScript 中 <= 是“不大于”的意思**（即 !(a > b)，处理为 !(b < a)）。同理 a >= b 处理为 b <= a。

相等比较有严格相等，**关系比较却没有“严格关系比较”**（strict relational comparison）。也就是说 **如果要避免 a < b 中发生隐式强制类型转换，我们只能确保 a 和 b 为相同的类型，除此之外别无他法。**

与 == 和 === 的完整性检查一样，**我们应该在必要和安全的情况下使用强制类型转换** ，如：42 < "43"。换句话说就是为了保证安全，**应该对关系比较中的值进行显式强制类型转换** ：

```js
var a = [ 42 ];
var b = "043";
a < b; // false -- 字符串比较！
Number( a ) < Number( b ); // true -- 数字比较！
```
#### 小结

本章介绍了 JavaScript 的数据类型之间的转换，即强制类型转换：包括显式和隐式。

强制类型转换常常为人诟病，但实际上很多时候它们是非常有用的。作为有使命感的 JavaScript 开发人员，我们有必要深入了解强制类型转换，这样就能取其精华，去其糟粕。

显式强制类型转换明确告诉我们哪里发生了类型转换，有助于提高代码可读性和可维护性。

隐式强制类型转换则没有那么明显，是其他操作的副作用。感觉上好像是显式强制类型转换的反面，实际上隐式强制类型转换也有助于提高代码的可读性。

在处理强制类型转换的时候要十分小心，尤其是隐式强制类型转换。在编码的时候，要知其然，还要知其所以然，并努力让代码清晰易读。