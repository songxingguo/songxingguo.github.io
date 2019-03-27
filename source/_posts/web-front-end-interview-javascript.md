title: Web前端面试题目及详解汇总-JavaScript部分
author: songxingguo
tags: []
categories:
  - 找工作
date: 2018-08-06 14:30:00
---
### 请指出document load和document ready的区别？

共同点：这两种事件都代表的是页面文档加载时触发。

异同点：

ready 事件的触发，表示文档结构已经加载完成（不包含图片等非文字媒体文件）。

onload 事件的触发，表示页面包含图片等文件在内的所有元素都加载完成。

<!-- more -->

### JavaScript中如何检测一个变量是一个String类型？请写出函数实现。

方法一：typeof

```js
function isString(obj){
    return typeof(obj) === "string"? true: false;
    // returntypeof obj === "string"? true: false;
}
```

方法二：constructor

```js
function isString(obj){
    return obj.constructor === String? true: false;
}
```

方法三：call()
```js
function isString(obj){
     return Object.prototype.toString.call(obj) === "[object String]"?true:false;
}
如：
var isstring = isString('xiaoming');
console.log(isstring);  // true
```

### 请用js去除字符串空格？

方法一：使用replace正则匹配的方法

  去除所有空格: str = str.replace(/\s*/g,"");      

  去除两头空格: str = str.replace(/^\s*|\s*$/g,"");

  去除左空格： str = str.replace( /^\s*/, “”);

  去除右空格： str = str.replace(/(\s*$)/g, "");

str为要去除空格的字符串，实例如下：

 ```js
 var str = " 23 23 ";
 var str2 = str.replace(/\s*/g,"");
 console.log(str2); // 2323
 ```

 来自—— [2018年web前端经典面试题及答案]

[2018年web前端经典面试题及答案]:https://www.cnblogs.com/wdlhao/p/8290436.html


### this 指向

1. this永远指向最后调用它的上级对象。

2. New 关键字可以改变 this 指向。

3. 自行改变 this 指向 call()、apply()、bind()。

4. 返回值是对象，this 指向对象，否则指向函数实例（null 也为对象）。

5. ES6 中箭头函数中 this 指向定义时所在对象。

> 严格模式中默认的 this 不再是 window，而是 undefined。

来自——[彻底理解js中this的指向，不必硬背。] 、 [浅析js之this --- 一次性搞懂this指向]

[彻底理解js中this的指向，不必硬背。]:https://www.cnblogs.com/pssp/p/5216085.html
[浅析js之this --- 一次性搞懂this指向]:https://www.cnblogs.com/cara-front-end/p/6762086.html

### 写出3个使用this的典型应用

- 在html元素事件属性中使用

  ```html
  <input type=”button” onclick=”showInfo(this);” value=”点击一下”/>
  ```

- 构造函数

  ```js
  function Animal(name, color) {
    this.name = name;
    this.color = color;
  }
  ```

- 访问当前事件

  ```html
  <input type=”button” id=”text” value=”点击一下”/>

  ```

  ```js
  Var btn = document.getElementById(“text”);

  Btn.onclick = function() {

    Alert(this.value); //此处的this是按钮元素

  }
  ```

- CSS expression表达式中使用this关键字

  ```
  <table width="100" height="100">
  <tr>
  <td>
  <div style="width: expression(this.parentElement.width); 
  height: expression(this.parentElement.height);">
  division element</div>
  </td>
  </tr>
  </table>
  ```
  这里的this指代div元素对象实例本身。

- apply()/call()求数组最值

  ```js
  var  numbers = [5, 458 , 120 , -215 ]; 
  var  maxInNumbers = Math.max.apply(this, numbers);  
  console.log(maxInNumbers);  // 458
  var maxInNumbers = Math.max.call(this,5, 458 , 120 , -215); 
  console.log(maxInNumbers);  // 458
  ```

来自——[四个使用this的典型应用]、[JavaScript中this的9种应用场景及三种复合应用场景]、[3个使用this的典型应用]

