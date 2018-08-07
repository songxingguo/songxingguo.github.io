title: Web前端面试题目及详解汇总-网络部分
author: songxingguo
tags: []
categories:
  - 找工作
date: 2018-08-06 14:27:00
---
### 简述同步和异步的区别

> 同步是阻塞模式，异步是非阻塞模式。

- 同步
  
  > 使用者通过 **单个线程** 调用服务；该线程发送请求，在服务 **运行时阻塞**，并且 **等待响应** 。
 
  同步就是指一个进程在执行某个请求的时候，若该请求需要一段时间才能返回信息，那么这个 **进程将会一直等待** 下去，直到收到 **返回信息才继续执行下去** 。
  
<!-- more -->

- 异步
  
  > 使用者通过 **两个线程** 调用服务；一个线程 **发送请求** ，而另一个单独的线程 **接收响应** 。

  异步是指 **进程不需要一直等下去** ，而是 **继续执行下面的操作** ，不管其他进程的状态。当有 **消息返回时系统会通知进程进行处理** ，这样可以提高执行的效率。
  
来自—— [同步请求和异步请求的区别]、[简述同步和异步的区别]、[阻塞与非阻塞的区别]

### sessionStorage 、localStorage 和 cookie 之间的区别

> **sessionStorage** 、**localStorage** 和 **cookie** 都用于浏览器本地存储。

- sessionStorage

  > **HTML5 Web Storage API** 提供；可以方便的在 **Web** 请求之间 **保存数据** ；将数据保存在 **session** 对象中。所谓 **session** ，是指用户在 **浏览某个网站** 时，从 **进入网站** 到 **浏览器关闭** 所 **经过的这段时间** ，也就是用户 **浏览这个网站所花费的时间** 。**session** 对象可以用来 **保存在这段时间内所要求保存的任何数据** 。**sessionStorage** 为 **临时保存** 。
  
   **sessionStorage** 的 **生命周期** 是在仅在 **当前会话** 下有效。**sessionStorage** 的概念很特别，引入了一个 **“浏览器窗口”**的概念。**sessionStorage** 是在 **同源的同窗口**（或 **tab** ）中，**始终存在的数据** 。也就是说只要这个 **浏览器窗口没有关闭** ，即使 **刷新页面** 或 **进入同源另一页面** ，**数据仍然存在** 。**关闭窗口** 后，**sessionStorage** 即 **被销毁** 。同时 **“独立”** 打开的 **不同窗口**，即使是 **同一页面** ，**sessionStorage** 对象也是 **不同的** 。

- localStorage

  > 将数据保存在 **客户端本地的硬件设备** (通常指 **硬盘** ，也可以是 **其他硬件设备** )中，即使 **浏览器被关闭** 了，该 **数据仍然存在** ，**下次打开浏览器** 访问网站时 **仍然可以继续使用** 。**localStorage** 为 **永久保存** 。**localStorage** 的 **生命周期**是 **永久的** ，**关闭页面或浏览器** 之后**localStorage** 中的数据也 **不会消失** 。**localStorage** 除非 **主动删除数据** ，否则 **数据永远不会消失** 。

- cookie

  > **HTML5之前的本地存储** ;浏览器的 **缓存机制** 提供了可以将 **用户数据** 存储在 **客户端上的方式** ，可以利用 **cookie** , **session** 等跟 **服务端** 进行 **数据交互** 。

- session

  ![cookie、sessionStorage与lacalStrage的区别]
  
共同点：都是 **保存在浏览器端** ，且 **同源的** 。

区别：cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递；cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。

而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。

来自—— [cookies、sessionStorage和localStorage解释及区别]

### session和cookie之间的区别


### Web Storage与Cookie相比存在的优势

- 存储空间更大：cookie为4KB，而WebStorage是5MB；

- 节省网络流量：WebStorage不会传送到服务器，存储在本地的数据可以直接获取，也不会像cookie一样美词请求都会传送到服务器，所以减少了客户端和服务器端的交互，节省了网络流量；

