title: 《深入浅出Node.js》-读书笔记-简介
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
date: 2018-10-05 10:18:00
---
## 前言

1995年，JavaScript随网景公司发布的Netscape Navigator 2.0发布，它最早命名为LiveScript，随后更名为JavaScript。

早在1994年，网景公司就公布了其Netscape Enterprise Server中的一种服务器端脚本实现，它的名字叫LiveWire，是最早的服务器端JavaScript，甚至早于浏览器中的JavaScript公布。

JavaScript一早就能运行在前后端，但风云变幻，在前后端各自的待遇却不尽相同。

CommonJS规范的提出，不断在完善JavaScript。ECMAScript标准的不断推进，令语言更加精炼简洁，不停地去芜存菁。

Node不仅满足JavaScript同时运行在前后端，而且性能还十分高效。

<!-- more -->

## Node 简介

### Node的诞生历程

Node的诞生历程如下所示。

2009年3月，Ryan Dahl在其博客上宣布准备基于V8创建一个轻量级的Web服务器并提供一套库。2009年5月，Ryan Dahl在GitHub上发布了最初的版本。

2009年12月和2010年4月，两届JSConf大会都安排了Node的讲座。2010年年底，Node获得硅谷云计算服务商Joyent公司的资助，其创始人Ryan Dahl加入Joyent公司全职负责Node的发展。

2011年7月，Node在微软的支持下发布了其Windows版本。2011年11月，Node超越Ruby on Rails，成为GitHub上关注度最高的项目（随后被Bootstrap项目超越，目前仍居第二）。

2012年1月底，Ryan Dahl在对Node架构设计满意的情况下，将掌门人的身份转交给Isaac Z. Schlueter，自己转向一些研究项目。Isaac Z. Schlueter是Node的包管理器NPM的作者，之后Node的版本发布和bug修复等工作由他接手。截至笔者执笔之日（2013年7月13日），发布的Node稳定版为v0.10.13，非稳定版为v0.11.4，NPM的官方模块数达到34 943个，模块的周下载量为1479万次。

随后，Node的发布计划主要集中在性能提升上，在v0.14之后，正式发布出v1.0版本。

#### 为什么是JavaScript

设计高性能，Web服务器的几个要点：事件驱动、非阻塞I/O。

相比之下，JavaScript比C的开发门槛要低，比Lua的历史包袱要少。尽管服务器端JavaScript存在已经很多年了，但是后端部分一直没有市场，可以说历史包袱为零，为其导入非阻塞I/O库没有额外阻力。另外，JavaScript在浏览器中有广泛的事件驱动方面的应用，暗合Ryan Dahl喜好基于事件驱动的需求。当时，第二次浏览器大战也渐渐分出高下，Chrome浏览器的JavaScript引擎V8摘得性能第一的桂冠，而且其基于新BSD许可证发布，自然受到Ryan Dahl的欢迎。考虑到 **高性能** 、**符合事件驱动** 、**没有历史包袱** 这3个主要原因，JavaScript成为了Node的实现语言。

#### 为什么叫Node

Node发展为一个强制不共享任何资源的单线程、单进程系统，包含十分适宜网络的库，为构建大型分布式应用程序提供基础设施，其目标也是成为一个构建快速、可伸缩的网络应用平台。

通过通信协议来组织许多Node，非常容易通过扩展来达成构建大型网络应用的目的。

每一个Node进程都构成这个网络应用中的一个节点，这是它名字所含意义的真谛。

#### Node给JavaScript带来的意义


Chrome浏览器和Node的组件构成如下图所示。

浏览器中除了V8作为JavaScript引擎外，还有一个WebKit布局引擎。

avaScript作为一门图灵完备的语言，长久以来却限制在浏览器的沙箱中运行，它的能力取决于浏览器中间层提供的支持有多少。

