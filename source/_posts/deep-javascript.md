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

简单地说，**词法作用域就是定义在词法阶段的作用域** 。换句话说，**词法作用域** 是 **由你在写代码时将变量和块作用域写在哪里来决定的** ，因此 **当词法分析器处理代码时会保持作用域不变**（大部分情况下是这样的）。

![作用域气泡](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%B0%94%E6%B3%A1.png)

作用域查找会在找到第一个匹配的标识符时停止。

遮蔽效应：在多层嵌套作用域中可以定义同名的标识符，内部的标识符“屏蔽”了外部的标识符。

> 全局变量会自动成为全局对象（比如浏览器中的window对象）的属性。因此通过这种技术可以访问那些被同名变量所屏蔽的全局变量。但非全局变量如果被屏蔽了，无论如何都无法被访问。

**无论函数在哪里被调用，也无论它如何被调用** ，它的 **词法作用域** 都 **只由函数被声明时所处的位置决定** 。

词法作用域查找只会查找一级标识符。

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

### 提升

函数作用域和块作用域的行为是一样的，可以总结为：任何声明在某个作用域内的变量，都将附属于这个作用域。

![提升](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E6%8F%90%E5%8D%87.png)

```js
a = 2;
var a;
console.log( a ); // 2
```
上面代码片段会以如下形式进行处理：

```js
var a;
a = 2;
console.log( a ); // 2
```
```js
console.log( a ); // undefined
var a = 2;
```
上面代码=码片段实际是按照以下流程处理的：

```js
var a;
console.log( a );
a = 2;
```
因此，打个比方，这个过程就好像变量和函数声明从它们在代码中出现的位置被“移动”
到了最上面。这个过程就叫作 **提升** 。

换句话说， **先有蛋（声明）后有鸡**（赋值）。

只有声明本身会被提升，而赋值或其他运行逻辑会留在原地。如果提升改变
了代码执行的顺序，会造成非常严重的破坏。

```
foo();
function foo() {
  console.log(a); // undefined
  var a = 2;
}
```
**foo 函数的声明（这个例子还包括实际函数的隐含值）被提升了** ，因此第一行中的调用可以正常执行。

另外值得注意的是，**每个作用域都会进行提升操作** 。尽管前面大部分的代码片段已经简化了（因为它们只包含全局作用域），而我们正在讨论的 foo(..) 函数自身也会在内部对 var a 进行提升（显然并不是提升到了整个程序的最上方）。因此这段代码实际上会被理解为下面的形式：

```js
function foo() {
  var a;
  console.log(a); // undefined
  a = 2;
}
foo();
```
可以看到，函数声明会被提升，但是函数表达式却不会被提升。

```js
foo(); // 不是 ReferenceError, 而是 TypeError!
var foo = function bar() {
  // ...
};
```
这段程序中的变量标识符 foo() 被提升并分配给所在作用域（在这里是全局作用域），因此 foo() 不会导致 ReferenceError。但是 foo 此时并没有赋值（如果它是一个函数声明而不是函数表达式，那么就会赋值）。foo() 由于 **对 undefined 值进行函数调用而导致非法操作** ，因此 **抛出 TypeError 异常** 。

同时也要记住，即使是具名的函数表达式，名称标识符在赋值之前也无法在所在作用域中使用：

```js
foo(); // TypeError
bar(); // ReferenceError
var foo = function bar() {
  // ...
};
```
这个代码片段经过提升后，实际上会被理解为以下形式：

```js
var foo;
foo(); // TypeError
bar(); // ReferenceError
foo = function() {
  var bar = ...self...
  // ...
}
```
#### 函数优先

函数声明和变量声明都会被提升。但是一个值得注意的细节（这个细节可以出现在有多个
“重复”声明的代码中）是函数会首先被提升，然后才是变量。

考虑以下代码：

```js
foo(); // 1
var foo;
function foo() {
  console.log(1);
}
foo = function() {
  console.log(2);
};
```
会输出 1 而不是 2 ！这个代码片段会被引擎理解为如下形式：

```js
function foo() {
  console.log(1);
}
foo(); // 1
foo = function() {
  console.log(2);
};
```
注意，var foo 尽管出现在 function foo()... 的声明之前，但它是重复的声明（因此被忽略了），因为函数声明会被提升到普通变量之前。

尽管重复的 var 声明会被忽略掉，但出现在后面的函数声明还是可以覆盖前面的。

```js
foo(); // 3
function foo() {
  console.log(1);
}
var foo = function() {
  console.log(2);
};
function foo() {
  console.log(3);
}
```

一个普通块内部的 **函数声明通常会被提升到所在作用域的顶部** ， **这个过程不会像下面的代码暗示的那样可以被条件判断所控制** ：

```
foo(); // "b"
var a = true;
if (a) {
  function foo() {
    console.log("a");
  }
} else {
  function foo() {
    console.log("b");
  }
}
```
但是需要注意这个行为并不可靠，在 JavaScript 未来的版本中有可能发生改变，因此 **应该尽可能避免在块内部声明函数** 。

#### 小结

我们习惯将 var a = 2; 看作一个声明，而实际上 JavaScript 引擎并不这么认为。它将 var a 和 a = 2 当作两个单独的声明， **第一个是编译阶段的任务** ，而 **第二个则是执行阶段的任务** 。

这意味着无论作用域中的声明出现在什么地方，都将在代码本身被执行前首先进行处理。可以将这个过程形象地想象成 **所有的声明（变量和函数）都会被“移动”到各自作用域的最顶端** ，这个过程被称为提升。

**声明本身会被提升** ，而包括函数表达式的赋值在内的 **赋值操作并不会提升** 。

要注意 **避免重复声明** ，特别是当普通的 var 声明和函数声明混合在一起的时候，否则会引起很多危险的问题！

### 作用域闭包

闭包是基于词法作用域书写代码时所产生的自然结果，不需要为了利用它们而有意识地创建闭包。

![闭包](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E9%97%AD%E5%8C%85.png)

**当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行。**

```js
function foo() {
  var a = 2;
  function bar() {
    console.log(a); // 2
  }
  bar();
}
foo();
```
这段代码看起来和嵌套作用域中的示例代码很相似。基于词法作用域的查找规则，函数bar() 可以访问外部作用域中的变量 a（这个例子中的是一个 RHS 引用查询）。

技术上来讲，也许是。但根据前面的定义，确切地说并不是。我认为最准确地用来解释**bar() 对 a 的引用的方法是词法作用域的查找规则** ，而 **这些规则只是闭包的一部分** 。（但却是非常重要的一部分！）

从纯学术的角度说，在上面的代码片段中，**函数 bar() 具有一个涵盖 foo() 作用域的闭包**（事实上，**涵盖了它能访问的所有作用域** ，比如全局作用域）。也可以认为 **bar() 被封闭在了 foo() 的作用域中** 。为什么呢？原因简单明了，因为 bar() 嵌套在 foo() 内部。

但是通过这种方式定义的闭包并不能直接进行观察，也无法明白在这个代码片段中闭包是如何工作的。我们可以 **很容易地理解词法作用域** ，而 **闭包则隐藏在代码之后的神秘阴影里** ，并不那么容易理解。

下面我们来看一段代码，清晰地展示了闭包：

```js
function foo() {
  var a = 2;
  function bar() {
    console.log(a);
  }
  return bar;
}
var baz = foo();
baz(); // 2 —— 朋友，这就是闭包的效果。
```
**函数 bar() 的词法作用域能够访问 foo() 的内部作用域** 。然后我们将 bar() 函数本身当作一个值类型进行传递。在这个例子中，我们将 bar 所引用的函数对象本身当作返回值。

在 foo() 执行后，其返回值（也就是内部的 bar() 函数）赋值给变量 baz 并调用 baz()，实际上只是通过不同的标识符引用调用了内部的函数 bar()。

bar() 显然可以被正常执行。但是在这个例子中，**它在自己定义的词法作用域以外的地方执行** 。

**在 foo() 执行后，通常会期待 foo() 的整个内部作用域都被销毁，因为我们知道引擎有垃圾回收器用来释放不再使用的内存空间** 。由于看上去 foo() 的内容不会再被使用，所以很自然地会考虑对其进行回收。

而 **闭包的“神奇”之处正是可以阻止这件事情的发生** 。事实上内部作用域依然存在，因此没有被回收。谁在使用这个内部作用域？原来是 **bar() 本身在使用** 。

 bar() 所声明的位置所赐，它拥有 **涵盖 foo() 内部作用域的闭包** ，使得该作用域能够一直存活，以供 bar() 在之后任何时间进行引用。

**bar() 依然持有对该作用域的引用** ，而 **这个引用** 就叫作 **闭包** 。

因此，在几微秒之后变量 baz 被实际调用（调用内部函数 bar），不出意料它可以访问定义时的词法作用域，因此它也可以如预期般访问变量 a。

这个函数在定义时的词法作用域以外的地方被调用。**闭包使得函数可以继续访问定义时的词法作用域** 。

当然，**无论使用何种方式对函数类型的值进行传递** ，**当函数在别处被调用时都可以观察到闭包** 。

```js
function foo() {
  var a = 2;
  function baz() {
    console.log(a); // 2
  }
  bar(baz);
}
function bar(fn) {
  fn(); // 妈妈快看呀，这就是闭包！
}
```
把内部函数 baz 传递给 bar，当调用这个内部函数时（现在叫作 fn），它涵盖的 foo() 内部作用域的闭包就可以观察到了，因为它能够访问 a。

传递函数当然也可以是间接的。

```js
var fn;
function foo() {
  var a = 2;
  function baz() {
    console.log(a);
  }
  fn = baz; // 将 baz 分配给全局变量
}
function bar() {
  fn(); // 妈妈快看呀，这就是闭包！
}
foo();
bar(); // 2
```
**无论通过何种手段将内部函数传递到所在的词法作用域以外，它都会持有对原始定义作用域的引用** ，**无论在何处执行这个函数都会使用闭包** 。

```js
function wait(message) {
  setTimeout(function timer() {
    console.log(message);
  },1000);
}
wait("Hello, closure!");
```
将一个内部函数（名为 timer）传递给 setTimeout(..)。**timer 具有涵盖 wait(..) 作用域的闭包** ，因此还保有对变量 message 的引用。

wait(..) 执行 1000 毫秒后，它的内部作用域并不会消失，timer 函数依然保有 wait(..)作用域的闭包。

深入到引擎的内部原理中，**内置的工具函数 setTimeout(..) 持有对一个参数的引用** ，这个参数也许叫作 fn 或者 func，或者其他类似的名字。**引擎会调用这个函数，在例子中就是内部的 timer 函数** ，而 **词法作用域在这个过程中保持完整** 。

这就是闭包。

或者，如果你很熟悉 jQuery（或者其他能说明这个问题的 JavaScript 框架），可以思考下面的代码：

```js
function setupBot(name, selector) {
  $(selector).click(function activator() {
    console.log("Activating: " + name);
  });
}
setupBot("Closure Bot 1", "#bot_1");
setupBot("Closure Bot 2", "#bot_2");
```
本质上无论何时何地，如果 **将函数（访问它们各自的词法作用域）当作第一级的值类型并到处传递** ，**你就会看到闭包在这些函数中的应用** 。在定时器、事件监听器、Ajax 请求、跨窗口通信、Web Workers 或者任何其他的异步（或者同步）任务中，只要使用了回调函数，实际上就是在使用闭包！

第 3 章介绍了 IIFE 模式。通常认为 IIFE 是典型的闭包例子，但根据先前对闭包的定义，我并不是很同意这个观点。

```js
var a = 2; 
(function IIFE() {
  console.log(a);
})();
```
虽然这段代码可以正常工作，但严格来讲它并不是闭包。为什么？因为函数（示例代码中
的 IIFE）并不是在它本身的词法作用域以外执行的。它在定义时所在的作用域中执行（而外部作用域，也就是全局作用域也持有 a）。**a 是通过普通的词法作用域查找而非闭包被发现的** 。

尽管技术上来讲，**闭包是发生在定义时的** ，但并不非常明显，就好像六祖慧能所说：“既非风动，亦非幡动，仁者心动耳。”。

尽管 IIFE 本身并不是观察闭包的恰当例子，但它的确创建了闭包，并且也是 **最常用来创建可以被封闭起来的闭包的工具** 。因此 IIFE 的确同闭包息息相关，即使 **本身并不会真的使用闭包** 。

#### 循环和闭包

要说明闭包，for 循环是最常见的例子。

```js
for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i);
  },i * 1000);
}
```
正常情况下，我们对这段代码行为的预期是分别输出数字 1~5，每秒一次，每次一个。

但实际上，这段代码在运行时会以每秒一次的频率输出五次 6。

首先解释 6 是从哪里来的。这个循环的终止条件是 i 不再 <=5。条件首次成立时 i 的值是6。因此，输出显示的是循环结束时 i 的最终值。

仔细想一下，这好像又是显而易见的，延迟函数的回调会在循环结束时才执行。事实上，
当定时器运行时即使每个迭代中执行的是 setTimeout(.., 0)，所有的回调函数依然是在循环结束后才会被执行，因此会每次输出一个 6 出来。

这里引伸出一个更深入的问题，代码中到底有什么缺陷导致它的行为同语义所暗示的不一
致呢？

缺陷是我们试图假设循环中的每个迭代在运行时都会给自己“捕获”一个 i 的副本。但是根据作用域的工作原理，实际情况是尽管循环中的五个函数是在各个迭代中分别定义的，但是 **它们都被封闭在一个共享的全局作用域中，因此实际上只有一个 i** 。

这样说的话，当然所有函数共享一个 i 的引用。循环结构让我们误以为背后还有更复杂的机制在起作用，但实际上没有。如果将延迟函数的回调重复定义五次，完全不使用循环，那它同这段代码是完全等价的。

下面回到正题。缺陷是什么？我们需要更多的闭包作用域，特别是在循环的过程中每个迭
代都需要一个闭包作用域。

第 3 章介绍过，IIFE 会通过 **声明并立即执行一个函数来创建作用域** 。

```js
for (var i = 1; i <= 5; i++) { 
  (function() {
    setTimeout(function timer() {
      console.log(i);
    },i * 1000);
  })();
}
```
我们现在显然拥有更多的词法作用域了。的确每个延迟函数都会将 IIFE 在每次迭代中创建的作用域封闭起来。

如果作用域是空的，那么仅仅将它们进行封闭是不够的。仔细看一下，我们的 IIFE 只是一个什么都没有的空作用域。它需要包含一点实质内容才能为我们所用。

它需要有自己的变量，用来在每个迭代中储存 i 的值：

```js
for (var i = 1; i <= 5; i++) { 
  (function() {
    var j = i;
    setTimeout(function timer() {
      console.log(j);
    },j * 1000);
  })();
}
```
行了！它能正常工作了！。

可以对这段代码进行一些改进：

```js
for (var i = 1; i <= 5; i++) { 
  (function(j) {
    setTimeout(function timer() {
      console.log(j);
    },j * 1000);
  })(i);
}
```
当然，这些 IIFE 也不过就是函数，因此我们可以将 i 传递进去，如果愿意的话可以将变量名定为 j，当然也可以还叫作 i。无论如何这段代码现在可以工作了。

在迭代内使用 IIFE 会为每个迭代都生成一个新的作用域，使得延迟函数的回调可以将新的作用域封闭在每个迭代内部，每个迭代中都会含有一个具有正确值的变量供我们访问。

#### 重返块作用域

本质上这是将一个块转换成一个可以被关闭的作用域。因此，下面这些看起来很酷的代码
就可以正常运行了：

```js
for (var i = 1; i <= 5; i++) {
  let j = i; // 是的，闭包的块作用域！
  setTimeout(function timer() {
    console.log(j);
  },j * 1000);
}
```
但是，这还不是全部！for 循环头部的 let 声明还会有一个特殊的行为。这个行为指出变量在循环过程中不止被声明一次，每次迭代都会声明。随后的每个迭代都会使用上一个迭代结束时的值来初始化这个变量。

```js
for (let i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i);
  },i * 1000);
}
```
#### 模块

```js
function foo() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log(something);
  }
  function doAnother() {
    console.log(another.join(" ! "));
  }
}
```
正如在这段代码中所看到的，这里并没有明显的闭包，只有两个私有数据变量 something和 another，以及 doSomething() 和 doAnother() 两个内部函数， **它们的词法作用域（而这就是闭包）也就是 foo() 的内部作用域** 。

接下来考虑以下代码：

```js
function CoolModule() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log(something);
  }
  function doAnother() {
    console.log(another.join(" ! "));
  }
  return {
    doSomething: doSomething,
    doAnother: doAnother
  };
}
var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```
这个模式在 JavaScript 中被称为模块。最常见的实现模块模式的方法通常被称为模块暴露，这里展示的是其变体。

首先，CoolModule() 只是一个函数，必须要通过调用它来创建一个模块实例。如果不执行外部函数，内部作用域和闭包都无法被创建。

其次，CoolModule() 返回一个用对象字面量语法 { key: value, ... } 来表示的对象。这个返回的对象中含有对内部函数而不是内部数据变量的引用。我们保持内部数据变量是隐藏且私有的状态。可以将这个对象类型的返回值看作本质上是模块的公共 API。

这个对象类型的返回值最终被赋值给外部的变量 foo，然后就可以通过它来访问 API 中的属性方法，比如 foo.doSomething()。

> 从模块中返回一个实际的对象并不是必须的，也可以直接返回一个内部函数。jQuery 就是一个很好的例子。jQuery 和 $ 标识符就是 jQuery 模块的公共 API，但它们本身都是函数（由于函数也是对象，它们本身也可以拥有属性）。

doSomething() 和 doAnother() 函数具有 **涵盖模块实例内部作用域的闭包** （通过调用 CoolModule() 实现）。当 **通过返回一个含有属性引用的对象的方式来将函数传递到词法作用域外部** 时，我们已经 **创造了可以观察和实践闭包的条件** 。

如果要更简单的描述，模块模式需要具备两个必要条件。

1. **必须有外部的封闭函数** ，**该函数必须至少被调用一次**（每次调用都会创建一个新的模块实例）。
2. **封闭函数必须返回至少一个内部函数** ，这样 **内部函数才能在私有作用域中形成闭包** ，并且 **可以访问或者修改私有的状态** 。

一个具有函数属性的对象本身并不是真正的模块。从方便观察的角度看，一个从函数调用所返回的，只有数据属性而没有闭包函数的对象并不是真正的模块。

上一个示例代码中有一个叫作 CoolModule() 的独立的模块创建器，可以被调用任意多次，每次调用都会创建一个新的模块实例。当只需要一个实例时，可以对这个模式进行简单的改进来实现单例模式：

```js
var foo = (function CoolModule() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log(something);
  }
  function doAnother() {
    console.log(another.join(" ! "));
  }
  return {
    doSomething: doSomething,
    doAnother: doAnother
  };
})();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```
我们将模块函数转换成了 IIFE（参见第 3 章），立即调用这个函数并将返回值直接赋值给单例的模块实例标识符 foo。

模块也是普通的函数，因此可以接受参数：

```js
function CoolModule(id) {
  function identify() {
    console.log(id);
  }
  return {
    identify: identify
  };
}
var foo1 = CoolModule("foo 1");
var foo2 = CoolModule("foo 2");
foo1.identify(); // "foo 1"
foo2.identify(); // "foo 2"
```
模块模式另一个简单但强大的变化用法是，命名将要作为公共 API 返回的对象：

```js
var foo = (function CoolModule(id) {
  function change() {
    // 修改公共 API
    publicAPI.identify = identify2;
  }
  function identify1() {
    console.log(id);
  }
  function identify2() {
    console.log(id.toUpperCase());
  }
  var publicAPI = {
    change: change,
    identify: identify1
  };
  return publicAPI;
})("foo module");
foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```
通过在模块实例的内部保留对公共 API 对象的内部引用，可以从内部对模块实例进行修改，包括添加或删除方法和属性，以及修改它们的值。

#### 现代的模块机制

大多数模块依赖加载器 / 管理器本质上都是将这种模块定义封装进一个友好的 API。这里并不会研究某个具体的库，为了宏观了解我会简单地介绍一些核心概念：

```js
var MyModules = (function Manager() {
  var modules = {};
  function define(name, deps, impl) {
    for (var i = 0; i < deps.length; i++) {
      deps[i] = modules[deps[i]];
    }
    modules[name] = impl.apply(impl, deps);
  }
  function get(name) {
    return modules[name];
  }
  return {
    define: define,
    get: get
  };
})();
```
这段代码的核心是 `modules[name] = impl.apply(impl, deps)`。为了模块的定义引入了包装函数（可以传入任何依赖），并且将返回值，也就是模块的 API，储存在一个根据名字来管理的模块列表中。

下面展示了如何使用它来定义模块：

```js
MyModules.define( "bar", [], function() {
function hello(who) {
return "Let me introduce: " + who;
}
return {
hello: hello
};
} );
MyModules.define( "foo", ["bar"], function(bar) {
var hungry = "hippo";
function awesome() {
console.log( bar.hello( hungry ).toUpperCase() );
}
return {
awesome: awesome
};
} );
var bar = MyModules.get( "bar" );
var foo = MyModules.get( "foo" );
console.log(
bar.hello( "hippo" )
); // Let me introduce: hippo
foo.awesome(); // LET ME INTRODUCE: HIPPO
```
"foo" 和 "bar" 模块都是通过一个返回公共 API 的函数来定义的。"foo" 甚至接受 "bar" 的示例作为依赖参数，并能相应地使用它。

为我们自己着想，应该多花一点时间来研究这些示例代码并完全理解闭包的作用吧。最重要的是要理解模块管理器没有任何特殊的“魔力”。它们符合前面列出的模块模式的两个特点：**为函数定义引入包装函数** ，并 **保证它的返回值和模块的 API 保持一致** 。

换句话说，模块就是模块，即使在它们外层加上一个友好的包装工具也不会发生任何变化。

#### 未来的模块机制

ES6 中为模块增加了一级语法支持。但通过模块系统进行加载时，ES6 会将文件当作独立的模块来处理。每个模块都可以导入其他模块或特定的 API 成员，同样也可以导出自己的API 成员。

ES6 的模块没有“行内”格式，必须被定义在独立的文件中（一个文件一个模块）。浏览器或引擎有一个默认的“模块加载器”（可以被重载，但这远超出了我们的讨论范围）可以在导入模块时异步地加载模块文件。

```js
bar.js
  function hello(who) {
    return "Let me introduce: " + who;
  }
  export hello;
foo.js
  // 仅从 "bar" 模块导入 hello()
  import hello from "bar";
  var hungry = "hippo";
  function awesome() {
    console.log(hello(hungry).toUpperCase());
  }
  export awesome;
baz.js
  // 导入完整的 "foo" 和 "bar" 模块
  module foo from "foo";
  module bar from "bar";
  console.log(bar.hello("rhino")); // Let me introduce: rhino
  foo.awesome(); // LET ME INTRODUCE: HIPPO
```