[四个使用this的典型应用]:http://www.cnblogs.com/kuangliu/p/3462727.html

[JavaScript中this的9种应用场景及三种复合应用场景]:https://www.jb51.net/article/72146.htm

[3个使用this的典型应用]:https://blog.csdn.net/cai181191/article/details/82718379


### 作用域和作用域链

#### 作用域

变量作用域： 全局作用域和局部作用域（在函数内部有用）。

全局作用域： function 声明的函数为全局的、不使用 var 声明的变量。

局部作用域：使用 var 声明的变量。

> JavaScript 没有块级作用域，JavaScript 作用域为函数作用域。

#### 作用域链

内部函数可以访问外部函数变量的机制，用链式查找决定哪些数据能被内部函数访问。

> 执行环境：js 的执行顺序是根据函数的调用来决定，函数被调用，函数环境变量对象就被压入到一个环境栈中，而函数执行之后，函数环境变量对象弹出，把控制权交给之前的执行环境变量对象。（保存在[Scope] 中）。

#### 如何理解闭包？

闭包： 内部函数的作用域链仍然保存这对父函数活动对象的引用。

闭包的作用：

- 可以读取自身函数外部的变量（沿着作用域链寻找）。 
- 让这些外部变量始终保存在内存中 。

> js函数内的变量值不是在编译的时候就确定的，而是等在运行时期再去寻找的。

来自——[JavaScript关于作用域、作用域链和闭包的理解]

[JavaScript关于作用域、作用域链和闭包的理解]:https://blog.csdn.net/whd526/article/details/70990994

### 事件循环（异步）

#### 什么是 Event Loop？

Event Loop 是一个很重要的概念，指的是计算机系统的一种运行机制。

JavaScript语言就采用这种机制，来解决单线程运行带来的一些问题。

![Event Loop](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-javascript/loop.png)

如果有很多任务需要执行，不外乎三种解决方法。

（1）排队。因为一个进程一次只能执行一个任务，只好等前面的任务执行完了，再执行后面的任务。

（2）新建进程。使用fork命令，为每个任务新建一个进程。

（3）新建线程。因为进程太耗费资源，所以如今的程序往往允许一个进程包含多个线程，由线程去完成任务。（进程和线程的详细解释，请看这里。）

JavaScript为一种单线程语言。（Worker API可以实现多线程，但是JavaScript本身始终是单线程的。）

#### 任务队列

所有任务可以分成两种，一种是 **同步任务**（synchronous），另一种是 **异步任务**（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