- 对于那种只需要在用户浏览一组页面期间保存而关闭浏览器后就可以丢弃的数据，sessionStorage会非常方便；

- 快速显示：有的数据存储在WebStorage上，再加上浏览器本身的缓存。获取数据时可以从本地获取会比从服务器端获取快得多，所以速度更快；

- 安全性：WebStorage不会随着HTTP header发送到服务器端，所以安全性相对于cookie来说比较高一些，不会担心截获，但是仍然存在伪造问题；

- WebStorage提供了一些方法，数据操作比cookie方便；

   - setItem (key, value) ——  保存数据，以键值对的方式储存信息。
   - getItem (key) ——  获取数据，将键值传入，即可获取到对应的value值。

   - removeItem (key) ——  删除单个数据，根据键值移除对应的信息。

   - clear () ——  删除所有的数据

   - key (index) —— 获取某个索引的key
  
- 独立的存储空间：每个域（包括子域）有独立的存储空间，各个存储空间是完全独立的，因此不会造成数据混乱。  

来自—— [cookies、sessionStorage和localStorage解释及区别]


#### Ajax的优缺点及工作原理？

> **Ajax** 是 **Asynchronous Javascript And XML** （异步 JavaScript 和 XML）的缩写，是指一种创建交互式网页应用的网页开发技术。

- Ajax = 异步 JavaScript 和 XML（标准通用标记语言的子集）。
- Ajax 是一种用于创建快速动态网页的技术。
- Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。 [1] 
- 通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
- 传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。

*注意：Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。*

传统的Web应用交互由用户触发一个HTTP请求到服务器,服务器对其进行处理后再返回一个新的HTHL页到客户端, 每当服务器处理客户端提交的请求时,客户都只能空闲等待,并且哪怕只是一次很小的交互、只需从服务器端得到很简单的一个数据,都要返回一个完整的HTML页,而用户每次都要浪费时间和带宽去重新读取整个页面。这个做法浪费了许多带宽，由于每次应用的交互都需要向服务器发送请求，应用的响应时间就依赖于服务器的响应时间。这导致了用户界面的响应比本地应用慢得多。

与此不同，AJAX应用可以仅向服务器发送并取回必需的数据，它使用SOAP或其它一些基于XML的Web Service接口，并在客户端采用JavaScript处理来自服务器的响应。因为在服务器和浏览器之间交换的数据大量减少，结果我们就能看到响应更快的应用。同时很多的处理工作可以在发出请求的客户端机器上完成，所以Web服务器的处理时间也减少了。

优点：
1.减轻服务器的负担,按需取数据,最大程度的减少冗余请求

2.局部刷新页面,减少用户心理和实际的等待时间,带来更好的用户体验

3.基于xml标准化,并被广泛支持,不需安装插件等,进一步促进页面和数据的分离

缺点：
1.AJAX大量的使用了javascript和ajax引擎,这些取决于浏览器的支持.在编写的时候考虑对浏览器的兼容性.

2.AJAX只是局部刷新,所以页面的后退按钮是没有用的.

3.对流媒体还有移动设备的支持不是太好等

 ![AJAX工作原理]
 
 1.创建ajax对象（XMLHttpRequest/ActiveXObject(Microsoft.XMLHttp)）

2.判断数据传输方式(GET/POST)

3.打开链接 open()

4.发送 send()

5.当ajax对象完成第四步（onreadystatechange）数据接收完成，判断http响应状态（status）200-300之间或者304（缓存）执行回调函数。

AJAX的工作原理
Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。并不是所有的用户请求都提交给服务器,像—些数据验证和数据处理等都交给Ajax引擎自己来做, 只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。
Ajax其核心有JavaScript、XMLHTTPRequest、DOM对象组成，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。让我们来了解这几个对象。
(1).XMLHTTPRequest对象
Ajax的一个最大的特点是无需刷新页面便可向服务器传输或读写数据(又称无刷新更新页面),这一特点主要得益于XMLHTTP组件XMLHTTPRequest对象。
XMLHttpRequest 对象方法描述 