![Chrome浏览器和Node的组件构成](https://graphbed.qiniu.songxingguo.com/Node-brief/Chrome%E6%B5%8F%E8%A7%88%E5%99%A8%E5%92%8CNode%E7%9A%84%E7%BB%84%E4%BB%B6%E6%9E%84%E6%88%90.jpg)

除了HTML、WebKit和显卡这些UI相关技术没有支持外，Node的结构与Chrome十分相似。

它们都是基于事件驱动的异步架构，浏览器通过事件驱动来服务界面上的交互，Node通过事件驱动来服务I/O。

在Node中，JavaScript可以随心所欲地访问本地文件，可以搭建WebSocket服务器端，可以连接数据库，可以如Web Workers一样玩转多进程。

JavaScript可以运行在不同的地方，不再继续限制在浏览器中与CSS样式表、DOM树打交道。

如果HTTP协议栈是水平面，Node就是浏览器在协议栈另一边的倒影。

**Node打破了过去JavaScript只能在浏览器中运行的局面。**

如同上文提及的关于浏览器的优势和限制，在node-webkit项目中，它将Node中的事件循环和WebKit的事件循环融合在一起，既可以通过它享受HTML、CSS带来的UI构建，也能通过它访问本地资源，将两者的优势整合到一起。

桌面应用程序的开发可以完全通过HTML、CSS、JavaScript完成。

### Node的特点

作为后端JavaScript的运行平台，Node保留了前端浏览器JavaScript中那些熟悉的接口，没有改写语言本身的任何特性，依旧基于作用域和原型链，区别在于它将前端中广泛运用的思想迁移到了服务器端。

#### 异步I/O

关于异步I/O，向前端工程师解释起来或许会容易一些，因为发起Ajax调用对于前端工程师而言是再熟悉不过的场景了。

下面的代码用于发起一个Ajax请求：

```js
$.post('/url', {title: '深入浅出Node.js'}, function (data) { 
	console.log('收到响应'); 
}); 
console.log('发送Ajax结束');
```
熟悉异步的用户必然知道，“收到响应”是在“发送Ajax结束”之后输出的。

在调用$.post()后，后续代码是被立即执行的，而“收到响应”的执行时间是不被预期的。

**异步调用中对于结果值的捕获是符合“Don't call me, I will call you”的原则的，这也是注重结果，不关心过程的一种表现。**

下图是一个经典的Ajax调用。

![经典的Ajax调用](https://graphbed.qiniu.songxingguo.com/Node-brief/%E7%BB%8F%E5%85%B8%E7%9A%84Ajax%E8%B0%83%E7%94%A8.jpg)

在Node中，异步I/O也很常见。

以读取文件为例，我们可以看到它与前端Ajax调用的方式是极其类似的：

```js
var fs = require('fs'); 
fs.readFile('/path', function (err, file) { 
	console.log('读取文件完成') 
}); 
console.log('发起读取文件');
```
这里的“发起读取文件”是在“读取文件完成”之前输出的。同样，“读取文件完成”的执行也取决于读取文件的异步调用何时结束。下图是一个经典的异步调用。

![经典的异步调用](https://graphbed.qiniu.songxingguo.com/Node-brief/%E7%BB%8F%E5%85%B8%E7%9A%84%E5%BC%82%E6%AD%A5%E8%B0%83%E7%94%A8.jpg)

在Node中，绝大多数的操作都以异步的方式进行调用。

在Node中，我们可以从语言层面很自然地进行并行I/O操作。每个调用之间无须等待之前的I/O调用结束。在编程模型上可以极大提升效率。

下面的两个文件读取任务的耗时取决于最慢的那个文件读取的耗时：

```js
fs.readFile('/path1', function (err, file) { 
  console.log('读取文件1完成'); 
}); 
fs.readFile('/path2', function (err, file) { 
  console.log('读取文件2完成'); 
});
```
而对于同步I/O而言，它们的耗时是两个任务的耗时之和。这里异步带来的优势是显而易见的。

### 事件与回调函数

随着Web 2.0时代的到来，JavaScript在前端担任了更多的职责，事件也得到了广泛的应用。

Node不像Rhino那样受Java的影响很大，而是将前端浏览器中应用广泛且成熟的事件引入后端，配合异步I/O，将事件点暴露给业务逻辑。

下面的例子展示的是Ajax异步提交的服务器端处理过程。Node创建一个Web服务器，并侦听8080端口。对于服务器，我们为其绑定了request事件，对于请求对象，我们为其绑定了data事件和end事件：

```js
var http = require('http'); 
var querystring = require('querystring'); 

// 侦听服务器的request事件 
http.createServer(function (req, res) { 
  var postData = ''; 
  req.setEncoding('utf8'); 
  // 侦听请求的data事件 
  req.on('data', function (trunk) { 
    postData += trunk; 
  }); 
  // 侦听请求的end事件 
  req.on('end', function () { 
    res.end(postData);
  }); 
}).listen(8080); console.log('服务器启动完成');
```
相应地，我们在前端为Ajax请求绑定了success事件，在发出请求后，只需关心请求成功时执行相应的业务逻辑即可，相关代码如下：

```js
$.ajax({ 
  'url': '/url', 'method': 'POST', 
  'data': {}, 
  'success': function (data) { 
    // success事件 
  } 
});
```
可见，无论在前端还是后端，事件都是常用的。

基于事件的编程方式具有轻量级、松耦合、只关注事务点等优势，但是在多个异步任务的场景下，事件与事件之间各自独立，如何协作是一个问题。

从前面可以看到，回调函数无处不在。这是因为在JavaScript中，我们将函数作为第一等公民来对待，可以将函数作为对象传递给方法作为实参进行调用。

回调函数也是最好的接受异步调用返回数据的方式。但是这种编程方式对于很多习惯同步思路编程的人来说，也许是十分不习惯的。代码的编写顺序与执行顺序并无关系，这对他们可能造成阅读上的障碍。在流程控制方面，因为穿插了异步方法和回调函数，与常规的同步方式相比，变得不那么一目了然了。

#### 单线程

Node保持了JavaScript在浏览器中单线程的特点。而且在Node中，JavaScript与其余线程是无法共享任何状态的。单线程的最大好处是不用像多线程编程那样处处在意状态的同步问题，这里没有死锁的存在，也没有线程上下文交换所带来的性能上的开销。

同样，单线程也有他自身的弱点，这些弱点是学习Node的过程中必须面对的。单线程的弱点具体有以下3方面。

- 无法利用多核CPU。
- 错误会引起整个应用退出，应用的健壮性值得考验。
- 大量计算占用CPU导致无法继续调用异步I/O。

像浏览器中JavaScript与UI共用一个线程一样，**JavaScript长时间执行会导致UI的渲染和响应被中断** 。在Node中，长时间的CPU占用也会导致后续的异步I/O发不出调用，已完成的异步I/O的回调函数也会得不到及时执行。

Web Workers能够创建工作线程来进行计算，以解决JavaScript大计算阻塞UI渲染的问题。工作线程为了不阻塞主线程，通过消息传递的方式来传递运行结果，这也使得工作线程不能访问到主线程中的UI。

Node采用了与Web Workers相同的思路来解决单线程中大计算量的问题：child_process。

**子进程的出现，意味着Node可以从容地应对单线程在健壮性和无法利用多核CPU方面的问题**。通过将计算分发到各个子进程，可以将大量计算分解掉，然后再通过进程之间的事件消息来传递结果，这可以很好地保持应用模型的简单和低依赖。

#### 跨平台

下图是Node基于libuv实现跨平台的架构示意图。

![Node基于libuv实现跨平台的架构示意图](https://graphbed.qiniu.songxingguo.com/Node-brief/Node%E5%9F%BA%E4%BA%8Elibuv%E5%AE%9E%E7%8E%B0%E8%B7%A8%E5%B9%B3%E5%8F%B0%E7%9A%84%E6%9E%B6%E6%9E%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

兼容Windows和*nix平台主要得益于Node在架构层面的改动，它在操作系统与Node上层模块系统之间构建了一层平台层架构，即libuv。目前，libuv已经成为许多系统实现跨平台的基础组件。

通过良好的架构，Node的第三方C++模块也可以借助libuv实现跨平台。目前，除了没有保持更新的C++模块外，大部分C++模块都能实现跨平台的兼容。

### Node的应用场景

#### I/O密集型

如果将所有的脚本语言拿到一处来评判，那么从单线程的角度来说，Node处理I/O的能力是值得竖起拇指称赞的。通常，说Node擅长I/O密集型的应用场景基本上是没人反对的。

Node面向网络且擅长并行I/O，能够有效地组织起更多的硬件资源，从而提供更多好的服务。

**I/O密集的优势主要在于Node利用事件循环的处理能力，而不是启动每一个线程为每一个请求服务，资源占用极少。**

#### 是否不擅长CPU密集型业务

我们将相同的斐波那契数列计算（F0=0，F1=1，Fn=F(n-1)+F(n-2)(n≥2)）分别用各种脚本语言写了算法实现，并进行了n=40的计算，以比较性能。这个测试主要偏重CPU栈操作，下表是其中一次运算耗时的排行。在这些脚本语言中（其中C和Go语言是静态语言，用于参考），Node是足够高效的，它优秀的运算能力主要来自V8的深度性能优化。

![计算斐波那契数列耗时排行](https://graphbed.qiniu.songxingguo.com/Node-brief/%E8%AE%A1%E7%AE%97%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97%E8%80%97%E6%97%B6%E6%8E%92%E8%A1%8C.jpg)

这样的测试结果尽管不能完全反映出各个语言的性能优劣，但已经可以表明Node在性能上不俗的表现。从另一个角度来说，这可以表明CPU密集型应用其实并不可怕。CPU密集型应用给Node带来的挑战主要是：**由于JavaScript单线程的原因，如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起** 。但是 **适当调整和分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起，这样既可同时享受到并行异步I/O的好处，又能充分利用CPU** 。

关于CPU密集型应用，Node的异步I/O已经解决了在单线程上CPU与I/O之间阻塞无法重叠利用的问题，I/O阻塞造成的性能浪费远比CPU的影响小。

Node虽然没有提供多线程用于计算支持，但是还是有以下两个方式来充分利用CPU。

- Node可以通过编写C/C++扩展的方式更高效地利用CPU，将一些V8不能做到性能极致的地方通过C/C++来实现。由上面的测试结果可以看到，通过C/C++扩展的方式实现斐波那契数列计算，速度比Java还快。

- 如果单线程的Node不能满足需求，甚至用了C/C++扩展后还觉得不够，那么通过子进程的方式，将一部分Node进程当做常驻服务进程用于计算，然后利用进程间的消息来传递结果，将计算与I/O分离，这样还能充分利用多CPU。

CPU密集不可怕，如何合理调度是诀窍。

#### 与遗留系统和平共处

在Web端，过去大多都是同步的方式编写的程序，这种串行调用下层应用数据的过程中充斥着串行的等待时间，如果采用多线程来解决这种串行等待，又或多或少地显得小题大作。在Node中，语言层面即可天然并行的特性在这种场景中显得十分有效。对于已有的稳定系统，并非意味着我们要抛弃掉。

雪球财经是从旧有的Java项目中分离出一个子项目，在这个子项目中，没有继续采用Java/JSP而是采用Node来完成Web端的开发，使得前端工程师在HTTP协议栈的两端能够高效灵活地开发，避免了Java烦琐的表达；另一方面，又利用Java作为后端接口和中间件，使其具有良好的稳定性。两者互相结合，取长补短。

#### 分布式应用

分布式应用意味着对可伸缩性的要求非常高。数据平台通常要在一个数据库集群中去寻找需要的数据。阿里巴巴开发了中间层应用NodeFox、ITier，将数据库集群做了划分和映射，查询调用依旧是针对单张表进行SQL查询，中间层分解查询SQL，并行地去多台数据库中获取数据并合并。NodeFox能实现对多台MySQL数据库的查询，如同查询一台MySQL一样，而ITier更强大，查询多个数据库如同查询单个数据库一样，这里的多个数据库是指不同的数据库，如MySQL或其他的数据库。

Node高效利用并行I/O的过程，也是高效使用数据库的过程。对于Node，这个行为只是一次普通的I/O。对于数据库而言，却是一次复杂的计算，所以也是进而充分压榨硬件资源的过程。

###  Node的使用者

使用者对于Node的各自倚重点也各不相同。经过整理，主要有下面几类。

- 前后端编程语言环境统一。这类倚重点的代表是雅虎。雅虎开放了Cocktail框架，利用自己深厚的前端沉淀，将YUI3这个前端框架的能力借助Node延伸到服务器端，使得使用者摆脱了日常工作中一边写JavaScript一边写PHP所带来的上下文交换负担。

- Node带来的高性能I/O用于实时应用。Voxer将Node应用在实时语音上。国内腾讯的朋友网将Node应用在长连接中，以提供实时功能，花瓣网、蘑菇街等公司通过socket.io实现实时通知的功能。

- 并行I/O使得使用者可以更高效地利用分布式环境。阿里巴巴和eBay是这方面的典型。阿里巴巴的NodeFox和eBay的ql.io都是借用Node并行I/O的能力，更高效地使用已有的数据。并行I/O，有效利用稳定接口提升Web渲染能力。雪球财经和LinkedIn的移动版网站均是这种案例，撇弃同步等待式的顺序请求，大胆采用并行I/O，加速数据的获取进而提升Web的渲染速度。

- 云计算平台提供Node支持。微软将Node引入Azure的开发中，阿里云、百度均纷纷在云服务器上提供Node应用托管服务，Joyent更是云计算中提供Node支持的代表。这类平台看重JavaScript带来的开发上的优势，以及低资源占用、高性能的特点。

- 游戏开发领域。游戏领域对实时和并发有很高的要求，网易开源了pomelo实时框架，可以应用在游戏和高实时应用中。

- 工具类应用。过去依赖Java或其他语言构建的前端工具类应用，纷纷被一些前端工程师用Node重写，用前端熟悉的语言为前端构建熟悉的工具。