import 可以将一个模块中的一个或多个 API 导入到当前作用域中，并分别绑定在一个变量上（在我们的例子里是 hello）。module 会将整个模块的 API 导入并绑定到一个变量上（在我们的例子里是 foo 和 bar）。export 会将当前模块的一个标识符（变量、函数）导出为公共 API。这些操作可以在模块定义中根据需要使用任意多次。

模块文件中的内容会被当作好像包含在作用域闭包中一样来处理，就和前面介绍的函数闭包模块一样。

#### 小结

闭包就好像从 JavaScript 中分离出来的一个充满神秘色彩的未开化世界，只有最勇敢的人才能够到达那里。但实际上它 **只是一个标准** ，显然就是关于 **如何在函数作为值按需传递的词法环境中书写代码的** 。

**当函数可以记住并访问所在的词法作用域，即使函数是在当前词法作用域之外执行，这时就产生了闭包。**

如果没能认出闭包，也不了解它的工作原理，在使用它的过程中就很容易犯错，比如在循环中。但同时闭包也是一个非常强大的工具，可以用多种形式来实现模块等模式。

模块有两个主要特征：（1）为创建内部作用域而调用了一个包装函数；（2）包装函数的返回值必须至少包括一个对内部函数的引用，这样就会创建涵盖整个包装函数内部作用域的闭包。

现在我们会发现代码中到处都有闭包存在，并且我们能够识别闭包然后用它来做一些有用的事！

#### 思考

**闭包至少需要两层嵌套函数，外层函数用于创建词法作用域，内层函数用于引用和访问外层函数的词法作用域。**

- 闭包（也称词法闭包或函数闭包）是指一个函数或函数引用，与一个引用环境绑定在一起。这个引用环境是一个存储该函数每个非局部变量（也叫自由变量）的表。

- 闭包不同于一般的函数，它允许一个函数在立即执行词法作用域之外调用时，仍可访问非本地变量。

[理解闭包](https://www.imooc.com/video/6489)

### 动态作用域

![动态作用域](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E8%AF%8D%E6%B3%95%E4%BD%9C%E7%94%A8%E5%9F%9F-%E9%99%84%E5%BD%95.png)

JavaScript 中的作用域就是词法作用域（事实上大部分语言都是基于词法作用域的）。

实际上动态作用域是 JavaScript 另一个重要机制 this 的表亲。

词法作用域是一套关于引擎如何寻找变量以及会在何处找到变量的规则。**词法作用域最重要的特征是它的定义过程发生在代码的书写阶段**（假设你没有使用eval() 或 with）。

动态作用域似乎暗示有很好的理由让作用域作为一个在运行时就被动态确定的形式，而不
是在写代码时进行静态确定的形式，事实上也是这样的。我们通过示例代码来说明：

```js
function foo() {
  console.log(a); // 2
}
function bar() {
  var a = 3;
  foo();
}
var a = 2;
bar();
```
词法作用域让 foo() 中的 a 通过 RHS 引用到了全局作用域中的 a，因此会输出 2。

而动态作用域并不关心函数和作用域是如何声明以及在何处声明的，只关心它们 **从何处调用**。换句话说，**作用域链是基于调用栈的，而不是代码中的作用域嵌套** 。

因此，如果 JavaScript 具有动态作用域，理论上，下面代码中的 foo() 在执行时将会输出 3。

```js
function foo() {
  console.log(a); // 3（不是 2 ！）
}
function bar() {
  var a = 3;
  foo();
}
var a = 2;
bar();
```
需要明确的是，事实上 **JavaScript 并不具有动态作用域** 。它 **只有词法作用域** ，简单明了。但是 **this 机制某种程度上很像动态作用域** 。

主要区别：**词法作用域** 是在写代码或者说 **定义时确定的** ，而 **动态作用域是** 在 **运行时确定的** 。（this 也是！）**词法作用域关注函数在何处声明** ，而 **动态作用域关注函数从何处调用** 。

最后，this 关注函数如何调用，这就表明了 this 机制和动态作用域之间的关系多么紧密。

### 块作用域的替代方案

![块作用域的替代方案](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%9D%97%E4%BD%9C%E7%94%A8%E5%9F%9F-%E9%99%84%E5%BD%95.png)

从 ES3 发布以来，JavaScript 中就有了块作用域，而with 和 catch 分句就是块作用域的两个小例子。

但随着 ES6 中引入了 let，我们的代码终于有了创建完整、不受约束的块作用域的能力。块作用域在功能上和代码风格上都拥有很多激动人心的新特性。

考虑下面的代码：

```js
{
  let a = 2;
  console.log(a); // 2
}
console.log(a); // ReferenceError
```
这段代码在 ES6 环境中可以正常工作。但是在 ES6 之前的环境中如何才能实现这个效果？答案是使用 catch。

```js
try {
  throw 2;
} catch(a) {
  console.log(a); // 2
}
console.log(a); // ReferenceError
```

#### Traceur

Traceur 会将我们的代码片段转换成什么样子？你能猜到的！

```
{
  try {
    throw undefined;
  } catch(a) {
    a = 2;
    console.log(a);
  }
}
console.log(a);
```
#### 隐式和显式作用域

let 作用域或 let 声明（对比前面的 let 定义）

```js
let(a = 2) {
  console.log(a); // 2
}
console.log(a); // ReferenceError
```
同隐式地劫持一个已经存在的作用域不同，let 声明会创建一个显示的作用域并与其进行绑定。显式作用域不仅更加突出，在代码重构时也表现得更加健壮。在语法上，通过强制性地将所有变量声明提升到块的顶部来产生更简洁的代码。这样更容易判断变量是否属于某个作用域。

但是这里有一个小问题，let 声明并不包含在 ES6 中。官方的 Traceur 编译器也不接受这种形式的代码。

我们有两个选择，使用合法的 ES6 语法并且在代码规范性上做一些妥协。

```js
/*let*/
{
  let a = 2;
  console.log(a);
}
console.log(a); // ReferenceError
```
let-er 是一个构建时的代码转换器，但它唯一的作用就是找到 let 声明并对其进行转换。它不会处理包括 let 定义在内的任何其他代码。你可以安全地将 let-er 应用在 ES6 代码转换的第一步，如果有必要，接下来也可以把代码传递给 Traceur 等工具。

此外，let-er 还有一个设置项 --es6，开启它（默认是关闭的）会改变生成代码的种类。开启这个设置项时 let-er 会生成完全标准的 ES6 代码，而不会生成通过try/catch 进行 hack的 ES3 替代方案：

```js
{
  let a = 2;
  console.log(a);
}
console.log(a); // ReferenceError
```
因此你马上就可以在 ES6 之前的所有环境中使用 let-er，当你只关注 ES6 环境时，可以开启设置项，这样就会生成标准的 ES6 代码。

更重要的，你甚至可以使用尚未成为 ES 官方标准的、更加好用的显式 let 声明。

#### 性能

首先，try/catch 的性能的确很糟糕，但技术层面上没有合理的理由来说明 try/catch 必须这么慢，或者会一直慢下去。

其次，IIFE 和 try/catch 并不是完全等价的，因为如果将一段代码中的任意一部分拿出来用函数进行包裹，会改变这段代码的含义，其中的 this、return、break 和 contine 都会发生变化。IIFE 并不是一个普适的解决方案，它 **只适合在某些情况下进行手动操作** 。

### this词法

![this词法](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/this%E4%BD%9C%E7%94%A8%E5%9F%9F.png)

ES6 中有一个主题用非常重要的方式将 this同词法作用域联系起来了。

ES6 添加了一个特殊的语法形式用于函数声明，叫作箭头函数。它看起来是下面这样的：

```js
var foo = a = >{
  console.log(a);
};
foo(2); // 2
```
这里称作“胖箭头”的写法通常被当作单调乏味且冗长（挖苦）的 function 关键字的简写。

但是箭头函数除了让你在声明函数时少敲几次键盘以外，还有更重要的作用。简单来说，下面的代码有问题：

```js
var obj = {
  id: "awesome",
  cool: function coolFn() {
    console.log(this.id);
  }
};
var id = "not awesome"obj.cool(); // 酷
setTimeout(obj.cool, 100); // 不酷
```
问题在于 cool() 函数丢失了同 this 之间的绑定。解决这个问题有好几种办法，但最长用的就是 var self = this;

使用起来如下所示：

```js
var obj = {
  count: 0,
  cool: function coolFn() {
    var self = this;
    if (self.count < 1) {
      setTimeout(function timer() {
        self.count++;
        console.log("awesome?");
      },
      100);
    }
  }
};
obj.cool(); // 酷吧？
```
var self = this 这种解决方案圆满解决了理解和正确使用 this 绑定的问题，并且没有把问题过于复杂化，它使用的是我们非常熟悉的工具：词法作用域。**self 只是一个可以通过词法作用域和闭包进行引用的标识符** ，不关心 this 绑定的过程中发生了什么。

ES6 的一个初衷就是帮助人们减少重复的场景，事实上包括修复某些习惯用法的问题，this 就是其中一个。

ES6 中的箭头函数引入了一个叫作 **this 词法的行为** ：

```js
var obj = {
  count: 0,
  cool: function coolFn() {
    if (this.count < 1) {
      setTimeout(() = >{ // 箭头函数是什么鬼东西？
        this.count++;
        console.log("awesome?");
      },
      100);
    }
  }
};
obj.cool(); // 很酷吧 ?
```
简单来说，箭头函数在涉及 this 绑定时的行为和普通函数的行为完全不一致。它放弃了所有普通 this 绑定的规则，取而代之的是 **用当前的词法作用域覆盖了 this 本来的值** 。

因此，这个代码片段中的箭头函数并非是以某种不可预测的方式同所属的 this 进行了解绑定，而只是“继承”了 cool() 函数的 this 绑定（因此调用它并不会出错）。

这样除了可以少写一些代码，我认为箭头函数将程序员们经常犯的一个错误给标准化了，也就是混淆了 this 绑定规则和词法作用域规则。

另一个导致箭头函数不够理想的原因是它们是匿名而非具名的。具名函数比匿名函数更可取的原因参见第 3 章。

在我看来，解决这个“问题”的另一个更合适的办法是正确使用和包含 this 机制。

```js
var obj = {
  count: 0,
  cool: function coolFn() {
    if (this.count < 1) {
      setTimeout(function timer() {
        this.count++; // this 是安全的
        // 因为 bind(..)
        console.log("more awesome");
      }.bind(this), 100); // look, bind()!
    }
  }
};
obj.cool(); // 更酷了。
```

## this和对象原型

### 关于this

![关于this](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%85%B3%E4%BA%8Ethis.png)

this 关键字是 JavaScript 中最复杂的机制之一。它是一个很特别的关键字，被自动定义在所有函数的作用域中。

#### 为什么要用this

下面我们来解释一下为什么要使用 this：

```js
function identify() {
  return this.name.toUpperCase();
}
function speak() {
  var greeting = "Hello, I'm " + identify.call(this);
  console.log(greeting);
}
var me = {
  name: "Kyle"
}
var you = {
  name: "Reader"
};
identify.call(me); // KYLE
identify.call(you); // READER
speak.call(me); // Hello, 我是 KYLE
speak.call(you); // Hello, 我是 READER
```
这段代码可以在不同的上下文对象（me 和 you）中重复使用函数 identify() 和 speak()，不用针对每个对象编写不同版本的函数。

如果不使用 this，那就需要给 identify() 和 speak() 显式传入一个上下文对象。

```js
function identify(context) {
  return context.name.toUpperCase();
}
function speak(context) {
  var greeting = "Hello, I'm " + identify(context);
  console.log(greeting);
}
identify(you); // READER
speak(me); //hello, 我是 KYLE
```
然而，**this 提供了一种更优雅的方式来隐式“传递”一个对象引用** ，因此可以 **将 API 设计得更加简洁并且易于复用** 。

#### 误解

##### 指向自身

不过现在我们先来分析一下这个模式，让大家看到 this 并不像我们所想的那样指向函数
本身。
我们想要记录一下函数 foo 被调用的次数，思考一下下面的代码：

```js
function foo(num) {
  console.log("foo: " + num);
  // 记录 foo 被调用的次数
  this.count++;
}
foo.count = 0;
var i;
for (i = 0; i < 10; i++) {
  if (i > 5) {
    foo(i);
  }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9
// foo 被调用了多少次？
console.log(foo.count); // 0 -- WTF?
```
console.log 语句产生了 4 条输出，证明 foo(..) 确实被调用了 4 次，但是 foo.count 仍然是 0。显然从字面意思来理解 this 是错误的。

执行 foo.count = 0 时，的确向函数对象 foo 添加了一个属性 count。但是函数内部代码 this.count 中的 this 并不是指向那个函数对象，所以虽然属性名相同，根对象却并不相同，困惑随之产生。

遇到这样的问题时，许多开发者并不会深入思考为什么 this 的行为和预期的不一致，也不会试图回答那些很难解决但却非常重要的问题。他们只会回避这个问题并使用其他方法来达到目的，比如 **创建另一个带有 count 属性的对象** 。

```js
function foo(num) {
  console.log("foo: " + num);
  // 记录 foo 被调用的次数
  data.count++;
}
var data = {
  count: 0
};
var i;
for (i = 0; i < 10; i++) {
  if (i > 5) {
    foo(i);
  }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9
// foo 被调用了多少次？
console.log(data.count); // 4
```
无法理解this 的含义和工作原理——而是返回舒适区，使用了一种更熟悉的技术：词法作用域。

如果要从函数对象内部引用它自身，那只使用 this 是不够的。一般来说你需要通过一个指向函数对象的词法标识符（变量）来引用它。

思考一下下面这两个函数：

```js
function foo() {
foo.count = 4; // foo 指向它自身
}
setTimeout(function() {
  // 匿名（没有名字的）函数无法指向自身
},10);
```
第一个函数被称为具名函数，在它内部可以使用 foo 来引用自身。

但是在第二个例子中，传入 setTimeout(..) 的回调函数没有名称标识符（这种函数被称为匿名函数），因此无法从函数内部引用自身。

> 还有一种传统的但是现在已经被弃用和批判的用法，是使用 arguments.callee 来引用当前正在运行的函数对象。这是唯一一种可以从匿名函数对象内部引用自身的方法。然而，更好的方式是避免使用匿名函数，至少在需要自引用时使用具名函数（表达式）。arguments.callee 已经被弃用，不应该再使用它。

所以，对于我们的例子来说，另一种解决方法是 **使用 foo 标识符替代 this 来引用函数对象** ：

```js
function foo(num) {
  console.log("foo: " + num);
  // 记录 foo 被调用的次数
  foo.count++;
}
foo.count = 0
var i;
for (i = 0; i < 10; i++) {
  if (i > 5) {
    foo(i);
  }
}关于this｜79
// foo: 6
// foo: 7
// foo: 8
// foo: 9
// foo 被调用了多少次？
console.log(foo.count); // 4
```
然而，这种方法同样回避了 this 的问题，并且完全依赖于变量 foo 的词法作用域。

另一种方法是 **强制 this 指向 foo 函数对象** ：

```js
function foo(num) {
  console.log("foo: " + num);
  // 记录 foo 被调用的次数
  // 注意，在当前的调用方式下（参见下方代码），this 确实指向 foo
  this.count++;
}
foo.count = 0;
var i;
for (i = 0; i < 10; i++) {
  if (i > 5) {
    // 使用 call(..) 可以确保 this 指向函数对象 foo 本身
    foo.call(foo, i);
  }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9
// foo 被调用了多少次？
console.log(foo.count); // 4
```

##### 它的作用域

第二种常见的误解是，this 指向函数的作用域。

需要明确的是，this 在任何情况下都不指向函数的词法作用域。在 JavaScript 内部，作用域确实和对象类似，可见的标识符都是它的属性。但是作用域“对象”无法通过 JavaScript 代码访问，它存在于 JavaScript 引擎内部。

思考一下下面的代码，它试图（但是没有成功）跨越边界，使用 this 来隐式引用函数的词法作用域：

```js
function foo() {
  var a = 2;
  this.bar();
}
function bar() {
  console.log(this.a);
}
foo(); // ReferenceError: a is not defined
```
首先，这段代码试图通过 this.bar() 来引用 bar() 函数。这是绝对不可能成功的，我们之后会解释原因。调用 bar() 最自然的方法是省略前面的 this，直接使用词法引用标识符。

此外，编写这段代码的开发者还试图使用 this 联通 foo() 和 bar() 的词法作用域，从而让bar() 可以访问 foo() 作用域里的变量 a。这是不可能实现的， **你不能使用 this 来引用一个词法作用域内部的东西** 。

#### this到底是什么

过 this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调
用时的各种条件。this 的绑定和函数声明的位置没有任何关系，**只取决于函数的调用方式** 。

当一个函数被调用时，会创建一个活动记录（有时候也称为执行上下文）。这个记录会包
含函数在哪里被调用（调用栈）、函数的调用方法、传入的参数等信息。**this 就是记录的其中一个属性** ，**会在函数执行的过程中用到** 。

#### 小结

学习 this 的第一步是明白 this 既不指向函数自身也不指向函数的词法作用域，你也许被这样的解释误导过，但其实它们都是错误的。

this 实际上是在函数被调用时发生的绑定，它指向什么 **完全取决于函数在哪里被调用** 。

### this全面解析

了每个函数的 this 是在调用时被绑定的，完全取决于函数的调用位置（也就是函数的调用方法）。

![this全面解析](http://p9myzkds7.bkt.clouddn.com/this%E5%85%A8%E9%9D%A2%E8%A7%A3%E6%9E%90.png)

#### 调用位置

调用位置：调用位置就是函数在代码中被调用的位置（而不是声明的位置）。

寻找调用位置就是寻找“函数被调用的位置”，最重要的是要分析调用栈（就是为了到达当前执行位置所调用的所有函数）。我们关心的 **调用位置** 就在 **当前正在执行的函数的前一个调用中** 。

下面我们来看看到底什么是调用栈和调用位置：

```js
function baz() {
  // 当前调用栈是：baz
  // 因此，当前调用位置是全局作用域
  console.log("baz");
  bar(); // <-- bar 的调用位置
}
function bar() {
  // 当前调用栈是 baz -> bar
  // 因此，当前调用位置在 baz 中
  console.log("bar");
  foo(); // <-- foo 的调用位置
}
function foo() {
  // 当前调用栈是 baz -> bar -> foo
  // 因此，当前调用位置在 bar 中
  console.log("foo");
}
baz(); // <-- baz 的调用位置
```
> 你可以把调用栈想象成一个函数调用链，就像我们在前面代码段的注释中所写的一样。但是这种方法非常麻烦并且容易出错。另一个查看调用栈的方法是使用浏览器的调试工具。

#### 绑定规则

##### 默认绑定

首先要介绍的是最常用的函数调用类型：独立函数调用。可以把这条规则看作是无法应用其他规则时的默认规则。

思考一下下面的代码：

```js
function foo() {
  console.log(this.a);
}
var a = 2;
foo(); // 2
```
你应该注意到的第一件事是，声明在全局作用域中的变量（比如 var a = 2）就是全局对象的一个同名属性。它们本质上就是同一个东西，并不是通过复制得到的，就像一个硬币的两面一样。

接下来我们可以看到当调用 foo() 时，this.a 被解析成了全局变量 a。为什么？因为在本例中，函数调用时应用了 this 的默认绑定，因此 this 指向全局对象。

那么我们怎么知道这里应用了默认绑定呢？可以通过分析调用位置来看看 foo() 是如何调用的。在代码中，foo() 是 **直接使用不带任何修饰的函数引用进行调用的** ，因此只能使用默认绑定，无法应用其他规则。

如果使用严格模式（strict mode），那么全局对象将无法使用默认绑定，因此 this 会绑定到 undefined：

```js
function foo() {
  "use strict";
  console.log(this.a);
}
var a = 2;
foo(); // TypeError: this is undefined
```
这里有一个微妙但是非常重要的细节，虽然 this 的绑定规则完全取决于调用位置，但是只有 foo() 运行在非 strict mode 下时，默认绑定才能绑定到全局对象；**严格模式下与 foo()的调用位置无关** ：

```js
function foo() {
  console.log(this.a);
}
var a = 2; (function() {
  "use strict";
  foo(); // 2
})();
```
##### 隐式绑定

另一条需要考虑的规则是 **调用位置是否有上下文对象** ，或者说 **是否被某个对象拥有或者包含** ，不过这种说法可能会造成一些误导。

思考下面的代码：

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo
};
obj.foo(); // 2
```

首先需要注意的是 foo() 的声明方式，及其之后是如何被当作引用属性添加到 obj 中的。

但是无论是直接在 obj 中定义还是先定义再添加为引用属性，这个函数严格来说都不属于 obj 对象。

然而，调用位置会使用 obj 上下文来引用函数，因此你可以说函数被调用时 obj 对象“拥有”或者“包含”它。

无论你如何称呼这个模式，当 foo() 被调用时，它的落脚点确实指向 obj 对象。当函数引用有上下文对象时，隐式绑定规则会把函数调用中的 this 绑定到这个上下文对象。因为调用 foo() 时 this 被绑定到 obj，因此 this.a 和 obj.a 是一样的。

**对象属性引用链中只有最顶层或者说最后一层会影响调用位置** 。举例来说：

```js
function foo() {
  console.log(this.a);
}
var obj2 = {
  a: 42,
  foo: foo
};
var obj1 = {
  a: 2,
  obj2: obj2
};
obj1.obj2.foo(); // 42
```
###### 隐式丢失

一个最常见的 this 绑定问题就是 **被隐式绑定的函数会丢失绑定对象** ，也就是说它会 **应用默认绑定** ，从而把 this 绑定到全局对象或者 undefined 上，取决于是否是严格模式。

思考下面的代码：

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo
};
var bar = obj.foo; // 函数别名！
var a = "oops, global"; // a 是全局对象的属性
bar(); // "oops, global"
```
虽然 bar 是 obj.foo 的一个引用，但是实际上，它 **引用的是 foo 函数本身** ，因此此时的 bar() 其实是一个不带任何修饰的函数调用，因此应用了默认绑定。

一种更微妙、更常见并且更出乎意料的情况发生在传入回调函数时：

```js
function foo() {
  console.log(this.a);
}
function doFoo(fn) {
  // fn 其实引用的是 foo
  fn(); // <-- 调用位置！
}
var obj = {
  a: 2,
  foo: foo
};
var a = "oops, global"; // a 是全局对象的属性
doFoo(obj.foo); // "oops, global"
```
参数传递其实就是一种隐式赋值，因此我们传入函数时也会被隐式赋值，所以结果和上一个例子一样。

如果把函数传入语言内置的函数而不是传入你自己声明的函数，会发生什么呢？结果是一样的，没有区别：

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo
};
var a = "oops, global"; // a 是全局对象的属性
setTimeout(obj.foo, 100); // "oops, global"
```
JavaScript 环境中内置的 setTimeout() 函数实现和下面的伪代码类似：

```js
function setTimeout(fn, delay) {
  // 等待 delay 毫秒
  fn(); // <-- 调用位置！
}
```
就像我们看到的那样，回调函数丢失 this 绑定是非常常见的。除此之外，还有一种情况 this 的行为会出乎我们意料：调用回调函数的函数可能会修改 this。在一些流行的JavaScript 库中事件处理器常会把回调函数的 this 强制绑定到触发事件的 DOM 元上。这在一些情况下可能很有用，但是有时它可能会让你感到非常郁闷。遗憾的是，这些工具通常无法选择是否启用这个行为。

##### 显式绑定

在分析隐式绑定时，我们必须在一个对象内部包含一个指向函数的属性，并通过这个属性间接引用函数，从而把 this 间接（隐式）绑定到这个对象上。

JavaScript 中的“所有”函数都有一些有用的特性（这和它们的 [[ 原型 ]] 有关——之后我们会详细介绍原型），可以用来解决这个问题。具体点说，可以使用函数的 **call(..) 和 apply(..) 方法** 。严格来说，JavaScript 的宿主环境有时会提供一些非常特殊的函数，它们并没有这两个方法。但是这样的函数非常罕见，JavaScript 提供的绝大多数函数以及你自己创建的所有函数都可以使用 call(..) 和 apply(..) 方法。

这两个方法是如何工作的呢？它们的第一个参数是一个对象，它们会把这个对象绑定到this，接着在调用函数时指定这个 this。因为你可以直接指定 this 的绑定对象，因此我们称之为显式绑定。

通过 foo.call(..)，我们可以在调用 foo 时强制把它的 this 绑定到 obj 上。

如果你传入了一个 **原始值**（字符串类型、布尔类型或者数字类型）来当作 this 的绑定对象，这个原始值会被转换成它的对象形式（也就是 new String(..)、new Boolean(..) 或者 new Number(..)）。这通常被称为“**装箱**”。

可惜，显式绑定仍然无法解决我们之前提出的丢失绑定问题。

###### 硬绑定

思考下面的代码：

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2
};
var bar = function() {
  foo.call(obj);
};
bar(); // 2
setTimeout(bar, 100); // 2
// 硬绑定的 bar 不可能再修改它的 this
bar.call(window); // 2
```
我们来看看这个变种到底是怎样工作的。我们创建了函数 bar()，并在它的内部手动调用了 foo.call(obj)，因此强制把 foo 的 this 绑定到了 obj。无论之后如何调用函数 bar，它总会手动在 obj 上调用 foo。这种绑定是一种显式的强制绑定，因此我们称之为硬绑定。

