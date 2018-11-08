title: 《深入浅出Node.js》-读书笔记-异步I/O
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
date: 2018-10-06 00:07:00
---
## 异步I/O

“异步”这个名词其实很早就诞生了，但它的大规模流行却是在Web 2.0浪潮中，它伴随着AJAX的第一个A（Asynchronous）席卷了Web。

前端编程算GUI编程的一种，其中充斥了各种Ajax和事件，这些都是典型的异步应用场景。

在众多高级编程语言或运行平台中，将异步作为主要编程方式和设计理念的，Node是首个。

Ryan Dahl最初期望设计出一个高性能的Web服务器，后来则演变为一个可以基于它构建各种高速、可伸缩网络应用的平台。

与Node的事件驱动、异步I/O设计理念比较相近的一个知名产品为Nginx。Nginx采用纯C编写，性能表现非常优异。

它们的区别在于，Nginx具备面向客户端管理连接的强大能力，但是它的背后依然受限于各种同步方式的编程语言。但Node却是全方位的，既可以作为服务器端去处理客户端带来的大量并发请求，也能作为客户端向网络中的各个应用进行并发请求。

Web的含义是网，Node的表现就如它的名字一样，是网络中灵活的一个节点。

<!-- more -->

### 为什么要异步I/O

#### 用户体验

异步的概念之所以首先在Web 2.0中火起来，是因为在浏览器中JavaScript在单线程上执行，而且它还与UI渲染共用一个线程。

《高性能JavaScript》一书中曾经总结过，如果脚本的执行时间超过100毫秒，用户就会感到页面卡顿，以为网页停止响应。

如果网页临时需要获取一个网络资源，通过同步的方式获取，那么JavaScript则需要等待资源完全从服务器端获取后才能继续执行，这期间UI将停顿，不响应用户的交互行为。

而采用异步请求，在下载资源期间，JavaScript和UI的执行都不会处于等待状态，可以继续响应用户的交互行为，给用户一个鲜活的页面。

同理，前端通过异步可以消除掉UI阻塞的现象，但是前端获取资源的速度也取决于后端的响应速度。

假如一个资源来自于两个不同位置的数据的返回，第一个资源需要M毫秒的耗时，第二个资源需要N毫秒的耗时。如果采用同步的方式，代码大致如下：

```js
// 消费时间为M
getData('from_db');
// 消费时间为N
getData('from_remote_api');
```
但是如果采用异步方式，第一个资源的获取并不会阻塞第二个资源，也即第二个资源的请求并不依赖第一个资源的结束。如此，我们可以享受到并发的优势，相关代码如下：

```js
getData('from_db', function (result) {
  // 消费时间为M
});
getData('from_remote_api', function (result) { 
  // 消费时间为N
});
```
对比两者的时间总消耗，前者为M+N，后者为max（M, N）。

随着应用复杂性的增加，情景将会变成M+N+…和max（M, N, …），同步与异步的优劣将会凸显出来。另一方面，随着网站或应用不断膨胀，数据将会分布到多台服务器上，分布式将会是常态。分布也意味着M与N的值会线性增长，这也会放大异步和同步在性能方面的差异。为了让读者感知到M和N值具体多昂贵，表3-1列出了从CPU一级缓存到网络的数据访问所需要的开销。

![不同的I/O类型及其对应的开销I/O类型](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E4%B8%8D%E5%90%8C%E7%9A%84IO%E7%B1%BB%E5%9E%8B%E5%8F%8A%E5%85%B6%E5%AF%B9%E5%BA%94%E5%BC%80%E9%94%80.png)

这就是异步I/O在Node中如此盛行，甚至将其作为主要理念进行设计的原因。

#### 资源分配

我们知道计算机在发展过程中将组件进行了抽象，分为I/O设备和计算设备。

假设业务场景中有一组互不相关的任务需要完成，现行的主流方法有以下两种。

- 单线程串行依次执行。
- 多线程并行完成。

