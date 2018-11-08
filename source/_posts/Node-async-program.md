title: 《深入浅出Node.js》-读书笔记-异步编程
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
  - ''
date: 2018-10-06 08:00:00
---
## 异步编程

V8和异步I/O在性能上带来的提升，前后端JavaScript编程风格一致，是Node能够迅速成功并流行起来的主要原因。

<!-- more -->

### 函数式编程

在开始异步编程之前，先得知晓JavaScript现今的回调函数和深层嵌套的来龙去脉。

在JavaScript中，**函数（function）作为一等公民** ，使用上非常自由，**无论调用它，或者作为参数，或者作为返回值均可** 。

函数的灵活性是JavaScript比较吸引人的地方之一，它与古老的Lisp语言颇具渊源。JavaScript在诞生之前，Brendan Eich借鉴了Scheme语言（Scheme作为Lisp的派生），吸收了函数式编程的精华，将函数作为一等公民便是典型案例。

函数式编程是JavaScript异步编程的基础。

#### 高阶函数

在通常的语言中，函数的参数只接受基本的数据类型或是对象引用，返回值也只是基本数据类型和对象引用。下面的代码为常规的参数传递和返回：

```js
function foo(x) { 
  return x;
}
```
**高阶函数** 则是可以把 **函数作为参数** ，或是将 **函数作为返回值** 的函数，如下面的代码所示：

```js
function foo(x) { 
  return function () {
    return x; 
  };
}
```

高阶函数可以将函数作为输入或返回值的变化看起来虽细小，但是对于C/C++语言而言，通过指针也可以达到相同的效果。但对于程序编写，高阶函数则比普通的函数要灵活许多。除了通常意义的 **函数调用返回** 外，还形成了一种 **后续传递风格** （Continuation Passing Style）的结果接收方式，而非单一的返回值形式。后续传递风格的程序编写将函数的业务重点从返回值转移到了回调函数中：

```js
function foo(x, bar) {
  return bar(x);
}
```
以上面的代码为例，对于相同的foo()函数，传入的bar参数不同，则可以得到不同的结果。

一个经典的例子便是数组的sort()方法，它是一个货真价实的高阶函数，可以接受一个方法作为参数参与运算排序：

```js
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b) {
  return a - b;
});
// [ 1, 5, 10, 25, 40, 100 ]
```
通过改动sort()方法的参数，可以决定不同的排序方式，从这里可以看出高阶函数的灵活性来。

结合Node提供的最基本的事件模块可以看到，事件的处理方式正是基于高阶函数的特性来完成的。在自定义事件实例中，通过为相同事件注册不同的回调函数，可以很灵活地处理业务逻辑。示例代码如下：

```js
var emitter = new events.EventEmitter();
emitter.on('event_foo', function () {
  // TODO
});
```
本书时常提到事件可以十分方便地进行复杂业务逻辑的解耦，它其实受益于高阶函数。

高阶函数在JavaScript中比比皆是，其中ECMAScript5中提供的一些数组方法（forEach()、map()、reduce()、reduceRight()、filter()、every()、some()）十分典型。

#### 偏函数用法

**偏函数用法** 是指 **创建一个调用另外一个部分——参数或变量已经预置的函数——的函数的用法** 。

这句话相对较为拗口，下面我们以实例来说明：

```js
var toString = Object.prototype.toString;
var isString = function (obj) { 
  return toString.call(obj) == '[object String]';
};
var isFunction = function (obj) {
  return toString.call(obj) == '[object Function]';
};
```
在JavaScript中进行类型判断时，我们通常会进行类似上述代码的方法定义。这段代码固然不复杂，只有两个函数的定义，但是里面存在的问题是我们需要重复去定义一些相似的函数，如果有更多的isXXX()，就会出现更多的冗余代码。为了解决重复定义的问题，我们引入一个新函数，这个新函数可以如工厂一样批量创建一些类似的函数。

在下面的代码中，我们通过isType()函数预先指定type的值，然后返回一个新的函数：

```js
var isType = function (type) { 
  return function (obj) {
     return toString.call(obj) == '[object ' + type + ']'; 
  };
};
var isString = isType('String');
var isFunction = isType('Function');
```
可以看出，引入isType()函数后，创建isString()、isFunction()函数就变得简单多了。**这种通过指定部分参数来产生一个新的定制函数的形式就是偏函数。**。

偏函数应用在异步编程中也十分常见，著名类库Underscore提供的after()方法即是偏函数应用，其定义如下：

```js
_.after = function(times, func) { 
  if (times <= 0) return func();
  return function() { 
    if (--times < 1) { 
      return func.apply(this, arguments); 
    }
  };
};
```
这个函数可以根据传入的times参数和具体方法，生成一个需要调用多次才真正执行实际函数的函数。

### 异步编程的优势与难点

曾经的单线程模型在同步I/O的影响下，由于I/O调用缓慢，在应用层面导致CPU与I/O无法重叠进行。

提升性能的方式过去多用多线程的方式解决，但是多线程的引入在业务逻辑方面制造的麻烦也不少。从操作系统调度多线程的上下文切换开销，到实际编程里的锁、同步等问题，让开发人员头疼的时候也并不少。

另一个解决I/O性能的方案是通过C/C++调用操作系统底层接口，自己手工完成异步I/O，这能够达到很高的性能，但是调试和开发门槛均十分高，在帮助业务解决问题上，需要花费较大的精力。

Node利用JavaScript及其内部异步库，将异步直接提升到业务层面，这是一种创新。

#### 优势

Node带来的最大特性莫过于 **基于事件驱动的非阻塞I/O模型** ，这是它的灵魂所在。

**非阻塞I/O可以使CPU与I/O并不相互依赖等待，让资源得到更好的利用** 。对于网络应用而言，并行带来的想象空间更大，延展而开的是分布式和云。并行使得各个单点之间能够更有效地组织起来，这也是Node在云计算厂商中广受青睐的原因，下图为异步I/O调用的示意图。