硬绑定的典型应用场景就是创建一个包裹函数，传入所有的参数并返回接收到的所有值：

```js
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
var obj = {
  a: 2
};
var bar = function() {
  return foo.apply(obj, arguments);
};
var b = bar(3); // 2 3
console.log(b); // 5
```
另一种使用方法是创建一个 i 可以重复使用的辅助函数：

```js
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
// 简单的辅助绑定函数
function bind(fn, obj) {
  return function() {
    return fn.apply(obj, arguments);
  };
}
var obj = {
  a: 2
};
var bar = bind(foo, obj);
var b = bar(3); // 2 3
console.log(b); // 5
```

由于硬绑定是一种非常常用的模式，所以在 ES5 中提供了内置的方法Function.prototype.bind，它的用法如下：

```js
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
var obj = {
  a: 2
};
var bar = foo.bind(obj);
var b = bar(3); // 2 3
console.log(b); // 5
```
bind(..) 会返回一个硬编码的新函数，它会把参数设置为 this 的上下文并调用原始函数。

###### API调用的“上下文”

第三方库的许多函数，以及 JavaScript 语言和宿主环境中许多新的内置函数，都提供了一个可选的参数，通常被称为“上下文”（context），其作用和 bind(..) 一样，确保你的回调函数使用指定的 this。

举例来说：

```js
function foo(el) {
  console.log(el, this.id);
}
var obj = {
  id: "awesome"
};
// 调用 foo(..) 时把 this 绑定到 obj
[1, 2, 3].forEach(foo, obj);
// 1 awesome 2 awesome 3 awesome
```
这些函数实际上就是通过 call(..) 或者 apply(..) 实现了显式绑定，这样你可以少些一些代码。

##### new绑定

在传统的面向类的语言中，“构造函数”是类中的一些特殊方法，使用 new 初始化类时会调用类中的构造函数。通常的形式是这样的：

```
something = new MyClass(..);
```
JavaScript 也有一个 new 操作符，使用方法看起来也和那些面向类的语言一样，绝大多数开发者都认为 JavaScript 中 new 的机制也和那些语言一样。然而，JavaScript 中 new 的机制实际上和面向类的语言完全不同。

首先我们重新定义一下 JavaScript 中的“构造函数”。在 JavaScript 中，构造函数只是一些使用 new 操作符时被调用的函数。它们并不会属于某个类，也不会实例化一个类。实际上，它们甚至都不能说是一种特殊的函数类型，它们 **只是被 new 操作符调用的普通函数而已** 。

举例来说，思考一下 Number(..) 作为构造函数时的行为，ES5.1 中这样描述它：

15.7.2 Number 构造函数
当 Number 在 new 表达式中被调用时，它是一个构造函数：它会初始化新创建的
对象。

所以，包括内置对象函数（比如 Number(..)，详情请查看第 3 章）在内的所有函数都可以用 new 来调用，这种函数调用被称为构造函数调用。这里有一个重要但是非常细微的区别：**实际上并不存在所谓的“构造函数”**，**只有对于函数的“构造调用”** 。

使用 new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行 [[ 原型 ]] 连接。
3. 这个新对象会绑定到函数调用的 this。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象。

思考下面的代码：

```js
function foo(a) {
  this.a = a;
}
var bar = new foo(2);
console.log(bar.a); // 2
```
使用 new 来调用 foo(..) 时，我们会构造一个新对象并把它绑定到 foo(..) 调用中的 this上。new 是最后一种可以影响函数调用时 this 绑定行为的方法，我们称之为 new 绑定。

#### 优先级

实际上，ES5 中内置的 Function.prototype.bind(..) 更加复杂。下面是 MDN 提供的一种bind(..) 实现，为了方便阅读我们对代码进行了排版：

```js
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if (typeof this !== "function") {
      // 与 ECMAScript 5 最接近的
      // 内部 IsCallable 函数
      throw new TypeError("Function.prototype.bind - what is trying " + "to be bound is not callable");
    }
    var aArgs = Array.prototype.slice.call(arguments, 1),
    fToBind = this,
    fNOP = function() {},
    fBound = function() {
      return fToBind.apply((this instanceof fNOP && oThis ? this: oThis), aArgs.concat(Array.prototype.slice.call(arguments));
    };
    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
  };
}
```
> 这种 bind(..) 是一种 polyfill 代码（polyfill 就是我们常说的刮墙用的腻子，polyfill 代码主要用于旧浏览器的兼容，比如说在旧的浏览器中并没有内置 bind 函数，因此可以使用 polyfill 代码在旧浏览器中实现新的功能），对于 new 使用的硬绑定函数来说，这段 polyfill 代码和 ES5 内置的bind(..) 函数并不完全相同（后面会介绍为什么要在 new 中使用硬绑定函数）。由于 polyfill 并不是内置函数，所以无法创建一个不包含 .prototype的函数，因此会具有一些副作用。如果你要在 new 中使用硬绑定函数并且依赖 polyfill 代码的话，一定要非常小心。

下面是 new 修改 this 的相关代码：

```js
this instanceof fNOP && oThis ? this: oThis
// ... 以及：
fNOP.prototype = this.prototype;
fBound.prototype = new fNOP();
```
我们并不会详细解释这段代码做了什么（这非常复杂并且不在我们的讨论范围之内），不过简单来说，这段代码会判断硬绑定函数是否是被 new 调用，如果是的话就会使用新创建的 this 替换硬绑定的 this。

那么，为什么要在 new 中使用硬绑定函数呢？直接使用普通函数不是更简单吗？之所以要在 new 中使用硬绑定函数，主要目的是预先设置函数的一些参数，这样在使用new 进行初始化时就可以只传入其余的参数。bind(..) 的功能之一就是可以把除了第一个参数（第一个参数用于绑定 this）之外的其他参数都传给下层的函数（这种技术称为“部分应用”，是“柯里化”的一种）。举例来说：

```js
function foo(p1, p2) {
  this.val = p1 + p2;
}
// 之所以使用 null 是因为在本例中我们并不关心硬绑定的 this 是什么
// 反正使用 new 时 this 会被修改
var bar = foo.bind(null, "p1");
var baz = new bar("p2");
baz.val; // p1p2
```
##### 判断this

现在我们可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的
顺序来进行判断：

1. 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。

  ```js
  var bar = new foo()
  ```
2. 函数是否通过 call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是指定的对象。

  ```js
  var bar = foo.call(obj2)
  ```
3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。

  ```js
  var bar = obj1.foo()
  ```
4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定到全局对象。

  ```js
  var bar = foo()
  ```
#### 绑定例外

##### 被忽略的this

如果你把 null 或者 undefined 作为 this 的绑定对象传入 call、apply 或者 bind，这些值在调用时会被忽略，实际应用的是默认绑定规则：

```js
function foo() {
  console.log(this.a);
}
var a = 2;
foo.call(null); // 2
```
那么什么情况下你会传入 null 呢？

一种非常常见的做法是使用 apply(..) 来“展开”一个数组，并当作参数传入一个函数。

类似地，bind(..) 可以对参数进行柯里化（预先设置一些参数），这种方法有时非常有用：

```js
function foo(a, b) {
  console.log("a:" + a + ", b:" + b);
}
// 把数组“展开”成参数
foo.apply(null, [2, 3]); // a:2, b:3
// 使用 bind(..) 进行柯里化
var bar = foo.bind(null, 2);
bar(3); // a:2, b:3
```
这两种方法都需要传入一个参数当作 this 的绑定对象。如果函数并不关心 this 的话，你仍然需要传入一个占位值，这时 null 可能是一个不错的选择，就像代码所示的那样。

> 尽管本书中未提到，但在 ES6 中，可以用 ... 操作符代替 apply(..) 来“展开”数组，foo(...[1,2]) 和 foo(1,2) 是一样的，这样可以避免不必要的this 绑定。可惜，在 ES6 中没有柯里化的相关语法，因此还是需要使用bind(..)。

##### 更安全的this

一种“更安全”的做法是传入一个特殊的对象，把 this 绑定到这个对象不会对你的程序产生任何副作用。就像网络（以及军队）一样，我们可以创建个“DMZ”（demilitarized zone，非军事区）对象——它就是一个空的非委托的对象。

如果我们在忽略 this 绑定时总是传入一个 DMZ 对象，那就什么都不用担心了，因为任何对于 this 的使用都会被限制在这个空对象中，不会对全局对象产生任何影响

由于这个对象完全是一个空对象，我自己喜欢用变量名 ø（这是数学中表示空集合符号的小写形式）来表示它。在大多数键盘（比如说 Mac 的 US 布局键盘）上都可以使用⌥ +o（Option-o）来打出这个符号。有些系统允许你为特殊符号设定快捷键。如果你不喜欢ø 符号或者你的键盘不太容易打出这个符号，那你可以换一个喜欢的名字来称呼它。

无论你叫它什么，在 JavaScript 中创建一个空对象最简单的方法都是Object.create(null)。Object.create(null) 和 {} 很 像， 但 是 并 不 会 创 建 Object.prototype 这个委托，所以它比 {}“更空。

```js
function foo(a, b) {
  console.log("a:" + a + ", b:" + b);
}
// 我们的 DMZ 空对象
varø = Object.create(null);
// 把数组展开成参数
foo.apply(ø, [2, 3]); // a:2, b:3
// 使用 bind(..) 进行柯里化
var bar = foo.bind(ø, 2);
bar(3); // a:2, b:3
```
使用变量名 ø 不仅让函数变得更加“安全”，而且可以提高代码的可读性，因为 ø 表示“我希望 this 是空”，这比 null 的含义更清楚。不过再说一遍，你可以用任何喜欢的名字来命名 DMZ 对象。

##### 间接引用

另一个需要注意的是，你有可能（有意或者无意地）创建一个函数的“间接引用”，在这种情况下，调用这个函数会应用默认绑定规则。

间接引用最容易在赋值时发生：

```js
function foo() {
  console.log(this.a);
}
var a = 2;
var o = {
  a: 3,
  foo: foo
};
var p = {
  a: 4
};
o.foo(); // 3
(p.foo = o.foo)(); // 2
```
赋值表达式 p.foo = o.foo 的返回值是目标函数的引用，因此 **调用位置是 foo()** 而不是p.foo() 或者 o.foo()。根据我们之前说过的，这里会应用默认绑定。

注意：对于默认绑定来说，决定 this 绑定对象的并不是调用位置是否处于严格模式，而是函数体是否处于严格模式。如果函数体处于严格模式，this 会被绑定到 undefined，否则 this 会被绑定到全局对象。

##### 软绑定

如果可以给默认绑定指定一个全局对象和 undefined 以外的值，那就可以实现和硬绑定相同的效果，同时保留隐式绑定或者显式绑定修改 this 的能力。

可以通过一种被称为软绑定的方法来实现我们想要的效果：

```js
if (!Function.prototype.softBind) {
  Function.prototype.softBind = function(obj) {
    var fn = this;
    // 捕获所有 curried 参数
    var curried = [].slice.call(arguments, 1);
    var bound = function() {
      return fn.apply((!this || this === (window || global)) ? obj: this curried.concat.apply(curried, arguments));
    };
    bound.prototype = Object.create(fn.prototype);
    return bound;
  };
}
```
除了软绑定之外，softBind(..) 的其他原理和 ES5 内置的 bind(..) 类似。它会对指定的函数进行封装，首先检查调用时的 this，如果 this 绑定到全局对象或者undefined，那就把指定的默认对象 obj 绑定到 this，否则不会修改 this。此外，这段代码还支持可选的柯里化。

#### this词法

箭头函数并不是使用 function 关键字定义的，而是使用被称为“胖箭头”的操作符 => 定义的。箭头函数不使用 this 的四种标准规则，而是根据外层（函数或者全局）作用域来决定 this。

我们来看看箭头函数的词法作用域：

```js
function foo() {
  // 返回一个箭头函数
  return (a) = >{
    //this 继承自 foo()
    console.log(this.a);
  };
}
var obj1 = {
  a: 2
};
var obj2 = {
  a: 3 100｜第2章
};
var bar = foo.call(obj1);
bar.call(obj2); // 2, 不是 3 ！
```
foo() 内部创建的箭头函数会捕获调用时 foo() 的 this。由于 foo() 的 this 绑定到 obj1，bar（引用箭头函数）的 this 也会绑定到 obj1，箭头函数的绑定无法被修改。（new 也不行！）

箭头函数最常用于回调函数中，例如事件处理器或者定时器：

```js
function foo() {
  setTimeout(() = >{
    // 这里的 this 在此法上继承自 foo()
    console.log(this.a);
  },
  100);
}
var obj = {
  a: 2
};
foo.call(obj); // 2
```

箭头函数可以像 bind(..) 一样确保函数的 this 被绑定到指定对象，此外，其重要性还体现在它用更常见的词法作用域取代了传统的 this 机制。实际上，在 ES6 之前我们就已经在使用一种几乎和箭头函数完全一样的模式。

```js
function foo() {
  var self = this; // lexical capture of this
  setTimeout(function() {
    console.log(self.a);
  },
  100);
}
var obj = {
  a: 2
};
foo.call(obj); // 2
```
虽然 self = this 和箭头函数看起来都可以取代 bind(..)，但是从本质上来说，它们想替代的是 this 机制。

如果你经常编写 this 风格的代码，但是绝大部分时候都会使用 self = this 或者箭头函数来否定 this 机制，那你或许应当：

1. 只使用词法作用域并完全抛弃错误 this 风格的代码；
2. 完全采用 this 风格，在必要时使用 bind(..)，尽量避免使用 self = this 和箭头函数。

#### 小结

如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断 this 的绑定对象。

1. 由 new 调用？绑定到新创建的对象。
2. 由 call 或者 apply（或者 bind）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到 undefined，否则绑定到全局对象。

一定要注意，有些调用可能在无意中使用默认绑定规则。如果想“更安全”地忽略 this 绑定，你可以使用一个 DMZ 对象，比如 ø = Object.create(null)，以保护全局对象。

ES6 中的箭头函数并不会使用四条标准的绑定规则，而是根据当前的词法作用域来决定this，具体来说，箭头函数会继承外层函数调用的 this 绑定（无论 this 绑定到什么）。这其实和 ES6 之前代码中的 self = this 机制一样。

### 对象

![语法、类型和内容](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%AF%B9%E8%B1%A11.png)

#### 语法

对象可以通过两种形式定义：**声明（文字）形式** 和 **构造形式** 。

对象的文字语法大概是这样：

```js
var myObj = {
  key: value
  // ...
};
```
构造形式大概是这样：

```js
var myObj = new Object();
myObj.key = value;
```
构造形式和文字形式生成的对象是一样的。唯一的区别是，在文字声明中你可以添加多个
键 / 值对，但是在构造形式中你必须逐个添加属性。

#### 类型

对象是 JavaScript 的基础。在 JavaScript 中一共有六种主要类型（术语是“语言类型”）：

- string
- number
- boolean
- null
- undefined
- object

注意，简单基本类型（string、boolean、number、null 和 undefined）本身并不是对象。null 有时会被当作一种对象类型，但是这其实只是语言本身的一个 bug，即对 null 执行 typeof null 时会返回字符串 "object"。实际上，null 本身是基本类型。

> 原理是这样的，不同的对象在底层都表示为二进制，在 JavaScript 中二进制前三位都为 0 的话会被判断为 object 类型，null 的二进制表示是全 0，自然前三位也是 0，所以执行 typeof 时会返回“object”。

实际上，JavaScript 中有许多特殊的对象子类型，我们可以称之为复杂基本类型。

**函数就是对象的一个子类型**（从技术角度来说就是“可调用的对象”）。JavaScript 中的函数是“一等公民”，因为它们 **本质上和普通的对象一样**（只是可以调用），所以 **可以像操作其他对象一样操作函数**（比如当作另一个函数的参数）。

**数组也是对象的一种类型** ，具备一些额外的行为。数组中内容的组织方式比一般的对象要稍微复杂一些。

##### 内置对象

JavaScript 中还有一些对象子类型，通常被称为内置对象。有些内置对象的名字看起来和简单基础类型一样，不过实际上它们的关系更复杂，我们稍后会详细介绍。

- String
- Number
- Boolean
- Object
- Function
- Array
- Date
- RegExp
- Error

是在 JavaScript 中，它们实际上只是一些内置函数。这些内置函数可以当作构造函数
（由 new 产生的函数调用——参见第 2 章）来使用，从而可以构造一个对应子类型的新对象。举例来说：

```js
var strPrimitive = "I am a string";
typeof strPrimitive; // "string"
strPrimitive instanceof String; // false
var strObject = new String("I am a string");
typeof strObject; // "object"
strObject instanceof String; // true

// 检查 sub-type 对象
Object.prototype.toString.call(strObject); // [object String]
```

在必要时语言会 **自动把字符串字面量转换成一个 String 对象** ，也就是说你并不需要显式创建一个对象。

思考下面的代码：

```js
var strPrimitive = "I am a string";
console.log( strPrimitive.length ); // 13
console.log( strPrimitive.charAt( 3 ) ); // "m"
```
使用以上两种方法，我们都可以直接在字符串字面量上访问属性或者方法，之所以可以这样做，是因为引擎自动把字面量转换成 String 对象，所以可以访问属性和方法。

同样的事也会发生在数值字面量上，如果使用类似 42.359.toFixed(2) 的方法，引擎会把 42 转换成 new Number(42)。对于布尔字面量来说也是如此。

null 和 undefined 没有对应的构造形式，它们只有文字形式。相反，Date 只有构造，没有文字形式。

对于 Object、Array、Function 和 RegExp（正则表达式）来说，无论使用文字形式还是构造形式，它们都是对象，不是字面量。在某些情况下，相比用文字形式创建对象，构造形式可以提供一些额外选项。由于这两种形式都可以创建对象，所以我们首选更简单的文字形式。建议只在需要那些额外选项时使用构造形式。

Error 对象很少在代码中显式创建，一般是在抛出异常时被自动创建。也可以使用 new Error(..) 这种构造形式来创建，不过一般来说用不着。

#### 内容

存储在对象容器内部的是这些属性的名称，它们就像指针（从技术角度来说就是引用）一样，指向这些值真正的存储位置。

思考下面的代码：

```js
var myObject = {
  a: 2
};
myObject.a; // 2
myObject["a"]; // 2
```
如果要访问 myObject 中 a 位置上的值，我们需要使用 . 操作符或者 [] 操作符。.a 语法通常被称为“ **属性访问** ”，["a"] 语法通常被称为“ **键访问** ”。实际上它们访问的是同一个位置，并且会返回相同的值 2，所以这两个术语是可以互换的。

这两种语法的主要区别在于 . 操作符要求属性名满足标识符的命名规范，而 [".."] 语法可以 **接受任意 UTF-8/Unicode 字符串作为属性名** 。举例来说，如果要引用名称为 "SuperFun!"的属性，那就必须使用 ["Super-Fun!"] 语法访问，因为 Super-Fun! 并不是一个有效的标识符属性名。

此外，由于 [".."] 语法使用字符串来访问属性，所以 **可以在程序中构造这个字符串** ，比如说：

```js
var myObject = {
  a: 2
};
var idx;
if (wantA) {
  idx = "a";
}
// 之后
console.log(myObject[idx]); // 2
```
在对象中，**属性名永远都是字符串** 。如果你使用 string（字面量）以外的其他值作为属性名，那它 **首先会被转换为一个字符串** 。即使是数字也不例外，虽然 **在数组下标中使用的的确是数字** ，但是 **在对象属性名中数字会被转换成字符串** ，所以当心不要搞混对象和数组中数字的用法：

```js
var myObject = { };
myObject[true] = "foo";
myObject[3] = "bar";
myObject[myObject] = "baz";
myObject["true"]; // "foo"
myObject["3"]; // "bar"
myObject["[object Object]"]; // "baz"
```
##### 可计算属性名

ES6 增加了 **可计算属性名** ，**可以在文字形式中使用 [] 包裹一个表达式来当作属性名** ：

```js
var prefix = "foo";
var myObject = { [prefix + "bar"] : "hello",
  [prefix + "baz"] : "world"
};
myObject["foobar"]; // hello
myObject["foobaz"]; // world
```
可计算属性名最常用的场景可能是 ES6 的符号（Symbol），本书中不作详细介绍。不过简单来说，它们是一种新的基础数据类型，包含一个不透明且无法预测的值（从技术角度来说就是一个字符串）。一般来说你不会用到符号的实际值（因为理论上来说在不同的 JavaScript 引擎中值是不同的），所以通常你接触到的是符号的名称，比如 Symbol.Something（这个名字是我编的）：

```js
var myObject = { 
  [Symbol.Something] : "hello world"
}
```
##### 属性与方法