如果创建多线程的开销小于并行执行，那么多线程的方式是首选的。多线程的代价在于创建线程和执行期线程上下文切换的开销较大。另外，在复杂的业务中，多线程编程经常面临锁、状态同步等问题，这是多线程被诟病的主要原因。但是多线程在多核CPU上能够有效提升CPU的利用率，这个优势是毋庸置疑的。

单线程顺序执行任务的方式比较符合编程人员按顺序思考的思维方式。它依然是最主流的编程方式，因为它易于表达。但是串行执行的缺点在于性能，任意一个略慢的任务都会导致后续执行代码被阻塞。在计算机资源中，通常I/O与CPU计算之间是可以并行进行的。但是同步的编程模型导致的问题是，I/O的进行会让后续任务等待，这造成资源不能被更好地利用。

操作系统会将CPU的时间片分配给其余进程，以公平而有效地利用资源，基于这一点，有的服务器为了提升响应能力，会通过启动多个工作进程来为更多的用户服务。但是对于这一组任务而言，它无法分发任务到多个进程上，所以依然无法高效利用资源，结束所有任务所需的时间将会较长。这种模式类似于加三倍服务器，达到占用更多资源来提升服务速度，它并没能真正改善问题。

**添加硬件资源是一种提升服务质量的方式，但它不是唯一的方式。**

单线程同步编程模型会因阻塞I/O导致硬件资源得不到更优的使用。多线程编程模型也因为编程中的死锁、状态同步等问题让开发人员头疼。


Node在两者之间给出了它的方案：**利用单线程，远离多线程死锁、状态同步等问题** ；**利用异步I/O，让单线程远离阻塞，以更好地使用CPU** 。

异步I/O可以算作Node的特色，因为它是首个大规模将异步I/O应用在应用层上的平台，它力求在单线程上将资源分配得更高效。为了弥补单线程无法利用多核CPU的缺点，Node提供了类似前端浏览器中Web Workers的子进程，该子进程可以通过工作进程高效地利用CPU和I/O。

异步I/O的提出是期望I/O的调用不再阻塞后续运算，将原有等待I/O完成的这段时间分配给其余需要的业务去执行。

下图为异步I/O的调用示意图。