![异步I/O调用的示意图](https://graphbed.qiniu.songxingguo.com/Node-async-program/%E5%BC%82%E6%AD%A5IO%E8%B0%83%E7%94%A8%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

如果采用传统的同步I/O模型，分布式计算中性能的折扣将会是明显的，如下图所示。

![同步I/O调用的示意图](https://graphbed.qiniu.songxingguo.com/Node-async-program/%E5%90%8C%E6%AD%A5IO%E8%B0%83%E7%94%A8%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

在第3章中，我们讨论过Node实现异步I/O的原理。**利用事件循环的方式，JavaScript线程像一个分配任务和处理结果的大管家，I/O线程池里的各个I/O线程都是小二，负责兢兢业业地完成分配来的任务，小二与管家之间互不依赖** ，所以 **可以保持整体的高效率** 。这个利用事件循环的经典调度方式在很多地方都存在应用，最典型的是UI编程，如iOS应用开发等。

这个模型的缺点则在于管家无法承担过多的细节性任务，如果承担太多，则会影响到任务的调度，管家忙个不停，小二却得不到活干，结局则是整体效率的降低。

换言之，**Node是为了解决编程模型中阻塞I/O的性能问题的，采用了单线程模型，这导致Node更像一个处理I/O密集问题的能手，而CPU密集型则取决于管家的能耐如何。**

如果形象地去评判的话，C语言是性能至尊，得益于V8性能的Node则是一流武林高手，在具备武功秘笈的情况下（调用C/C++扩展模块），Node的能力可以逼近顶尖之列。

由于事件循环模型需要应对海量请求，海量请求同时作用在单线程上，就需要防止任何一个计算耗费过多的CPU时间片。至于是计算密集型，还是I/O密集型，只要计算不影响异步I/O的调度，那就不构成问题。建议对CPU的耗用不要超过10 ms，或者将大量的计算分解为诸多的小量计算，通过setImmediate()进行调度。只要合理利用Node的异步模型与V8的高性能，就可以充分发挥CPU和I/O资源的优势。

#### 难点

Node令异步编程如此风行，这也是异步编程首次大规模出现在业务层面。它借助异步I/O模型及V8高性能引擎，突破单线程的性能瓶颈，让JavaScript在后端达到实用价值。另一方面，它也统一了前后端JavaScript的编程模型。对于异步编程带来的新鲜感与不适感，开发者们有着不同程度的感受。接下来，我们梳理一下异步编程的难点，以更好地利用Node。

1. 难点1：异常处理

过去我们处理异常时，通常使用类Java的try/catch/final语句块进行异常捕获，示例代码如下：

```js
try {
  JSON.parse(json);
} catch (e) {
  // TODO
}
```
但是这对于异步编程而言并不一定适用。第3章提到过，异步I/O的实现主要包含两个阶段：提交请求和处理结果。这两个阶段中间有事件循环的调度，两者彼此不关联。异步方法则通常在第一个阶段提交请求后立即返回，因为异常并不一定发生在这个阶段，try/catch的功效在此处不会发挥任何作用。异步方法的定义如下所示：

```js
var async = function (callback) { 
  process.nextTick(callback);
};
```
调用async()方法后，callback被存放起来，直到下一个事件循环（Tick）才会取出来执行。尝试对异步方法进行try/catch操作只能捕获当次事件循环内的异常，对callback执行时抛出的异常将无能为力，示例代码如下：

```js
try { 
   async(callback);
} catch (e) { 
  // TODO
}
```
Node在处理异常上形成了一种约定，**将异常作为回调函数的第一个实参传回，如果为空值，则表明异步调用没有异常抛出** ：

```
async(function (err, results) { 
  // TODO
});
```
在我们自行编写的异步方法上，也需要去遵循这样一些原则：

原则一：必须执行调用者传入的回调函数；

原则二：正确传递回异常供调用者判断。

示例代码如下：

```js
var async = function (callback) {
  process.nextTick(function() { 
    var results = something;
    if (error) { 
      return callback(error);
    } 
    callback(null, results);
  });
};
```
在异步方法的编写中，另一个容易犯的错误是对用户传递的回调函数进行异常捕获，示例代码如下：

```js
try {
  req.body = JSON.parse(buf, options.reviver); 
  callback();
} catch (err){ 
  err.body = buf;
  err.status = 400; 
  callback(err);
}
```
上述代码的意图是捕获JSON.parse()中可能出现的异常，但是却不小心包含了用户传递的回调函数。这意味着如果回调函数中有异常抛出，将会进入catch()代码块中执行，于是回调函数将会被执行两次。这显然不是预期的情况，可能导致业务混乱。正确的捕获应当为：

```js
try { 
  req.body = JSON.parse(buf, options.reviver);
} catch (err){ 
  err.body = buf;
  err.status = 400; return callback(err);
}
callback();
```
**在编写异步方法时，只要将异常正确地传递给用户的回调方法即可，无须过多处理。**

2. 难点2：函数嵌套过深

这或许是Node被人诟病最多的地方。在前端开发中，DOM事件相对而言不会存在互相依赖或需要多个事件一起协作的场景，较少存在异步多级依赖的情况。下面的代码为彼此独立的DOM事件绑定：

```js
$(selector).click(function (event) {
  // TODO
});
$(selector).change(function (event) { 
   // TODO
});
```
但是对于Node而言，事务中存在多个异步调用的场景比比皆是。比如一个遍历目录的操作，其代码如下：

```js
fs.readdir(path.join(__dirname, '..'), function (err, files) {  
  files.forEach(function (filename, index) {
    fs.readFile(filename, 'utf8', function (err, file) {
      // TODO
    }); 
  });
});
```
对于上述场景，由于两次操作存在依赖关系，函数嵌套的行为也许情有可原。那么，在网页渲染的过程中，通常需要数据、模板、资源文件，这三者互相之间并不依赖，但最终渲染结果中三者缺一不可。如果采用默认的异步方法调用，程序也许将会如下所示：

```js
fs.readFile(template_path, 'utf8', function (err, template) { 
  db.query(sql, function (err, data) {
    l10n.get(function (err, resources) { 
      // TODO
    }); 
  });
});
```
这在结果的保证上是没有问题的，问题在于这并没有利用好异步I/O带来的并行优势。这是异步编程的典型问题，为此有人曾说，因为嵌套的深度，未来最难看的代码必将从Node中诞生。但是实际情况没有想象得那么糟糕，且看后面如何解决该问题。

3. 难点3：阻塞代码

对于进入JavaScript世界不久的开发者，比较纳闷这门编程语言竟然没有sleep()这样的线程沉睡功能，**唯独能用于延时操作的只有setInterval()和setTimeout()这两个函数** 。但是让人惊讶的是，**这两个函数并不能阻塞后续代码的持续执行** 。所以，有多半的开发者会写出下述这样的代码来实现sleep(1000)的效果：

```js
// TODO
var start = new Date();
while (new Date() - start < 1000) { 
   // TODO
}
// 需要阻塞的代码
```
但是事实是糟糕的，这段代码会持续占用CPU进行判断，与真正的线程沉睡相去甚远，完全破坏了事件循环的调度。由于Node单线程的原因，CPU资源全都会用于为这段代码服务，导致其余任何请求都会得不到响应。**遇见这样的需求时，在统一规划业务逻辑之后，调用setTimeout()的效果会更好。**

4. 难点4：多线程编程

我们在谈论JavaScript的时候，通常谈的是单一线程上执行的代码，这在浏览器中指的是JavaScript执行线程与UI渲染共用的一个线程；在Node中，只是没有UI渲染的部分，模型基本相同。对于服务器端而言，如果服务器是多核CPU，单个Node进程实质上是没有充分利用多核CPU的。随着现今业务的复杂化，对于多核CPU利用的要求也越来越高。浏览器提出了Web Workers，它通过将JavaScript执行与UI渲染分离，可以很好地利用多核CPU为大量计算服务。同时前端Web Workers也是一个利用消息机制合理使用多核CPU的理想模型。下图为Web Workers的工作示意图。

![Web Workers的工作示意图](https://graphbed.qiniu.songxingguo.com/Node-async-program/Web%20Workers%E7%9A%84%E5%B7%A5%E4%BD%9C%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

遗憾在于前端浏览器存在对标准的滞后性，Web Workers并没有广泛应用起来。另外Web Workers能解决利用CPU和减少阻塞UI渲染，但是不能解决UI渲染的效率问题。Node借鉴了这个模式，child_process是其基础API，cluster模块是更深层次的应用。借助Web Workers的模式，开发人员要更多地去面临跨线程的编程，这对于以往的JavaScript编程经验是较少考虑的。

5. 难点5：异步转同步

习惯异步编程的同学，也许能够从容面对异步编程带来的副产品，比如嵌套回调、业务分散等问题。Node提供了绝大部分的异步API和少量的同步API，偶尔出现的同步需求将会因为没有同步API让开发者突然无所适从。目前，Node中试图同步式编程，但并不能得到原生支持，需要借助库或者编译等手段来实现。但对于异步调用，通过良好的流程控制，还是能够将逻辑梳理成顺序式的形式。

### 异步编程解决方案

目前，异步编程的主要解决方案有如下3种。

- 事件发布/订阅模式。
- Promise/Deferred模式。
- 流程控制库。

#### 事件发布/订阅模式

事件监听器模式是一种广泛用于 **异步编程的模式，是回调函数的事件化** ，又称发布/订阅模式。

Node自身提供的events模块（ http://nodejs.org/docs/latest/api/events.html ）是发布/订阅模式的一个简单实现，Node中部分模块都继承自它，这个模块比前端浏览器中的大量DOM事件简单，不存在事件冒泡，也不存在preventDefault()、stopPropagation()和stopImmediatePropagation()等控制事件传递的方法。它具有addListener/on()、once()、removeListener()、removeAllListeners()和emit()等基本的事件监听模式的方法实现。事件发布/订阅模式的操作极其简单，示例代码如下：

```
// 订阅
emitter.on("event1", function (message) {
  console.log(message);
});
// 发布
emitter.emit('event1', "I am message!);
```
可以看到，**订阅事件就是一个高阶函数的应用** 。事件发布/订阅模式可以实现一个事件与多个回调函数的关联，这些回调函数又称为 **事件侦听器** 。**通过emit()发布事件后，消息会立即传递给当前事件的所有侦听器执行** 。**侦听器可以很灵活地添加和删除，使得事件和具体处理逻辑之间可以很轻松地关联和解耦。**

事件发布/订阅模式自身并无同步和异步调用的问题，但在Node中，emit()调用多半是伴随事件循环而异步触发的，所以我们说事件发布/订阅广泛应用于异步编程。

事件发布/订阅模式常常用来解耦业务逻辑，事件发布者无须关注订阅的侦听器如何实现业务逻辑，甚至不用关注有多少个侦听器存在，数据通过消息的方式可以很灵活地传递。在一些典型场景中，可以通过事件发布/订阅模式进行组件封装，将不变的部分封装在组件内部，将容易变化、需自定义的部分通过事件暴露给外部处理，这是一种典型的逻辑分离方式。在这种事件发布/订阅式组件中，事件的设计非常重要，因为它关乎外部调用组件时是否优雅，从某种角度来说事件的设计就是组件的接口设计。

从另一个角度来看，**事件侦听器模式也是一种钩子（hook）机制，利用钩子导出内部数据或状态给外部的调用者。**


Node中的很多对象大多具有黑盒的特点，功能点较少，如果不通过事件钩子的形式，我们就无法获取对象在运行期间的中间值或内部状态。这种通过事件钩子的方式，可以使编程者不用关注组件是如何启动和执行的，只需关注在需要的事件点上即可。下面的HTTP请求是典型场景：

```js
var options = { 
  host: 'www.google.com',
  port: 80, path: '/upload',
  method: 'POST'
};
var req = http.request(options, function (res) { 
  console.log('STATUS: ' + res.statusCode);
  console.log('HEADERS: ' + JSON.stringify(res.headers)); res.setEncoding('utf8');
  res.on('data', function (chunk) { 
    console.log('BODY: ' + chunk);
  }); 
  res.on('end', function () {
    // TODO 
  });
});
req.on('error', function (e) {
  console.log('problem with request: ' + e.message);
});
// write data to request body
req.write('data\n');
req.write('data\n');
req.end();
```
在这段HTTP请求的代码中，程序员只需要将视线放在error、data、end这些业务事件点上即可，至于内部的流程如何，无需过于关注。

值得一提的是，Node对事件发布/订阅的机制做了一些额外的处理，这大多是基于健壮性而考虑的。下面为两个具体的细节点。

- 如果 **对一个事件添加了超过10个侦听器，将会得到一条警告** 。这一处设计与Node自身单线程运行有关，设计者认为侦听器太多可能导致内存泄漏，所以存在这样一条警告。调用emitter.setMaxListeners(0)；可以将这个限制去掉。另一方面，由于事件发布会引起一系列侦听器执行，如果事件相关的侦听器过多，可能存在过多占用CPU的情景。

- 为了处理异常，**EventEmitter对象对error事件进行了特殊对待** 。如果运行期间的错误触发了error事件，EventEmitter会检查是否有对error事件添加过侦听器。如果添加了，这个错误将会交由该侦听器处理，否则这个错误将会作为异常抛出。如果外部没有捕获这个异常，将会引起线程退出。一个健壮的EventEmitter实例应该对error事件做处理。

1. 继承events模块

实现一个继承EventEmitter的类是十分简单的，以下代码是Node中Stream对象继承EventEmitter的例子：

```js
var events = require('events');
function Stream() {
  events.EventEmitter.call(this);
}
util.inherits(Stream, events.EventEmitter);
```
Node在util模块中封装了继承的方法，所以此处可以很便利地调用。开发者可以通过这样的方式轻松继承EventEmitter类，利用事件机制解决业务问题。**在Node提供的核心模块中，有近半数都继承自EventEmitter。**

2. 利用事件队列解决雪崩问题

在事件订阅/发布模式中，通常也有一个once()方法，通过它添加的侦听器只能执行一次，在执行之后就会将它与事件的关联移除。这个特性常常可以帮助我们过滤一些重复性的事件响应。下面我们介绍一下如何采用once()来解决雪崩问题。

在计算机中，缓存由于存放在内存中，访问速度十分快，常常用于加速数据访问，让绝大多数的请求不必重复去做一些低效的数据读取。所谓雪崩问题，就是在高访问量、大并发量的情况下缓存失效的情景，此时大量的请求同时涌入数据库中，数据库无法同时承受如此大的查询请求，进而往前影响到网站整体的响应速度。以下是一条数据库查询语句的调用：

```js
var select = function (callback) { 
  db.select("SQL", function (results) {
    callback(results); 
  });
};
```
如果站点刚好启动，这时缓存中是不存在数据的，而如果访问量巨大，同一句SQL会被发送到数据库中反复查询，会影响服务的整体性能。一种改进方案是添加一个状态锁，相关代码如下：

```js
var status = "ready";
var select = function (callback) {
  if (status === "ready") { 
    status = "pending";
    db.select("SQL", function (results) { 
      status = "ready";
      callback(results); 
    });
  }
};
```
但是在这种情景下，连续地多次调用select()时，只有第一次调用是生效的，后续的select()是没有数据服务的，这个时候可以引入事件队列，相关代码如下：

```js
var proxy = new events.EventEmitter();
var status = "ready";
var select = function (callback) {
proxy.once("selected", callback); 
  if (status === "ready") {
    status = "pending"; 
    db.select("SQL", function (results) {
        proxy.emit("selected", results); 
        status = "ready";
     }); 
  }
};
```
这里我们利用了once()方法，将所有请求的回调都压入事件队列中，利用其执行一次就会将监视器移除的特点，保证每一个回调只会被执行一次。对于相同的SQL语句，保证在同一个查询开始到结束的过程中永远只有一次。SQL在进行查询时，新到来的相同调用只需在队列中等待数据就绪即可，一旦查询结束，得到的结果可以被这些调用共同使用。这种方式能节省重复的数据库调用产生的开销。由于Node单线程执行的原因，此处无须担心状态同步问题。这种方式其实也可以应用到其他远程调用的场景中，即使外部没有缓存策略，也能有效节省重复开销。

此处可能因为存在侦听器过多引发的警告，需要调用setMaxListeners(0)移除掉警告，或者设更大的警告阈值。

once()方法产生的效果，也可以在著名的Gearman异步应用框架中实现。但在JavaScript中，实现这个效果十分容易。

3. 多异步之间的协作方案

事件发布/订阅模式有着它的优点。**利用高阶函数的优势，侦听器作为回调函数可以随意添加和删除，它帮助开发者轻松处理随时可能添加的业务逻辑。** 也可以 **隔离业务逻辑，保持业务逻辑单元的职责单一。** 一般而言，事件与侦听器的关系是一对多，但在异步编程中，也会出现事件与侦听器的关系是多对一的情况，也就是说一个业务逻辑可能依赖两个通过回调或事件传递的结果。前面提及的回调嵌套过深的原因即是如此。

这里我们尝试通过原生代码解决“难点2”中为了最终结果的处理而导致可以并行调用但实际只能串行执行的问题。我们的目标是既要享受异步I/O带来的性能提升，也要保持良好的编码风格。这里以渲染页面所需要的模板读取、数据读取和本地化资源读取为例简要介绍一下，相关代码如下：

```js
var count = 0;
var results = {};
var done = function (key, value) {
  results[key] = value;
  count++;
  if (count === 3) { // 渲染页面
    render(results); 
  }
};
fs.readFile(template_path, "utf8", function (err, template) { 
  done("template", template);
});
db.query(sql, function (err, data) {
  done("data", data);
});
l10n.get(function (err, resources) { 
  done("resources", resources);
});
```
由于多个异步场景中回调函数的执行并不能保证顺序，且回调函数之间互相没有任何交集，所以需要借助一个第三方函数和第三方变量来处理异步协作的结果。通常，我们把这个用于检测次数的变量叫做哨兵变量。聪明的你也许已经想到利用偏函数来处理哨兵变量和第三方函数的关系了，相关代码如下：

```js
var after = function (times, callback) {
  var count = 0, results = {}; 
  return function (key, value) {
    results[key] = value; 
    count++;
    if (count === times) { 
      callback(results);
    } 
  };
}; 
var done = after(times, render);
```
上述方案实现了多对一的目的。如果业务继续增长，我们依然可以继续利用发布/订阅方式来完成多对多的方案，相关代码如下：

```js
var emitter = new events.Emitter();
var done = after(times, render);

emitter.on("done", done);
emitter.on("done", other);

fs.readFile(template_path, "utf8", function (err, template) {
  emitter.emit("done", "template", template);
});
db.query(sql, function (err, data) { 
  emitter.emit("done", "data", data);
});
l10n.get(function (err, resources) {
  emitter.emit("done", "resources", resources);
});
```
这种方案结合了前者用简单的偏函数完成多对一的收敛和事件订阅/发布模式中一对多的发散。在上面的方法中，有一个令调用者不那么舒服的问题，那就是调用者要去准备这个done()函数，以及在回调函数中需要从结果中把数据一个一个提取出来，再进行处理。另一个方案则是来自笔者自己写的EventProxy模块，它是对事件订阅/发布模式的扩充，可以自由订阅组合事件。由于依旧采用的是事件订阅/发布模式，与Node十分契合，相关代码如下：

```js
var proxy = new EventProxy();
proxy.all("template", "data", "resources", function (template, data, resources) { 
  // TODO
});

fs.readFile(template_path, "utf8", function (err, template) {    
  proxy.emit("template", template);
});
db.query(sql, function (err, data) {
  proxy.emit("data", data);
});
l10n.get(function (err, resources) { 
  proxy.emit("resources", resources);
});
```
EventProxy提供了一个all()方法来订阅多个事件，当每个事件都被触发之后，侦听器才会执行。另外的一个方法是tail()方法，它与all()方法的区别在于all()方法的侦听器在满足条件之后只会执行一次，tail()方法的侦听器则在满足条件时执行一次之后，如果组合事件中的某个事件被再次触发，侦听器会用最新的数据继续执行。

all()方法带来的另一个改进则是：**在侦听器中返回数据的参数列表与订阅组合事件的事件列表是一致对应的。** 除此之外，在异步的场景下，我们常常需要从一个接口多次读取数据，此时触发的事件名或许是相同的。EventProxy提供了after()方法来实现事件在执行多少次后执行侦听器的单一事件组合订阅方式，示例代码如下：

```js
var proxy = new EventProxy();
proxy.after("data", 10, function (datas) { 
  // TODO
});
```
这段代码表示执行10次data事件后执行侦听器。这个侦听器得到的数据为10次按事件触发次序排序的数组。

**EventProxy模块除了可以应用于Node中外，还可以用在前端浏览器中。**

4. EventProxy的原理

EventProxy来自于Backbone的事件模块，Backbone的事件模块是Model、View模块的基础功能，在前端有广泛的使用。它在每个非all事件触发时都会触发一次all事件，相关代码如下：

```js
// Trigger an event, firing all bound callbacks. Callbacks are passed the
// same arguments as `trigger` is, apart from the event name.
// Listening for `"all"` passes the true event name as the first argument
trigger : function(eventName) { 
  var list, calls, ev, callback, args;
  var both = 2; 
  if (!(calls = this._callbacks)) return this;
  while (both--) { 
    ev = both ? eventName : 'all';
    if (list = calls[ev]) { 
      for (var i = 0, l = list.length; i < l; i++) {
        if (!(callback = list[i])) { 
          list.splice(i, 1); i--; l--;
        } else { 
          args = both ? Array.prototype.slice.call(arguments, 1) : arguments;
          callback[0].apply(callback[1] || this, args); 
        }
      } 
    }
  } 
  return this;
}
```
EventProxy则是将all当做一个事件流的拦截层，在其中注入一些业务来处理单一事件无法解决的异步处理问题。类似的扩展方法还有all()、tail()、after()、not()和any()等。

5. EventProxy的异常处理

EventProxy在事件发布/订阅模式的基础上还完善了异常处理。在异步方法中，异常处理需要占用一定比例的精力。在过去一段时间内，我们都是通过额外添加error事件来进行异常统一处理的，代码大致如下：

```js
exports.getContent = function (callback) {
  var ep = new EventProxy(); 
  ep.all('tpl', 'data', function (tpl, data) {
    // 成功回调 
    callback(null, {
      template: tpl, 
      data: data
    }); 
  });
  // 侦听error事件 
  ep.bind('error', function (err) {
    // 卸载掉所有处理函数 
    ep.unbind();
    // 异常回调 
    callback(err);
  }); 
  fs.readFile('template.tpl', 'utf-8', function (err, content) {
    if (err) { 
      // 一旦发生异常，一律交给error事件的处理函数处理
      return ep.emit('error', err); 
    }
    ep.emit('tpl', content); });
    db.get('some sql', function (err, result) { if (err) {
    // 一旦发生异常，一律交给error事件的处理函数处理 
    return ep.emit('error', err);
    } 
    ep.emit('data', result);
  });
};
```
因为异常处理的原因，代码量一下子多起来了，而EventProxy在实践过程中改进了这个问题，相关代码如下：

```js
exports.getContent = function (callback) {
  var ep = new EventProxy(); 
  ep.all('tpl', 'data', function (tpl, data) {
    // 成功回调 
    callback(null, {
      template: tpl, data: data
    }); 
  });
  //绑定错误处理函数 
  ep.fail(callback);
  fs.readFile('template.tpl', 'utf-8', ep.done('tpl'));
  db.get('some sql', ep.done('data'));
};
```
在上述代码中，EventProxy提供了fail()和done()这两个实例方法来优化异常处理，使得开发者将精力关注在业务部分，而不是在异常捕获上。关于fail()方法的实现，可以参见以下的变换：

```js
ep.fail(callback);
```
上面这行代码等价于下面的代码：

```js
ep.fail(function (err) { 
  callback(err);
});
```
又等价于：

```js
ep.bind('error', function (err) { 
  // 卸载掉所有处理函数
  ep.unbind(); 
  // 异常回调
  callback(err);
});
```
而done()方法的实现，也可参见以下的变换：

```js
ep.done('tpl');
```
它等价于：

```js
function (err, content) {
  if (err) { 
    // 一旦发生异常，一律交给error事件处理函数处理
    return ep.emit('error', err); 
  }
  ep.emit('tpl', content);
}
```
同时，done()方法也接受一个函数作为参数，相关代码如下所示：

```js
ep.done(function (content) {
  // TODO // 这里无须考虑异常
  ep.emit('tpl', content);
});
```
这段代码等价于：

```js
function (err, content) { 
  if (err) {
    // 一旦发生异常，一律交给error事件的处理函数处理 
    return ep.emit('error', err);
  } 
  (function (content) {
    // TODO 
    // 这里无须考虑异常
    ep.emit('tpl', content); 
  }(content));
}
```
当只传入一个回调函数时，需要手工调用emit()触发事件。另一个改进是同时传入事件名和回调函数，相关代码如下：

```js
ep.done('tpl', function (content) { 
  // content.replace('s', 'S');
  // TODO 
  // 无须关注异常
  return content;
});
```
在这种方式下，我们无须在回调函数中处理事件的触发，只需将处理过的数据返回即可。返回的结果将在done()方法中用作事件的数据而触发。

这里的fail()和done()十分类似Promise模式中的fail()和done()。换句话而言，这可以算作事件发布/订阅模式向Promise模式的借鉴。这样的完善既提升了程序的健壮性，同时也降低了代码量。

#### Promise/Deferred模式

使用事件的方式时，执行流程需要被预先设定。即便是分支，也需要预先设定，这是由发布/订阅模式的运行机制所决定的。下面为普通的Ajax调用：

```js
$.get('/api', { 
  success: onSuccess,
  error: onError, 
  complete: onComplete
})
```
在上面的异步调用中，必须严谨地设置目标。那么是否有一种先执行异步调用，延迟传递处理的方式呢？答案是Promise/Deferred模式。

Promise/Deferred模式在JavaScript框架中最早出现于Dojo的代码中，被广为所知则来自于jQuery 1.5版本，该版本几乎重写了Ajax部分，使得调用Ajax时可以通过如下的形式进行：

```js
$.get('/api')
.success(onSuccess) 
.error(onError)
.complete(onComplete);
```
这使得即使不调用success()、error()等方法，Ajax也会执行，这样的调用方式比预先传入回调让人觉得舒适一些。在原始的API中，一个事件只能处理一个回调，而通过Deferred对象，可以对事件加入任意的业务处理逻辑，示例代码如下：

```
$.get('/api') 
.success(onSuccess1)
.success(onSuccess2);
```
Promise/Deferred模式在2009年时被Kris Zyp抽象为一个提议草案，发布在CommonJS规范中。随着使用Promise/Deferred模式的应用逐渐增多，CommonJS草案目前已经抽象出了Promises/A、Promises/B、Promises/D这样典型的异步Promise/Deferred模型，这使得异步操作可以以一种优雅的方式出现。

**异步的广度使用使得回调、嵌套出现，但是一旦出现深度的嵌套，就会让编程的体验变得不愉快，而Promise/Deferred模式在一定程度上缓解了这个问题。** 这里我们将着重介绍Promises/A来以点代面介绍Promise/Deferred模式。

1. Promises/A

Promise/Deferred模式其实包含两部分，即Promise和Deferred。这里暂且不提两者的区别是什么，先看看Promises/A的行为吧。

Promises/A提议对单个异步操作做出了这样的抽象定义，具体如下所示。

- Promise操作只会处在3种状态的一种：未完成态、完成态和失败态。
- Promise的状态只会出现从未完成态向完成态或失败态转化，不能逆反。完成态和失败态不能互相转化。
- Promise的状态一旦转化，将不能被更改。

Promise的状态转化示意图如下图所示。

![Promise的状态转化示意图](https://graphbed.qiniu.songxingguo.com/Node-async-program/Promise%E7%9A%84%E7%8A%B6%E6%80%81%E8%BD%AC%E5%8C%96%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

在API的定义上，Promises/A提议是比较简单的。一个Promise对象只要具备then()方法即可。但是对于then()方法，有以下简单的要求。

- 接受完成态、错误态的回调方法。在操作完成或出现错误时，将会调用对应方法。
- 可选地支持progress事件回调作为第三个方法。
- then()方法只接受function对象，其余对象将被忽略。
- then()方法继续返回Promise对象，以实现链式调用。

then()方法的定义如下：

```js
then(fulfilledHandler, errorHandler, progressHandler)
```
为了演示Promises/A提议，这里我们尝试通过继承Node的events模块来完成一个简单的实现，相关代码如下：

```js
var Promise = function () { 
  EventEmitter.call(this);
};
util.inherits(Promise, EventEmitter);
Promise.prototype.then = function (fulfilledHandler, errorHandler, progressHandler) {
  if (typeof fulfilledHandler === 'function') { 
    // 利用once()方法，保证成功回调只执行一次
    this.once('success', fulfilledHandler); 
  }
  if (typeof errorHandler === 'function') { 
    // 利用once()方法，保证异常回调只执行一次
    this.once('error', errorHandler); 
  }
  if (typeof progressHandler === 'function') { 
    this.on('progress', progressHandler);
  } 
  return this;
};
```
这里看到then()方法所做的事情是将回调函数存放起来。为了完成整个流程，还需要触发执行这些回调函数的地方，实现这些功能的对象通常被称为Deferred，即延迟对象，示例代码如下：

```js
var Deferred = function () { 
  this.state = 'unfulfilled';
  this.promise = new Promise();
};
Deferred.prototype.resolve = function (obj) {
  this.state = 'fulfilled'; 
  this.promise.emit('success', obj);
};
Deferred.prototype.reject = function (err) { 
  this.state = 'failed';
  this.promise.emit('error', err);
};
Deferred.prototype.progress = function (data) {
  this.promise.emit('progress', data);
};
```
这里的状态和方法之间的对应关系如下图所示。

![状态和方法之间的对应关系](https://graphbed.qiniu.songxingguo.com/Node-async-program/%E7%8A%B6%E6%80%81%E5%92%8C%E6%96%B9%E6%B3%95%E4%B9%8B%E9%97%B4%E7%9A%84%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB.jpg)

利用Promises/A提议的模式，我们可以对一个典型的响应对象进行封装，相关代码如下：

```js
res.setEncoding('utf8');
res.on('data', function (chunk) {
  console.log('BODY: ' + chunk);
});
res.on('end', function () { 
  // Done
});
res.on('error', function (err) {
  // Error
});
```
上述代码可以转换为如下的简略形式：

```js
res.then(function () {
  // Done
}, function (err) {
  // Error
}, function (chunk) {
  console.log('BODY: ' + chunk);
});
```
要实现如此简单的API，只需要简单地改造一下即可，相关代码如下：

```js
var promisify = function (res) {
  var deferred = new Deferred(); 
  var result = '';
  res.on('data', function (chunk) { 
    result += chunk;
    deferred.progress(chunk); 
  });
  res.on('end', function () { 
    promise.resolve(result);
  }); 
  res.on('error', function (err) {
    deferred.reject(err); 
  });
  return deferred.promise;
};
```
如此就得到了简单的结果。这里返回deferred.promise的目的是为了不让外部程序调用resolve()和reject()方法，更改内部状态的行为交由定义者处理。下面为定义好Promise后的调用示例：

```js
promisify(res).then(function () {
  // Done
}, function (err) {
  // Error
}, function (chunk) {
  // progress 
  console.log('BODY: ' + chunk);
});
```
这里回到Promise和Deferred的差别上。从上面的代码可以看出，Deferred主要是用于内部，用于维护异步模型的状态；Promise则作用于外部，通过then()方法暴露给外部以添加自定义逻辑。Promise和Deferred的整体关系如下图所示。

![Promise和Deferred的整体关系](https://graphbed.qiniu.songxingguo.com/Node-async-program/Promise%E5%92%8CDeferred%E7%9A%84%E6%95%B4%E4%BD%93%E5%85%B3%E7%B3%BB.jpg)

与事件发布/订阅模式相比，Promise/Deferred模式的API接口和抽象模型都十分简洁。从上图中也可以看出，它将业务中不可变的部分封装在了Deferred中，将可变的部分交给了Promise。此时问题就来了，对于不同的场景，都需要去封装和改造其Deferred部分，然后才能得到简洁的接口。如果场景不常用，封装花费的时间与带来的简洁相比并不一定划算。

Promise是高级接口，事件是低级接口。低级接口可以构成更多更复杂的场景，高级接口一旦定义，不太容易变化，不再有低级接口的灵活性，但对于解决典型问题非常有效。Promises/A的模型抽象在几种Promise提议中相对简洁。


这里再介绍一下Q。Q模块是Promises/A规范的一个实现，可以通过npm install q进行安装使用。它对Node中常见回调函数的Promise实现如下：

```js
/** 
* Creates a Node-style callback that will resolve or reject the deferred
* promise. * @returns a nodeback
*/
defer.prototype.makeNodeResolver = function () {
  var self = this; 
  return function (error, value) {
    if (error) { 
      self.reject(error);
    } else if (arguments.length > 2) { 
      self.resolve(array_slice(arguments, 1));
    } else { 
      self.resolve(value);
    } 
  };
};
```
可以看到这里是一个高阶函数的使用，makeNodeResolver返回了一个Node风格的回调函数。对于fs.readFile()的调用，将会演化为如下形式：

```js
var readFile = function (file, encoding) { 
  var deferred = Q.defer();
  fs.readFile(file, encoding, deferred.makeNodeResolver()); 
  return deferred.promise;
};
```
定义之后的调用示例如下：

```js
readFile("foo.txt", "utf-8").then(function (data) { 
  // Success case
}, function (err) { 
  // Failed case
});
```
Promise通过封装异步调用，实现了正向用例和反向用例的分离以及逻辑处理延迟，这使得回调函数相对优雅。

前面分析了Q对Node异步回调的处理。事实上，异步编程中需要花费很多精力进行异常的判断和处理，为了分离异常和正常情况，我写了一个模块memeda用于处理makeNodeResolver相似的事情。在下面的调用示例中可以看到，正常结果和异常结果被分离到两个函数中：

```js
var failing = require('memeda').failing;
fs.readFile(file, encoding, failing(function (err) {
  // TODO
}).passing(function (data) {
  // TODO
}));
```
我们可以对Q和memeda模块略做比较。两者相似之处在于分离逻辑，使开发者侧重关注正常情况。不同之处在于Q通过promise()可以实现延迟处理，以及通过多次调用then()附加更多结果处理逻辑。可以看到，Promise需要封装，但是强大，具备很强的侵入性；纯粹的函数则较为轻量，但功能相对弱小。

2. Promise中的多异步协作

在Promise的介绍中说过，主要解决的是单个异步操作中存在的问题。回到我们的难点，当我们需要处理多个异步调用时，又该如何处理呢？类似于EventProxy，这里给出了一个简单的原型实现，相关代码如下：

```js
Deferred.prototype.all = function (promises) { 
  var count = promises.length;
  var that = this; 
  var results = [];
  promises.forEach(function (promise, i) { 
    promise.then(function (data) {
      count--; 
      results[i] = data;
      if (count === 0) { 
        that.resolve(results);
      } 
    }, function (err) {
      that.reject(err); 
    });
  }); 
  return this.promise;
};
```
对于多次文件的读取场景，以下面的代码为例，all()方法将两个单独的Promise重新抽象组合成一个新的Promise：

```js
var promise1 = readFile("foo.txt", "utf-8");
var promise2 = readFile("bar.txt", "utf-8");
var deferred = new Deferred();
deferred.all([promise1, promise2]).then(function (results) {
  // TODO
}, function (err) {
  // TODO
});
```
这里通过all()方法抽象多个异步操作。只有所有异步操作成功，这个异步操作才算成功，一旦其中一个异步操作失败，整个异步操作就失败。本节的代码主要用于描述Promise的原理，在成熟度上并未如when和Q模块。在实际的应用中，可以通过NPM安装这两个模块，它们是完整的Promise提议的实现。

3. Promise的进阶知识

在API的暴露上，Promise模式比原始的事件侦听和触发略为优美，它的缺陷则是需要为不同的场景封装不同的API，没有直接的原生事件那么灵活。但对于经典的场景，封装出API的成本也并不高，值得一做。Promise的秘诀其实在于对队列的操作。这里介绍一个实际的案例，我在处理自动化测试时，要跟远程服务器之间进行多次指令发送，这些指令是按顺序依次进行的。在Node中，网络库是完全异步的，无法在编程层面实现像其他语言那般的同步调用。由于网站界面通常都是由前端工程师完成的，用JavaScript编写自动化测试可以减轻他们切换环境的痛苦，所以不能因为无法同步调用就放弃掉Node。解决同步调用问题的答案也就是采用Deferred模式。

现在有一组纯异步的API，为了完成一串事情，我们的代码大致如下：

```js
obj.api1(function(value1) {
  obj.api2(value1,
  function(value2) {
    obj.api3(value2,
    function(value3) {
      obj.api4(value3,
      function(value4) {
        callback(value4);
      });
    });
  });
});
```
由于有按每个步骤依次执行的需求，所以必须嵌套执行。但那样我们会得到难看的嵌套，超过10个连续嵌套就会让代码十分难看。于是我们得到了“Pyramid of Doom”，译为中文，是谓“恶魔金字塔”。相信初入Node世界的人，也写过不少此类代码。下面我们通过普通的函数将上面的代码尝试展开：

```js
var handler1 = function(value1) {
  obj.api2(value1, handler2);
};
var handler2 = function(value2) {
  obj.api3(value2, handler3);
};
var handler3 = function(value3) {
  obj.api4(value3, hander4);
};
var handler4 = function(value4) {
  callback(value4);
});
```
obj.api1(handler1);对于喜欢利用事件的开发者，我们展开后的代码又将会是怎样的情况呢？具体如下所示：

```js
var emitter = new event.Emitter();
emitter.on("step1",function() {
  obj.api1(function(value1) {
    emitter.emit("step2", value1);
  });
});
emitter.on("step2",function(value1) {
  obj.api2(value1,
  function(value2) {
    emitter.emit("step3", value2);
  });
});
emitter.on("step3",function(value2) {
  obj.api3(value2,
  function(value3) {
    emitter.emit("step4", value3);
  });
});
emitter.on("step4",function(value3) {
  obj.api4(value3,
  function(value4) {
    callback(value4);
  });
});
emitter.emit("step1");
```
利用事件展开后的效果变得越来越糟糕了。与纯粹嵌套相比，代码量明显增加了，这显然不会带来良好的编程体验。为此，我们需要一种更好的方式。支持序列执行的Promise
理想的编程体验应当是前一个的调用结果作为下一个调用的开始，是传说中的链式调用，相关代码如下：

```js
promise()
.then(obj.api1)
.then(obj.api2)
.then(obj.api3)
.then(obj.api4)
.then(function(value4) { 
  // Do something with value4
},
function(error) { 
  // Handle any error from step1 through step4
})
.done();
```
尝试改造一下代码以实现链式调用，具体如下所示：

```js
var Deferred = function() {
  this.promise = new Promise();
};
// 完成态
Deferred.prototype.resolve = function(obj) {
  var promise = this.promise;
  var handler;
  while ((handler = promise.queue.shift())) {
    if (handler && handler.fulfilled) {
      var ret = handler.fulfilled(obj);
      if (ret && ret.isPromise) {
        ret.queue = promise.queue;
        this.promise = ret;
        return;
      }
    }
  }
};
// 失败态
Deferred.prototype.reject = function(err) {
  var promise = this.promise;
  var handler;
  while ((handler = promise.queue.shift())) {
    if (handler && handler.error) {
      var ret = handler.error(err);
      if (ret && ret.isPromise) {
        ret.queue = promise.queue;
        this.promise = ret;
        return;
      }
    }
  }
};
// 生成回调函数
Deferred.prototype.callback = function() {
  var that = this;
  return function(err, file) {
    if (err) {
      return that.reject(err);
    }
    that.resolve(file);
  };
};
var Promise = function() { // 队列用于存储待执行的回调函数
  this.queue = [];
  this.isPromise = true;
};
Promise.prototype.then = function(fulfilledHandler, errorHandler, progressHandler) {
  var handler = {};
  if (typeof fulfilledHandler === 'function') {
    handler.fulfilled = fulfilledHandler;
  }
  if (typeof errorHandler === 'function') {
    handler.error = errorHandler;
  }
  this.queue.push(handler);
  return this;
};
```
这里我们以两次文件读取作为例子，以验证该设计的可行性。这里假设读取第二个文件是依赖于第一个文件中的内容的，相关代码如下：

```js
var readFile1 = function(file, encoding) {
  var deferred = new Deferred();
  fs.readFile(file, encoding, deferred.callback());
  return deferred.promise;
};
var readFile2 = function(file, encoding) {
  var deferred = new Deferred();
  fs.readFile(file, encoding, deferred.callback());
  return deferred.promise;
};
readFile1('file1.txt', 'utf8').then(function(file1) {
  return readFile2(file1.trim(), 'utf8');
}).then(function(file2) {
  console.log(file2);
});
```
将这段代码存为sequence.js文件。执行该代码，将会得到以下的输出结果：

```js
$ node sequence.js
I am file2
```
要让Promise支持链式执行，主要通过以下两个步骤。

（1）将所有的回调都存到队列中。
（2）Promise完成时，逐个执行回调，一旦检测到返回了新的Promise对象，停止执行，然后将当前Deferred对象的promise引用改变为新的Promise对象，并将队列中余下的回调转交给它。

写到这里，你是否明了恶魔金字塔该如何优化？再次重申，这里的代码主要用于研究Promise的实现原理。在更多细节的优化方面，Q或者when等Promise库做得更好，实际应用时请采用这些成熟库。

将API Promise化

这里仍然会发现，为了体验更好的API，需要做较多的准备工作。这里提供了一个方法可以批量将方法Promise化，相关代码如下：

```js
// smooth(fs.readFile);
var smooth = function (method) {
  return function() {
    var deferred = new Deferred();
    var args = Array.prototype.slice.call(arguments, 1);
    args.push(deferred.callback());
    method.apply(null, args);
    return deferred.promise;
  };
};
```
于是前面的两次文件读取的构造：

```js
var readFile1 = function(file, encoding) {
  var deferred = new Deferred();
  fs.readFile(file, encoding, deferred.callback());
  return deferred.promise;
};
var readFile2 = function(file, encoding) {
  var deferred = new Deferred();
  fs.readFile(file, encoding, deferred.callback());
  return deferred.promise;
};
```
可以简化为：

```js
var readFile = smooth(fs.readFile);
```
要实现同样的效果，代码量将会锐减到：

```js
var readFile = smooth(fs.readFile);
readFile('file1.txt', 'utf8').then(function(file1) {
  return readFile(file1.trim(), 'utf8');
}).then(function(file2) { 
  // file2 => I am file2
  console.log(file2);
});
```

#### 流程控制库

1. 尾触发与Next

除了事件和Promise外，还有一类方法是需要手工调用才能持续执行后续调用的，我们将此类方法叫做尾触发，常见的关键词是next。事实上，尾触发目前应用最多的地方是Connect的中间件。

这里我们暂且不关注Connect的具体应用，先看一下Connect的API暴露方式，相关代码如下：

```js
var app = connect(); // Middleware
app.use(connect.staticCache());
app.use(connect.static(__dirname + '/public'));
app.use(connect.cookieParser());
app.use(connect.session());
app.use(connect.query());
app.use(connect.bodyParser());
app.use(connect.csrf());
app.listen(3001);
```
在通过use()方法注册好一系列中间件后，监听端口上的请求。中间件利用了尾触发的机制，最简单的中间件如下：

```js
function (req, res, next) {
  // 中间件
}
```
每个中间件传递请求对象、响应对象和尾触发函数，通过队列形成一个处理流，如下图所示。

![中间件通过队列形成一个处理流](https://graphbed.qiniu.songxingguo.com/Node-async-program/%E4%B8%AD%E9%97%B4%E4%BB%B6%E9%80%9A%E8%BF%87%E9%98%9F%E5%88%97%E5%BD%A2%E6%88%90%E4%B8%80%E4%B8%AA%E5%A4%84%E7%90%86%E6%B5%81.jpg)

**中间件机制使得在处理网络请求** 时，可以像 **面向切面编程** 一样 **进行过滤、验证、日志等功能，而不与具体业务逻辑产生关联，以致产生耦合。** 

下面我们来看Connect的核心实现，相关代码如下：

```js
function createServer() {
  function app(req, res) {
    app.handle(req, res);
  }
  utils.merge(app, proto);
  utils.merge(app, EventEmitter.prototype);
  app.route = '/';
  app.stack = [];
  for (var i = 0; i < arguments.length; ++i) {
    app.use(arguments[i]);
  }
  return app;
};
```
这段代码通过如下代码创建了HTTP服务器的request事件处理函数：

```js
function app(req, res){ 
  app.handle(req, res); 
}
```
但真正的核心代码是

```js
app.stack = [];
```
这句。stack属性是这个服务器内部维护的中间件队列。通过调用use()方法我们可以将中间件放进队列中。下面的代码为use()方法的重要部分：

```js
app.use = function(route, fn) {
  // some code 
  this.stack.push({
    route: route,
    handle: fn
  });
  return this;
};
```
此时就建好处理模型了。接下来，结合Node原生http模块实现监听即可。监听函数的实现如下：

```js
app.listen = function() {
  var server = http.createServer(this);
  return server.listen.apply(server, arguments);
};
```
最终回到app.handle()方法，每一个监听到的网络请求都将从这里开始处理。该方法的代码如下：

```js
app.handle = function(req, res, out) { 
  // some code
  next();
};
```
原始的next()方法较为复杂，下面是简化后的内容，其原理十分简单，取出队列中的中间件并执行，同时传入当前方法以实现递归调用，达到持续触发的目的：

```js
function next(err) {
  // some code 
  // next callback
  layer = stack[index++];
  layer.handle(req, res, next);
}
```
**所有嫌异步编程复杂的开发者均可以参考Connect的流式处理，这对于划分业务逻辑、逐步处理均有效。**

值得提醒的是，尽管中间件这种尾触发模式并不要求每个中间方法都是异步的，但是如果每个步骤都采用异步来完成，实际上只是串行化的处理，没办法通过并行的异步调用来提升业务的处理效率。流式处理可以将一些串行的逻辑扁平化，但是并行逻辑处理还是需要搭配事件或者Promise完成的，这样业务在纵向和横向都能够各自清晰。

在Connect中，尾触发十分适合处理 **网络请求** 的场景。**将复杂的处理逻辑拆解为简洁、单一的处理单元，逐层次地处理请求对象和响应对象。**

2. async

接下来，我们要介绍最知名的流程控制模块async。async长期占据NPM依赖榜的前三名，可见在Node开发中，流程控制是开发过程中的基本需求。async模块提供了20多个方法用于处理异步的各种协作模式，这里我们介绍几种典型用法。

异步的串行执行

这里我们依旧采用前面读取两个文件的例子，看一下async是如何解决“ **恶魔金字塔** ”问题的。**async提供了series()方法来实现一组任务的串行执行** ，示例代码如下：

```js
async.series([function(callback) {
  fs.readFile('file1.txt', 'utf-8', callback);
},
function(callback) {
  fs.readFile('file2.txt', 'utf-8', callback);
}],function(err, results) {
  // results => [file1.txt, file2.txt]
});  
```
这段代码等价于：

```js
fs.readFile('file1.txt', 'utf-8',
function(err, content) {
  if (err) {
    return callback(err);
  }
  fs.readFile('file1.txt', 'utf-8',
  function(err, data) {
    if (err) {
      return callback(err);
    }
    callback(null, [content, data]);
  });
});
```
这段代码值得玩味的是回调函数。可以发现，series()方法中传入的函数callback()并非由使用者指定。事实上，此处的回调函数由async通过高阶函数的方式注入，这里隐含了特殊的逻辑。每个callback()执行时会将结果保存起来，然后执行下一个调用，直到结束所有调用。最终的回调函数执行时，队列里的异步调用保存的结果以数组的方式传入。这里的异常处理规则是一旦出现异常，就结束所有调用，并将异常传递给最终回调函数的第一个参数。

异步的并行执行

当我们需要通过并行来提升性能时，async提供了parallel()方法，用以并行执行一些异步操作。以下为读取两个文件的并行版本：

```js
async.parallel([function(callback) {
  fs.readFile('file1.txt', 'utf-8', callback);
},
function(callback) {
  fs.readFile('file2.txt', 'utf-8', callback);
}],function(err, results) { 
  // results => [file1.txt, file2.txt]
});
```
上面这段代码等价于下面的代码：

```js
var counter = 2;
var results = [];
var done = function(index, value) {
  results[index] = value;
  counter--;
  if (counter === 0) {
    callback(null, results);
  }
};
// 只传递第一个异常
var hasErr = false;
var fail = function(err) {
  if (!hasErr) {
    hasErr = true;
    callback(err);
  }
};
fs.readFile('file1.txt', 'utf-8',
function(err, content) {
  if (err) {
    return fail(err);
  }
  done(0, content);
});
fs.readFile('file2.txt', 'utf-8',
function(err, data) {
  if (err) {
    return fail(err);
  }
  done(1, data);
});
```
同样，通过async编写的代码既没有深度的嵌套，也没有复杂的状态判断，它的诀窍依然来自于注入的回调函数。parallel()方法对于异常的判断依然是一旦某个异步调用产生了异常，就会将异常作为第一个参数传入给最终的回调函数。只有所有异步调用都正常完成时，才会将结果以数组的方式传入。
也许你还记得EventProxy的方案，如下所示：

```js
var EventProxy = require('eventproxy');
var proxy = new EventProxy();
proxy.all('content', 'data',function(content, data) {
  callback(null, [content, data]);
}) proxy.fail(callback);
fs.readFile('file1.txt', 'utf-8', proxy.done('content'));
fs.readFile('file2.txt', 'utf-8', proxy.done('data'));
```
与通过async编写所产生的代码量相差并不大。EventProxy虽然基于事件发布/订阅模式而设计，但也用到了与async相同的原理，通过特殊的回调函数来隐含返回值的处理。所不同的是，在async的框架模式下，这个回调函数由async封装后传递出来，而EventProxy则通过done()和fail()方法来生成新的回调函数。这两种实现方式都是高阶函数的应用。

异步调用的依赖处理

series()适合无依赖的异步串行执行，但当前一个的结果是后一个调用的输入时，series()方法就无法满足需求了。所幸，这种典型场景的需求，async提供了waterfall()方法来满足，相关代码如下：

```js
async.waterfall([function(callback) {
  fs.readFile('file1.txt', 'utf-8',
  function(err, content) {
    callback(err, content);
  });
},
function(arg1, callback) {
  // arg1 => file2.txt 
  fs.readFile(arg1, 'utf-8', function (err, content) {
  callback(err, content);
});
},
function(arg1, callback) {
  // arg1 => file3.txt 
  fs.readFile(arg1, 'utf-8', function (err, content) {
  callback(err, content);
});}],function(err, result) {
  // result => file4.txt
});  
```
这段代码等价于如下代码：

```js
fs.readFile('file1.txt', 'utf-8',
function(err, data1) {
  if (err) {
    return callback(err);
  }
  fs.readFile(data1, 'utf-8',
  function(err, data2) {
    if (err) {
      return callback(err);
    }
    fs.readFile(data2, 'utf-8',
    function(err, data3) {
      if (err) {
        return callback(err);
      }
      callback(null, data3);
    });
  });
});
```
自动依赖处理

在现实的业务环境中，具有很多复杂的依赖关系，这些业务或是异步，或是同步。这种混杂的编程环境经常让人处于理不清顺序的情况。为此，async提供了一个强大的方法auto()实现复杂业务处理。

假设我们的业务场景如下：

（1）从磁盘读取配置文件。
（2）根据配置文件连接MongoDB。
（3）根据配置文件连接Redis。
（4）编译静态文件。
（5）上传静态文件到CDN。
（6）启动服务器。

简单映射一下上述业务：

```js
{
  readConfig: function() {},
  connectMongoDB: function() {},
  connectRedis: function() {},
  complieAsserts: function() {},
  uploadAsserts: function() {},
  startup: function() {}
}
```
接下来分析一下依赖关系。可以看出，connectMongoDB和connectRedis依赖readConfig，uploadAsserts依赖complieAsserts，startup则依赖所有完成。依赖关系如下：

```js
var deps = {
  readConfig: function(callback) { // read config file
    callback();
  },
  connectMongoDB: ['readConfig',
  function(callback) { // connect to mongodb
    callback();
  }],
  connectRedis: ['readConfig',
  function(callback) { // connect to redis
    callback();
  }],
  complieAsserts: function(callback) { // complie asserts
    callback();
  },
  uploadAsserts: ['complieAsserts',
  function(callback) { // upload to assert
    callback();
  }],
  startup: ['connectMongoDB', 'connectRedis', 'uploadAsserts',
  function(callback) { // startup
  }]
};
```
auto()方法能根据依赖关系自动分析，以最佳的顺序执行以上业务：

```js
async.auto(deps);
```
转换到EventProxy的实现，则需要更细腻的事件分配，相关代码如下：

```js
proxy.assp('readtheconfig',function() {
  // read config file 
  proxy.emit('readConfig');
}).on('readConfig',function() { 
  // connect to mongodb
  proxy.emit('connectMongoDB');
}).on('readConfig',function() {
  // connect to redis 
  proxy.emit('connectRedis');
}).assp('complietheasserts',function() { 
  // complie asserts
  proxy.emit('complieAsserts');
}).on('complieAsserts',function() {
  // upload to assert 
  proxy.emit('uploadAsserts');
}).all('connectMongoDB', 'connectRedis', 'uploadAsserts',function() { 
  // Startup
});
```

小结

本节主要介绍async的几种常见用法。此外，async还提供了forEach、map等类ECMAScript5中数组的方法，更多细节可关注 https://github.com/caolan/async 。

3. Step

另一个知名的流程控制库是Tim Caswell的Step，它比async更轻量，在API的暴露上也更具备一致性，因为它只有一个接口Step。通过npm install step即可安装使用。示例代码如下：

```js
Step(task1, task2, task3);
```
Step接受任意数量的任务，所有的任务都将会串行依次执行。下面的示例代码将依次读取文件：

```js
Step(function readFile1() {
  fs.readFile('file1.txt', 'utf-8', this);
},
function readFile2(err, content) {
  fs.readFile('file2.txt', 'utf-8', this);
},
function done(err, content) {
  console.log(content);
});
```
可以看到，Step与前面介绍的事件模式、Promise甚至async都不同的一点在于Step用到了this关键字。事实上，它是Step内部的一个next()方法，将异步调用的结果传递给下一个任务作为参数，并调用执行。

并行任务执行

那么，Step如何实现多个异步任务并行执行呢？this具有一个parallel()方法，它告诉Step，需要等所有任务完成时才进行下一个任务，相关代码如下：

```js
Step(function readFile1() {
  fs.readFile('file1.txt', 'utf-8', this.parallel());
  fs.readFile('file2.txt', 'utf-8', this.parallel());
},
function done(err, content1, content2) { // content1 => file1
  // content2 => file2 console.log(arguments);
});
```
使用parallel()的时候需要小心的是，如果异步方法的结果传回的是多个参数，Step将只会取前两个参数，相关代码如下：

```js
var asyncCall = function(callback) {
  process.nextTick(function() {
    callback(null, 'result1', 'result2');
  });
};
```
在调用parallel()时，result2将会被丢弃。

Step的parallel()方法的原理是每次执行时将内部的计数器加1，然后返回一个回调函数，这个回调函数在异步调用结束时才执行。当回调函数执行时，将计数器减1。当计数器为0的时候，告知Step所有异步调用结束了，Step会执行下一个方法。

Step与async相同的是异常处理，一旦有一个异常产生，这个异常会作为下一个方法的第一个参数传入。

结果分组

Step提供的另外一个方法是group()，它类似于parallel()的效果，但是在结果传递上略有不同。下面的代码用于读取一个目录，然后迭代其中文件的操作：

```js
Step(function readDir() {
  fs.readdir(__dirname, this);
},
function readFiles(err, results) {
  if (err) throw err;
  // Create a new group 
  var group = this.group();
  results.forEach(function(filename) {
    if (/\.js$/.test(filename)) {
      fs.readFile(__dirname + "/" + filename, 'utf8', group());
    }
  });
},
function showAll(err, files) {
  if (err) throw err;
  console.dir(files);
});
```
我们注意到有两次group()的调用。第一次调用是告知Step要并行执行，第二次调用的结果将会生成一个回调函数，而回调函数接受的返回值将会按组存储。parallel()传递给下一个任务的结果是如下形式：

```js
function(err, result1, result2, ...);

```
group()传递的结果是：

```js
function(err, results);
```
这个函数返回的数据保存在数组中。

4. wind

这里还要介绍一种思路完全不同的异步编程方案wind（https://github.com/JeffreyZhao/wind）。它的前身为Jscex，由国内知名码农赵劼完成开发。它为JavaScript语言提供了一个monadic扩展，能够显著提高一些常见场景下的异步编程体验。

异步编程有时需要面临的场景非常特殊，下面我们由一个冒泡排序来了解wind的特殊之处：

```js
var compare = function(x, y) {
  return x - y;
};
var swap = function(a, i, j) {
  var t = a[i];
  a[i] = a[j];
  a[j] = t;
};
var bubbleSort = function(array) {
  for (var i = 0; i < array.length; i++) {
    for (var j = 0; j < array.length - i - 1; j++) {
      if (compare(array[j], array[j + 1]) > 0) {
        swap(array, j, j + 1);
      }
    }
  }
};
```
现在我们要添加的需求是，将这个冒泡排序动画起来。这意味着在swap()方法中需要添加动画逻辑，这在JavaScript中并不是一件难事，困难的地方在于动画需要延时的方式完成。但在JavaScript中只有setTimeout()能够实现延时功能（用while判断时间的方式不可取，这在前面有所描述）。我们知道，setTimeout()是一个异步方法，在执行后，将立即返回。所以，难点出现在：

- 动画执行时无法停止排序算法的执行；
- 排序算法的继续执行将会启动更多动画。

因此，逐步骤的动画将难以实现，而wind在解决这个问题上体现出了它的独特魅力之处，相关代码如下：

```js
var compare = function(x, y) {
  return x - y;
};
var swapAsync = eval(Wind.compile("async",function(a, i, j) {
  $await(Wind.Async.sleep(20));
  // 暂停20毫秒 
  var t = a[i];
  a[i] = a[j];
  a[j] = t;
  paint(a); // 重绘数组
}));
var bubbleSort = eval(Wind.compile("async",function(array) {
  for (var i = 0; i < array.length; i++) {
    for (var j = 0; j < array.length - i - 1; j++) {
      if (compare(array[j], array[j + 1]) > 0) {
        $await(swapAsync(array, j, j + 1));
      }
    }
  }
}));
```
上述代码实现了暂停20毫秒、绘制动画、继续排序的效果。从代码的角度来说，这里虽然介入了异步方法，但是并没有如同其他异步流程控制库那样变得异步化，逻辑并没有因为异步被拆分。同时可以注意到，我们的代码中引入了一些新的东西：

- eval(Wind.compile("async",function() {}));
- $await();
- Wind.Async.sleep(20);

下面我们将详细介绍以上3行代码的特异之处。

异步任务定义eval()函数在业界一向是一个需要谨慎对待的函数，Douglas Crockford更是深恶痛绝地将其称为魔鬼，因为它能访问上下文和编译器，可能导致上下文混乱。大多数利用eval()函数的人都不能把握好它的用法，导致Douglas Crockford认为它是JavaScript可有可无的功能。

但是在wind的世界里，恰好反Douglas Crockford之道而行之，巧妙地利用了eval()访问上下文的特性。Wind.compile()会将普通的函数进行编译，然后交给eval()执行。换言之，eval(Wind.compile("async", function () {}));定义了异步任务。Wind.Async.sleep();则内置了对setTimeout()的封装。

$await()与任务模型

在定义完异步方法后，wind提供了$await()方法实现等待完成异步方法。但事实上，它并不是一个方法，也不存在于上下文中，只是一个等待的占位符，告之编译器这里需要等待。$await()接受的参数是一个任务对象，表示等待任务结束后才会执行后续操作。每一个异步操作都可以转化为一个任务，wind正是基于任务模型实现的。下面的代码用于将fs.readFile()调用转化为一个任务模型：

```js
var Wind = require("wind");
var Task = Wind.Async.Task;
var readFileAsync = function(file, encoding) {
  return Task.create(function(t) {
    fs.readFile(file, encoding,
    function(err, file) {
      if (err) {
        t.complete("failure", err);
      } else {
        t.complete("success", file);
      }
    });
  });
};
```
除了通过eval(Wind.compile("async", function () {}));定义任务外，正式的任务创建方法为Task.create()。执行readFileAsync()进行偏函数转换得到真正的任务。异步方法在执行结束时，可以通过complete()传递failure或success信息，告知任务执行完毕。如果是failure则可以通过try/catch捕获异常。这略微有些打破前述try/catch无法捕获回调函数中异常的定论。下面的代码为调用readFileAsync()得到一个任务的示例：

```js
var task = readFileAsync('file1.txt', 'utf-8');
```

下面我们如同介绍async或者Step的串行执行示例一样，尝试感受一下wind的风采：

```js
var serial = eval(Wind.compile("async",
function() {
  var file1 = $await(readFileAsync('file1.txt', 'utf-8'));
  console.log(file1);
  var file2 = $await(readFileAsync('file2.txt', 'utf-8'));
  console.log(file2);
  try {
    var file3 = $await(readFileAsync('file3.txt', 'utf-8'));
  } catch(err) {
    console.log(err);
  }
}));
serial().start();
```
执行上述代码，将得到如下输出：

```
file1
file2
{ [Error: ENOENT, open 'file3.txt'] errno: 34, code: 'ENOENT', path: 'file3.txt' }
```
异步方法在JavaScript中通常会立即返回，在wind中做到了不阻塞CPU但阻塞代码的目的。接下来我们尝试下并行的效果，相关代码如下：

```js
var parallel = eval(Wind.compile("async",
function() {
  var result = $await(Task.whenAll({
    file1: readFileAsync('file1.txt', 'utf-8'),
    file2: readFileAsync('file2.txt', 'utf-8')
  }));
  console.log(result.file1);
  console.log(result.file2);
}));
parallel().start();
```
得到输出：

file1
file2

wind提供了whenAll()来处理并发，通过$await关键字将等待配置的所有任务完成后才向下继续执行。

异步方法转换辅助函数

可以看到，除了eval(Wind.compile("async", function () {}))在实际代码中稍显冗长外，异步调用在代码层面上已经与同步调用相差无几。这十分适合从已有的采用同步编写方式的代码向Node迁移，可以省掉重写代码的开销。如同Promise/Deferred模式可以让异步编程模型变简单，这种近同步编程的体验需要我们额外或者提前完成的事情是：将异步方法任务化。这种任务化的过程可以看作是Promise/Deferred的封装。如果每个方法都如readFileAsync一般去定义，将会是一个庞大的工作量。wind提供了两个方法来辅助转换：


- Wind.Async.Binding.fromCallback
- Wind.Async.Binding.fromStandard

在Node中异步方法的回调传值有两种，一种是无异常的调用，通常只有一个参数返回，如下所示：

```js
fs.exists("/etc/passwd", function (exists) { 
  // exists参数表示是否存在
});
```
而fromCallback用于转换这类异步调用为wind中的任务。

另一类是带异常的调用，遵循规范将返回参数列表的第一个参数作为异常标示，如下所示：

```js
fs.readFile('file1.txt', function (err, data) {
  // err表示异常
});
```
而fromStandard用于转换这类异步调用到wind中的任务。

是故，readFileAsync的定义其实只要一行代码即可实现：

```js
var readFileAsync = Wind.Async.Binding.fromStandard(fs.readFile);
```
5. 流程控制小结

从本书介绍的各个流程控制案例来看，从解决“恶魔金字塔”到解决异步协作的方法有多种，几个类库几乎各显神通。异步编程虽然相对复杂，但并非难事，相同的问题通过各种技巧依然能将复杂的事情简化。

这里简单对比下几种方案的区别：事件发布/订阅模式相对算是一种较为原始的方式，Promise/Deferred模式贡献了一个非常不错的异步任务模型的抽象。而上述的这些异步流程控制方案与Promise/Deferred模式的思路不同，Promise/Deferred的重头在于封装异步的调用部分，流程控制库则显得没有模式，将处理重点放置在回调函数的注入上。从自由度上来讲，async、Step这类流控库要相对灵活得多。EventProxy库则主要借鉴事件发布/订阅模式和流程控制库通过高阶函数生成回调函数的方式实现。

除了async、Step、EventProxy、wind等方案外，还有一类通过源代码编译的方案来实现流程控制的简化，streamline是典型的例子。这类例子并不在本章的讨论范围内，如果读者有兴趣，可以自行查阅相关资料。

### 异步并发控制

在陆续介绍的各种异步编程方法里，解决的问题无外乎保持异步的性能优势，提升编程体验，但是这里有一个过犹不及的案例。

在Node中，我们可以十分方便地利用异步发起并行调用。使用下面的代码，我们可以轻松发起100次异步调用：

```js
for (var i = 0, i < 100; i++) {
  async();
}
```
但是如果并发量过大，我们的下层服务器将会吃不消。如果是对文件系统进行大量并发调用，操作系统的文件描述符数量将会被瞬间用光，抛出如下错误：

```
Error: EMFILE, too many open files
```
可以看出，异步I/O与同步I/O的显著差距：同步I/O因为每个I/O都是彼此阻塞的，在循环体中，总是一个接着一个调用，不会出现耗用文件描述符太多的情况，同时性能也是低下的；对于异步I/O，虽然并发容易实现，但是由于太容易实现，依然需要控制。换言之，尽管是要压榨底层系统的性能，但还是需要给予一定的过载保护，以防止过犹不及。

#### bagpipe的解决方案

如何对既有的异步API添加过载保护，我们期望的当然不是去改动API。

我写的bagpipe模块的解决思路是这样的。

- 通过一个队列来控制并发量。
- 如果当前活跃（指调用发起但未执行回调）的异步调用量小于限定值，从队列中取出执行。
- 如果活跃调用达到限定值，调用暂时存放在队列中。每个异步调用结束时，从队列中取出新的异步调用执行。

bagpipe的API主要暴露了一个push()方法和full事件，示例代码如下：

```js
/**
* 推入方法，参数。最后一个参数为回调函数 * @param {Function} method 异步方法
* @param {Mix} args 参数列表，最后一个参数为回调函数 
*/
Bagpipe.prototype.push = function(method) {
  var args = [].slice.call(arguments, 1);
  var callback = args[args.length - 1];
  if (typeof callback !== 'function') {
    args.push(function() {});
  }
  if (this.options.disabled || this.limit < 1) {
    method.apply(null, args);
    return this;
  }
  // 队列长度也超过限制值时
  if (this.queue.length < this.queueLength || !this.options.refuse) {
    this.queue.push({
      method: method,
      args: args
    });
  } else {
    var err = new Error('Too much async call in queue');
    err.name = 'TooMuchAsyncCallError';
    callback(err);
  }
  if (this.queue.length > 1) {
    this.emit('full', this.queue.length);
  }
  this.next();
  return this;
};
```
这里的实现细节类似于前文的smooth()。push()方法依然是通过函数变换的方式实现，假设第一个参数是方法，最后一个参数是回调函数，其余为其他参数，其核心实现如下：

```js
/**
* 推入方法，参数。最后一个参数为回调函数 * @param {Function} method 异步方法
* @param {Mix} args 参数列表，最后一个参数为回调函数 
*/
Bagpipe.prototype.push = function(method) {
  var args = [].slice.call(arguments, 1);
  var callback = args[args.length - 1];
  if (typeof callback !== 'function') {
    args.push(function() {});
  }
  if (this.options.disabled || this.limit < 1) {
    method.apply(null, args);
    return this;
  }
  // 队列长度也超过限制值时
  if (this.queue.length < this.queueLength || !this.options.refuse) {
    this.queue.push({
      method: method,
      args: args
    });
  } else {
    var err = new Error('Too much async call in queue');
    err.name = 'TooMuchAsyncCallError';
    callback(err);
  }
  if (this.queue.length > 1) {
    this.emit('full', this.queue.length);
  }
  this.next();
  return this;
};
```
将调用推入队列后，调用一次next()方法尝试触发。next()方法的定义如下：

```js
/*! 
* 继续执行队列中的后续动作
*/
Bagpipe.prototype.next = function() {
  var that = this;
  if (that.active < that.limit && that.queue.length) {
    var req = that.queue.shift();
    that.run(req.method, req.args);
  }
};
```
next()方法主要判断活跃调用的数量，如果正常，将调用内部方法run()来执行真正的调用。这里为了判断回调函数是否执行，采用了一个注入代码的技巧，具体代码如下：

```js
/*!
* 执行队列中的方法 
*/
Bagpipe.prototype.run = function(method, args) {
  var that = this;
  that.active++;
  var callback = args[args.length - 1];
  var timer = null;
  var called = false;
  // inject logic
  args[args.length - 1] = function(err) { // anyway, clear the timer
    if (timer) {
      clearTimeout(timer);
      timer = null;
    }
    // if timeout, don't execute if (!called) {
    that._next();
    callback.apply(null, arguments);
  } else { // pass the outdated error
    if (err) {
      that.emit('outdated', err);
    }
  }
};
var timeout = that.options.timeout;
if (timeout) {
  timer = setTimeout(function() { // set called as true
    called = true;
    that._next();
    // pass the exception var err = new Error(timeout + 'ms timeout');
    err.name = 'BagpipeTimeoutError';
    err.data = {
      name: method.name,
      method: method.toString(),
      args: args.slice(0, -1)
    };
    callback(err);
  },
  timeout);
}
method.apply(null, args);
};
```
用户传入的回调函数被真正执行前，被封装替换过。这个封装的回调函数内部的逻辑将活跃值的计数器减1后，主动调用next()执行后续等待的异步调用。

bagpipe类似于打开了一道窗口，允许异步调用并行进行，但是严格限定上限。仅仅在调用push()时分开传递，并不对原有API有任何侵入。

拒绝模式

事实上，bagpipe还有一些深度的使用方式。对于大量的异步调用，也需要分场景进行区分，因为涉及并发控制，必然会造成部分调用需要进行等待。如果调用有实时方面的需求，那么需要快速返回，因为等到方法被真正执行时，可能已经超过了等待时间，即使返回了数据，也没有意义了。这种场景下需要快速失败，让调用方尽早返回，而不用浪费不必要的等待时间。bagpipe为此支持了拒绝模式。

拒绝模式的使用只要设置下参数即可，相关代码如下：

```js
// 设定最大并发数为10
var bagpipe = new Bagpipe(10, { 
  refuse: true
});
```
在拒绝模式下，如果等待的调用队列也满了之后，新来的调用就直接返给它一个队列太忙的拒绝异常。

超时控制

造成队列拥塞的主要原因是异步调用耗时太久，调用产生的速度远远高于执行的速度。为了防止某些异步调用使用了太多的时间，我们需要设置一个时间基线，将那些执行时间太久的异步调用清理出活跃队列，让排队中的异步调用尽快执行。否则在拒绝模式下，会有太多的调用因为某个执行得慢，导致得到拒绝异常。相对而言，这种场景下得到拒绝异常显得比较无辜。为了公平地对待在实时需求场景下的每个调用，必须要控制每个调用的执行时间，将那些害群之马踢出队伍。

为此，bagpipe也提供了超时控制。超时控制是为异步调用设置一个时间阈值，如果异步调用没有在规定时间内完成，我们先执行用户传入的回调函数，让用户得到一个超时异常，以尽早返回。然后让下一个等待队列中的调用执行。

超时的设置如下：

```js
// 设定最大并发数为10
var bagpipe = new Bagpipe(10, { 
  timeout: 3000
});
```
小结

异步调用的并发限制在不同场景下的需求不同：非实时场景下，让超出限制的并发暂时等待执行已经可以满足需求；但在实时场景下，需要更细粒度、更合理的控制。


#### async的解决方案

无独有偶，async也提供了一个方法用于处理异步调用的限制：parallelLimit()。如下是async的示例代码：

```js
async.parallelLimit([function(callback) {
  fs.readFile('file1.txt', 'utf-8', callback);
},
function(callback) {
  fs.readFile('file2.txt', 'utf-8', callback);
}], 1, function(err, results) {
  // TODO
}); 
```
parallelLimit()与parallel()类似，但多了一个用于限制并发数量的参数，使得任务只能同时并发一定数量，而不是无限制并发。

parallelLimit()方法的缺陷在于无法动态地增加并行任务。为此，async提供了queue()方法来满足该需求，这对于遍历文件目录等操作十分有效。以下是queue()的示例代码：

```js
var q = async.queue(function(file, callback) {
  fs.readFile(file, 'utf-8', callback);
},
2);
q.drain = function() {
  // 完成了队列中的所有任务
};
fs.readdirSync('.').forEach(function(file) {
  q.push(file,
  function(err, data) {
    // TODO 
  });
});
```
尽管queue()实现了动态添加并行任务，但是相比parallelLimit()，由于queue()接收的参数是固定的，它丢失了parallelLimit()的多样性，我私心地认为bagpipe更灵活，可以添加任意类型的异步任务，也可以动态添加异步任务，同时还能够在实时处理场景中加入拒绝模式和超时控制。在实际应用中，开发者可以根据场景进行取舍。

### 总结

在接触Node的过程中，很多人粗略地接触了几个回调函数之后就放弃了。尽管异步编程略微艰难，但是并非一无是处，一旦习惯，就显得自然。从社区和过往的经验而言，JavaScript异步编程的难题已经基本解决，无论是通过事件，还是通过Promise/Deferred模式，或者流程控制库。相信在掌握以上技巧之后，异步编程不是难事，习惯异步编程之后，将会收获许多值得享受的编程体验。
本章主要介绍了主流的几种异步编程解决方案，这是目前JavaScript中主要使用的方案。但对于其他语言而言，还有协程（coroutine）等方式。但是由于Node基于V8的原因，在目前EMCAScript5的实现下还不支持协程。这些标准和规范还在制定中，所以暂时不作介绍。未来的V8如果支持Generator，也将在Node中能直接使用。最后，因为人们总是习惯性地以线性的方式进行思考，以致异步编程相对较为难以掌握。这个世界以异步运行的本质是不会因为大家线性思维的惯性而改变。就像日出月落不会因为你的心情而改变其自有的运行轨迹。