如果访问的对象属性是一个函数，有些开发者喜欢使用不一样的叫法以作区分。由于函数很容易被认为是属于某个对象，在其他语言中，属于对象（也被称为“类”）的函数通常被称为“方法”，因此把“属性访问”说成是“方法访问”也就不奇怪了。

从技术角度来说，函数永远不会“属于”一个对象，所以把对象内部引用的函数称为“方法”似乎有点不妥。

确实，有些函数具有 this 引用，有时候这些 this 确实会指向调用位置的对象引用。但是这种用法从本质上来说并没有把一个函数变成一个“方法”，因为 this 是在运行时根据调用位置动态绑定的，所以 **函数和对象的关系最多也只能说是间接关系** 。

无论返回值是什么类型，每次访问对象的属性就是属性访问。如果属性访问返回的是一个函数，那它也并不是一个“方法”。**属性访问返回的函数和其他函数没有任何区别**（除了可能发生的隐式绑定 this，就像我们刚才提到的）。

举例来说：

```js
function foo() {
  console.log("foo");
}
var someFoo = foo; // 对 foo 的变量引用
var myObject = {
  someFoo: foo
};
foo; // function foo(){..}
someFoo; // function foo(){..}
myObject.someFoo; // function foo(){..}
```
someFoo 和 myObject.someFoo 只是对于同一个函数的不同引用，并不能说明这个函数是特别的或者“属于”某个对象。如果 foo() 定义时在内部有一个 this 引用，那这两个函数引用的唯一区别就是 myObject.someFoo 中的 this 会被隐式绑定到一个对象。无论哪种引用形式都不能称之为“方法”。

最保险的说法可能是，“函数”和“方法”在 JavaScript 中是可以互换的。

即使你在对象的文字形式中声明一个函数表达式，这个函数也不会“属于”这个对象——它们 **只是对于相同函数对象的多个引用** 。

```js
var myObject = {
  foo: function() {
    console.log("foo");
  }
};
var someFoo = myObject.foo;
someFoo; // function foo(){..}
myObject.foo; // function foo(){..}
```
##### 数组

![数组、对象复制和属性描述](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%AF%B9%E8%B1%A12.png)

数组也支持 [] 访问形式，不过就像我们之前提到过的，数组有一套更加结构化的值存储
机制（不过仍然不限制值的类型）。**数组期望的是数值下标，也就是说值存储的位置（通常被称为索引）是整数** ，比如说 0 和 42：

```js
var myArray = [ "foo", 42, "bar" ];
myArray.length; // 3
myArray[0]; // "foo"
myArray[2]; // "bar"
```
数组也是对象，所以虽然每个下标都是整数，你仍然 **可以给数组添加属性** ：

```js
var myArray = [ "foo", 42, "bar" ];
myArray.baz = "baz";
myArray.length; // 3
myArray.baz; // "baz"
```
可以看到虽然 **添加了命名属性** （无论是通过 . 语法还是 [] 语法），**数组的 length 值并未发生变化** 。

你完全可以把数组当作一个普通的键 / 值对象来使用，并且不添加任何数值索引，但是这并不是一个好主意。数组和普通的对象都根据其对应的行为和用途进行了优化，所以最好只用对象来存储键 / 值对，**只用数组来存储数值下标 / 值对** 。

注意：如果你试图向数组添加一个属性，但是 **属性名“看起来”像一个数字** ，那 **它会变成一个数值下标** （因此会修改数组的内容而不是添加一个属性）：

```js
var myArray = [ "foo", 42, "bar" ];
myArray["3"] = "baz";
myArray.length; // 4
myArray[3]; // "baz"
```
##### 复制对象

举例来说，思考一下这个对象：

```js
function anotherFunction() {
  /*..*/
}
var anotherObject = {
  c: true
};
var anotherArray = [];
var myObject = {
  a: 2,
  b: anotherObject,
  // 引用，不是复本！
  c: anotherArray,
  // 另一个引用！
  d: anotherFunction
};
anotherArray.push(anotherObject, myObject);
```
如何准确地表示 myObject 的复制呢？

首先，我们应该判断它是 **浅复制** 还是 **深复制** 。对于 **浅拷贝** 来说，复制出的新对象中 a 的值会复制旧对象中 a 的值，也就是 2，但是新对象中 b、c、d 三个属性其实只是三个引用，它们和旧对象中 b、c、d 引用的对象是一样的。对于 **深复制** 来说，除了复制 myObject 以外还会复制 anotherObject 和 anotherArray。这时问题就来了，anotherArray 引用了 anotherObject 和 myObject，所以又需要复制 myObject，这样就会由于循环引用导致死循环。

我们是应该检测循环引用并终止循环（不复制深层元素）？还是应当直接报错或者是选择
其他方法？

除此之外，我们还不确定“复制”一个函数意味着什么。有些人会 **通过 toString() 来序列化一个函数的源代码**（但是结果取决于 JavaScript 的具体实现，而且不同的引擎对于不同类型的函数处理方式并不完全相同）。

对于 JSON 安全（也就是说可以被 **序列化为一个 JSON 字符串** 并且 **可以根据这个字符串解析出一个结构和值完全一样的对象** ）的对象来说，有一种巧妙的复制方法：

```js
var newObj = JSON.parse( JSON.stringify( someObj ) );
```
当然，这种方法需要保证对象是 **JSON 安全** 的，所以只适用于部分情况。

相比深复制，浅复制非常易懂并且问题要少得多，所以 **ES6** 定义了 **Object.assign(..) 方法** 来实现浅复制。Object.assign(..) 方法的第一个参数是目标对象，之后还可以跟一个或多个源对象。它会遍历一个或多个源对象的所有可枚举（enumerable，参见下面的代码）的自有键（owned key，很快会介绍）并把它们复制（使用 = 操作符赋值）到目标对象，最后返回目标对象，就像这样：

```js
var newObj = Object.assign( {}, myObject );

newObj.a; // 2
newObj.b === anotherObject; // true
newObj.c === anotherArray; // true
newObj.d === anotherFunction; // true
```
> 需要注意的一点是，由于 Object.assign(..) 就是使用 = 操作符来赋值，所以 **源对象属性的一些特性（比如 writable）不会被复制到目标对象** 。

##### 属性描述符

ES5 之前，JavaScript 语言本身并没有提供可以直接检测属性特性的方法，比如判断属性是否是只读。

但是从 ES5 开始，所有的属性都具备了属性描述符。

思考下面的代码：

```js
var myObject = {
  a: 2
};
Object.getOwnPropertyDescriptor(myObject, "a");
// {
// value: 2,
// writable: true,
// enumerable: true,
// configurable: true
// }
```
如你所见，这个普通的对象属性对应的属性描述符（也被称为“数据描述符”，因为它只保存一个数据值）可不仅仅只是一个 2。它还包含另外三个特性：writable（可写）、enumerable（可枚举）和 configurable（可配置）。

在创建普通属性时 **属性描述符会使用默认值** ，我们也可以使用**Object.defineProperty(..)** 来 **添加一个新属性或者修改一个已有属性**（如果它是 configurable）并对特性进行设置。

举例来说：

```js
var myObject = {};
Object.defineProperty(myObject, "a", {
  value: 2,
  writable: true,
  configurable: true,
  enumerable: true
});
myObject.a; // 2
```
我们使用 defineProperty(..) 给 myObject 添加了一个普通的属性并显式指定了一些特性。

然而，一般来说你不会使用这种方式，除非你想修改属性描述符。

1. ###### Writable

writable 决定 **是否可以修改属性的值** 。

思考下面的代码：

```js
var myObject = {};
Object.defineProperty(myObject, "a", {
  value: 2,
  writable: false,
  // 不可写！
  configurable: true,
  enumerable: true
});
myObject.a = 3;
myObject.a; // 2
```
如你所见，我们对于属性值的修改静默失败（silently failed）了。如果在严格模式下，这种方法会出错：

```js
"use strict";
var myObject = {};
Object.defineProperty(myObject, "a", {
  value: 2,
  writable: false,
  // 不可写！
  configurable: true,
  enumerable: true
});
myObject.a = 3; // TypeError
```
TypeError 错误表示我们无法修改一个不可写的属性。

> 之后我们会介绍 getter 和 setter，不过简单来说，你可以把 writable:false 看作是属性不可改变，相当于你定义了一个空操作 setter。严格来说，如果要和 writable:false 一致的话，你的 setter 被调用时应当抛出一个 TypeError 错误。

2. ###### Configurable

**只要属性是可配置的，就可以使用 defineProperty(..) 方法来修改属性描述符** ：

```js
var myObject = {
  a: 2
};
myObject.a = 3;
myObject.a; // 3
Object.defineProperty(myObject, "a", {
  value: 4,
  writable: true,
  configurable: false,
  // 不可配置！
  对象｜113 enumerable: true
});
myObject.a; // 4
myObject.a = 5;
myObject.a; // 5
Object.defineProperty(myObject, "a", {
  value: 6,
  writable: true,
  configurable: true,
  enumerable: true
}); // TypeError
```
最后一个 defineProperty(..) 会产生一个 TypeError 错误，不管是不是处于严格模式，尝试修改一个不可配置的属性描述符都会出错。注意：如你所见，**把 configurable 修改成 false 是单向操作，无法撤销！**

> 要注意有一个小小的例外：即便属性是 configurable:false，我们还是可以把 writable 的状态由 true 改为 false，但是无法由 false 改为 true。

除了无法修改，**configurable:false 还会禁止删除这个属性** ：

```js
var myObject = {
  a: 2
};
myObject.a; // 2
delete myObject.a;
myObject.a; // undefined
Object.defineProperty(myObject, "a", {
  value: 2,
  writable: true,
  configurable: false,
  enumerable: true
});
myObject.a; // 2
delete myObject.a;
myObject.a; // 2
```
如你所见，最后一个 delete 语句（静默）失败了，因为属性是不可配置的。

在本例中，delete 只用来直接删除对象的（可删除）属性。如果对象的某个属性是某个对象 / 函数的最后一个引用者，对这个属性执行 delete 操作之后，这个未引用的对象 / 函数就可以被垃圾回收。但是，**不要把 delete 看作一个释放内存的工具**（就像 C/C++ 中那样），**它就是一个删除对象属性的操作** ，仅此而已。

3. ###### Enumerable

从名字就可以看出，这个描述符控制的是属性 **是否会出现在对象的属性枚举中**  ，比如说for..in 循环。如果把 enumerable 设置成 false，这个属性就不会出现在枚举中，虽然仍然可以正常访问它。相对地，设置成 true 就会让它出现在枚举中。

用户定义的所有的普通属性默认都是 enumerable，这通常就是你想要的。但是如果你不希望某些特殊属性出现在枚举中，那就把它设置成 enumerable:false。

##### 不变性

![不可变性](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%AF%B9%E8%B1%A13.png)

有时候你会希望属性或者对象是不可改变（无论有意还是无意）的，在 ES5 中可以通过很多种方法来实现。

很重要的一点是，所有的方法创建的都是 **浅不变性**，也就是说，它们 **只会影响目标对象和它的直接属性** 。**如果目标对象引用了其他对象（数组、对象、函数，等），其他对象的内容不受影响，仍然是可变的** ：

```js
myImmutableObject.foo; // [1,2,3]
myImmutableObject.foo.push( 4 );
myImmutableObject.foo; // [1,2,3,4]
```
假设代码中的 myImmutableObject 已经被创建而且是不可变的，但是为了保护它的内容myImmutableObject.foo，你还需要使用下面的方法让 foo 也不可变。

> 在 JavaScript 程序中很少需要深不可变性。有些特殊情况可能需要这样做，但是根据通用的设计模式，如果你发现需要密封或者冻结所有的对象，那你或许应当退一步，重新思考一下程序的设计，让它能更好地应对对象值的改变。

1. ###### 对象常量

结合 **writable:false 和 configurable:false** 就可以创建一个 **真正的常量属性**（不可修改、重定义或者删除）：

```js
var myObject = {};
Object.defineProperty(myObject, "FAVORITE_NUMBER", {
  value: 42,
  writable: false,
  configurable: false
});
```
2. ###### 禁止扩展

如果你想 **禁止一个对象添加新属性并且保留已有属性** ， 可以使用**Object.preventExtensions(..)**：

```js
var myObject = {
  a: 2
};
Object.preventExtensions(myObject);
myObject.b = 3;
myObject.b; // undefined
```
在非严格模式下，创建属性 b 会静默失败。在严格模式下，将会抛出 TypeError 错误。

3. ###### 密封

**Object.seal(..)** 会创建一个**“密封”的对象** ，这个方法实际上会在一个 **现有对象上调用Object.preventExtensions(..) 并把所有现有属性标记为configurable:false** 。

所以，密封之后不仅 **不能添加新属性** ，也 **不能重新配置或者删除任何现有属性**（虽然可以修改属性的值）。

4. ###### 冻结

**Object.freeze(..)** 会创建一个 **冻结对象** ，这个方法实际上会 **在一个现有对象上调用 Object.seal(..)**  并 **把所有“数据访问”属性标记为 writable:false，这样就无法修改它们的值** 。

这个方法是你可以应用在对象上的级别最高的不可变性，它会禁止对于对象本身及其任意直接属性的修改（不过就像我们之前说过的，这个对象引用的其他对象是不受影响的）。

你可以“ **深度冻结** ”一个对象，具体方法为，**首先在这个对象上调用Object.freeze(..)** ，然后 **遍历它引用的所有对象并在这些对象上调用 Object.freeze(..)**v。但是一定要小心，因为这样做 **有可能会在无意中冻结其他（共享）对象** 。

##### [[Get]]

属性访问在实现时有一个微妙却非常重要的细节，思考下面的代码：

```js
var myObject = {
  a: 2
};
myObject.a; // 2
```
myObject.a 是一次属性访问，但是这条语句并不仅仅是在 myObjet 中查找名字为 a 的属性，虽然看起来好像是这样。

在语言规范中，myObject.a 在 myObject 上实际上是实现了 [[Get]] 操作（有点像函数调用：[[Get]]()）。**对象默认的内置 [[Get]] 操作首先在对象中查找是否有名称相同的属性** ，如果 **找到就会返回这个属性的值** 。

然而，如果 **没有找到名称相同的属性** ，**按照 [[Get]] 算法的定义会遍历可能存在的 [[Prototype]] 链** ，也就是原型链。

如果 **无论如何都没有找到名称相同的属性** ，那 **[[Get]] 操作会返回值 undefined** ：

```js
var myObject = {
  a: 2
};
myObject.b; // undefined
```
注意，这种方法和访问变量时是不一样的。**如果你引用了一个当前词法作用域中不存在的变量，并不会像对象属性一样返回 undefined，而是会抛出一个 ReferenceError 异常** ：

```js
var myObject = {
  a: undefined
};
myObject.a; // undefined
myObject.b; // undefined
```
从返回值的角度来说，这两个引用没有区别——它们都返回了 undefined。然而，尽管乍看之下没什么区别，实际上底层的 [[Get]] 操作对 myObject.b 进行了更复杂的处理。

由于仅 **根据返回值无法判断出到底变量的值为 undefined 还是变量不存在** ，所以 [[Get]]操作返回了 undefined。

##### [[Put]]

既然有可以获取属性值的 [[Get]] 操作，就一定有对应的 [[Put]] 操作。

你可能会认为给对象的属性赋值会触发 [[Put]] 来设置或者创建这个属性。但是实际情况并不完全是这样。

**[[Put]] 被触发时，实际的行为取决于许多因素，包括对象中是否已经存在这个属性（这是最重要的因素）**。

如果已经存在这个属性，[[Put]] 算法大致会检查下面这些内容。

1. 属性是否是访问描述符（参见 3.3.9 节）？如果是并且存在 setter 就调用 setter。

2. 属性的数据描述符中 writable 是否是 false ？如果是，在非严格模式下静默失败，在严格模式下抛出 TypeError 异常。

3. 如果都不是，将该值设置为属性的值。

如果对象中不存在这个属性，[[Put]] 操作会更加复杂。

##### Getter和Setter

对象默认的 [[Put]] 和 [[Get]] 操作分别可以控制属性值的设置和获取。

> 在语言的未来 / 高级特性中，有可能可以改写整个对象（不仅仅是某个属性）的默认 [[Get]] 和 [[Put]] 操作。

在 ES5 中可以使用 getter 和 setter 部分改写默认操作，但是 **只能应用在单个属性上，无法应用在整个对象上** 。**getter 是一个隐藏函数，会在获取属性值时调用** 。**setter 也是一个隐藏函数，会在设置属性值时调用** 。

当你给一个属性定义 getter、setter 或者两者都有时，这个属性会被定义为“访问描述
符”（和“数据描述符”相对）。**对于访问描述符来说，JavaScript 会忽略它们的 value 和 writable 特性** ，取而代之的是 **关心 set 和 get**（还有 configurable 和 enumerable）特性。

思考下面的代码：

```js
var myObject = {
  // 给 a 定义一个 getter
  get a() {
    return 2;
  }
};
Object.defineProperty(myObject, // 目标对象
"b", // 属性名
118｜第3章 { // 描述符
  // 给 b 设置一个 getter
  get: function() {
    return this.a * 2
  },
  // 确保 b 会出现在对象的属性列表中
  enumerable: true
});
myObject.a; // 2
myObject.b; // 4
```
不管是对象文字语法中的 get a() { .. }，还是 defineProperty(..) 中的显式定义，二者都会在对象中 **创建一个不包含值的属性** ，**对于这个属性的访问会自动调用一个隐藏函数** ，它的返回值会被当作属性访问的返回值：

```js
var myObject = {
  // 给 a 定义一个 getter
  get a() {
    return 2;
  }
};
myObject.a = 3;
myObject.a; // 2
```
由于我们 **只定义了 a 的 getter，所以对 a 的值进行设置时 set 操作会忽略赋值操作，不会抛出错误** 。而且即便有合法的 setter，由于我们自定义的 getter 只会返回 2，所以 set 操作是没有意义的。

为了让属性更合理，还应当定义 setter，和你期望的一样，setter 会覆盖单个属性默认的  [[Put]]（也被称为赋值）操作。通常来说 **getter 和 setter 是成对出现的**（只定义一个的话通常会产生意料之外的行为）：

```js
var myObject = {
  // 给 a 定义一个 getter
  get a() {
    return this._a_;
  },
  // 给 a 定义一个 setter
  set a(val) {
    this._a_ = val * 2;
  }
};
myObject.a = 2;
myObject.a; // 4
```

> 在本例中，实际上我们把赋值（[[Put]]）操作中的值 2 存储到了另一个变量 _a_ 中。名称 _a_ 只是一种惯例，没有任何特殊的行为——和其他普通属性一样。

##### 存在性

![存在性和遍历](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%AF%B9%E8%B1%A14.png)

myObject.a 的属性访问返回值可能是 undefined，但是这个值有可能是属性中存储的 undefined，也可能是因为属性不存在所以返回 undefined。那么如何区分这两种情况呢？

我们可以在不访问属性值的情况下判断对象中是否存在这个属性：

```js
var myObject = {
  a: 2
}; ("a" in myObject); // true
("b" in myObject); // false
myObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("b"); // false
```
in 操作符会检查属性是否在对象及其 [[Prototype]] 原型链中。相比之下，**hasOwnProperty(..) 只会检查属性是否在 myObject 对象中** ，**不会检查[[Prototype]] 链** 。

有的 **普通对象都可以通过对于 Object.prototype 的委托来访问hasOwnProperty(..)** ，但是有的对象可能没有连接到 Object.prototype（通过 Object.create(null) 来创建。在这种情况下，形如 myObejct.hasOwnProperty(..)就会失败。
 
这时可以使用一种更加强硬的方法来进行判断：**Object.prototype.hasOwnProperty.call(myObject,"a")** ，它 **借用基础的hasOwnProperty(..) 方法并把它显式绑定** 到 myObject 上。

> 看起来 in 操作符可以检查容器内是否有某个值，但是它实际上 **检查的是某个属性名是否存在** 。**对于数组来说这个区别非常重要** ，4 in [2, 4, 6] 的结果并不是你期待的 True，因为 [2, 4, 6] 这个数组中包含的属性名是 0、1、2，没有 4。

1. 枚举

之前介绍 enumerable 属性描述符特性时我们简单解释过什么是“可枚举性”，现在详细介绍一下：

```js
var myObject = {};
Object.defineProperty(myObject, "a",
// 让 a 像普通属性一样可以枚举
{
  enumerable: true,
  value: 2
});
Object.defineProperty(myObject, "b",
// 让 b 不可枚举
{
  enumerable: false,
  value: 3
});
myObject.b; // 3
("b" in myObject); // true
myObject.hasOwnProperty("b"); // true
// .......
for (var k in myObject) {
  console.log(k, myObject[k]);
}
// "a" 2
```
可以看到，myObject.b 确实存在并且有访问值，但是却不会出现在 for..in 循环中（尽管可以通过 in 操作符来判断是否存在）。原因是 **“可枚举”就相当于“可以出现在对象属性的遍历中”**。

> 在数组上应用 for..in 循环有时会产生出人意料的结果，因为 **这种枚举不仅会包含所有数值索引，还会包含所有可枚举属性** 。最好 **只在对象上应用for..in 循环** ，如果 **要遍历数组就使用传统的 for 循环来遍历数值索引** 。

也可以通过另一种方式来区分属性是否可枚举：

```js
var myObject = {};
Object.defineProperty(myObject, "a",
// 让 a 像普通属性一样可以枚举
{
  enumerable: true,
  value: 2
});
Object.defineProperty(对象｜121 myObject, "b",
// 让 b 不可枚举
{
  enumerable: false,
  value: 3
});
myObject.propertyIsEnumerable("a"); // true
myObject.propertyIsEnumerable("b"); // false
Object.keys(myObject); // ["a"]
Object.getOwnPropertyNames(myObject); // ["a", "b"]
```
**propertyIsEnumerable(..) 会检查给定的属性名是否直接存在于对象中**（而不是在原型链上）并且 **满足 enumerable:true** 。

**Object.keys(..)** 会返回一个数组，包含 **所有可枚举属性** ，**Object.getOwnPropertyNames(..)会返回一个数组** ，包含 **所有属性，无论它们是否可枚举** 。

**in 和 hasOwnProperty(..) 的区别在于是否查找 [[Prototype]] 链** ，然而，**Object.keys(..)和 Object.getOwnPropertyNames(..)** 都 **只会查找对象直接包含的属性** 。

**并没有内置的方法可以获取 in 操作符使用的属性列表**（对象本身的属性以及 [[Prototype]] 链中的所有属性，参见第 5 章）。不过你 **可以递归遍历某个对象的整条 [[Prototype]] 链** 并 **保存每一层中使用 Object.keys(..) 得到的属性列表**——只包含可枚举属性。

#### 遍历

for..in 循环可以用来遍历对象的可枚举属性列表（包括 [[Prototype]] 链）。但是如何遍历属性的值呢？

对于数值索引的数组来说，可以 **使用标准的 for 循环来遍历值** ：

