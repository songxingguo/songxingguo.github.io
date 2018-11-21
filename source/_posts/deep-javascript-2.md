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

大多数开发者认为，像 JavaScript 这样的动态语言是没有类型（type）的。让我们来看看ES5.1 规范（http://www.ecma-international.org/ecma-262/5.1/）对此是如何界定的：

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