![异步I/O的调用示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E5%BC%82%E6%AD%A5IO%E7%9A%84%E8%B0%83%E7%94%A8%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

### 异步I/O实现现状

异步I/O在Node中应用最为广泛，但是它并非Node的原创。

它的优秀之处并非原创，它的原创之处并不优秀”，以之评价他自己创造的JavaScript一样，Node的优秀之处也并非原创。

#### 异步I/O与非阻塞I/O

从实际效果而言，异步和非阻塞都达到了我们并行I/O的目的。但是从计算机内核I/O而言，异步/同步和阻塞/非阻塞实际上是两回事。

**操作系统内核对于I/O只有两种方式：阻塞与非阻塞。** 在调用阻塞I/O时，应用程序需要等待I/O完成才返回结果，如下图所示。

![调用阻塞I/O的过程](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E8%B0%83%E7%94%A8%E9%98%BB%E5%A1%9EIO%E7%9A%84%E8%BF%87%E7%A8%8B.jpg)

阻塞I/O的一个特点是调用之后一定要等到系统内核层面完成所有操作后，调用才结束。以读取磁盘上的一段文件为例，系统内核在完成磁盘寻道、读取数据、复制数据到内存中之后，这个调用才结束。


阻塞I/O造成CPU等待I/O，浪费等待时间，CPU的处理能力不能得到充分利用。为了提高性能，内核提供了非阻塞I/O。非阻塞I/O跟阻塞I/O的差别为调用之后会立即返回，如下图所示。

![调用非阻塞I/O的过程](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E8%B0%83%E7%94%A8%E9%9D%9E%E9%98%BB%E5%A1%9EIO%E7%9A%84%E8%BF%87%E7%A8%8B.jpg)

操作系统对计算机进行了抽象，将所有输入输出设备抽象为文件。内核在进行文件I/O操作时，通过文件描述符进行管理，而文件描述符类似于应用程序与系统内核之间的凭证。应用程序如果需要进行I/O调用，需要先打开文件描述符，然后再根据文件描述符去实现文件的数据读写。此处非阻塞I/O与阻塞I/O的区别在于阻塞I/O完成整个获取数据的过程，而非阻塞I/O则不带数据直接返回，要获取数据，还需要通过文件描述符再次读取。

非阻塞I/O返回之后，CPU的时间片可以用来处理其他事务，此时的性能提升是明显的。
但非阻塞I/O也存在一些问题。由于完整的I/O并没有完成，立即返回的并不是业务层期望的数据，而仅仅是当前调用的状态。为了获取完整的数据，应用程序需要重复调用I/O操作来确认是否完成。这种重复调用判断操作是否完成的技术叫做轮询，下面我们就来简要介绍这种技术。

任意技术都并非完美的。阻塞I/O造成CPU等待浪费，非阻塞带来的麻烦却是需要轮询去确认是否完全完成数据获取，它会让CPU处理状态判断，是对CPU资源的浪费。这里我们且看轮询技术是如何演进的，以减小I/O状态判断的CPU损耗。

现存的轮询技术主要有以下这些。

- read。它是最原始、性能最低的一种，通过重复调用来检查I/O的状态来完成完整数据的读取。在得到最终数据前，CPU一直耗用在等待上。下图为通过read进行轮询的示意图。

  ![通过read进行轮询的示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E9%80%9A%E8%BF%87read%E8%BF%9B%E8%A1%8C%E8%BD%AE%E8%AF%A2%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

- select。它是在read的基础上改进的一种方案，通过对文件描述符上的事件状态来进行判断。下图为通过select进行轮询的示意图。

  ![通过select进行轮询的示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E9%80%9A%E8%BF%87select%E8%BF%9B%E8%A1%8C%E8%BD%AE%E8%AF%A2%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

  select轮询具有一个较弱的限制，那就是由于它采用一个1024长度的数组来存储状态，所以它最多可以同时检查1024个文件描述符。

- poll。该方案较select有所改进，采用链表的方式避免数组长度的限制，其次它能避免不需要的检查。但是当文件描述符较多的时候，它的性能还是十分低下的。下图为通过poll实现轮询的示意图，它与select相似，但性能限制有所改善。

  ![通过poll实现轮询的示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E9%80%9A%E8%BF%87poll%E5%AE%9E%E7%8E%B0%E8%BD%AE%E8%AF%A2%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

- epoll。该方案是Linux下效率最高的I/O事件通知机制，在进入轮询的时候如果没有检查到I/O事件，将会进行休眠，直到事件发生将它唤醒。它是真实利用了事件通知、执行回调的方式，而不是遍历查询，所以不会浪费CPU，执行效率较高。下图为通过epoll方式实现轮询的示意图。

  ![通过epoll方式实现轮询的示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E9%80%9A%E8%BF%87epoll%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0%E8%BD%AE%E8%AF%A2%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

- kqueue。该方案的实现方式与epoll类似，不过它仅在FreeBSD系统下存在。

轮询技术满足了非阻塞I/O确保获取完整数据的需求，但是对于应用程序而言，它仍然只能算是一种同步，因为应用程序仍然需要等待I/O完全返回，依旧花费了很多时间来等待。等待期间，CPU要么用于遍历文件描述符的状态，要么用于休眠等待事件发生。结论是它不够好。

#### 理想的非阻塞异步I/O

尽管epoll已经利用了事件来降低CPU的耗用，但是休眠期间CPU几乎是闲置的，对于当前线程而言利用率不够。

我们期望的完美的异步I/O应该是应用程序发起非阻塞调用，无须通过遍历或者事件唤醒等方式轮询，可以直接处理下一个任务，只需在I/O完成后通过信号或回调将数据传递给应用程序即可。下图为理想中的异步I/O示意图。

![理想中的异步I/O示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E7%90%86%E6%83%B3%E4%B8%AD%E7%9A%84%E5%BC%82%E6%AD%A5IO%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

幸运的是，在Linux下存在这样一种方式，它原生提供的一种异步I/O方式（AIO）就是通过信号或回调来传递数据的。

但不幸的是，只有Linux下有，而且它还有缺陷——AIO仅支持内核I/O中的O_DIRECT方式读取，导致无法利用系统缓存。

#### 现实的异步I/O

现实比理想要骨感一些，但是要达成异步I/O的目标，并非难事。前面我们将场景限定在了单线程的状况下，多线程的方式会是另一番风景。通过让部分线程进行阻塞I/O或者非阻塞I/O加轮询技术来完成数据获取，让一个线程进行计算处理，通过线程之间的通信将I/O得到的数据进行传递，这就轻松实现了异步I/O（尽管它是模拟的），示意图如下图所示。

![异步I/O](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E5%BC%82%E6%AD%A5IO.jpg)

glibc的AIO便是典型的线程池模拟异步I/O。然而遗憾的是，它存在一些难以忍受的缺陷和bug，不推荐采用。libev的作者Marc Alexander Lehmann重新实现了一个异步I/O的库：libeio。libeio实质上依然是采用线程池与阻塞I/O模拟异步I/O。最初，Node在*nix平台下采用了libeio配合libev实现I/O部分，实现了异步I/O。在Node v0.9.3中，自行实现了线程池来完成异步I/O。

另一种我迟迟没有透露的异步I/O方案则是Windows下的IOCP，它在某种程度上提供了理想的异步I/O：调用异步方法，等待I/O完成之后的通知，执行回调，用户无须考虑轮询。但是它的内部其实仍然是线程池原理，不同之处在于这些线程池由系统内核接手管理。

IOCP的异步I/O模型与Node的异步调用模型十分近似。在Windows平台下采用了IOCP实现异步I/O。

由于Windows平台和*nix平台的差异，Node提供了libuv作为抽象封装层，使得所有平台兼容性的判断都由这一层来完成，并保证上层的Node与下层的自定义线程池及IOCP之间各自独立。Node在编译期间会判断平台条件，选择性编译unix目录或是win目录下的源文件到目标程序中，其架构如下图所示。

![基于libuv的框架示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E5%9F%BA%E4%BA%8Elibuv%E7%9A%84%E6%A1%86%E6%9E%B6%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

需要强调一点的是，这里的I/O不仅仅只限于磁盘文件的读写。*nix将计算机抽象了一番，磁盘文件、硬件、套接字等几乎所有计算机资源都被抽象为了文件，因此这里描述的阻塞和非阻塞的情况同样能适合于套接字等。

另一个需要强调的地方在于我们时常提到Node是单线程的，这里的单线程仅仅只是JavaScript执行在单线程中罢了。在Node中，无论是*nix还是Windows平台，内部完成I/O任务的另有线程池。

#### 事件循环

首先，我们着重强调一下Node自身的执行模型——事件循环，正是它使得回调函数十分普遍。

在进程启动时，Node便会创建一个类似于while(true)的循环，每执行一次循环体的过程我们称为Tick。每个Tick的过程就是查看是否有事件待处理，如果有，就取出事件及其相关的回调函数。如果存在关联的回调函数，就执行它们。然后进入下个循环，如果不再有事件处理，就退出进程。流程图如下图所示。

![Tick流程图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/Tick%E6%B5%81%E7%A8%8B%E5%9B%BE.jpg)

#### 观察者

在每个Tick的过程中，如何判断是否有事件需要处理呢？这里必须要引入的概念是观察者。每个事件循环中有一个或者多个观察者，而判断是否有事件要处理的过程就是向这些观察者询问是否有要处理的事件。

这个过程就如同饭馆的厨房，厨房一轮一轮地制作菜肴，但是要具体制作哪些菜肴取决于收银台收到的客人的下单。厨房每做完一轮菜肴，就去问收银台的小妹，接下来有没有要做的菜，如果没有的话，就下班打烊了。在这个过程中，收银台的小妹就是观察者，她收到的客人点单就是关联的回调函数。当然，如果饭馆经营有方，它可能有多个收银员，就如同事件循环中有多个观察者一样。收到下单就是一个事件，一个观察者里可能有多个事件。

浏览器采用了类似的机制。事件可能来自用户的点击或者加载某些文件时产生，而这些产生的事件都有对应的观察者。在Node中，事件主要来源于网络请求、文件I/O等，这些事件对应的观察者有文件I/O观察者、网络I/O观察者等。观察者将事件进行了分类。

事件循环是一个典型的生产者/消费者模型。异步I/O、网络请求等则是事件的生产者，源源不断为Node提供不同类型的事件，这些事件被传递到对应的观察者那里，事件循环则从观察者那里取出事件并处理。

在Windows下，这个循环基于IOCP创建，而在*nix下则基于多线程创建。

#### 请求对象

对于一般的（非异步）回调函数，函数由我们自行调用，如下所示：

```js
var forEach = function (list, callback) { 
  for (var i = 0; i < list.length; i++) {
    callback(list[i], i, list); 
  }
};
```
对于Node中的异步I/O调用而言，回调函数却不由开发者来调用。那么从我们发出调用后，到回调函数被执行，中间发生了什么呢？事实上，从JavaScript发起调用到内核执行完I/O操作的过渡过程中，存在一种中间产物，它叫做请求对象。

下面我们以最简单的fs.open()方法来作为例子，探索Node与底层之间是如何执行异步I/O调用以及回调函数究竟是如何被调用执行的：

```js
fs.open = function(path, flags, mode, callback) { 
  // ...
  binding.open(pathModule._makeLong(path), stringToFlags(flags),
  mode, callback);
};
```
fs.open()的作用是根据指定路径和参数去打开一个文件，从而得到一个文件描述符，这是后续所有I/O操作的初始操作。从前面的代码中可以看到，JavaScript层面的代码通过调用C++核心模块进行下层的操作。下图为调用示意图。

![调用示意图](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E8%B0%83%E7%94%A8%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

从JavaScript调用Node的核心模块，核心模块调用C++内建模块，内建模块通过libuv进行系统调用，这是Node里经典的调用方式。这里libuv作为封装层，有两个平台的实现，实质上是调用了uv_fs_open()方法。在uv_fs_open()的调用过程中，我们创建了一个FSReqWrap请求对象。从JavaScript层传入的参数和当前方法都被封装在这个请求对象中，其中我们最为关注的回调函数则被设置在这个对象的oncomplete_sym属性上：

```
req_wrap->object_->Set(oncomplete_sym, callback);
```
对象包装完毕后，在Windows下，则调用QueueUserWorkItem()方法将这个FSReqWrap对象推入线程池中等待执行，该方法的代码如下所示：

```
QueueUserWorkItem(&uv_fs_thread_proc, req, WT_EXECUTEDEFAULT)
```
QueueUserWorkItem()方法接受3个参数：第一个参数是将要执行的方法的引用，这里引用的是uv_fs_thread_proc，第二个参数是uv_fs_thread_proc方法运行时所需要的参数；第三个参数是执行的标志。当线程池中有可用线程时，我们会调用uv_fs_thread_proc()方法。uv_fs_thread_ proc()方法会根据传入参数的类型调用相应的底层函数。以uv_fs_open()为例，实际上调用fs__open()方法。

至此，JavaScript调用立即返回，由JavaScript层面发起的异步调用的第一阶段就此结束。JavaScript线程可以继续执行当前任务的后续操作。当前的I/O操作在线程池中等待执行，不管它是否阻塞I/O，都不会影响到JavaScript线程的后续执行，如此就达到了异步的目的。

请求对象是异步I/O过程中的重要中间产物，所有的状态都保存在这个对象中，包括送入线程池等待执行以及I/O操作完毕后的回调处理。

#### 执行回调

组装好请求对象、送入I/O线程池等待执行，实际上完成了异步I/O的第一部分，回调通知是第二部分。
线程池中的I/O操作调用完毕之后，会将获取的结果储存在req->result属性上，然后调用PostQueuedCompletionStatus()通知IOCP，告知当前对象操作已经完成：

```
PostQueuedCompletionStatus((loop)->iocp, 0, 0, &((req)->overlapped))
```
PostQueuedCompletionStatus()方法的作用是向IOCP提交执行状态，并将线程归还线程池。通过PostQueuedCompletionStatus()方法提交的状态，可以通过GetQueuedCompletionStatus()提取。在这个过程中，我们其实还动用了事件循环的I/O观察者。在每次Tick的执行中，它会调用IOCP相关的GetQueuedCompletionStatus()方法检查线程池中是否有执行完的请求，如果存在，会将请求对象加入到I/O观察者的队列中，然后将其当做事件处理。

I/O观察者回调函数的行为就是取出请求对象的result属性作为参数，取出oncomplete_sym属性作为方法，然后调用执行，以此达到调用JavaScript中传入的回调函数的目的。

至此，整个异步I/O的流程完全结束，如下图所示。

![整个异步I/O的流程](https://graphbed.qiniu.songxingguo.com/Node-async-IO/%E6%95%B4%E4%B8%AA%E5%BC%82%E6%AD%A5IO%E7%9A%84%E6%B5%81%E7%A8%8B.jpg)

事件循环、观察者、请求对象、I/O线程池这四者共同构成了Node异步I/O模型的基本要素。

Windows下主要通过IOCP来向系统内核发送I/O调用和从内核获取已完成的I/O操作，配以事件循环，以此完成异步I/O的过程。在Linux下通过epoll实现这个过程，FreeBSD下通过kqueue实现，Solaris下通过Event ports实现。不同的是线程池在Windows下由内核（IOCP）直接提供，*nix系列下由libuv自行实现。

#### 小结

从前面实现异步I/O的过程描述中，我们可以提取出异步I/O的几个关键词：单线程、事件循环、观察者和I/O线程池。这里单线程与I/O线程池之间看起来有些悖论的样子。由于我们知道JavaScript是单线程的，所以按常识很容易理解为它不能充分利用多核CPU。事实上，在Node中，除了JavaScript是单线程外，Node自身其实是多线程的，只是I/O线程使用的CPU较少。另一个需要重视的观点则是，除了用户代码无法并行执行外，所有的I/O（磁盘I/O和网络I/O等）则是可以并行起来的。

### 非I/O的异步API

尽管我们在介绍Node的时候，多数情况下都会提到异步I/O，但是Node中其实还存在一些与I/O无关的异步API，这一部分也值得略微关注一下，它们分别是setTimeout()、setInterval()、setImmediate()和process.nextTick()。

#### 定时器

setTimeout()和setInterval()与浏览器中的API是一致的，分别用于单次和多次定时执行任务。它们的实现原理与异步I/O比较类似，只是不需要I/O线程池的参与。调用setTimeout()或者setInterval()创建的定时器会被插入到定时器观察者内部的一个红黑树中。每次Tick执行时，会从该红黑树中迭代取出定时器对象，检查是否超过定时时间，如果超过，就形成一个事件，它的回调函数将立即执行。

下图提到的主要是setTimeout()的行为。setInterval()与之相同，区别在于后者是重复性的检测和执行。

![setTimeout()的行为](https://graphbed.qiniu.songxingguo.com/Node-async-IO/setTimeout%28%29%E7%9A%84%E8%A1%8C%E4%B8%BA.jpg)

定时器的问题在于，它并非精确的（在容忍范围内）。尽管事件循环十分快，但是如果某一次循环占用的时间较多，那么下次循环时，它也许已经超时很久了。譬如通过setTimeout()设定一个任务在10毫秒后执行，但是在9毫秒后，有一个任务占用了5毫秒的CPU时间片，再次轮到定时器执行时，时间就已经过期4毫秒。

#### process.nextTick()

在未了解process.nextTick()之前，很多人也许为了立即异步执行一个任务，会这样调用setTimeout()来达到所需的效果：

```js
setTimeout(function () { 
  // TODO
}, 0);
```
由于事件循环自身的特点，定时器的精确度不够。而事实上，采用定时器需要动用红黑树，创建定时器对象和迭代等操作，而setTimeout(fn, 0)的方式较为浪费性能。实际上，process.nextTick()方法的操作相对较为轻量，具体代码如下：

```js
process.nextTick = function(callback) { 
// on the way out, don't bother.
// it won't get fired anyway 
if (process._exiting) return;
if (tickDepth >= process.maxTickDepth)
maxTickWarn();

var tock = { callback: callback }; if (process.domain) tock.domain = process.domain;
nextTickQueue.push(tock); 
if (nextTickQueue.length) {
  process._needTickCallback(); }
};
```
每次调用process.nextTick()方法，只会将回调函数放入队列中，在下一轮Tick时取出执行。定时器中采用红黑树的操作时间复杂度为O(lg(n))，nextTick()的时间复杂度为O(1)。相较之下，process.nextTick()更高效。

#### setImmediate()

setImmediate()方法与process.nextTick()方法十分类似，都是将回调函数延迟执行。在Node v0.9.1之前，setImmediate()还没有实现，那时候实现类似的功能主要是通过process.nextTick()来完成，该方法的代码如下所示：

```js
process.nextTick(function () { 
  console.log('延迟执行');
});
console.log('正常执行');
```
上述代码的输出结果如下：

正常执行
延迟执行

而用setImmediate()实现时，相关代码如下：

```js
setImmediate(function () {
 console.log('延迟执行');
});
console.log('正常执行');
```
其结果完全一样：

正常执行
延迟执行

但是两者之间其实是有细微差别的。将它们放在一起时，又会是怎样的优先级呢。示例代码如下：

```js
process.nextTick(function () {
  console.log('nextTick延迟执行');
});
setImmediate(function () { 
  console.log('setImmediate延迟执行');
});
console.log('正常执行');
```
其执行结果如下：

正常执行
nextTick延迟执行
setImmediate延迟执行

从结果里可以看到，process.nextTick()中的回调函数执行的优先级要高于setImmediate()。这里的原因在于事件循环对观察者的检查是有先后顺序的，process.nextTick()属于idle观察者，setImmediate()属于check观察者。在每一个轮循环检查中，idle观察者先于I/O观察者，I/O观察者先于check观察者。

在具体实现上，process.nextTick()的回调函数保存在一个数组中，setImmediate()的结果则是保存在链表中。在行为上，process.nextTick()在每轮循环中会将数组中的回调函数全部执行完，而setImmediate()在每轮循环中执行链表中的一个回调函数。如下的示例代码可以佐证：

```js
// 加入两个nextTick()的回调函数
process.nextTick(function () {
  console.log('nextTick延迟执行1');
});
process.nextTick(function () { 
  console.log('nextTick延迟执行2');
});
// 加入两个setImmediate()的回调函数
setImmediate(function () { 
  console.log('setImmediate延迟执行1');
  // 进入下次循环 
  process.nextTick(function () {
    console.log('强势插入'); 
  });
});
setImmediate(function () {
  console.log('setImmediate延迟执行2');
});
console.log('正常执行');
```
其执行结果如下：

正常执行
nextTick延迟执行1
nextTick延迟执行2
setImmediate延迟执行1
强势插入
setImmediate延迟执行2

从执行结果上可以看出，当第一个setImmediate()的回调函数执行后，并没有立即执行第二个，而是进入了下一轮循环，再次按process.nextTick()优先、setImmediate()次后的顺序执行。之所以这样设计，是为了保证每轮循环能够较快地执行结束，防止CPU占用过多而阻塞后续I/O调用的情况。

### 事件驱动与高性能服务器

事件驱动的实质，即通过主循环加事件触发的方式来运行程序。

尽管本章只用了fs.open()方法作为例子来阐述Node如何实现异步I/O。而实质上，异步I/O不仅仅应用在文件操作中。对于网络套接字的处理，Node也应用到了异步I/O，网络套接字上侦听到的请求都会形成事件交给I/O观察者。事件循环会不停地处理这些网络I/O事件。如果JavaScript有传入回调函数，这些事件将会最终传递到业务逻辑层进行处理。利用Node构建Web服务器，正是在这样一个基础上实现的，其流程图如下图所示。

![Node构建Web服务器](https://graphbed.qiniu.songxingguo.com/Node-async-IO/Node%E6%9E%84%E5%BB%BAWeb%E6%9C%8D%E5%8A%A1%E5%99%A8.jpg)

下面为几种经典的服务器模型，这里对比下它们的优缺点。

- 同步式。对于同步式的服务，一次只能处理一个请求，并且其余请求都处于等待状态。
- 每进程/每请求。为每个请求启动一个进程，这样可以处理多个请求，但是它不具备扩展性，因为系统资源只有那么多。
- 每线程/每请求。为每个请求启动一个线程来处理。尽管线程比进程要轻量，但是由于每个线程都占用一定内存，当大并发请求到来时，内存将会很快用光，导致服务器缓慢。每线程/每请求的扩展性比每进程/每请求的方式要好，但对于大型站点而言依然不够。


每线程/每请求的方式目前还被Apache所采用。Node通过事件驱动的方式处理请求，无须为每一个请求创建额外的对应线程，可以省掉创建线程和销毁线程的开销，同时操作系统在调度任务时因为线程较少，上下文切换的代价很低。这使得服务器能够有条不紊地处理请求，即使在大量连接的情况下，也不受线程上下文切换开销的影响，这是Node高性能的一个原因。

事件驱动带来的高效已经渐渐开始为业界所重视。知名服务器Nginx，也摒弃了多线程的方式，采用了和Node相同的事件驱动。如今，Nginx大有取代Apache之势。Node具有与Nginx相同的特性，不同之处在于Nginx采用纯C写成，性能较高，但是它仅适合于做Web服务器，用于反向代理或负载均衡等服务，在处理具体业务方面较为欠缺。Node则是一套高性能的平台，可以利用它构建与Nginx相同的功能，也可以处理各种具体业务，而且与背后的网络保持异步畅通。两者相比，Node没有Nginx在Web服务器方面那么专业，但场景更大，自身性能也不错。在实际项目中，我们可以结合它们各自优点，以达到应用的最优性能。

事实上，Node的异步I/O并非首创，但却是第一个成功的平台。在那之前，也有一些知名的基于事件驱动的实现，具体如下所示。

- Ruby的Event Machine。
- Perl的AnyEvent。
- Python的Twisted。

在这些平台上采用事件驱动的方式时，需要花一定精力了解这些库。这些库没能成功的原因则是同步I/O库的存在。本章描述的异步I/O实现，其主旨是使I/O操作与CPU操作分离。奈何这些语言平台上的标准I/O库都是阻塞式的，一旦事件循环中存在阻塞I/O，将导致其余I/O无法立即进行，性能会急剧下降，其效果类似于同步式服务，其他请求将不能立即处理。

JavaScript中的作用域和函数在浏览器端已有成熟的应用，也很好地帮助了Ryan Dahl实现它的想法。JavaScript在服务器端近乎空白，使得Node没有任何历史包袱，而Node在性能上的表现使得它一下子就在社区中流行起来了。

### 总结

本章介绍了异步I/O和另一些非I/O的异步方法。可以看出，事件循环是异步实现的核心，它与浏览器中的执行模型基本保持了一致。而像古老的Rhino，尽管是较早就能在服务器端运行的JavaScript运行时，但是执行模型并不像浏览器采用事件驱动，而是像其他语言一般采用同步I/O作为主要模型，这造成它在性能上无所发挥。Node正是依靠构建了一套完善的高性能异步I/O框架，打破了JavaScript在服务器端止步不前的局面。