```js
var myArray = [1, 2, 3];
for (var i = 0; i < myArray.length; i++) {
console.log( myArray[i] );
}
// 1 2 3
```
这实际上并不是在遍历值，而是遍历下标来指向值，如 myArray[i]。

ES5 中增加了一些数组的辅助迭代器，包括 forEach(..)、every(..) 和 some(..)。每种 **辅助迭代器** 都可以接受一个回调函数并把它应用到数组的每个元素上，唯一的区别就是它们 **对于回调函数返回值的处理方式不同** 。

**forEach(..)** 会遍历数组中的所有值并忽略回调函数的返回值。**every(..)** 会一直运行直到回调函数返回 false（或者“假”值），**some(..)** 会一直运行直到回调函数返回 true（或者“真”值）。

every(..) 和 some(..) 中特殊的返回值和普通 for 循环中的 break 语句类似，它们会提前终止遍历。

使用 for..in 遍历对象是 **无法直接获取属性值的** ，因为它实际上遍历的是对象中的所有可枚举属性，你 **需要手动获取属性值** 。

> 遍历数组下标时采用的是数字顺序（for 循环或者其他迭代器），但是 **遍历对象属性时的顺序是不确定的** ，**在不同的 JavaScript 引擎中可能不一样** 。因此，在不同的环境中需要保证一致性时，一定不要相信任何观察到的顺序，它们是不可靠的。

那么如何直接遍历值而不是数组下标（或者对象属性）呢？幸好，ES6 增加了一种用来遍
历数组的 **for..of 循环语法**（如果对象本身定义了迭代器的话也可以遍历对象）：

```js
var myArray = [1, 2, 3];
for (var v of myArray) {
  console.log(v);
}
// 1
// 2
// 3
```
**for..of 循环** 首先会向被访问对象请求一个迭代器对象，然后通过调用迭代器对象的 **next() 方法** 来遍历所有返回值。

数组有内置的 @@iterator，因此 for..of 可以直接应用在数组上。我们 **使用内置的 @@iterator 来手动遍历数组**，看看它是怎么工作的：

```
var myArray = [ 1, 2, 3 ];
var it = myArray[Symbol.iterator]();
it.next(); // { value:1, done:false }
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { done:true }
```
> 我们使用 ES6 中的符号 Symbol.iterator 来获取对象的 @@iterator 内部属性。之前我们简单介绍过符号（Symbol，参见 3.3.1 节），跟这里的原理是相同的。引用类似 iterator 的特殊属性时要使用符号名，而不是符号包含的值。此外，虽然看起来很像一个对象，但是 @@iterator 本身并不是一个迭代器对象，而是一个返回迭代器对象的函数——这点非常精妙并且非常重要。

如你所见，调用迭代器的 next() 方法会返回形式为 { value: .. , done: .. } 的值，
value 是当前的遍历值，done 是一个布尔值，表示是否还有可以遍历的值。

注意，和值“3”一起返回的是 done:false，乍一看好像很奇怪，你必须再调用一次
next() 才能得到 done:true，从而确定完成遍历。

和数组不同，普通的对象没有内置的 @@iterator，所以无法自动完成 for..of 遍历。

当然，你可以给任何想遍历的对象 **定义 @@iterator** ，举例来说：

```js
var myObject = {
  a: 2,
  b: 3
};
Object.defineProperty(myObject, Symbol.iterator, {
  enumerable: false,
  writable: false,
  configurable: true,
  value: function() {
    var o = this;
    var idx = 0;
    var ks = Object.keys(o);
    return {
      next: function() {
        return {
          value: o[ks[idx++]],
          done: (idx > ks.length)
        };
      }
    };
  }
});
// 手动遍历 myObject
var it = myObject[Symbol.iterator]();
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { value:undefined, done:true }
// 用 for..of 遍历 myObject
for (var v of myObject) {
  console.log(v);
}
// 2
// 3
```
> 我们使用 Object.defineProperty(..) 定义了我们自己的 @@iterator（主要是为了让它不可枚举），不过注意，我们把符号当作可计算属性名（本章之前有介绍）。此外，也可以直接在定义对象时进行声明，比如 var myObject = {a:2, b:3, [Symbol.iterator]: function() { /* .. */ } }。

for..of 循环每次调用 myObject 迭代器对象的 next() 方法时，内部的指针都会向前移动并返回对象属性列表的下一个值（再次提醒，需要注意遍历对象属性 / 值时的顺序）。

实际上，你甚至可以定义一个 **“无限”迭代器** ，它永远不会“结束”并且总会返回一个新值（比如随机数、递增值、唯一标识符，等等）。你可能永远不会在 for..of 循环中使用这样的迭代器，因为它永远不会结束，你的程序会被挂起：

```js
var randoms = { [Symbol.iterator] : function() {
    return {
      next: function() {
        return {
          value: Math.random()
        };
      }
    };
  }
};
var randoms_pool = [];
for (var n of randoms) {
  randoms_pool.push(n);
  // 防止无限运行！
  if (randoms_pool.length === 100) break;
}
```
这个迭代器会生成“无限个”随机数，因此我们添加了一条 break 语句，防止程序被挂起。

#### 小结

JavaScript 中的对象有字面形式（比如 var a = { .. }）和构造形式（比如 var a = new Array(..)）。字面形式更常用，不过有时候构造形式可以提供更多选项。

许多人都以为“JavaScript 中万物都是对象”，这是错误的。对象是 6 个（或者是 7 个，取决于你的观点）基础类型之一。对象有包括 function 在内的子类型，不同子类型具有不同的行为，比如内部标签 [object Array] 表示这是对象的子类型数组。

对象就是键 / 值对的集合。可以通过 .propName 或者 ["propName"] 语法来获取属性值。访问属性时，引擎擎实际上会调用内部的默认 [[Get]] 操作（在设置属性值时是 [[Put]]），[[Get]] 操作会检查对象本身是否包含这个属性，如果没找到的话还会查找 [[Prototype]]链（参见第 5 章）。

属性的特性可以通过属性描述符来控制，比如 writable 和 configurable。此外，可以使用Object.preventExtensions(..)、Object.seal(..) 和 Object.freeze(..) 来设置对象（及其属性）的不可变性级别。

属性不一定包含值——它们可能是具备 getter/setter 的“访问描述符”。此外，属性可以是可枚举或者不可枚举的，这决定了它们是否会出现在 for..in 循环中。

你可以使用 ES6 的 for..of 语法来遍历数据结构（数组、对象，等等）中的值，for..of会寻找内置或者自定义的 @@iterator 对象并调用它的 next() 方法来遍历数据值。

### 混合对象“类”

面向类的设计模式：实例化（instantiation）、继承（inheritance）和（相对）多态（polymorphism）。

我们将会看到，这些概念实际上无法直接对应到 JavaScript 的对象机制，因此我们会介绍许多 JavaScript 开发者所使用的解决方法（比如混入，mixin）。

![类](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E7%B1%BB.png)

#### 类理论

类 / 继承描述了一种代码的组织结构形式——一种在软件中对真实世界中问题领域的建模方法。

面向对象编程强调的是数据和操作数据的行为本质上是互相关联的（当然，不同的数据有不同的行为），因此好的设计就是把数据以及和它相关的行为打包（或者说封装）起来。

类的另一个核心概念是多态，这个概念是说父类的通用行为可以被子类用更特殊的行为重写。实际上，相对多态性允许我们从重写行为中引用基础行为。

##### “类”设计模式

面向对象设计模式，比如迭代器模式、观察者模式、工厂模式、单例模式。

过程化编程，这种代码只包含过程（函数）调用，没有高层的抽象。或许老师还教过你最好使用类把过程化风格的“意大利面代码”转换成结构清晰、组织良好的代码。

##### JavaScript中的“类”

JavaScript 只有一些近似类的语法元素（比如 new 和 instanceof），不过在后来的 ES6 中新增了一些元素，比如 class 关键字。

#### 类的机制

##### 建造

“类”和“实例”的概念来源于房屋建造。

把类和实例对象之间的关系看作是直接关系而不是间接关系通常更有助于理解。类通过复制操作被实例化为对象形式：

![被实例化为对象形式](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E8%A2%AB%E5%AE%9E%E4%BE%8B%E5%8C%96%E4%B8%BA%E5%AF%B9%E8%B1%A1%E5%BD%A2%E5%BC%8F.png)

如你所见，箭头的方向是从左向右、从上向下，它表示概念和物理意义上发生的复制操作。

##### 构造函数

类实例是由一个特殊的类方法构造的，这个方法名通常和类名相同，被称为构造函数。这个方法的任务就是初始化实例需要的所有信息（状态）。

举例来说，思考下面这个关于类的伪代码（编造出来的语法）：

```js
class CoolGuy {
  specialTrick = nothing CoolGuy(trick) {
    specialTrick = trick
  }
  showOff() {
    output("Here's my trick: ", specialTrick)
  }
}
```
我们可以调用类构造函数来生成一个CoolGuy实例：

```js
Joe = new CoolGuy("jumping rope") 
Joe.showOff() // 这是我的绝技：跳绳
```
注意，CoolGuy 类有一个 CoolGuy() 构造函数，执行 new CoolGuy() 时实际上调用的就是它。构造函数会返回一个对象（也就是类的一个实例），之后我们可以在这个对象上调用 showOff() 方法，来输出指定 CoolGuy 的特长。

类构造函数属于类，而且通常和类同名。此外，构造函数大多需要用 new 来调，这样语言引擎才知道你想要构造一个新的类实例。

#### 类的继承

在面向类的语言中，你可以先定义一个类，然后定义一个继承前者的类。

后者通常被称为“子类”，前者通常被称为“父类”。

定义好一个子类之后，相对于父类来说它就是一个独立并且完全不同的类。子类会包含父类行为的原始副本，但是也可以重写所有继承的行为甚至定义新行为。

首先回顾一下本章前面部分提出的 Vehicle 和 Car 类。思考下面关于类继承的伪代码：

```js
class Vehicle {
  engines = 1 ignition() {
    output("Turning on my engine.");
  }
  drive() {
    ignition();
    output("Steering and moving forward!")
  }
}
class Car inherits Vehicle {
  wheels = 4 drive() {
    inherited: drive() output("Rolling on all ", wheels, " wheels!")
  }
}
class SpeedBoat inherits Vehicle {
  engines = 2 ignition() {
    output("Turning on my ", engines, " engines.")
  }
  pilot() {
    inherited: drive() output("Speeding through the water with ease!")
  }
}
```
我们通过定义 Vehicle 类来假设一种发动机，一种点火方式，一种驾驶方法。但是你不可能制造一个通用的“交通工具”，因为这个类只是一个抽象的概念。

接下来我们定义了两类具体的交通工具：Car 和 SpeedBoat。它们都从 Vehicle 继承了通用的特性并根据自身类别修改了某些特性。汽车需要四个轮子，快艇需要两个发动机，因此它必须启动两个发动机的点火装置。

##### 多态

Car 重写了继承自父类的 drive() 方法，但是之后 Car 调用了 inherited:drive() 方法，这表明 Car 可以引用继承来的原始 drive() 方法。快艇的 pilot() 方法同样引用了原始 drive() 方法

这个技术被称为多态或者虚拟多态。在本例中，更恰当的说法是相对多态。

多态是一个非常广泛的话题，我们现在所说的“相对”只是多态的一个方面：任何方法都可以引用继承层次中高层的方法（无论高层的方法名和当前方法名是否相同）。之所以说 “相对”是因为我们并不会定义想要访问的绝对继承层次（或者说类），而是使用相对引用“查找上一层”。

多态的另一个方面是，在继承链的不同层次中一个方法名可以被多次定义，当调用方法时会自动选择合适的定义。

> 在传统的面向类的语言中 super 还有一个功能，就是从子类的构造函数中通过super 可以直接调用父类的构造函数。通常来说这没什么问题，因为对于真正的类来说，构造函数是属于类的。然而，在 JavaScript 中恰好相反——实际上 **“类”是属于构造函数的（类似 Foo.prototype... 这样的类型引用）** 。由于 **JavaScript 中父类和子类的关系只存在于两者构造函数对应的 .prototype 对象中** ，因此 **它们的构造函数之间并不存在直接联系** ，从而 **无法简单地实现两者的相对引用** 。

多态并不表示子类和父类有关联，子类得到的只是父类的一份副本。类的继承其实就是复制。

##### 多重继承

有些面向类的语言允许你继承多个“父类”。多重继承意味着所有父类的定义都会被复制到子类中。

如果两个父类中都定义了 drive() 方法的话，子类引用的是哪个呢？难道每次都需要手动指定具体父类的 drive() 方法吗？这样多态继承的很多优点就存在了。

除此之外，还有一种被称为钻石问题的变种。在钻石问题中，子类 D 继承自两个父类（B和 C），这两个父类都继承自 A。如果 A 中有 drive() 方法并且 B 和 C 都重写了这个方法（多态），那当 D 引用 drive() 时应当选择哪个版本呢（B:drive() 还是C:drive()）？

![为钻石问题的变种](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E9%92%BB%E7%9F%B3%E9%97%AE%E9%A2%98%E7%9A%84%E5%8F%98%E7%A7%8D.png)

#### 混入

使用混入实现多重继承。

在继承或者实例化时，JavaScript 的对象机制并不会自动执行复制行为。简单来说，
JavaScript 中只有对象，并不存在可以被实例化的“类”。一个对象并不会被复制到其他对象，它们会被关联起来。

由于在其他语言中类表现出来的都是复制行为，因此 JavaScript 开发者也想出了一个方法来模拟类的复制行为，这个方法就是混入。接下来我们会看到两种类型的混入：显式和隐式。

##### 显式混入

首先我们来回顾一下之前提到的 Vehicle 和 Car。由于 JavaScript 不会自动实现 Vehicle 到 Car 的复制行为，所以我们需要手动实现复制功能。这个功能在许多库和框架中被称为 extend(..)，但是为了方便理解我们称之为 mixin(..)。

```js
// 非常简单的 mixin(..) 例子 :
function mixin(sourceObj, targetObj) {
  for (var key in sourceObj) {
    // 只会在不存在的情况下复制
    if (! (key in targetObj)) {
      targetObj[key] = sourceObj[key];
    }
  }
  return targetObj;
}
var Vehicle = {
  engines: 1,
  ignition: function() {
    console.log("Turning on my engine.");
  },
  drive: function() {
    this.ignition();
    console.log("Steering and moving forward!");
  }
};
var Car = mixin(Vehicle, {
  wheels: 4,
  drive: function() {
    Vehicle.drive.call(this);
    console.log("Rolling on all " + this.wheels + " wheels!");
  }
});
```
> 有一点需要注意，我们处理的已经不再是类了，因为在 JavaScript 中不存在类，Vehicle 和 Car 都是对象，供我们分别进行复制和粘贴。

###### 再说多态

我们来分析一下这条语句：Vehicle.drive.call( this )。这就是我所说的显式多态。还记得吗，在之前的伪代码中对应的语句是 inherited:drive()，我们称之为相对多态。
JavaScript（在 ES6 之前；参见附录 A）并没有相对多态的机制。所以，由于 Car 和
Vehicle 中都有 drive() 函数，为了指明调用对象，我们必须使用绝对（而不是相对）引用。我们通过名称显式指定 Vehicle 对象并调用它的 drive() 函数。

但是如果直接执行 Vehicle.drive()，函数调用中的 this 会被绑定到 Vehicle 对象而不是 Car 对象（参见第 2 章），这并不是我们想要的。因此，我们会使用 .call(this)（参见第 2章）来确保 drive() 在 Car 对象的上下文中执行。

> 如果函数 Car.drive() 的名称标识符并没有和 Vehicle.drive() 重叠（或者说“屏蔽”；参见第 5 章）的话，我们就不需要实现方法多态，因为调用mixin(..) 时会把函数 Vehicle.drive() 的引用复制到 Car 中，因此我们可以直接访问 this.drive()。正是由于存在标识符重叠，所以必须使用更加复杂的显式伪多态方法。

###### 混合复制

回顾一下之前提到的 mixin(..) 函数：

```js
// 非常简单的 mixin(..) 例子 :
function mixin(sourceObj, targetObj) {混合对象“类”｜137
  for (var key in sourceObj) {
    // 只会在不存在的情况下复制
    if (! (key in targetObj)) {
      targetObj[key] = sourceObj[key];
    }
  }
  return targetObj;
}
```
现在我们来分析一下 mixin(..) 的工作原理。它会遍历 sourceObj（本例中是 Vehicle）的属性，如果在 targetObj（本例中是 Car）没有这个属性就会进行复制。由于我们是在目标对象初始化之后才进行复制，因此一定要小心不要覆盖目标对象的原有属性。

如果我们是先进行复制然后对 Car 进行特殊化的话，就可以跳过存在性检查。不过这种方法并不好用并且效率更低，所以不如第一种方法常用：

```js
// 另一种混入函数，可能有重写风险
function mixin(sourceObj, targetObj) {
  for (var key in sourceObj) {
    targetObj[key] = sourceObj[key];
  }
  return targetObj;
}
var Vehicle = {
  // ...
};
// 首先创建一个空对象并把 Vehicle 的内容复制进去
var Car = mixin(Vehicle, {});
// 然后把新内容复制到 Car 中
mixin({
  wheels: 4,
  drive: function() {
    // ...
  }
},Car);
```
这两种方法都可以把不重叠的内容从 Vehicle 中显性复制到 Car 中。“混入”这个名字来源于这个过程的另一种解释：**Car 中混合了 Vehicle 的内容** ，就像你把巧克力片混合到你最喜欢的饼干面团中一样。

复制操作完成后，Car 就和 Vehicle 分离了，向 Car 中添加属性不会影响 Vehicle，反之亦然。

JavaScript 中的函数无法（用标准、可靠的方法）真正地复制，所以你只能复制对共享函数对象的引用（函数就是对象；参见第 3 章）。如果你修改了共享的函数对象（比如ignition()），比如添加了一个属性，那 Vehicle 和 Car 都会受到影响。

###### 寄生继承

显式混入模式的一种变体被称为“寄生继承”，它既是显式的又是隐式的，主要推广者是Douglas Crockford。

下面是它的工作原理：

```js
//“传统的 JavaScript 类”Vehicle
function Vehicle() {
  this.engines = 1;
}
Vehicle.prototype.ignition = function() {
  console.log("Turning on my engine.");
};
Vehicle.prototype.drive = function() {
  this.ignition();
  console.log("Steering and moving forward!");
};

//“寄生类”Car
function Car() {
  // 首先，car 是一个 Vehicle
  var car = new Vehicle();
  // 接着我们对 car 进行定制
  car.wheels = 4;
  // 保存到 Vehicle::drive() 的特殊引用
  var vehDrive = car.drive;
  // 重写 Vehicle::drive()
  car.drive = function() {
    vehDrive.call(this);
    console.log("Rolling on all " + this.wheels + " wheels!");
    return car;
  }
}
var myCar = new Car();
myCar.drive();
// 发动引擎。
// 手握方向盘！
// 全速前进！
```
如你所见，首先我们复制一份 Vehicle 父类（对象）的定义，然后混入子类（对象）的定义（如果需要的话保留到父类的特殊引用），然后用这个复合对象构建实例。

##### 隐式混入

隐式混入和之前提到的显式伪多态很像，因此也具备同样的问题。

思考下面的代码：

```js
var Something = {
  cool: function() {
    this.greeting = "Hello World";
    this.count = this.count ? this.count + 1 : 1;
  }
};
Something.cool();
Something.greeting; // "Hello World"
Something.count; // 1
var Another = {
  cool: function() {
    // 隐式把 Something 混入 Another
    Something.cool.call(this);
  }
};
Another.cool();
Another.greeting; // "Hello World"
Another.count; // 1（count 不是共享状态）
```
通过在构造函数调用或者方法调用中使用 Something.cool.call( this )，我们实际上“借用”了函数 Something.cool() 并在 Another 的上下文中调用了它（通过 this 绑定；参加第 2 章）。最终的结果是 Something.cool() 中的赋值操作都会应用在 Another 对象上而不是Something 对象上。

#### 小结

类是一种设计模式。许多语言提供了对于面向类软件设计的原生语法。JavaScript 也有类似的语法，但是和其他语言中的类完全不同。

类意味着复制。

传统的类被实例化时，它的行为会被复制到实例中。类被继承时，行为也会被复制到子类中。

多态（在继承链的不同层次名称相同但是功能不同的函数）看起来似乎是从子类引用父类，但是本质上引用的其实是复制的结果。

JavaScript 并不会（像类那样）自动创建对象的副本。

混入模式（无论显式还是隐式）可以用来模拟类的复制行为，但是通常会产生丑陋并且脆弱的语法，比如显式伪多态（OtherObj.methodName.call(this, ...)），这会让代码更加难懂并且难以维护。

此外，显式混入实际上无法完全模拟类的复制行为，因为对象（和函数！别忘了函数也是对象）只能复制引用，无法复制被引用的对象或者函数本身。忽视这一点会导致许多问题。总地来说，在 JavaScript 中模拟类是得不偿失的，虽然能解决当前的问题，但是可能会埋下更多的隐患。

因此，我们把 Something 的行为“混入”到了 Another 中。

虽然这类技术利用了 this 的重新绑定功能，但是 Something.cool.call( this ) 仍然无法变成相对（而且更灵活的）引用，所以使用时千万要小心。通常来说，尽量避免使用这样的结构，以保证代码的整洁和可维护性。

### 原型

![原型](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%8E%9F%E5%9E%8B.png)

#### [[Prototype]]

JavaScript 中的对象有一个 **特殊的 [[Prototype]] 内置属性** ，其实就是 **对于其他对象的引用** 。几乎所有的对象在创建时 [[Prototype]] 属性都会 **被赋予一个非空的值** 。

思考下面的代码：

```js
var myObject = {
  a:2
};
myObject.a; // 2
```
[[Prototype]] 引用有什么用呢？在第 3 章中我们说过，当你试图引用对象的属性时会触发 [[Get]] 操作，比如 myObject.a。对于默认的 [[Get]] 操作来说，第一步是检查对象本身是否有这个属性，如果有的话就使用它。

但是如果 a 不在 myObject 中，就需要使用对象的 [[Prototype]] 链了。
对于默认的 [[Get]] 操作来说，如果无法在对象本身找到需要的属性，就会继续访问对象的 [[Prototype]] 链：

```js
var anotherObject = {
a:2
};
// 创建一个关联到 anotherObject 的对象
var myObject = Object.create( anotherObject );
myObject.a; // 2
```
稍后我们会介绍 Object.create(..) 的原理，现在只需要知道它会 **创建一个对象并把这个对象的 [[Prototype]] 关联到指定的对象** 。

现在 myObject 对象的 [[Prototype]] 关联到了 anotherObject。显然 myObject.a 并不存在，但是尽管如此，属性访问仍然成功地（在 anotherObject 中）找到了值 2。

但是，如果 anotherObject 中也找不到 a 并且 [[Prototype]] 链不为空的话，就会继续查找下去。