(2).JavaScript
JavaScript是一在浏览器中大量使用的编程语言。

(3).DOM Document Object Model
DOM是给HTML和XML文件使用的一组API。它提供了文件的结构表述，让你可以改变其中的內容及可见物。其本质是建立网页与Script或程序语言沟通的桥梁。所有WEB开发人员可操作及建立文件的属性、方法及事件都以对象来展现（例如，document就代表“文件本身“这个对像，table对象则代表HTML的表格对象等等）。这些对象可以由当今大多数的浏览器以Script来取用。一个用HTML或XHTML构建的网页也可以看作是一组结构化的数据，这些数据被封在DOM（Document Object Model）中，DOM提供了网页中各个对象的读写的支持。
(4).XML
可扩展的标记语言（Extensible Markup Language）具有一种开放的、可扩展的、可自描述的语言结构，它已经成为网上数据和文档传输的标准,用于其他应用程序交换数据 。
 
 来自—— [AJAX 简介]、[Ajax的优缺点及工作原理？]、[AJAX工作原理及其优缺点]
 
### 28、http和tcp有什么区别？
 
[HTML中href、src区别]: https://blog.csdn.net/annsheshira23/article/details/51133709
[rel、href、src、url的区别]:https://blog.csdn.net/chengshaolei2012/article/details/72847770
[史上最全的CSS hack方式一览]:https://blog.csdn.net/freshlover/article/details/12132801
[CSS hack大全]:https://www.duitang.com/static/csshack.html
[今天才知道css hack是什么]:https://www.cnblogs.com/miniyk/p/3734664.html
[CSS Hack是什么意思？css hack有什么用？]:https://www.w3cschool.cn/css3/question-10231625.html
[你想知道的css hack知识全都帮你整理好了]:https://www.w3cschool.cn/css/css-hack.html
[CSS参考手册]:http://phpstudy.php.cn/css3/
[同步请求和异步请求的区别]: https://blog.csdn.net/goodshot/article/details/7244053
[简述同步和异步的区别]:https://www.cnblogs.com/Renyi-Fan/p/7808756.html#_label0_5
[阻塞与非阻塞的区别]:https://www.cnblogs.com/orez88/articles/2513460.html
[HTML文档的根元素是 html 元素]:https://blog.csdn.net/ixygj197875/article/details/79737953
[px,em,rem单位转换工具]:http://pxtoem.com/
[彻底弄懂px,em和rem的区别]:https://www.cnblogs.com/langee/p/6890362.html
[什么叫优雅降级和渐进增强？]:https://www.cnblogs.com/Renyi-Fan/p/7808756.html#_label0_7
[主流浏览器内核]:http://p9myzkds7.bkt.clouddn.com/web-interview/%E4%B8%BB%E6%B5%81%E6%B5%8F%E8%A7%88%E5%99%A8%E5%86%85%E6%A0%B8.png
[浏览器的内核分别是什么？]:https://www.cnblogs.com/maggie-pan/p/6391355.html
[五大流浏览器内核及其代表]:https://blog.csdn.net/u014753892/article/details/52713841
[cookie、sessionStorage与lacalStrage的区别]: http://p9myzkds7.bkt.clouddn.com/web-interview/cookie%E3%80%81sessionStorage%E4%B8%8ElacalStrage%E7%9A%84%E5%8C%BA%E5%88%AB.png
[cookies、sessionStorage和localStorage解释及区别]:https://www.cnblogs.com/pengc/p/8714475.html
[AJAX工作原理]:http://p9myzkds7.bkt.clouddn.com/web-interview/AJAX%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.gif
[AJAX 简介]:https://www.w3cschool.cn/ajax/nr583fns.html
[Ajax的优缺点及工作原理？]:https://www.cnblogs.com/wdlhao/p/8290436.html#_label3
[AJAX工作原理及其优缺点]:https://www.cnblogs.com/SanMaoSpace/archive/2013/06/15/3137180.html