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

### 函数作用域和块作用域

作用域包含了一系列的“气泡”，每一个都可以作为容器，其中包含了标识符（变量、函数）的定义。这些气泡互相嵌套并且整齐地排列成蜂窝型，排列的结构是在写代码时定义的。


#### 函数作用域

![函数作用域](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%87%BD%E6%95%B0%E4%BD%9C%E7%94%A8%E5%9F%9F.png)

函数作用域的含义是指，属于这个函数的全部变量都可以在整个函数的范围内使用及复
用（事实上在嵌套的作用域中也可以使用）。

可以把变量和函数包裹在一个函数的作用域中，然后用这个作用域
来“隐藏”它们。

##### 避免冲突

“隐藏”作用域中的变量和函数可以避免同名标识符之间的冲突。

- 全局命名空间

  ```js
  var MyReallyCoolLibrary = {
    awesome: "stuff",
    doSomething: function() {
      // ...
    },
    doAnotherThing: function() {
      // ...
      26｜第3章
    }
  };
  ```
- 模块管理

#### 函数表达式

在任意代码片段外部添加包装函数，可以将内部的变量和函数定义“隐藏”起来，外部作用域无法访问包装函数内部的任何内容。

```js
var a = 2;
function foo() { // <-- 添加这一行
  var a = 3;
  console.log(a); // 3
} // <-- 以及这一行
foo(); // <-- 以及这一行
console.log(a); // 2
```
如果函数不需要函数名（或者至少函数名可以不污染所在作用域），并且能够自动运行，这将会更加理想。