这个过程会持续到找到匹配的属性名或者查找完整条 [[Prototype]] 链。如果是后者的话，[[Get]] 操作的返回值是 undefined。

使用 for..in 遍历对象时原理和查找 [[Prototype]] 链类似，任何可以通过原型链访问到（并且是 enumerable，参见第 3 章）的属性都会被枚举。使用 in 操作符来检查属性在对象中是否存在时，同样会 **查找对象的整条原型链**（无论属性是否可枚举）：

```js
var anotherObject = {
  a: 2
};
// 创建一个关联到 anotherObject 的对象
var myObject = Object.create(anotherObject);
for (var k in myObject) {
  console.log("found: " + k);
}
// found: a
("a" in myObject); // true
```
因此，当你通过各种语法进行属性查找时都会查找 [[Prototype]] 链，直到找到属性或者查找完整条原型链。

##### Object.prototype

**所有普通的 [[Prototype]] 链最终都会指向内置的 Object.prototype** 。由于所有的“普通”（内置，不是特定主机的扩展）对象都“源于”（或者说把 [[Prototype]] 链的顶端设置为）这个 Object.prototype 对象，所以它 **包含 JavaScript 中许多通用的功能** 。

有些功能你应该已经很熟悉了， 比如说 .toString() 和 .valueOf()， 第 3 章还介绍过 .hasOwnProperty(..)。稍后我们还会介绍 .isPrototypeOf(..)，这个你可能不太熟悉。

##### 属性设置和屏蔽

给一个对象设置属性并不仅仅是添加一个新属性或者修改已有的属性值。现在我们完整地讲解一下这个过程：

```js
myObject.foo = "bar";
```
如果 myObject 对象中包含名为 foo 的普通数据访问属性，这条赋值语句只会修改已有的属性值。

如果 foo 不是直接存在于 myObject 中，[[Prototype]] 链就会被遍历，类似 [[Get]] 操作。

如果 **原型链上找不到 foo** ，foo 就会被直接添加到 myObject 上。

然而，如果 **foo 存在于原型链上层** ，赋值语句 myObject.foo = "bar" 的行为就会有些不同（而且可能很出人意料）。

如果属性名 foo 既出现在 myObject 中也出现在 myObject 的 [[Prototype]] 链上层，那么就会发生屏蔽。myObject 中包含的 foo 属性会屏蔽原型链上层的所有 foo 属性，因为 myObject.foo 总是会选择原型链中最底层的 foo 属性。

屏蔽比我们想象中更加复杂。下面我们分析一下如果 foo 不直接存在于 myObject 中而是 **存在于原型链上层时 myObject.foo = "bar" 会出现的三种情况** 。

1. 如果 **在 [[Prototype]] 链上层存在名为 foo 的普通数据访问属性**（参见第 3 章）并且 **没有被标记为只读** （writable:false），那就会直接在 myObject 中添加一个名为 foo 的新属性，它是 **屏蔽属性**。

2. 如果 **在 [[Prototype]] 链上层存在 foo** ，但是 **它被标记为只读**（writable:false），那么 **无法修改已有属性或者在 myObject 上创建屏蔽属性** 。如果运行在严格模式下，代码会抛出一个错误。否则，这条赋值语句会被忽略。总之，**不会发生屏蔽** 。

3. 如果  **在 [[Prototype]] 链上层存在 foo**  并且 **它是一个 setter**（参见第 3 章），那就 **一定会调用这个 setter** 。**foo 不会被添加到（或者说屏蔽于）myObject，也不会重新定义 foo 这个 setter** 。

大多数开发者都认为如果向 [[Prototype]] 链上层已经存在的属性（[[Put]]）赋值，就一定会触发屏蔽，但是如你所见，三种情况中只有一种（第一种）是这样的。

如果你希望在第二种和第三种情况下也屏蔽 foo，那就不能使用 = 操作符来赋值，而是使用 **Object.defineProperty(..)**（参见第 3 章）来向 myObject 添加 foo。

> 第二种情况可能是最令人意外的，只读属性会阻止 [[Prototype]] 链下层隐式创建（屏蔽）同名属性。这样做主要是为了模拟类属性的继承。你可以把原型链上层的 foo 看作是父类中的属性，它会被 myObject 继承（复制），这样一来 myObject 中的 foo 属性也是只读，所以无法创建。但是一定要注意，实际上并不会发生类似的继承复制（参见第 4 章和第 5 章）。这看起来有点奇怪，**myObject 对象竟然会因为其他对象中有一个只读 foo 就不能包含 foo 属性** 。更奇怪的是，**这个限制只存在于 = 赋值中** ，**使用 Object.defineProperty(..) 并不会受到影响** 。

有些情况下会隐式产生屏蔽，一定要当心。思考下面的代码：

```js
var anotherObject = {
  a: 2
};

var myObject = Object.create(anotherObject);

anotherObject.a; // 2
myObject.a; // 2

anotherObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("a"); // false

myObject.a++; // 隐式屏蔽！

anotherObject.a; // 2
myObject.a; // 3

myObject.hasOwnProperty("a"); // true
```
尽管 myObject.a++ 看起来应该（通过委托）查找并增加 anotherObject.a 属性，但是别忘了 ++ 操作相当于 myObject.a = myObject.a + 1。因此 ++ 操作首先 **会通过 [[Prototype]]查找属性 a 并从 anotherObject.a 获取当前属性值 2** ，然后 **给这个值加 1** ，接着 **用 [[Put]]将值 3 赋给 myObject 中新建的屏蔽属性 a** ，天呐！

修改委托属性时一定要小心。如果想让 anotherObject.a 的值增加，唯一的办法是anotherObject.a++。

#### “类”

JavaScript 和面向类的语言不同，它并没有类来作为对象的抽象模式或者说蓝图。JavaScript 中只有对象。

实际上，JavaScript 才是真正应该被称为“面向对象”的语言，因为它是少有的可以 **不通过类，直接创建对象** 的语言。

在 JavaScript 中，类无法描述对象的行，（因为根本就不存在类！）对象直接定义自己的行为。再说一遍，**JavaScript 中只有对象** 。

##### “类”函数

这种奇怪的“类似类”的行为利用了函数的一种特殊特性：所有的函数默认都会拥有一个名为 prototype 的公有并且不可枚举（参见第 3 章）的属性，它会指向另一个对象：

```js
function Foo() {
  // ...
}
Foo.prototype; // { }
```
这个对象通常被称为 Foo 的原型，因为我们通过名为 Foo.prototype 的属性引用来访问它。

最直接的解释就是，这个对象是在调用 new Foo()（参见第 2 章）时创建的，最后会被（有点武断地）关联到这个“Foo 点 prototype”对象上。

我们来验证一下：

```js
function Foo() {
  // ...
}
var a = new Foo();
Object.getPrototypeOf( a ) === Foo.prototype; // true
```
调用 new Foo() 时会创建 a（具体的 4 个步骤参见第 2 章），其中的一步就是给 a 一个内部的 [[Prototype]] 链接，关联到 Foo.prototype 指向的那个对象。

在面向类的语言中，类可以被复制（或者说实例化）多次，就像用模具制作东西一样。我们在第 4 章中看到过，之所以会这样是因为实例化（或者继承）一个类就意味着“把类的行为复制到物理对象中”，对于每一个新实例来说都会重复这个过程。

但是在 JavaScript 中，并没有类似的复制机制。你 **不能创建一个类的多个实例** ，**只能创建多个对象** ，它们 **[[Prototype]] 关联的是同一个对象** 。但是 **在默认情况下并不会进行复制** ，因此 **这些对象之间并不会完全失去联系，它们是互相关联的** 。

**new Foo() 会生成一个新对象（我们称之为 a），这个新对象的内部链接 [[Prototype]] 关联的是 Foo.prototype 对象。**

最后我们得到了两个对象，它们之间互相关联，就是这样。我们并没有初始化一个类，实际上 **我们并没有从“类”中复制任何行为到一个对象中** ，**只是让两个对象互相关联** 。

实际上，绝大多数 JavaScript 开发者不知道的秘密是，new Foo() 这个函数调用实际上并没有直接创建关联，这个关联只是一个意外的副作用。 **new Foo() 只是间接完成了我们的目标** ：**一个关联到其他对象的新对象** 。

那么有没有 **更直接的方法** 来做到这一点呢？当然！功臣就是**Object.create(..)** 。

###### 关于名称

在 JavaScript 中，我们并 **不会将一个对象（“类”）复制到另一个对象**（“实例”），**只是将它们关联起来**。从视觉角度来说，[[Prototype]] 机制如下图所示，箭头从右到左，从下到上：

![[[Prototype]] 机制](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%5B%5BPrototype%5D%5D%20%E6%9C%BA%E5%88%B6.png)

这个机制通常被称为 **原型继承**（稍后我们会分析具体代码），它常常被视为动态语言版本的类继承。这个名称主要是为了对应面向类的世界中“继承”的意义，但是违背（写作违背，读作推翻）了动态脚本中对应的语义。

“继承”这个词会让人产生非常强的心理预期（参见第 4 章）。**仅仅在前面加上“原型”并不能区分出 JavaScript 中和类继承几乎完全相反的行为** ，因此在过去 20 年中造成了极大的误解。

因此我认为这个容易混淆的组合术语“原型继承”（以及使用其他面向类的术语比如“类”、“构造函数”、“实例”、“多态”，等等）严重影响了大家对于 JavaScript 机制真实原理的理解。

**继承意味着复制操作，JavaScript（默认）并不会复制对象属性** 。相反，**JavaScript 会在两个对象之间创建一个关联，这样一个对象就可以通过委托访问另一个对象的属性和函数** 。

**委托**（参见第 6 章）这个术语可以更加准确地描述 **JavaScript 中对象的关联机制**。

还有个偶尔会用到的 JavaScript 术语差异继承。基本原则是在描述对象行为时， **使用其不同于普遍描述的特质** 。举例来说，描述汽车时你会说汽车是有四个轮子的一种交通工具，但是你不会重复描述交通工具具备的通用特性（比如引擎）。

如果你把 **JavaScript 中对象的所有委托行为都归结到对象本身并且把对象看作是实物的话** ，那就（差不多）可以理解差异继承了。

但是和原型继承一样，差异继承会更多是你脑中构建出的模型，而非真实情况。它忽略了一个事实，那就是对象 B 实际上并不是被差异构造出来的，我们 **只是定义了 B 的一些指定特性** ，**其他没有定义的东西都变成了“洞”** 。而 **这些洞（或者说缺少定义的空白处）最终会被委托行为“填满”** 。

默认情况下，对象并不会像差异继承暗示的那样通过复制生成。因此，差异继承也不适合用来描述 JavaScript 的 [[Prototype]] 机制。

当然，如果你喜欢，完全可以使用差异继承这个术语，但是 **无论如何它只适用于你脑中的模型，并不符合引擎的真实行为** 。

##### “构造函数”

好了，回到之前的代码：

```js
function Foo() {
// ...
}
var a = new Foo();
```
到底是什么让我们认为 Foo 是一个“类”呢？

其中一个原因是我们看到了关键字 new，在面向类的语言中构造类实例时也会用到它。另一个原因是，看起来我们执行了类的构造函数方法，**Foo() 的调用方式很像初始化类时类构造函数的调用方式** 。

除了令人迷惑的“构造函数”语义外，Foo.prototype 还有另一个绝招。思考下面的代码：

```js
function Foo() {
  // ...
}
Foo.prototype.constructor === Foo; // true
var a = new Foo();
a.constructor === Foo; // true
```
**Foo.prototype 默认（在代码中第一行声明时！）有一个公有并且不可枚举的属性 .constructor** ，这个属性 **引用的是对象关联的函数**（本例中是 Foo）。此外，我们可以看到通过“构造函数”调用 new Foo() 创建的对象也有一个 .constructor 属性，指向“创建这个对象的函数”。

> 实际上 a 本身并没有 .constructor 属性。而且，虽然 a.constructor 确实指向 Foo 函数，但是这个属性并不是表示 a 由 Foo“构造”。

哦耶，好吧……按照 JavaScript 世界的惯例，“类”名首字母要大写，所以名字写作 Foo 而非 foo 似乎也提示它是一个“类”。显而易见，是吧 ?!

>这个惯例影响力非常大，以至于如果你用 new 来调用小写方法或者不用 new 调用首字母大写的函数，许多 JavaScript 开发者都会责怪你。这很令人吃惊，我们竟然会如此努力地维护 JavaScript 中（假）“面向类”的权力，尽管对于JavaScript 引擎来说首字母大写没有任何意义。

1. ###### 构造函数还是调用

实际上，Foo 和你程序中的其他函数没有任何区别。函数本身并不是构造函数，然而，当你在普通的函数调用前面加上 new 关键字之后，就会把这个函数调用变成一个“构造函数调用”。实际上，new 会劫持所有普通函数并用构造对象的形式来调用它。

举例来说：

```js
function NothingSpecial() {
console.log( "Don't mind me!" );
}
var a = new NothingSpecial();
// "Don't mind me!"
a; // {}
```
NothingSpecial 只是一个普通的函数，但是使用 new 调用时，它就会构造一个对象并赋值给 a，这看起来像是 new 的一个副作用（无论如何都会构造一个对象）。这个调用是一个构造函数调用，但是 NothingSpecial 本身并不是一个构造函数。

换句话说，在 JavaScript 中对于“构造函数”最准确的解释是，**所有带 new 的函数调用** 。

函数不是构造函数，但是当且仅当使用 new 时，函数调用会变成“构造函数调用”。

##### 技术

JavaScript 开发者绞尽脑汁想要模仿类的行为：

```js
function Foo(name) {
  this.name = name;
}
Foo.prototype.myName = function() {
  return this.name;
};
var a = new Foo("a");
var b = new Foo("b");
a.myName(); // "a"
b.myName(); // "b"
```
这段代码展示了另外两种“面向类”的技巧：

1. this.name = name 给每个对象（也就是 a 和 b，参见第 2 章中的 this 绑定）都添加了 .name 属性，有点像类实例封装的数据值。

2. Foo.prototype.myName = ... 可能个更有趣的技巧，它会给 Foo.prototype 对象添加一个属性（函数）。现在，a.myName() 可以正常工作，但是你可能会觉得很惊讶，这是什么原理呢？

在这段代码中，看起来似乎创建 a 和 b 时会把 Foo.prototype 对象复制到这两个对象中，然而事实并不是这样。

在本章开头介绍默认 [[Get]] 算法时我们介绍过 [[Prototype]] 链，以及当属性不直接存在于对象中时如何通过它来进行查找。

因此，在创建的过程中，**a 和 b 的内部 [[Prototype]] 都会关联到 Foo.prototype 上** 。当 a 和 b 中无法找到 myName 时，它会（通过委托，参见第 6 章）在Foo.prototype 上找到。

###### 回顾“构造函数”

之前讨论 .constructor 属性时我们说过，看起来 a.constructor === Foo 为真意味着 a 确实有一个指向 Foo 的 .constructor 属性，但是事实不是这样。

这是一个很不幸的误解。实际上，**.constructor 引用同样被委托给了 Foo.prototype** ，而 **Foo.prototype.constructor 默认指向 Foo** 。

把 .constructor 属性指向 Foo 看作是 a 对象由 Foo“构造”非常容易理解，但这只不过是一种虚假的安全感。**a.constructor 只是通过默认的 [[Prototype]] 委托指向 Foo** ，这和 “构造”毫无关系。相反，对于 .constructor 的错误理解很容易对你自己产生误导。

举例来说，**Foo.prototype 的 .constructor 属性只是 Foo 函数在声明时的默认属性** 。如果 **你创建了一个新对象并替换了函数默认的 .prototype 对象引用** ，那么**新对象并不会自动获得 .constructor 属性** 。

思考下面的代码：

```js
function Foo() {
  /* .. */
}
Foo.prototype = {
  /* .. */
}; // 创建一个新原型对象
var a1 = new Foo();
a1.constructor === Foo; // false!
a1.constructor === Object; // true!
```
Object(..) 并没有“构造”a1，对吧？看起来应该是 Foo()“构造”了它。大部分开发者都认为是 Foo() 执行了构造工作，但是问题在于，如果你认为“constructor”表示“由……构造”的话，a1.constructor 应该是 Foo，但是它并不是 Foo ！

到底怎么回事？ a1 并没有 .constructor 属性，所以它会委托 [[Prototype]] 链上的 Foo.prototype。但是这个对象也没有 .constructor 属性（不过默认的 Foo.prototype 对象有这个属性！），所以它会继续委托，这次会委托给委托链顶端的 Object.prototype。这个对象有 .constructor 属性，指向内置的 Object(..) 函数。

当然，你可以给 Foo.prototype 添加一个 .constructor 属性，不过这需要手动添加一个符合正常行为的不可枚举（参见第 3 章）属性。

举例来说：

```js
function Foo() {
  /* .. */
}
Foo.prototype = {
  /* .. */
}; // 创建一个新原型对象

// 需要在 Foo.prototype 上“修复”丢失的 .constructor 属性
// 新对象属性起到 Foo.prototype 的作用
// 关于 defineProperty(..)，参见第 3 章
Object.defineProperty(Foo.prototype, "constructor", {
  enumerable: false,
  writable: true,
  configurable: true,
  value: Foo // 让 .constructor 指向 Foo
});
```
修复 .constructor 需要很多手动操作。

实际上，**对象的 .constructor 会默认指向一个函数，这个函数可以通过对象的 .prototype引用** 。“constructor”和“prototype”这两个词本身的含义可能适用也可能不适用。最好的办法是记住这一点 **“constructor 并不表示被构造”** 。

**.constructor 并不是一个不可变属性** 。它是不可枚举（参见上面的代码）的，但是它的值是可写的（可以被修改）。此外，你可以给任意 [[Prototype]] 链中的任意对象添加一个名为 constructor 的属性或者对其进行修改，你可以任意对其赋值。

和 [[Get]] 算法查找 [[Prototype]] 链的机制一样，.constructor 属性引用的目标可能和你想的完全不同。

结论？一些随意的对象属性引用，比如 a1.constructor，实际上是不被信任的，它们不一定会指向默认的函数引用。此外，很快我们就会看到，稍不留神 a1.constructor 就可能会指向你意想不到的地方。

a1.constructor 是一个非常不可靠并且不安全的引用。通常来说要尽量避免使用这些引用。

#### （原型）继承

实际上，我们已经了解了通常被称作原型继承的机制，a 可以“继承”Foo.prototype 并访问 Foo.prototype 的 myName() 函数。但是之前我们只把继承看作是类和类之间的关系，并没有把它看作是类和实例之间的关系：

![原型继承](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%5B%5BPrototype%5D%5D%20%E6%9C%BA%E5%88%B6.png)

还记得这张图吗，它不仅展示出 **对象（实例）a1 到 Foo.prototype 的委托关系** ，还展示出 **Bar.prototype 到 Foo.prototype 的委托关系** ，而后者和类继承很相似，只有箭头的方向不同。图中由下到上的箭头表明这是委托关联，不是复制操作。

下面这段代码使用的就是典型的“原型风格”：

```js
function Foo(name) {
  this.name = name;
}

Foo.prototype.myName = function() {
  return this.name;
};

function Bar(name, label) {
  Foo.call(this, name);
  this.label = label;
}
// 我们创建了一个新的 Bar.prototype 对象并关联到 Foo.prototype
Bar.prototype = Object.create(Foo.prototype);

// 注意！现在没有 Bar.prototype.constructor 了
// 如果你需要这个属性的话可能需要手动修复一下它

Bar.prototype.myLabel = function() {
  return this.label;
};

var a = new Bar("a", "obj a");

a.myName(); // "a"
a.myLabel(); // "obj a"
```
这段代码的核心部分就是语句 Bar.prototype = Object.create( Foo.prototype )。调用 **Object.create(..)** 会凭空 **创建一个“新”对象** 并 **把新对象内部的 [[Prototype]] 关联到你指定的对象**（本例中是 Foo.prototype）。

换句话说，这条语句的意思是：“创建一个新的 Bar.prototype 对象并把它关联到 Foo.prototype”。声明 function Bar() { .. } 时，和其他函数一样，Bar 会有一个 .prototype 关联到默认的对象，但是这个对象并不是我们想要的 Foo.prototype。因此我们创建了一个新对象并把它关联到我们希望的对象上，直接把原始的关联对象抛弃掉。

注意，下面这 **两种方式是常见的错误做法** ，实际上它们都存在一些问题：

```js
// 和你想要的机制不一样！
Bar.prototype = Foo.prototype;
// 基本上满足你的需求，但是可能会产生一些副作用 :(
Bar.prototype = new Foo();
```
Bar.prototype = Foo.prototype 并不会创建一个关联到 Bar.prototype 的新对象，**它只是让 Bar.prototype 直接引用 Foo.prototype 对象** 。因此当你执行类似Bar.prototype.myLabel = ... 的赋值语句时会直接修改 Foo.prototype 对象本身。显然这不是你想要的结果，否则你根本不需要 Bar 对象，直接使用 Foo 就可以了，这样代码也会更简单一些。

Bar.prototype = new Foo() 的确会创建一个关联到 Bar.prototype 的新对象。但是 **它使用了 Foo(..) 的“构造函数调用”** ，如果 **函数 Foo 有一些副作用**（比如写日志、修改状态、注册到其他对象、给 this 添加数据属性，等等）的话，就 **会影响到 Bar() 的“后代”** ，后果不堪设想。

因此，要创建一个合适的关联对象，我们必须使用 **Object.create(..)** 而不是使用具有副作用的 Foo(..)。这样做唯一的缺点就是 **需要创建一个新对象然后把旧对象抛弃掉，不能直接修改已有的默认对象。**

如果能有一个标准并且可靠的方法来修改对象的 [[Prototype]] 关联就好了。在 ES6 之前，我们只能通过 **设置 .__proto__ 属性** 来实现，但是这个方法 **并不是标准并且无法兼容所有浏览器** 。ES6 添加了辅助函数 **Object.setPrototypeOf(..)** ，可以 **用标准并且可靠的方法来修改关联** 。

我们来对比一下两种把 Bar.prototype 关联到 Foo.prototype 的方法：

```js
// ES6 之前需要抛弃默认的 Bar.prototype
Bar.ptototype = Object.create( Foo.prototype );
// ES6 开始可以直接修改现有的 Bar.prototype
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```
如果忽略掉 Object.create(..) 方法带来的轻微性能损失（抛弃的对象需要进行垃圾回
收），它实际上比 ES6 及其之后的方法更短而且可读性更高。不过无论如何，这是两种完全不同的语法。

##### 检查“类”关系

假设有对象 a，如何寻找对象 a 委托的对象（如果存在的话）呢？在传统的面向类环境中，**检查一个实例（JavaScript 中的对象）的继承祖先**（JavaScript 中的委托关联）通常被称为内省（或者反射）。

思考下面的代码：

```js
function Foo() {
  // ...
}

Foo.prototype.blah = ...;

var a = new Foo();
```
我们如何 **通过内省找出 a 的“祖先”**（委托关联）呢？第一种方法是站在“类”的角度来判断：