![主线程和任务队列的示意图](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-javascript/%E4%B8%BB%E7%BA%BF%E7%A8%8B%E5%92%8C%E4%BB%BB%E5%8A%A1%E9%98%9F%E5%88%97%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

只要主线程空了，就会去读取"任务队列"，这就是JavaScript的运行机制。这个过程会不断重复。

同步和异步任务执行过程：

![同步和异步任务](https://graphbed.qiniu.songxingguo.com/%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF.png)

#### 事件和回调函数

"任务队列"中的事件，除了IO设备的事件以外，还包括一些用户产生的事件（比如鼠标点击、页面滚动等等）。只要指定过回调函数，这些事件发生时就会进入"任务队列"，等待主线程读取。

所谓"回调函数"（callback），就是那些会被主线程挂起来的代码。**异步任务必须指定回调函数** ，**当主线程开始执行异步任务，就是执行对应的回调函数** 。

**"任务队列"是一个先进先出的数据结构，排在前面的事件，优先被主线程读取。** 主线程的读取过程基本上是自动的，只要执行栈一清空，"任务队列"上第一位的事件就自动进入主线程。但是，由于存在后文提到的"定时器"功能，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。

#### Event Loop

主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）。

![Event Loop](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-javascript/Event%20Loop.png)

上图中，主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码调用各种外部API，它们在"任务队列"中加入各种事件（click，load，done）。**只要栈中的代码执行完毕，主线程就会去读取"任务队列"** ，依次执行那些事件所对应的回调函数。

**执行栈中的代码（同步任务），总是在读取"任务队列"（异步任务）之前执行。**

```js
var req = new XMLHttpRequest();
req.open('GET', url);    
req.onload = function (){};    
req.onerror = function (){};    
req.send();
```
上面代码中的req.send方法是Ajax操作向服务器发送数据，它是一个异步任务，意味着只有当前脚本的所有代码执行完，系统才会去读取"任务队列"。所以，它与下面的写法等价。

```js
var req = new XMLHttpRequest();
req.open('GET', url);
req.send();
req.onload = function (){};    
req.onerror = function (){}; 
```
也就是说，指定回调函数的部分（onload和onerror），在send()方法的前面或后面无关紧要，因为它们属于执行栈的一部分，系统总是执行完它们，才会去读取"任务队列"。

#### 定时器

除了放置异步任务的事件，"任务队列"还可以放置定时事件，即指定某些代码在多少时间之后执行。

定时器功能主要由setTimeout()和setInterval()这两个函数来完成，它们的内部运行机制完全一样，区别在于前者指定的代码是一次性执行，后者则为反复执行。以下主要讨论setTimeout()。

setTimeout()接受两个参数，第一个是回调函数，第二个是推迟执行的毫秒数。

```js
console.log(1);
setTimeout(function(){console.log(2);},1000);
console.log(3);
```
上面代码的执行结果是1，3，2，因为setTimeout()将第二行推迟到1000毫秒之后执行。

如果将setTimeout()的第二个参数设为0，就表示当前代码执行完（执行栈清空）以后，立即执行（0毫秒间隔）指定的回调函数。

```js
setTimeout(function(){console.log(1);}, 0);
console.log(2);
```
上面代码的执行结果总是2，1，因为只有在执行完第二行以后，系统才会去执行"任务队列"中的回调函数。

总之，setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，也就是说，尽可能早得执行。 **它在"任务队列"的尾部添加一个事件，因此要等到同步任务和"任务队列"现有的事件都处理完，才会得到执行** 。

> HTML5标准规定了setTimeout()的第二个参数的最小值（最短间隔），不得低于4毫秒，如果低于这个值，就会自动增加。在此之前，老版本的浏览器都将最短间隔设为10毫秒。另外，对于那些DOM的变动（尤其是涉及页面重新渲染的部分），通常不会立即执行，而是每16毫秒执行一次。这时使用requestAnimationFrame()的效果要好于setTimeout()。

需要注意的是，setTimeout()只是将事件插入了"任务队列"，必须等到当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数。要是当前代码耗时很长，有可能要等很久，所以 **并没有办法保证，回调函数一定会在setTimeout()指定的时间执行** 。

#### Node.js的Event Loop

Node.js也是单线程的Event Loop，但是它的运行机制不同于浏览器环境。

![Node.js的Event Loop](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-javascript/Node.js%E7%9A%84Event%20Loop.png)

根据上图，Node.js的运行机制如下。

（1）V8引擎解析JavaScript脚本。

（2）解析后的代码，调用Node API。

（3）libuv库负责Node API的执行。它将不同的任务分配给不同的线程，形成一个Event Loop（事件循环），以异步的方式将任务的执行结果返回给V8引擎。

（4）V8引擎再将结果返回给用户。

除了setTimeout和setInterval这两个方法，Node.js还提供了另外两个与"任务队列"有关的方法：process.nextTick和setImmediate。

process.nextTick方法可以在当前"执行栈"的尾部----下一次Event Loop（主线程读取"任务队列"）之前----触发回调函数。也就是说，它指定的任务总是发生在所有异步任务之前。setImmediate方法则是在当前"任务队列"的尾部添加事件，也就是说，它指定的任务总是在下一次Event Loop时执行，这与setTimeout(fn, 0)很像。

```
process.nextTick(function A() {
  console.log(1);
  process.nextTick(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 1
// 2
// TIMEOUT FIRED
```

上面代码中，由于process.nextTick方法指定的回调函数，总是在当前"执行栈"的尾部触发，所以 **不仅函数A比setTimeout指定的回调函数timeout先执行，而且函数B也比timeout先执行** 。这说明，如果有多个process.nextTick语句（不管它们是否嵌套），将全部在当前"执行栈"执行。

现在，再看setImmediate。

```js
setImmediate(function A() {
  console.log(1);
  setImmediate(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0);
```

上面代码中，setImmediate与setTimeout(fn,0)各自添加了一个回调函数A和timeout，都是在下一次Event Loop触发。那么，哪个回调函数先执行呢？答案是不确定。运行结果可能是1--TIMEOUT FIRED--2，也可能是TIMEOUT FIRED--1--2。

令人困惑的是，Node.js文档中称，setImmediate指定的回调函数，总是排在setTimeout前面。实际上，这种情况只发生在递归调用的时候。

```js
setImmediate(function (){
  setImmediate(function A() {
    console.log(1);
    setImmediate(function B(){console.log(2);});
  });

  setTimeout(function timeout() {
    console.log('TIMEOUT FIRED');
  }, 0);
});
// 1
// TIMEOUT FIRED
// 2
```

上面代码中，setImmediate和setTimeout被封装在一个setImmediate里面，它的运行结果总是1--TIMEOUT FIRED--2，这时函数A一定在timeout前面触发。至于2排在TIMEOUT FIRED的后面（即函数B在timeout后面触发），是因为setImmediate总是将事件注册到下一轮Event Loop，所以函数A和timeout是在同一轮Loop执行，而函数B在下一轮Loop执行。

我们由此得到了process.nextTick和setImmediate的一个重要区别： **多个process.nextTick语句总是在当前"执行栈"一次执行完，多个setImmediate可能则需要多次loop才能执行完。** 事实上，这正是Node.js 10.0版添加setImmediate方法的原因，否则像下面这样的递归调用process.nextTick，将会没完没了，主线程根本不会去读取"事件队列"！

```js
process.nextTick(function foo() {
  process.nextTick(foo);
});
```

事实上，现在要是你写出递归的process.nextTick，Node.js会抛出一个警告，要求你改成setImmediate。

另外，由于process.nextTick指定的回调函数是在本次"事件循环"触发，而setImmediate指定的是在下次"事件循环"触发，所以很显然，前者总是比后者发生得早，而且执行效率也高（因为不用检查"任务队列"）。

#### JS事件循环机制（event loop）之宏任务、微任务

```js
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');
```
正确答案是:
script start, script end, promise1, promise2, setTimeout

JS异步还有一个机制，就是遇到宏任务，先执行宏任务，将宏任务放入event queue，然后再执行微任务，将微任务放入eventqueue，但是，这两个queue不是一个queue。当你往外拿的时候先从微任务里拿这个回调函数，然后再从宏任务的queue拿宏任务的回调函数，如下图：

![宏任务与微任务的执行过程](https://graphbed.qiniu.songxingguo.com/%E5%AE%8F%E4%BB%BB%E5%8A%A1%E4%B8%8E%E5%BE%AE%E4%BB%BB%E5%8A%A1.png)

宏任务一般包括：整体代码script，setTimeout，setInterval。

微任务：Promise，process.nextTick

##### 宏任务（task）

浏览器为了能够使得JS内部task与DOM任务能够有序的执行，会在一个task执行结束后，在下一个 task 执行开始前，对页面进行重新渲染。

鼠标点击、setTimeout，setImmediate，MessageChannel 都会产生一个新的宏任务。


##### 微任务（Microtasks ）

微任务通常来说就是需要在当前 task 执行结束后立即执行的任务。

pormise,Object.observe(已废弃), MutationObserver会产生一个新的宏任务。

来自——[什么是 Event Loop？]、[JavaScript 运行机制详解：再谈Event Loop]、[Tasks, microtasks, queues and schedules]、[JS事件循环机制（event loop）之宏任务、微任务]、[宏任务和微任务：setTimeout和Promise执行顺序]、[事件循环机制]、[javascript中的事件循环event-loop]

[javascript中的事件循环event-loop]:https://www.cnblogs.com/xiaohuochai/p/8527618.html

[事件循环机制]:https://www.jianshu.com/p/12b9f73c5a4f

[JS事件循环机制（event loop）之宏任务、微任务]:https://segmentfault.com/a/1190000014940904

[什么是 Event Loop？]:http://www.ruanyifeng.com/blog/2013/10/event_loop.html

[JavaScript 运行机制详解：再谈Event Loop]:http://www.ruanyifeng.com/blog/2014/10/event-loop.html

[Tasks, microtasks, queues and schedules]:https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/?utm_source=html5weekly

[宏任务和微任务：setTimeout和Promise执行顺序]:https://blog.csdn.net/RedaTao/article/details/81504532

### 函数防抖和函数节流

能节省浏览器CPU资源，又能让页面浏览更加顺畅，不会因为js的执行而发生卡顿。这就是函数节流和函数防抖要做的事。

**函数节流** 是指一定时间内js方法只跑一次。比如人的眨眼睛，就是一定时间内眨一次。这是函数节流最形象的解释。

**函数防抖** 是指频繁触发的情况下，只有足够的空闲时间，才执行代码一次。比如生活中的坐公交，就是一定时间内，如果有人陆续刷卡上车，司机就不会开车。只有别人没刷卡了，司机才开车。

#### 函数节流

 函数节流应用的实际场景，多数在监听页面元素滚动事件的时候会用到。因为滚动事件，是一个高频触发的事件。以下是监听页面元素滚动的示例代码：

```js
// 函数节流
var canRun = true;
document.getElementById("throttle").onscroll = function(){
    if(!canRun){
        // 判断是否已空闲，如果在执行中，则直接return
        return;
    }

    canRun = false;
    setTimeout(function(){
        console.log("函数节流");
        canRun = true;
    }, 300);
};
```

- 函数节流的要点是，声明一个变量当标志位，记录当前代码是否在执行。
- 如果空闲，则可以正常触发方法执行。
- 如果代码正在执行，则取消这次方法执行。

#### 函数防抖

函数防抖的应用场景，最常见的就是用户注册时候的手机号码验证和邮箱验证了。只有等用户输入完毕后，前端才需要检查格式是否正确，如果不正确，再弹出提示语。以下还是以页面元素滚动监听的例子，来进行解析：

```js
// 函数防抖
var timer = false;
document.getElementById("debounce").onscroll = function(){
    clearTimeout(timer); // 清除未执行的代码，重置回初始化状态

    timer = setTimeout(function(){
        console.log("函数防抖");
    }, 300);
};  
```

- 函数防抖的要点，也是需要一个setTimeout来辅助实现。延迟执行需要跑的代码。
- 如果方法多次触发，则把上次记录的延迟执行代码用clearTimeout清掉，重新开始。
- 如果计时完毕，没有方法进来访问触发，则执行代码。

 函数防抖的实现重点，就是巧用setTimeout做缓存池，而且可以轻易地清除待执行的代码。其实，用队列的方式也可以做到这种效果。这里就不深入了。

来自——[JavaScript函数节流和函数防抖之间的区别]

[JavaScript函数节流和函数防抖之间的区别]:https://www.cnblogs.com/walls/p/6399837.html

### 原型 / 构造函数 / 实例

- 原型(prototype): 一个简单的对象，用于实现对象的属性继承。可以简单的理解成对象的爹。在 Firefox 和 Chrome 中，每个 JavaScript 对象中都包含一个 `__proto__ `(非标准)的属性指向它爹(该对象的原型)，可 `obj.__proto__` 进行访问。

- 构造函数: 可以通过 new 来新建一个对象的函数。

- 实例: 通过构造函数和 new 创建出来的对象，便是实例。 实例通过 `__proto__` 指向原型，通过 constructor 指向构造函数。

**三者的关系**

```
实例.__proto__ === 原型

原型.constructor === 构造函数

构造函数.prototype === 原型

原型.constructor === 构造函数
```

![三者的关系](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-html-css/168e9d9b940c4c6f)

> 形象理解： 实例、原型、构造函数的关系就好像一家三口，儿子（实例）的手（`__proto__`）拉着父亲(原型)，父亲和母亲分别通过 constructor 和 prototype 牵着彼此，并且通过 `__proto__` 指向自己的父亲（原型）。

> 层级之间的指向都是使用 `__proto__` ，同层级指向使用 prototype 。

> **上图有有错误，实例并无 constructor 属性。**

**prototype 是啥，`__proto__` 又是啥，他们之间啥关系？**

- JavaScript 中大部分类型的值都拥有 `__proto__` 属性。

- 只有 function 对象才有 prototype 属性。

来自——[原型 / 构造函数 / 实例]

[原型 / 构造函数 / 实例]: https://juejin.im/post/5c64d15d6fb9a049d37f9c20

### 执行上下文(EC)

> 执行上下文可以简单理解为一个对象。

#### 包含三个部分：

- 变量对象（VO）
- 作用域链（词法作用域）
- this 指向

#### 类型

- 全局执行上下文
- 函数执行上下文
- eval执行上下文

#### 代码执行过程

- 创建 全局上下文 (global EC)
- 全局执行上下文 (caller) 逐行 自上而下 执行。遇到函数时，函数执行上下文 (callee) 被push到执行栈顶层。
- 函数执行上下文被激活，成为 active EC, 开始执行函数中的代码，caller 被挂起。
- 函数执行完后，callee 被pop移除出执行栈，控制权交还全局上下文 (caller)，继续执行。

### for in 和 for of的区别

for in遍历的是数组的索引（即键名），而for of遍历的是数组元素值。

- for in 循环特别适合遍历对象。

### 浅拷贝和深拷贝



### 即时函数


### 垃圾回收

### 闭包内存（内存泄漏如何处理）

[浅析闭包和内存泄露的问题](https://www.cnblogs.com/yakun/p/3932026.html)

[闭包](https://www.2cto.com/kf/201802/719998.html)

[闭包会造成内存泄漏吗？](https://www.cnblogs.com/developer-ios/p/6014335.html)

### 垃圾回收机制（JC机制、V8）

[JavaScript垃圾回收机制](https://www.cnblogs.com/zhwl/p/4664604.html)

[js垃圾回收机制和引起内存泄漏的操作](https://blog.csdn.net/yingzizizizizizzz/article/details/77333996)

[javascript的垃圾回收机制与内存管理](https://blog.csdn.net/oliver_web/article/details/53957021)


### V8 垃圾回收

[浅谈V8引擎中的垃圾回收机制](https://segmentfault.com/a/1190000000440270)

[浅谈Chrome V8引擎中的垃圾回收机制](https://www.cnblogs.com/liangdaye/p/4654734.html)

[V8 JavaScript引擎研究（三）垃圾回收器的实现](https://www.cnblogs.com/wolfx/p/5919574.html)


### 冒泡，事件模型

[事件冒泡](https://songxingguo.github.io/2018/08/26/JavaScript-event/)

### JavaScript API

### asyn 和 wait 

### 函数无 return 返回 undefined

### 你如何获取浏览器URL中查询字符串中的参数？

### js 字符串操作函数

### 怎样添加、移除、移动、复制、创建和查找节点？

### 比较typeof与instanceof？

### 什么是跨域？跨域请求资源的方法有哪些？

### 谈谈垃圾回收机制方式及内存管理

### 开发过程中遇到的内存泄露情况，如何解决的？

### javascript面向对象中继承实现？JS继承与原型问题

### JavaScript 数组(Array)对象

### 模块化编程

### 一个页面从URL到加载显示完成，都发生了什么？

### 队列、堆、栈的区别？