```js
var a = 2;
(function foo() { // <-- 添加这一行
  var a = 3;
  console.log(a); // 3
})(); // <-- 以及这一行
console.log(a); // 2
```
包装函数的声明以 (function... 而不仅是以 function... 开始。

>区分函数声明和表达式最简单的方法是看 function 关键字出现在声明中的位
置（不仅仅是一行代码，而是整个声明中的位置）。如果 function 是声明中
的第一个词，那么就是一个函数声明，否则就是一个函数表达式。

函数声明和函数表达式之间最重要的区别是它们的名称标识符将会绑定在何处。

(function foo(){ .. }) 作为函数表达式意味着 foo 只能在 .. 所代表的位置中被访问，外部作用域则不行。foo 变量名被隐藏在自身中意味着不会非必要地污染外部作用域。

##### 匿名和具名

```
setTimeout( function() {
  console.log("I waited 1 second!");
}, 1000 );
```

这叫做匿名函数表达式，因为 function().. 没有名称标识符。函数表达式可以是匿名的，而函数声明则不可以省略函数名——在 JavaScript 的语法中这是非法的。

匿名函数表达式的缺点：

- 调试困难。
- 函数只能使用已经过期的 arguments.callee 引用。
- 降低了代码的可读性和可理解性。

行内函数表达式非常强大且有用——匿名和具名之间的区别并不会对这点有任何影响。给函数表达式指定一个函数名可以有效解决以上问题。始终给函数表达式命名是一个最佳实践：

```js
setTimeout(function timeoutHandler() { // <-- 快看，我有名字了！
  console.log("I waited 1 second!");
},1000);
```

#### 立即执行函数表达式

![立即执行函数表达式](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E7%AB%8B%E5%8D%B3%E6%89%A7%E8%A1%8C%E5%87%BD%E6%95%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F.png)

```js
var a = 2; (function foo() {
  var a = 3;
  console.log(a); // 3
})();
console.log(a); // 2
```
##### (function foo(){ .. })()

(function foo(){ .. })(),第一个 ( ) 将函数变成表达式，第二个 ( ) 执行了这个函数。

IIFE，代表立即执行函数表达式（Immediately Invoked Function Expression）。

函数名对 IIFE 当然不是必须的，IIFE 最常见的用法是使用一个匿名函数表达式。虽然使用具名函数的 IIFE 并不常见，但它具有上述匿名函数表达式的所有优势，因此也是一个值得推广的实践。

```js
var a = 2; (function IIFE() {
  var a = 3;
  console.log(a); // 3
})();
console.log(a); // 2
```

##### 改进的形式：(function(){ .. }())

改进的形式: (function(){ .. }()), 将用来调用的 () 括号被移进了用来包装的 ( ) 括号中。

##### 作为函数调用并传递参数进去

IIFE 的另一个非常普遍的进阶用法是把它们当作函数调用并传递参数进去。

```js
var a = 2; 
(function IIFE(global) {
  var a = 3;
  console.log(a); // 3
  console.log(global.a); // 2
})(window);
console.log(a); // 2
```
这对于改进代码风格是非常有帮助的。

##### 解决 undefined 标识符的默认值被错误覆盖

另外一个应用场景是解决 undefined 标识符的默认值被错误覆盖导致的异常（虽
然不常见）。将一个参数命名为 undefined，但是在对应的位置不传入任何值，这样就可以保证在代码块中 undefined 标识符的值真的是 undefined：

```js
undefined = true; // 给其他代码挖了一个大坑！绝对不要这样做！
(function IIFE(undefined) {
  var a;
  if (a === undefined) {
    console.log("Undefined is safe here!");
  }
})();
```

##### 倒置代码的运行顺序

IIFE 还有一种变化的用途是倒置代码的运行顺序，将需要运行的函数放在第二位，在 IIFE执行之后当作参数传递进去。

```js
(function IIFE(def) {
  def(window);
})(function def(global) {
  var a = 3;
  console.log(a); // 3
  console.log(global.a); // 2
});
```
函数表达式 def 定义在片段的第二部分，然后当作参数（这个参数也叫作 def）被传递进 IIFE 函数定义的第一部分中。最后，参数 def（也就是传递进去的函数）被调用，并将 window 传入当作 global 参数的值。

#### 块作用域

![块作用域](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%9D%97%E4%BD%9C%E7%94%A8%E5%9F%9F.png)

块作用域是一个用来对之前的最小授权原则进行扩展的工具，将代码从在函数中隐藏信息扩展为在块中隐藏信息。

##### with

用 with 从对象中创建出的作用域仅在 with 声明中而非外部作用域中有效。

##### try/catch

JavaScript 的 ES3 规范中规定 try/catch 的 catch 分句会创建一个块作用域，其中声明的变量仅在 catch 内部有效。

```js
try {
  undefined(); // 执行一个非法操作来强制制造一个异常
} catch(err) {
  console.log(err); // 能够正常执行！
}
console.log(err); // ReferenceError: err not found
```
##### let

let 关键字可以将变量绑定到所在的任意作用域中（通常是 { .. } 内部）。换句话说，let为其声明的变量隐式地了所在的块作用域。

```js
var foo = true;
if (foo) {
  let bar = foo * 2;
  bar = something(bar);
  console.log(bar);
}
console.log(bar); // ReferenceError
```
通常来讲，显式的代码优于隐式或一些精巧但不清晰的代码。显式的块作用域风格非常容易书写，并且和其他语言中块作用域的工作原理一致。

```js
var foo = true;
if (foo) {
  { // <-- 显式的快
    let bar = foo * 2;
    bar = something(bar);
    console.log(bar);
  }
}
console.log(bar); // ReferenceError
```
使用 let 进行的声明不会在块作用域中进行提升。声明的代码被运行之前，声明并不
“存在”。

```js
{
  console.log(bar); // ReferenceError!
  let bar = 2;
}
```
###### 垃圾收集

```js
function process(data) {
  // 在这里做点有趣的事情
}
var someReallyBigData = {..
};
process(someReallyBigData);
var btn = document.getElementById("my_button");
btn.addEventListener("click",
function click(evt) {
  console.log("button clicked");
},/*capturingPhase=*/false);
```
click 函数的点击回调并不需要 someReallyBigData 变量。理论上这意味着当 process(..) 执行后，在内存中占用大量空间的数据结构就可以被垃圾回收了。但是，由于 click 函数形成了一个覆盖整个作用域的闭包，JavaScript 引擎极有可能依然保存着这个结构（取决于具体实现）。

块作用域可以打消这种顾虑，可以 **让引擎清楚地知道没有必要继续保存** someReallyBigData 了：

```js
function process(data) {
  // 在这里做点有趣的事情
}
// 在这个块中定义的内容可以销毁了！
{
  let someReallyBigData = {..
  };
  process(someReallyBigData);
}
var btn = document.getElementById("my_button");
btn.addEventListener("click",
function click(evt) {
  console.log("button clicked");
},/*capturingPhase=*/false);
```
###### let循环

```js
for (let i=0; i<10; i++) {
  console.log( i );
}
console.log( i ); // ReferenceError
```
for 循环头部的 let 不仅将 i 绑定到了 for 循环的块中，事实上它将其重新绑定到了循环的每一个迭代中，确保使用上一个循环迭代结束时的值重新进行赋值。

下面通过另一种方式来说明每次迭代时进行重新绑定的行为：

```js
{
  let j;
  for (j = 0; j < 10; j++) {
    let i = j; // 每个迭代重新绑定！
    console.log(i);
  }
}
```
由于 let 声明附属于一个新的作用域而不是当前的函数作用域（也不属于全局作用域），当代码中存在对于函数作用域中 var 声明的隐式依赖时，就会有很多隐藏的陷阱，如果用 let 来替代 var 则需要在代码重构的过程中付出额外的精力。

##### const

const，同样可以用来创建块作用域变量，但其值是固定的（常量）。之后任何试图修改值的操作都会引起错误。

```js
var foo = true;
if (foo) {
  var a = 2;
  const b = 3; // 包含在 if 中的块作用域常量
  a = 3; // 正常 !
  b = 4; // 错误 !
}
console.log(a); // 3
console.log(b); // ReferenceError!
```