```
a instanceof Foo; // true
```
instanceof 操作符的左操作数是一个普通的对象，右操作数是一个函数。instanceof 回答的问题是：在 a 的整条 [[Prototype]] 链中是否有指向 Foo.prototype 的对象？
可惜，这个方法 **只能处理对象（a）和函数（带 .prototype 引用的 Foo）之间的关系** 。如果 **你想判断两个对象（比如 a 和 b）之间是否通过 [[Prototype]] 链关联，只用 instanceof 无法实现** 。

> 如果使用内置的 .bind(..) 函数来生成一个硬绑定函数（参见第 2 章）的话，该函数是没有 .prototype 属性的。在这样的函数上使用 instanceof 的话，目标函数的 .prototype 会代替硬绑定函数的 .prototype。

> 通常我们不会在“构造函数调用”中使用硬绑定函数，不过如果你这么做的话，实际上相当于直接调用目标函数。同理，在硬绑定函数上使用instanceof 也相当于直接在目标函数上使用 instanceof。

下面这段荒谬的代码试图站在“类”的角度使用 instanceof 来判断两个对象的关系：

```js
// 用来判断 o1 是否关联到（委托）o2 的辅助函数
function isRelatedTo(o1, o2) {
  function F() {}
  F.prototype = o2;
  return o1 instanceof F;
}

var a = {};
var b = Object.create(a);

isRelatedTo(b, a); // true
```
在 isRelatedTo(..) 内部我们声明了一个一次性函数 F，把它的 .prototype 重新赋值并指向对象 o2，然后判断 o1 是否是 F 的一个“实例”。显而易见，o1 实际上并没有继承 F 也不是由 F 构造，所以这种方法非常愚蠢并且容易造成误解。问题的关键在于思考的角度，**强行在 JavaScript 中应用类的语义（在本例中就是使用 instanceof）就会造成这种尴尬的局面** 。

下面是第二种判断 [[Prototype]] 反射的方法，它更加简洁：

```js
Foo.prototype.isPrototypeOf( a ); // true
```
注意，在本例中，我们实际上并不关心（甚至不需要）Foo，我们只需要一个可以用来判
断的对象（本例中是 Foo.prototype）就行。isPrototypeOf(..) 回答的问题是：**在 a 的整条 [[Prototype]] 链中是否出现过 Foo.prototype ？**

同样的问题，同样的答案，但是在第二种方法中并 **不需要间接引用函数**（Foo），**它的 .prototype 属性会被自动访问**。

我们只需要两个对象就可以判断它们之间的关系。举例来说：

```js
// 非常简单：b 是否出现在 c 的 [[Prototype]] 链中？
b.isPrototypeOf( c );
```
注意，这个方法并不需要使用函数（“类”），它直接使用 b 和 c 之间的对象引用来判断它们的关系。换句话说，语言内置的 isPrototypeOf(..) 函数就是我们的isRelatedTo(..) 函数。

我们也可以 **直接获取一个对象的 [[Prototype]] 链** 。在 ES5 中，标准的方法是：

```js
Object.getPrototypeOf( a );
```

可以验证一下，这个对象引用是否和我们想的一样：

```js
Object.getPrototypeOf( a ) === Foo.prototype; // true
```
绝大多数（不是所有！）浏览器也支持一种非标准的方法来访问内部 [[Prototype]] 属性：

```js
a.__proto__ === Foo.prototype; // true
```
这个奇怪的 .__proto__（在 ES6 之前并不是标准！）属性“神奇地”引用了内部的
[[Prototype]] 对象，如果你想直接查找（甚至可以通过 .__proto__.__ptoto__... 来遍历）原型链的话，这个方法非常有用。

和我们之前说过的 .constructor 一样，.__proto__ 实际上并不存在于你正在使用的对象中（本例中是 a）。实际上，它和其他的常用函数（.toString()、.isPrototypeOf(..)，等等）一样，**存在于内置的 Object.prototype 中** 。

此外，.__proto__ 看起来很像一个属性，但是实际上它更像一个 getter/setter。

.__proto__ 的实现大致上是这样的：

```js
Object.defineProperty(Object.prototype, "__proto__", {
  get: function() {
    return Object.getPrototypeOf(this);
  },
  set: function(o) {
    // ES6 中的 setPrototypeOf(..)
    Object.setPrototypeOf(this, o);
    return o;
  }
});
```
因此，访问（获取值）a.__proto__ 时，实际上是调用了 a.__proto__()（调用 getter 函数）。虽然 getter 函数存在于 Object.prototype 对象中，但是它的 this 指向对象 a（this的绑定规则参见第 2 章），所以和 Object.getPrototypeOf( a ) 结果相同。

**.__proto__ 是可设置属性** ，之前的代码中使用 ES6 的 **Object.setPrototypeOf(..)** 进行设置。然而，通常来说你不需要修改已有对象的 [[Prototype]]。

> ES6 中的 class 关键字可以在内置的类型（比如 Array）上实现类似“子类”的功能。

我们只有在一些特殊情况下（我们前面讨论过）需要设置函数默认 .prototype 对象的
[[Prototype]]，让它引用其他对象（除了 Object.prototype）。这样可以避免使用全新的对象替换默认对象。此外，最好把 [[Prototype]] 对象关联看作是只读特性，从而增加代码的可读性。

>JavaScript 社区中对于双下划线有一个非官方的称呼，他们会把类似 __proto__的属性称为“笨蛋（dunder）”。所以，JavaScript 潮人会把 __proto__ 叫作“笨蛋 proto”。

#### 对象关联

[[Prototype]] 机制就是存在于对象中的一个内部链接，它会引用其他对象。

通常来说，这个链接的作用是：如果在对象上没有找到需要的属性或者方法引用，引擎就会继续在 [[Prototype]] 关联的对象上进行查找。同理，如果在后者中也没有找到需要的引用就会继续查找它的 [[Prototype]]，以此类推。这一系列对象的链接被称为“原型链”。

##### 创建关联

还记得吗，本章前面曾经说过 Object.create(..) 是一个大英雄，现在是时候来弄明白为什么了：

```js
var foo = {
  something: function() {
    console.log("Tell me something good...");
  }
};
var bar = Object.create(foo);
bar.something(); // Tell me something good...
```
Object.create(..) 会创建一个新对象（bar）并把它关联到我们指定的对象（foo），这样我们就可以充分发挥 [[Prototype]] 机制的威力（委托）并且避免不必要的麻烦（比如 **使用 new 的构造函数调用会生成 .prototype 和 .constructor 引用** ）。

> **Object.create(null)** 会创建一个 **拥有空（ 或 者 说 null）[[Prototype]]链接的对象** ，这个对象无法进行委托。由于这个 **对象没有原型链** ，所以 **instanceof 操作符（之前解释过）无法进行判断** ，因此总是会返回 false。

**这些特殊的空 [[Prototype]] 对象通常被称作“字典”**，它们 **完全不会受到原型链的干扰** ，因此 **非常适合用来存储数据** 。

我们并不需要类来创建两个对象之间的关系，**只需要通过委托来关联对象就足够了** 。而Object.create(..) 不包含任何“类的诡计”，所以它可以完美地创建我们想要的关联关系。

###### Object.create()的polyfill代码

Object.create(..) 是在 ES5 中新增的函数，所以在 ES5 之前的环境中（比如旧 IE）如果要支持这个功能的话就需要使用一段简单的 polyfill 代码，它部分实现了 Object.
create(..) 的功能：

```js
if (!Object.create) {
  Object.create = function(o) {
    function F() {}
    F.prototype = o;
    return new F();
  };
}
```
这段 polyfill 代码使用了一个一次性函数 F，我们通过改写它的 .prototype 属性使其指向想要关联的对象，然后再使用 new F() 来构造一个新对象进行关联。

由于 Object.create(..c) 可以被模拟，因此这个函数被应用得非常广泛。标准 ES5 中内置的 Object.create(..) 函数还提供了一系列附加功能，但是 ES5 之前的版本不支持这些功能。通常来说，这些功能的应用范围要小得多，但是出于完整性考虑，我们还是介绍一下：

```js
var anotherObject = {
  a: 2
};

var myObject = Object.create(anotherObject, {
  b: {
    enumerable: false,
    writable: true,
    configurable: false,
    value: 3
  },
  c: {
    enumerable: true,
    writable: false,
    configurable: false,
    value: 4
  }
});

myObject.hasOwnProperty("a"); // false
myObject.hasOwnProperty("b"); // true
myObject.hasOwnProperty("c"); // true

myObject.a; // 2
myObject.b; // 3
myObject.c; // 4
```
Object.create(..) 的第二个参数指定了需要添加到新对象中的属性名以及这些属性的属性描述符（参见第 3 章）。因为 ES5 之前的版本无法模拟属性操作符，所以 polyfill 代码无法实现这个附加功能。

通常来说并不会使用 Object.create(..) 的附加功能，所以对于大多数开发者来说，上面那段 polyfill 代码就足够了。

有些开发者更加严谨，他们认为只有能被完全模拟的函数才应该使用 polyfill 代码。由于Object.create(..) 是只能部分模拟的函数之一，所以这些狭隘的人认为如果你需要在 ES5之前的环境中使用 Object.create(..) 的特性，那不要使用 polyfill 代码，而是使用一个自定义函数并且名字不能是 Object.create。你可以把你自己的函数定义成这样：

```js
function createAndLinkObject(o) {
  function F() {}
  F.prototype = o;
  return new F();
}

var anotherObject = {
  a: 2
};

var myObject = createAndLinkObject(anotherObject);

myObject.a; // 2
```
##### 关联关系是备用

看起来对象之间的关联关系是处理“缺失”属性或者方法时的一种备用选项。这个说法有
点道理，但是我认为这并不是 [[Prototype]] 的本质。

思考下面的代码：

```js
var anotherObject = {
  cool: function() {
    console.log("cool!");
  }
};
var myObject = Object.create(anotherObject);
myObject.cool(); // "cool!"
```
由于存在 [[Prototype]] 机制，这段代码可以正常工作。但是如果你这样写只是为了让
myObject 在无法处理属性或者方法时可以使用备用的 anotherObject，那么你的软件就会变得有点“神奇”，而且很难理解和维护。

这并不是说任何情况下都不应该选择备用这种设计模式，但是这在 JavaScript 中并不是很常见。所以如果你使用的是这种模式，那或许应当退后一步并重新思考一下这种模式是否合适。

>  在 ES6 中有一个被称为“代理”（Proxy）的高端功能，它实现的就是“方法无法找到”时的行为。

千万不要忽略这个微妙但是非常重要的区别。

当你给开发者设计软件时，假设要调用 myObject.cool()，如果 myObject 中不存在 cool()时这条语句也可以正常工作的话，那你的 API 设计就会变得很“神奇”，对于未来维护你软件的开发者来说这可能不太好理解。

但是你可以让你的 API 设计不那么“神奇”，同时仍然能发挥 [[Prototype]] 关联的威力：

```js
var anotherObject = {
  cool: function() {
    console.log("cool!");
  }
};
var myObject = Object.create(anotherObject);
myObject.doCool = function() {
  this.cool(); // 内部委托！
};
myObject.doCool(); // "cool!"
```
这里我们调用的 myObject.doCool() 是实际存在于 myObject 中的，这可以让我们的 API 设计更加清晰（不那么“神奇”）。从内部来说，我们的实现遵循的是委托设计模式（参见第6 章），通过 [[Prototype]] 委托到 anotherObject.cool()。

换句话说，**内部委托比起直接委托可以让 API 接口设计更加清晰** 。

#### 小结

如果要访问对象中并不存在的一个属性，[[Get]] 操作（参见第 3 章）就会查找对象内部[[Prototype]] 关联的对象。这个关联关系实际上定义了一条“原型链”（有点像嵌套的作用域链），在查找属性时会对它进行遍历。

所有普通对象都有内置的 Object.prototype，指向原型链的顶端（比如说全局作域），如果在原型链中找不到指定的属性就会停止。toString()、valueOf() 和其他一些通用的功能都存在于 Object.prototype 对象上，因此语言中所有的对象都可以使用它们。

关联两个对象最常用的方法是使用 new 关键词进行函数调用，在调用的 4 个步骤（第 2章）中会创建一个关联其他对象的新对象。

使用 new 调用函数时会把新对象的 .prototype 属性关联到“其他对象”。带 new 的函数调用通常被称为“构造函数调用”，尽管它们实际上和传统面向类语言中的类构造函数不一样。

虽然这些 JavaScript 机制和传统面向类语言中的“类初始化”和“类继承”很相似，但是 JavaScript 中的机制有一个核心区别，那就是不会进行复制，对象之间是通过内部的[[Prototype]] 链关联的。

出于各种原因，以“继承”结尾的术语（包括“原型继承”）和其他面向对象的术语都无法帮助你理解 JavaScript 的真实机制（不仅仅是限制我们的思维模式）。

相比之下，“委托”是一个更合适的术语，因为对象之间的关系不是复制而是委托。

### 行为委托

[[Prototype]] 机制就是指对象中的一个内部链接引用另一个对象。

如果在第一个对象上没有找到需要的属性或者方法引用，引擎就会继续在 [[Prototype]]关联的对象上进行查找。同理，如果在后者中也没有找到需要的引用就会继续查找它的[[Prototype]]，以此类推。这一系列对象的链接被称为“原型链”。

换句话说，JavaScript 中这个机制的本质就是对象之间的关联关系。

![行为委托](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E8%A1%8C%E4%B8%BA%E5%A7%94%E6%89%98.png)

#### 面向委托的设计

##### 类理论

假设我们需要在软件中建模一些类似的任务（“XYZ”、“ABC”等）。

如果使用类，那设计方法可能是这样的：定义一个通用父（基）类，可以将其命名为Task，在 Task 类中定义所有任务都有的行为。接着定义子类 XYZ 和 ABC，它们都继承自Task 并且会添加一些特殊的行为来处理对应的任务。

非常重要的是，**类设计模式鼓励你在继承时使用方法重写**（和多态），比如说在 XYZ 任务中重写 Task 中定义的一些通用方法，甚至在添加新行为时通过 super 调用这个方法的原始版本。你会发现许多行为可以先“抽象”到父类然后再用子类进行特殊化（重写）。

下面是对应的伪代码：

```js
class Task {
  id;
  // 构造函数 Task()
  Task(ID) {
    id = ID;
  }
  outputTask() {
    output(id);
  }
}
class XYZ inherits Task {
  label;
  // 构造函数 XYZ()
  XYZ(ID, Label) {
    super(ID);
    label = Label;
  }
  outputTask() {
    super();
    output(label);
  }
}
class ABC inherits Task {
  // ...
}
```
现在你可以实例化子类 XYZ 的一些副本然后使用这些实例来执行任务“XYZ”。这些实例会复制 Task 定义的通用行为以及 XYZ 定义的特殊行为。同理，ABC 类的实例也会复制 Task的行为和 ABC 的行为。在构造完成后，你通常只需要操作这些实例（而不是类），因为每个实例都有你需要完成任务的所有行为。

##### 委托理论

首先你会定义一个名为 Task 的对象（和许多 JavaScript 开发者告诉你的不同，它既不是类也不是函数），它会包含所有任务都可以使用（写作使用，读作委托）的具体行为。接着，对于每个任务（“XYZ”、“ABC”）你都会定义一个对象来存储对应的数据和行为。你会把特定的任务对象都关联到 Task 功能对象上，让它们在需要的时候可以进行委托。

基本上你可以想象成，执行任务“XYZ”需要两个兄弟对象（XYZ 和 Task）协作完成。但
是我们并不需要把这些行为放在一起，通过类的复制，我们可以把它们分别放在各自独立
的对象中，需要时可以允许 XYZ 对象委托给 Task。

下面是推荐的代码形式，非常简单：

```js
Task = {
  setID: function(ID) {
    this.id = ID;
  },
  outputID: function() {
    console.log(this.id);
  }
};
// 让 XYZ 委托 Task
XYZ = Object.create(Task);
XYZ.prepareTask = function(ID, Label) {
  this.setID(ID);
  this.label = Label;
};
XYZ.outputTaskDetails = function() {
  this.outputID();
  console.log(this.label);
};
// ABC = Object.create( Task );
// ABC ... = ...
```
在 这 段 代 码 中，Task 和 XYZ 并 不 是 类（ 或 者 函 数 ）， 它 们 是 对 象。XYZ 通 过 Object.create(..) 创建，它的 [[Prototype]] 委托了 Task 对象。

相比于面向类（或者说面向对象），我会把这种编码风格称为“**对象关联**”（OLOO，objects linked to other objects）。我们真正关心的只是 XYZ 对象（和 ABC 对象）委托了Task 对象。

在 JavaScript 中，[[Prototype]] 机制会把对象关联到其他对象。

**对象关联风格** 的代码还有一些 **不同之处** 。

1. 在上面的代码中，id 和 label 数据成员都是直接存储在 XYZ 上（而不是 Task）。通常来说，在 [[Prototype]] 委托中最好 **把状态保存在委托者（XYZ、ABC）而不是委托目标（Task）上** 。

2. 在类设计模式中，我们故意让父类（Task）和子类（XYZ）中都有 outputTask 方法，这样就可以利用重写（多态）的优势。在委托行为中则恰好相反：我们会 **尽量避免在[[Prototype]] 链的不同级别中使用相同的命名** ，否则就需要使用笨拙并且脆弱的语法来消除引用歧义（参见第 4 章）。

3. **this.setID(ID)；XYZ 中的方法首先会寻找 XYZ 自身是否有 setID(..)，但是 XYZ 中并没有这个方法名，因此会通过 [[Prototype]] 委托关联到 Task 继续寻找，这时就可以找到setID(..) 方法。** 此外，由于调用位置触发了 this 的隐式绑定规则（参见第 2 章），因此虽然 setID(..) 方法在 Task 中，运行时 this 仍然会绑定到 XYZ，这正是我们想要的。

在之后的代码中我们还会看到 this.outputID()，原理相同。

换句话说，我们和 XYZ 进行交互时可以使用 Task 中的通用方法，因为 XYZ 委托了 Task。

委托行为意味着某些对象（XYZ）在找不到属性或者方法引用时会把这个请求委托给另一个对象（Task）。

这是一种极其强大的设计模式，和父类、子类、继承、多态等概念完全不同。在你的脑海中对象并不是按照父类到子类的关系垂直组织的，而是 **通过任意方向的委托关联并排组织的** 。

> 在 API 接口的设计中，委托最好在内部实现，不要直接暴露出去。在之前的例子中我们并没有让开发者通过 API 直接调用 XYZ.setID()。（当然，可以这么做！）相反，我们把委托隐藏在了 API 的内部，XYZ.prepareTask(..) 会委托 Task.setID(..)。

1. 互相委托（禁止）

你无法在两个或两个以上互相（双向）委托的对象之间创建循环委托。如果你把 B 关联到 A 然后试着把 A 关联到 B，就会出错。

很遗憾（并不是非常出乎意料，但是有点烦人）这种方法是被禁止的。如果你引用了一个两边都不存在的属性或者方法，那就会在 [[Prototype]] 链上产生一个无限递归的循环。

但是如果所有的引用都被严格限制的话，B 是可以委托 A 的，反之亦然。因此，互相委托理论上是可以正常工作的，在某些情况下这是非常有用的。

之所以要禁止互相委托，是因为引擎的开发者们发现在设置时检查（并禁止！）一次无限循环引用要更加高效，否则每次从对象中查找属性时都需要进行检查。

2. 调试

我们来简单介绍一个容易让开发者感到迷惑的细节。通常来说，JavaScript 规范并不会控制浏览器中开发者工具对于特定值或者结构的表示方式，浏览器和引擎可以自己选择合适的方式来进行解析，因此浏览器和工具的解析结果并不一定相同。

##### 比较思维模型

我们会通过一些示例（Foo、Bar）代码来比较一下两种设计模式（面向对象和对象关联）具体的实现方法。下面是典型的（“原型”）面向对象风格：

```js
function Foo(who) {
  this.me = who;
}

Foo.prototype.identify = function() {
  return "I am " + this.me;
};

function Bar(who) {
  Foo.call(this, who);
}
Bar.prototype = Object.create(Foo.prototype);

Bar.prototype.speak = function() {
  alert("Hello, " + this.identify() + ".");
};

var b1 = new Bar("b1");
var b2 = new Bar("b2");

b1.speak();
b2.speak();
```
子类 Bar 继承了父类 Foo，然后生成了 b1 和 b2 两个实例。b1 委托了Bar.prototype，后者委托了 Foo.prototype。这种风格很常见，你应该很熟悉了。

下面我们看看如何使用对象关联风格来编写功能完全相同的代码：

```js
Foo = {
  init: function(who) {
    this.me = who;
  },
  identify: function() {
    return "I am " + this.me;
  }
};

Bar = Object.create(Foo);

Bar.speak = function() {
  alert("Hello, " + this.identify() + ".");
};

var b1 = Object.create(Bar);
b1.init("b1");
var b2 = Object.create(Bar);
b2.init("b2");

b1.speak();
b2.speak();
```
这段代码中我们同样利用 [[Prototype]] 把 b1 委托给 Bar 并把 Bar 委托给 Foo，和上一段代码一模一样。我们仍然实现了三个对象之间的关联。

但是非常重要的一点是，这段代码简洁了许多，我们只是把对象关联起来，并不需要那些既复杂又令人困惑的模仿类的行为（构造函数、原型以及 new）。

下面我们看看两段代码对应的思维模型。

首先，类风格代码的思维模型强调实体以及实体间的关系：

![类风格代码的思维模型](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E7%B1%BB%E9%A3%8E%E6%A0%BC%E4%BB%A3%E7%A0%81%E7%9A%84%E6%80%9D%E7%BB%B4%E6%A8%A1%E5%9E%8B%E5%BC%BA.png)

实际上这张图有点不清晰 / 误导人，因为它还展示了许多技术角度不需要关注的细节（但是你必须理解它们）！从图中可以看出这是一张十分复杂的关系网。此外，如果你跟着图中的箭头走就会发现，JavaScript 机制有很强的内部连贯性。

举例来说，JavaScript 中的函数之所以可以访问 call(..)、apply(..) 和 bind(..)（参见第 2 章），就是因为函数本身是对象。而函数对象同样有 [[Prototype]] 属性并且关联到 Function.prototype 对象，因此所有函数对象都可以通过委托调用这些默认方法。JavaScript 能做到这一点，你也可以！

好，下面我们来看一张简化版的图，它更“清晰”一些——只展示了必要的对象和关系：

![简化版的图](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E7%AE%80%E5%8C%96%E7%89%88%E7%9A%84%E5%9B%BE%EF%BC%8C.png)

仍然很复杂，是吧？虚线表示的是 Bar.prototype 继承 Foo.prototype 之后丢失的 .constructor属性引用（参见 5.2.3 节的“回顾‘构造函数’”部分），它们还没有被修复。即使移除这些虚线，这个思维模型在你处理对象关联时仍然非常复杂。

现在我们看看对象关联风格代码的思维模型：

