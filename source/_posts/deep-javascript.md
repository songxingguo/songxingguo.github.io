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