![对象关联风格代码的思维模型](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E5%AF%B9%E8%B1%A1%E5%85%B3%E8%81%94%E9%A3%8E%E6%A0%BC%E4%BB%A3%E7%A0%81%E7%9A%84%E6%80%9D%E7%BB%B4%E6%A8%A1%E5%9E%8B.png)

通过比较可以看出，对象关联风格的代码显然更加简洁，因为这种代码只关注一件事：对象之间的关联关系。

其他的“类”技巧都是非常复杂并且令人困惑的。去掉它们之后，事情会变得简单许多（同时保留所有功能）。

#### 类与对象

##### 控件“类”

你可能已经习惯了面向对象设计模式，所以很快会想到一个包含所有通用控件行为的父类
（可能叫作 Widget）和继承父类的特殊控件子类（比如 Button）。

下面这段代码展示的是如何在不使用任何“类”辅助库或者语法的情况下，使用纯
JavaScript 实现类风格的代码：

```js
// 父类
function Widget(width, height) {
  this.width = width || 50;
  this.height = height || 50;
  this.$elem = null;
}

Widget.prototype.render = function($where) {
  if (this.$elem) {
    this.$elem.css({
      width: this.width + "px",
      height: this.height + "px"
    }).appendTo($where);
  }
};

// 子类
function Button(width, height, label) {
  // 调用“super”构造函数
  Widget.call(this, width, height);
  this.label = label || "Default";
  this.$elem = $("<button>").text(this.label);
}

// 让 Button“继承”Widget
Button.prototype = Object.create(Widget.prototype);

// 重写 render(..)
Button.prototype.render = function($where) {
  //“super”调用
  Widget.prototype.render.call(this, $where);
  this.$elem.click(this.onClick.bind(this));
};

Button.prototype.onClick = function(evt) {
  console.log("Button '" + this.label + "' clicked!");
};

$(document).ready(function() {
  var $body = $(document.body);
  var btn1 = new Button(125, 30, "Hello");
  var btn2 = new Button(150, 40, "World");
  btn1.render($body);
  btn2.render($body);
});
```
在面向对象设计模式中我们需要先在父类中定义基础的 render(..)，然后在子类中重写
它。子类并不会替换基础的 render(..)，只是添加一些按钮特有的行为。

可以看到代码中出现了丑陋的显式伪多态（参见第 4 章），即通过 Widget.call 和 Widget.prototype.render.call 从“子类”方法中引用“父类”中的基础方法。呸！

##### ES6的class语法糖

附录 A 会详细介绍 ES6 的 class 语法糖，不过这里可以简单介绍一下如何使用 class 来实现相同的功能：

```js
class Widget {
  constructor(width, height) {
    this.width = width || 50;
    this.height = height || 50;
    this.$elem = null;
  }
  render($where) {
    if (this.$elem) {
      this.$elem.css({
        width: this.width + "px",
        height: this.height + "px"
      }).appendTo($where);
    }
  }
}

class Button extends Widget {
  constructor(width, height, label) {
    super(width, height);
    this.label = label || "Default";
    this.$elem = $("<button>").text(this.label);
  }
  render($where) {
    super($where);
    this.$elem.click(this.onClick.bind(this));
  }
  onClick(evt) {
    console.log("Button '" + this.label + "' clicked!");
  }
}

$(document).ready(function() {
  var $body = $(document.body);
  var btn1 = new Button(125, 30, "Hello");
  var btn2 = new Button(150, 40, "World");
  btn1.render($body);
  btn2.render($body);
});
```
毫无疑问，使用 ES6 的 class 之后，上一段代码中许多丑陋的语法都不见了，super(..)函数棒极了。（尽管深入探究就会发现并不是那么完美！）

尽管语法上得到了改进，但实际上这里并没有真正的类，class 仍然是通过 [[Prototype]]机制实现的，因此我们仍然面临第 4 章至第 6 章提到的思维模式不匹配问题。附录 A 会详细介绍 ES6 的 class 语法及其实现细节，我们会看到为什么解决语法上的问题无法真正解除对于 JavaScript 中类的误解，尽管它看起来非常像一种解决办法！

无论你使用的是传统的原型语法还是 ES6 中的新语法糖，你仍然需要用“类”的概念来对问题（UI 控件）进行建模。

##### 委托控件对象

下面的例子使用对象关联风格委托来更简单地实现 Widget/Button：

```js
var Widget = {
  init: function(width, height) {
    this.width = width || 50;
    this.height = height || 50;
    this.$elem = null;
  },
  insert: function($where) {
    if (this.$elem) {
      this.$elem.css({
        width: this.width + "px",
        height: this.height + "px"
      }).appendTo($where);
    }
  }
};

var Button = Object.create(Widget);

Button.setup = function(width, height, label) {
  // 委托调用
  this.init(width, height);
  this.label = label || "Default";
  this.$elem = $("<button>").text(this.label);
};

Button.build = function($where) {
  // 委托调用
  this.insert($where);
  this.$elem.click(this.onClick.bind(this));
};

Button.onClick = function(evt) {
  console.log("Button '" + this.label + "' clicked!");
};

$(document).ready(function() {
  var $body = $(document.body);
  var btn1 = Object.create(Button);
  btn1.setup(125, 30, "Hello");
  var btn2 = Object.create(Button);
  btn2.setup(150, 40, "World");
  btn1.build($body);
  btn2.build($body);
});
```
使用对象关联风格来编写代码时不需要把 Widget 和 Button 当作父类和子类。相反，Widget 只是一个对象，包含一组通用的函数，任何类型的控件都可以委托，Button 同样只是一个对。（当然，它会通过委托关联到 Widget ！）

从设计模式的角度来说，我们并没有像类一样在两个对象中都定义相同的方法名render(..)，相反，我们定义了两个更具描述性的方法名（insert(..) 和 build(..)）。同理，初始化方法分别叫作 init(..) 和 setup(..)。

在委托设计模式中，除了建议使用不相同并且更具描述性的方法名之外，还要通过对象关联避免丑陋的显式伪多态调用（Widget.call 和 Widget.prototype.render.call），代之以简单的相对委托调用 this.init(..) 和 this.insert(..)。

从语法角度来说，我们同样没有使用任何构造函数、.prototype 或 new，实际上也没必要使用它们。

如果你仔细观察就会发现，之前的一次调用（var btn1 = new Button(..)）现在变成了两次（var btn1 = Object.create(Button) 和 btn1.setup(..)）。乍一看这似乎是一个缺点（需要更多代码）。

**对象关联可以更好地支持关注分离**（separation of concerns）原则，创建和初始化并不需要合并为一个步骤。

#### 更简洁的设计

对象关联除了能让代码看起来更简洁（并且更具扩展性）外还可以通过行为委托模式简化
代码结构。我们来看最后一个例子，它展示了对象关联如何简化整体设计。

在传统的类设计模式中，我们会把基础的函数定义在名为 Controller 的类中，然后派生两个子类 LoginController 和 AuthController，它们都继承自 Controller 并且重写了一些基础行为：

```js
// 父类
function Controller() {
  this.errors = [];
}

Controller.prototype.showDialog(title, msg) {
  // 给用户显示标题和消息
};

Controller.prototype.success = function(msg) {
  this.showDialog("Success", msg);
};

Controller.prototype.failure = function(err) {
  this.errors.push(err);
  this.showDialog("Error", err);
};

// 子类
function LoginController() {
  Controller.call(this);
}

// 把子类关联到父类
LoginController.prototype = Object.create(Controller.prototype);
LoginController.prototype.getUser = function() {
  return document.getElementById("login_username").value;
};

LoginController.prototype.getPassword = function() {
  return document.getElementById("login_password").value;
};

LoginController.prototype.validateEntry = function(user, pw) {
  user = user || this.getUser();
  pw = pw || this.getPassword();
  if (! (user && pw)) {
    return this.failure("Please enter a username & password!");
  } else if (user.length < 5) {
    return this.failure("Password must be 5+ characters!");
  }
  // 如果执行到这里说明通过验证
  return true;
};

// 重写基础的 failure()
LoginController.prototype.failure = function(err) {
  //“super”调用
  Controller.prototype.failure.call(this, "Login invalid: " + err);
};

// 子类
function AuthController(login) {
  Controller.call(this);
  // 合成
  this.login = login;
}

// 把子类关联到父类
AuthController.prototype = Object.create(Controller.prototype);
AuthController.prototype.server = function(url, data) {
  return $.ajax({
    url: url,
    data: data
  });
};

AuthController.prototype.checkAuth = function() {
  var user = this.login.getUser();
  var pw = this.login.getPassword();
  if (this.login.validateEntry(user, pw)) {
    this.server("/check-auth", {
      user: user,
      pw: pw
    }).then(this.success.bind(this)).fail(this.failure.bind(this));
  }
};

// 重写基础的 success()
AuthController.prototype.success = function() {
  //“super”调用
  Controller.prototype.success.call(this, "Authenticated!");
};

// 重写基础的 failure()
AuthController.prototype.failure = function(err) {
  //“super”调用
  Controller.prototype.failure.call(this, "Auth Failed: " + err);
};

var auth = new AuthController();
auth.checkAuth(
  // 除了继承，我们还需要合成
  new LoginController()
);
```
所有控制器共享的基础行为是 success(..)、failure(..) 和 showDialog(..)。 子类LoginController 和 AuthController 通过重写 failure(..) 和 success(..) 来扩展默认基础类行为。此外，注意 AuthController 需要一个 LoginController 的实例来和登录表单进行交互，因此这个实例变成了一个数据属性。

另一个需要注意的是我们在继承的基础上进行了一些合成。AuthController 需要使用LoginController，因此我们实例化后者（new LoginController()）并用一个类成员属性this.login 来引用它，这样 AuthController 就可以调用 LoginController 的行为。

> 你可能想让 AuthController 继承 LoginController 或者相反，这样我们就通过继承链实现了真正的合成。但是这就是类继承在问题领域建模时会产生的问题，因为 AuthController 和 LoginController 都不具备对方的基础行为，所以这种继承关系是不恰当的。我们的解决办法是进行一些简单的合成从而让它们既不必互相继承又可以互相合作。

##### 反类

但是，我们真的需要用一个 Controller 父类、两个子类加上合成来对这个问题进行建模
吗？能不能使用对象关联风格的行为委托来实现更简单的设计呢？当然可以！

```js
var LoginController = {
  errors: [],
  getUser: function() {
    return document.getElementById("login_username").value;
  },
  getPassword: function() {
    return document.getElementById("login_password").value;
  },
  validateEntry: function(user, pw) {
    user = user || this.getUser();
    pw = pw || this.getPassword();
    if (! (user && pw)) {
      return this.failure("Please enter a username & password!");
    } else if (user.length < 5) {
      return this.failure("Password must be 5+ characters!");
    }
    // 如果执行到这里说明通过验证
    return true;
  },
  showDialog: function(title, msg) {
    // 给用户显示标题和消息
  },
  failure: function(err) {
    this.errors.push(err);
    this.showDialog("Error", "Login invalid: " + err);
  }
};

// 让 AuthController 委托 LoginController
var AuthController = Object.create(LoginController);
AuthController.errors = [];
AuthController.checkAuth = function() {
  var user = this.getUser();
  var pw = this.getPassword();
  if (this.validateEntry(user, pw)) {
    this.server("/check-auth", {
      user: user,
      pw: pw
    }).then(this.accepted.bind(this)).fail(this.rejected.bind(this));
  }
};

AuthController.server = function(url, data) {
  return $.ajax({
    url: url,
    data: data
  });
};

AuthController.accepted = function() {
  this.showDialog("Success", "Authenticated!")
};

AuthController.rejected = function(err) {
  this.failure("Auth Failed: " + err);
};
```
由于 AuthController 只是一个对象（LoginController 也一样），因此我们不需要实例化（比如 new AuthController()），只需要一行代码就行：

```js
AuthController.checkAuth();
```
借助对象关联，你可以简单地向委托链上添加一个或多个对象，而且同样不需要实例化：

```js
var controller1 = Object.create( AuthController );
var controller2 = Object.create( AuthController );
```
在行为委托模式中，AuthController 和 LoginController 只是对象，它们之间是兄弟关系，并不是父类和子类的关系。代码中 AuthController 委托了 LoginController，反向委托也完全没问题。

这种模式的重点在于只需要两个实体（LoginController 和 AuthController），而之前的模式需要三个。

我们不需要 Controller 基类来“共享”两个实体之间的行为，因为委托足以满足我们需要的功能。同样，前面提到过，我们也不需要实例化类，因为它们根本就不是类，它们只是对象。此外，我们也不需要合成，因为两个对象可以通过委托进行合作。

最后，我们避免了面向类设计模式中的多态。我们在不同的对象中没有使用相同的函数名 success(..) 和 failure(..)，这样就不需要使用丑陋的显示伪多态。相反，在AuthController 中它们的名字是 accepted(..) 和 rejected(..)——可以更好地描述它们的行为。

总结：我们用一种（极其）简单的设计实现了同样的功能，这就是对象关联风格代码和行为委托设计模式的力量。

#### 更好的语法

ES6 的 class 语法可以简洁地定义类方法，这个特性让 class 乍看起来更有吸引力（附录A 会介绍为什么要避免使用这个特性）：

```js
class Foo {
  methodName() { /* .. */ }
}
```
我们终于可以抛弃定义中的关键字 function 了，对所有 JavaScript 开发者来说真是大快人心！

你可能注意到了，在之前推荐的对象关联语法中出现了许多 function，看起来违背了对象关联的简洁性。但是实际上大可不必如此！

在 ES6 中我们可以 **在任意对象的字面形式中使用简洁方法声明**（concise method
declaration），所以对象关联风格的对象可以这样声明（和 class 的语法糖一样）：

```js
var LoginController = {
  errors: [],
  getUser() { // 妈妈再也不用担心代码里有 function 了！
    // ...
  },
  getPassword() {
    // ...
  }
  // ...
};
```
唯一的区别是对象的字面形式仍然需要使用“,”来分隔元素，而 class 语法不需要。这个区别对于整体的设计来说无关紧要。

此外，在 ES6 中，你可以使用对象的字面形式（这样就可以使用简洁方法定义）来
改 写 之 前 繁 琐 的 属 性 赋 值 语 法（ 比 如 AuthController 的 定 义 ）， 然 后 用 Object.setPrototypeOf(..) 来修改它的 [[Prototype]]：

```js
// 使用更好的对象字面形式语法和简洁方法
var AuthController = {
  errors: [],
  checkAuth() {
    // ...
  },
  server(url, data) {
    // ...
  }
  // ...
};

// 现在把 AuthController 关联到 LoginController
Object.setPrototypeOf(AuthController, LoginController);
```
使用 ES6 的简洁方法可以让对象关联风格更加人性化（并且仍然比典型的原型风格代码更加简洁和优秀）。你完全不需要使用类就能享受整洁的对象语法！

##### 反词法

简洁方法有一个非常小但是非常重要的缺点。思考下面的代码：

```js
var Foo = {
  bar() { /*..*/ },
  baz: function baz() { /*..*/ }
};
```
去掉语法糖之后的代码如下所示：

```js
var Foo = {
  bar: function() { /*..*/ },
  baz: function baz() { /*..*/ }
};
```
看到区别了吗？由于函数对象本身没有名称标识符，所以bar()的缩写形式（function()..）实际上会变成一个匿名函数表达式并赋值给 bar 属性。相比之下，具名函数表达式（function baz()..）会额外给 .baz 属性附加一个词法名称标识符 baz。

然后呢？在本书第一部分“作用域和闭包”中我们分析了匿名函数表达式的三大主要缺
点，下面我们会简单介绍一下这三个缺点，然后和简洁方法定义进行对比。

匿名函数没有 name 标识符，这会导致：

1. 调试栈更难追踪；
2. 自我引用（递归、事件（解除）绑定，等等）更难；
3. 代码（稍微）更难理解。

简洁方法没有第 1 和第 3 个缺点。

去掉语法糖的版本使用的是匿名函数表达式，通常来说并不会在追踪栈中添加 name，但是简洁方法很特殊，会给对应的函数对象设置一个内部的 name 属性，这样理论上可以用在追踪栈中。（但是追踪的具体实现是不同的，因此无法保证可以使用。）

很不幸，简洁方法无法避免第 2 个缺点，它们不具备可以自我引用的词法标识符。思考下面的代码：

```js
var Foo = {
  bar: function(x) {
    if (x < 10) {
      return Foo.bar(x * 2);
    }行为委托｜185
    return x;
  },
  baz: function baz(x) {
    if (x < 10) {
      return baz(x * 2);
    }
    return x;
  }
};
```
在本例中使用 Foo.bar(x*2) 就足够了，但是在许多情况下无法使用这种方法，比如多个对象通过代理共享函数、使用 this 绑定，等等。这种情况下最好的办法就是使用函数对象的 name 标识符来进行真正的自我引用。

使用简洁方法时一定要小心这一点。如果你需要自我引用的话，那最好使用传统的具名函
数表达式来定义对应的函数（ · baz: function baz(){..}· ），不要使用简洁方法。

#### 内省

如果你写过许多面向类的程序（无论是使用 JavaScript 还是其他语言），那你可能很熟悉自省。自省就是检查实例的类型。类实例的自省主要目的是通过创建方式来判断对象的结构和功能。

下面的代码使用 instanceof（参见第 5 章）来推测对象 a1 的功能：

```js
function Foo() {
  // ...
}
Foo.prototype.something = function(){
  // ...
}

var a1 = new Foo();

// 之后

if (a1 instanceof Foo) {
  a1.something();
}
```
因 为 Foo.prototype（ 不 是 Foo ！） 在 a1 的 [[Prototype]] 链 上（ 参 见 第 5 章 ）， 所 以instanceof 操作（会令人困惑地）告诉我们 a1 是 Foo“类”的一个实例。知道了这点后，我们就可以认为 a1 有 Foo“类”描述的功能。

当然，Foo 类并不存在，只有一个普通的函数 Foo，它引用了 a1 委托的对象（Foo.prototype）。从语法角度来说，instanceof 似乎是检查 a1 和 Foo 的关系，但是实际上它想说的是 a1 和 Foo.prototype（引用的对象）是互相关联的。


instanceof 语法会产生语义困惑而且非常不直观。如果你想检查对象 a1 和某个对象的关系，那必须使用另一个引用该对象的函数才行——你不能直接判断两个对象是否关联。

还记得本章之前介绍的抽象的 Foo/Bar/b1 例子吗，简单来说是这样的：

```js
function Foo() { /* .. */ }
Foo.prototype...

function Bar() { /* .. */ }
Bar.prototype = Object.create( Foo.prototype );

var b1 = new Bar( "b1" );
```
如果要使用 instanceof 和 .prototype 语义来检查本例中实体的关系，那必须这样做：

```js
// 让 Foo 和 Bar 互相关联
Bar.prototype instanceof Foo; // true
Object.getPrototypeOf( Bar.prototype ) === Foo.prototype; // true
Foo.prototype.isPrototypeOf( Bar.prototype ); // true

// 让 b1 关联到 Foo 和 Bar
b1 instanceof Foo; // true
b1 instanceof Bar; // true
Object.getPrototypeOf( b1 ) === Bar.prototype; // true
Foo.prototype.isPrototypeOf( b1 ); // true
Bar.prototype.isPrototypeOf( b1 ); // true
```
显然这是一种非常糟糕的方法。举例来说，（使用类时）你最直观的想法可能是使用 Bar instanceof Foo（因为很容易把“实例”理解成“继承”），但是在 JavaScript 中这是行不通的，你必须使用 Bar.prototype instanceof Foo。

还有一种常见但是可能更加脆弱的内省模式，许多开发者认为它比 instanceof 更好。这种模式被称为“鸭子类型”。这个术语源自这句格言“如果看起来像鸭子，叫起来像鸭子，那就一定是鸭子。”

举例来说：

```js
if (a1.something) {
  a1.something();
}
```
我们并没有检查 a1 和委托 something() 函数的对象之间的关系，而是假设如果 a1 通过了测试 a1.something 的话，那 a1 就一定能调用 .something()（无论这个方法存在于 a1 自身还是委托到其他对象）。这个假设的风险其实并不算很高。

但是“鸭子类型”通常会在测试之外做出许多关于对象功能的假设，这当然会带来许多风险（或者说脆弱的设计）。

ES6 的 Promise 就是典型的“鸭子类型”（之前解释过，本书并不会介绍 Promise）。

出于各种各样的原因，我们需要判断一个对象引用是否是 Promise，但是判断的方法是检查对象是否有 then() 方法。换句话说，如果对象有 then() 方法，ES6 的 Promise 就会认为这个对象是“可持续”（thenable）的，因此会期望它具有 Promise 的所有标准行为。

如果有一个不是 Promise 但是具有 then() 方法的对象，那你千万不要把它用在 ES6 的Promise 机制中，否则会出错。

这个例子清楚地解释了“鸭子类型”的危害。你应该尽量避免使用这个方法，即使使用也要保证条件是可控的。

现在回到本章想说的对象关联风格代码，其内省更加简洁。我们先来回顾一下之前的 Foo/Bar/b1 对象关联例子（只包含关键代码）：

```js
var Foo = { /* .. */ };

var Bar = Object.create( Foo );
Bar...

var b1 = Object.create( Bar );
```
使用对象关联时，所有的对象都是通过 [[Prototype]] 委托互相关联，下面是内省的方法，非常简单：

```js
// 让 Foo 和 Bar 互相关联
Foo.isPrototypeOf(Bar); // true
Object.getPrototypeOf(Bar) === Foo; // true

// 让 b1 关联到 Foo 和 Bar
Foo.isPrototypeOf(b1); // true
Bar.isPrototypeOf(b1); // true
Object.getPrototypeOf(b1) === Bar; // true

```
我们没有使用 instanceof，因为它会产生一些和类有关的误解。现在我们想问的问题是“你是我的原型吗？”我们并不需要使用间接的形式，比如 Foo.prototype 或者繁琐的 Foo.prototype.isPrototypeOf(..)。

我觉得和之前的方法比起来，这种方法显然更加简洁并且清晰。再说一次，我们认为JavaScript 中对象关联比类风格的代码更加简洁（而且功能相同）。

#### 小结

在软件架构中你可以选择是否使用类和继承设计模式。大多数开发者理所当然地认为类是唯一（合适）的代码组织方式，但是本章中我们看到了另一种更少见但是更强大的设计模式：行为委托。

行为委托认为对象之间是兄弟关系，互相委托，而不是父类和子类的关系。JavaScript 的[[Prototype]] 机制本质上就是行为委托机制。也就是说，我们可以选择在 JavaScript 中努力实现类机制（参见第 4 和第 5 章），也可以拥抱更自然的[[Prototype]] 委托机制。

当你只用对象来设计代码时，不仅可以让语法更加简洁，而且可以让代码结构更加清晰。对象关联（对象之前互相关联）是一种编码风格，它倡导的是直接创建和关联对象，不把它们抽象成类。对象关联可以用基于 [[Prototype]] 的行为委托非常自然地实现。