title: Web前端面试题目及详解汇总-网络部分
author: songxingguo
tags: 
  - 找工作
categories:
  - 备忘录
date: 2018-08-06 14:27:00
---
### 简述同步和异步的区别

> 同步是阻塞模式，异步是非阻塞模式。

> 是否需要等待结果。

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


### 异步和同步（JS为单线程）

[Anker—工作学习笔记](https://www.cnblogs.com/Anker/p/5965654.html)

[同步和异步，区别](https://blog.csdn.net/ideality_hunter/article/details/53453285)

[js中的同步和异步的个人理解](https://blog.csdn.net/qq_22855325/article/details/72958345)

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

1. 减轻服务器的负担,按需取数据,最大程度的减少冗余请求

2. 局部刷新页面,减少用户心理和实际的等待时间,带来更好的用户体验

3. 基于xml标准化,并被广泛支持,不需安装插件等,进一步促进页面和数据的分离

缺点：

1. AJAX大量的使用了javascript和ajax引擎,这些取决于浏览器的支持.在编写的时候考虑对浏览器的兼容性.

2. AJAX只是局部刷新,所以页面的后退按钮是没有用的.

3. 对流媒体还有移动设备的支持不是太好等

![AJAX工作原理]

1. 创建ajax对象（XMLHttpRequest/ActiveXObject(Microsoft.XMLHttp)）

2. 判断数据传输方式(GET/POST)

3. 打开链接 open()

4. 发送 send()

5. 当ajax对象完成第四步（onreadystatechange）数据接收完成，判断http响应状态（status）200-300之间或者304（缓存）执行回调函数。

AJAX的工作原理

Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。并不是所有的用户请求都提交给服务器,像—些数据验证和数据处理等都交给Ajax引擎自己来做, 只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。

Ajax其核心有JavaScript、XMLHTTPRequest、DOM对象组成，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。让我们来了解这几个对象。

1. XMLHTTPRequest对象

    Ajax的一个最大的特点是无需刷新页面便可向服务器传输或读写数据(又称无刷新更新页面),这一特点主要得益于XMLHTTP组件XMLHTTPRequest对象。

2. JavaScript

    JavaScript是一在浏览器中大量使用的编程语言。

3. DOM Document Object Model

    DOM是给HTML和XML文件使用的一组API。它提供了文件的结构表述，让你可以改变其中的內容及可见物。其本质是建立网页与Script或程序语言沟通的桥梁。所有WEB开发人员可操作及建立文件的属性、方法及事件都以对象来展现（例如，document就代表“文件本身“这个对像，table对象则代表HTML的表格对象等等）。这些对象可以由当今大多数的浏览器以Script来取用。一个用HTML或XHTML构建的网页也可以看作是一组结构化的数据，这些数据被封在DOM（Document Object Model）中，DOM提供了网页中各个对象的读写的支持。

4. XML

    可扩展的标记语言（Extensible Markup Language）具有一种开放的、可扩展的、可自描述的语言结构，它已经成为网上数据和文档传输的标准,用于其他应用程序交换数据 。

 来自—— [AJAX 简介]、[Ajax的优缺点及工作原理？]、[AJAX工作原理及其优缺点]

### Ajax 原生JavaScritp实现

```js
var Ajax={
  get: function(url, fn) {
    // XMLHttpRequest对象用于在后台与服务器交换数据   
    var xhr = new XMLHttpRequest();            
    xhr.open('GET', url, true);
    xhr.onreadystatechange = function() {
      // readyState == 4说明请求已完成
      if (xhr.readyState == 4 && xhr.status == 200 || xhr.status == 304) { 
        // 从服务器获得数据 
        fn.call(this, xhr.responseText);  
      }
    };
    xhr.send();
  },
  // datat应为'a=a1&b=b1'这种字符串格式，在jq里如果data为对象会自动将对象转成这种字符串格式
  post: function (url, data, fn) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST", url, true);
    // 添加http头，发送信息至服务器时内容编码类型
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");  
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4 && (xhr.status == 200 || xhr.status == 304)) {
        fn.call(this, xhr.responseText);
      }
    };
    xhr.send(data);
  }
}
```

jQuery 实现

```js
$.ajax({
    url: ,
    type: '',
    dataType: '',
    data: {
          
    },
    success: function(){
         
    },
    error: function(){
          
    }
})
```

来自——[原生js实现Ajax]

[原生js实现Ajax]:https://www.cnblogs.com/colima/p/5339227.html


### XMLHttpRequest 对象

XMLHttpRequest 对象提供了对 HTTP 协议的完全的访问，包括做出 POST 和 HEAD 请求以及普通的 GET 请求的能力。XMLHttpRequest 可以同步或异步地返回 Web 服务器的响应，并且能够以文本或者一个 DOM 文档的形式返回内容。

尽管名为 XMLHttpRequest，它并不限于和 XML 文档一起使用：它可以接收任何形式的文本文档。

XMLHttpRequest 对象是名为 AJAX 的 Web 应用程序架构的一项关键功能。

#### 属性

##### readyState

HTTP 请求的状态.当一个 XMLHttpRequest 初次创建时，这个属性的值从 0 开始，直到接收到完整的 HTTP 响应，这个值增加到 4。

5 个状态中每一个都有一个相关联的非正式的名称，下表列出了状态、名称和含义：

![5 个状态](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/5%20%E4%B8%AA%E7%8A%B6%E6%80%81.png)

readyState 的值不会递减，除非当一个请求在处理过程中的时候调用了 abort() 或 open() 方法。每次这个属性的值增加的时候，都会触发 onreadystatechange 事件句柄。

##### responseText

目前为止为服务器接收到的响应体（不包括头部），或者如果还没有接收到数据的话，就是空字符串。

如果 readyState 小于 3，这个属性就是一个空字符串。当 readyState 为 3，这个属性返回目前已经接收的响应部分。如果 readyState 为 4，这个属性保存了完整的响应体。

如果响应包含了为响应体指定字符编码的头部，就使用该编码。否则，假定使用 Unicode UTF-8。

##### responseXML

对请求的响应，解析为 XML 并作为 Document 对象返回。

##### status

由服务器返回的 HTTP 状态代码，如 200 表示成功，而 404 表示 "Not Found" 错误。当 readyState 小于 3 的时候读取这一属性会导致一个异常。

##### statusText

这个属性用名称而不是数字指定了请求的 HTTP 的状态代码。也就是说，当状态为 200 的时候它是 "OK"，当状态为 404 的时候它是 "Not Found"。和 status 属性一样，当 readyState 小于 3 的时候读取这一属性会导致一个异常。

#### 事件句柄

##### onreadystatechange

每次 readyState 属性改变的时候调用的事件句柄函数。当 readyState 为 3 时，它也可能调用多次。

#### 方法

##### abort()

取消当前响应，关闭连接并且结束任何未决的网络活动。

这个方法把 XMLHttpRequest 对象重置为 readyState 为 0 的状态，并且取消所有未决的网络活动。例如，如果请求用了太长时间，而且响应不再必要的时候，可以调用这个方法。

##### getAllResponseHeaders()

把 HTTP 响应头部作为未解析的字符串返回。

如果 readyState 小于 3，这个方法返回 null。否则，它返回服务器发送的所有 HTTP 响应的头部。头部作为单个的字符串返回，一行一个头部。每行用换行符 "\r\n" 隔开。

##### getResponseHeader()

返回指定的 HTTP 响应头部的值。其参数是要返回的 HTTP 响应头部的名称。可以使用任何大小写来制定这个头部名字，和响应头部的比较是不区分大小写的。

该方法的返回值是指定的 HTTP 响应头部的值，如果没有接收到这个头部或者 readyState 小于 3 则为空字符串。如果接收到多个有指定名称的头部，这个头部的值被连接起来并返回，使用逗号和空格分隔开各个头部的值。

##### open()

初始化 HTTP 请求参数，例如 URL 和 HTTP 方法，但是并不发送请求。

##### send()

发送 HTTP 请求，使用传递给 open() 方法的参数，以及传递给该方法的可选请求体。

##### setRequestHeader()

向一个打开但未发送的请求设置或添加一个 HTTP 请求。

来自——[XML DOM - XMLHttpRequest 对象]

[XML DOM - XMLHttpRequest 对象]:http://www.w3school.com.cn/xmldom/dom_http.asp


### HTTP 头部子字段

有些请求头部由 XMLHttpRequest 自动设置而不是由这个方法设置，以符合 HTTP 协议。这包括如下和代理相关的头部：

- Host
- Connection
- Keep-Alive
- Accept-charset
- Accept-Encoding
- If-Modified-Since
- If-None-Match
- If-Range
- Range

来自——[HTTP 响应头信息]、[HTTP头部信息解释分析(详细整理)]

[HTTP 响应头信息]:http://www.runoob.com/http/http-header-fields.html

[HTTP头部信息解释分析(详细整理)]:https://www.cnblogs.com/jiangxiaobo/p/5499488.html

### 网络分层

#### OSI七层模型

OSI七层协议模型主要是：应用层（Application）、表示层（Presentation）、会话层（Session）、传输层（Transport）、网络层（Network）、数据链路层（Data Link）、物理层（Physical）。

#### TCP/IP四层模型

TCP/IP是一个四层的体系结构，主要包括：应用层、运输层、网际层和网络接口层。从实质上讲，只有上边三层，网络接口层没有什么具体的内容。

![四层的体系结构](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%9B%9B%E5%B1%82%E7%9A%84%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.png)

![四层的体系结构](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%9B%9B%E5%B1%82%E7%9A%84%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%842.png)

#### 五层体系结构

五层体系结构包括：应用层、运输层、网络层、数据链路层和物理层。 

五层协议只是OSI和TCP/IP的综合，实际应用还是TCP/IP的四层结构。为了方便可以把下两层称为网络接口层。

三种模型结构： 

![三种模型结构](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%9E%8B%E7%BB%93%E6%9E%84%EF%BC%9A%20.png)

![三种模型结构](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%9E%8B%E7%BB%93%E6%9E%842.png)

#### 各层的作用

1、物理层：

主要定义物理设备标准，如网线的接口类型、光纤的接口类型、各种传输介质的传输速率等。它的主要作用是传输比特流（就是由1、0转化为电流强弱来进行传输,到达目的地后在转化为1、0，也就是我们常说的数模转换与模数转换）。这一层的数据叫做比特。 　　

2、数据链路层：

定义了如何让格式化数据以进行传输，以及如何让控制对物理介质的访问。这一层通常还提供错误检测和纠正，以确保数据的可靠传输。 　　

3、网络层：

在位于不同地理位置的网络中的两个主机系统之间提供连接和路径选择。Internet的发展使得从世界各站点访问信息的用户数大大增加，而网络层正是管理这种连接的层。 　　

4、运输层：

定义了一些传输数据的协议和端口号（WWW端口80等），如： 
TCP（transmission control protocol –传输控制协议，传输效率低，可靠性强，用于传输可靠性要求高，数据量大的数据） 
UDP（user datagram protocol–用户数据报协议，与TCP特性恰恰相反，用于传输可靠性要求不高，数据量小的数据，如QQ聊天数据就是通过这种方式传输的）。 主要是将从下层接收的数据进行分段和传输，到达目的地址后再进行重组。常常把这一层数据叫做段。 　　
5、会话层：

通过运输层（端口号：传输端口与接收端口）建立数据传输的通路。主要在你的系统之间发起会话或者接受会话请求（设备之间需要互相认识可以是IP也可以是MAC或者是主机名） 　　

6、表示层：

可确保一个系统的应用层所发送的信息可以被另一个系统的应用层读取。例如，PC程序与另一台计算机进行通信，其中一台计算机使用扩展二一十进制交换码（EBCDIC），而另一台则使用美国信息交换标准码（ASCII）来表示相同的字符。如有必要，表示层会通过使用一种通格式来实现多种数据格式之间的转换。 　　

7、应用层：

是最靠近用户的OSI层。这一层为用户的应用程序（例如电子邮件、文件传输和终端仿真）提供网络服务。

来自——[OSI七层协议模型、TCP/IP四层模型和五层协议体系结构之间的关系]

[OSI七层协议模型、TCP/IP四层模型和五层协议体系结构之间的关系]:https://www.cnblogs.com/wxd0108/p/7597216.html


### 应用层协议以及端口号

#### 应用层协议端口号对应

- DNS 53/tcp或/udp
- SMTP 25/tcp
- POP3 110/tcp
- HTTP 80/tcp
- HTTPS 443/udp
- TELNET 23/tcp
- FTP 20/21/tcp
- tftp 69/udp
- IMAP 143/tcp
- snmp 161/udp
- snmptrap 162/udp

##### 基于TCP的应用层协议

![基于TCP的应用层协议](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%9F%BA%E4%BA%8ETCP%E7%9A%84%E5%BA%94%E7%94%A8%E5%B1%82%E5%8D%8F%E8%AE%AE.png)

##### 基于UDP的应用层协议

![基于UDP的应用层协议](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%9F%BA%E4%BA%8EUDP%E7%9A%84%E5%BA%94%E7%94%A8%E5%B1%82%E5%8D%8F%E8%AE%AE.png)


![应用层协议](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%BA%94%E7%94%A8%E5%B1%82%E5%B8%B8%E8%A7%81%E7%9A%84%E5%8D%8F%E8%AE%AE)

来自——[应用层常见的协议及对应的端口号]、[常见应用层协议端口号]、[TCP/IP协议-应用层协议端口号及各层协议数据单元]]

[应用层常见的协议及对应的端口号]:https://blog.csdn.net/mellymengyan/article/details/51115521
[常见应用层协议端口号]:https://blog.csdn.net/sdnu111111111/article/details/38129109
[TCP/IP协议-应用层协议端口号及各层协议数据单元]:https://blog.csdn.net/baidu_35692628/article/details/77747223

### HTTP 协议

#### HTTP1.0 HTTP 1.1 HTTP 2.0主要区别

##### HTTP 1.0 与 HTTP 1.1 的区别

- 长连接

  HTTP 1.0需要使用keep-alive参数来告知服务器端要建立一个长连接，而HTTP1.1默认支持长连接。

- 节约带宽

   HTTP 1.1支持只发送header信息(不带任何body信息)，如果服务器认为客户端有权限请求服务器，则返回100，否则返回401。客户端如果接受到100，才开始把请求body发送到服务器。
   
- HOST域

  现在可以web server例如tomat，设置虚拟站点是非常常见的，也即是说，web server上的多个虚拟站点可以共享同一个ip和端口。
  
#### HTTP1.1 HTTP 2.0主要区别

- 多路复用

  HTTP2.0使用了多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比HTTP1.1大了好几个数量级。

- 数据压缩

  HTTP1.1不支持header数据的压缩，HTTP2.0使用HPACK算法对header的数据进行压缩，这样数据体积小了，在网络上传输就会更快。
  
- 服务器推送

   意思是说，当我们对支持HTTP2.0的web server请求数据的时候，服务器会顺便把一些客户端需要的资源一起推送到客户端，免得客户端再次创建连接发送请求到服务器端获取。这种方式非常合适加载静态资源。

来自——[HTTP1.0 HTTP 1.1 HTTP 2.0主要区别]、[深入研究：HTTP2 的真正性能到底如何]、[HTTP2.0的奇妙日常]、[一分钟预览 HTTP2 特性和抓包分析]

[HTTP1.0 HTTP 1.1 HTTP 2.0主要区别]:https://blog.csdn.net/linsongbin1/article/details/54980801/
[深入研究：HTTP2 的真正性能到底如何]:https://segmentfault.com/a/1190000007219256
[HTTP,HTTP2.0,SPDY,HTTPS你应该知道的一些事]:http://www.alloyteam.com/2016/07/httphttp2-0spdyhttps-reading-this-is-enough/
[HTTP2.0的奇妙日常]:http://www.alloyteam.com/2015/03/http2-0-di-qi-miao-ri-chang/
[一分钟预览 HTTP2 特性和抓包分析]:https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&amp;mid=2651551351&amp;idx=2&amp;sn=a56ff090060f97e11e856aef2622a717&amp;chksm=8025a1b6b75228a0080fa971222b3cb7c3179ba5474028b8fa4656619073c4c14d76cf83cd86&amp;scene=0#wechat_redirect

### HTTPS

![http和https的区别](https://graphbed.qiniu.songxingguo.com/interview/http%E5%92%8Chttps.JPG)

如上图所示 HTTPS 相比 HTTP 多了一层 SSL/TLS

SSL（Secure Socket Layer，安全套接字层）：1994年为 Netscape 所研发，SSL 协议位于 TCP/IP 协议与各种应用层协议之间，为数据通讯提供安全支持。

TLS（Transport Layer Security，传输层安全）：其前身是 SSL，它最初的几个版本（SSL 1.0、SSL 2.0、SSL 3.0）由网景公司开发，1999年从 3.1 开始被 IETF 标准化并改名，发展至今已经有 TLS 1.0、TLS 1.1、TLS 1.2 三个版本。SSL3.0和TLS1.0由于存在安全漏洞，已经很少被使用到。TLS 1.3 改动会比较大，目前还在草案阶段，目前使用最广泛的是TLS 1.1、TLS 1.2。

#### 加密算法：

据记载，公元前400年，古希腊人就发明了置换密码；在第二次世界大战期间，德国军方启用了“恩尼格玛”密码机，所以密码学在社会发展中有着广泛的用途。

##### 对称加密

有流式、分组两种，加密和解密都是使用的同一个密钥。

例如：DES、AES-GCM、ChaCha20-Poly1305等

##### 非对称加密

加密使用的密钥和解密使用的密钥是不相同的，分别称为：公钥、私钥，公钥和算法都是公开的，私钥是保密的。非对称加密算法性能较低，但是安全性超强，由于其加密特性，非对称加密算法能加密的数据长度也是有限的。

例如：RSA、DSA、ECDSA、 DH、ECDHE

##### 哈希算法

将任意长度的信息转换为较短的固定长度的值，通常其长度要比信息小得多，且算法不可逆。

例如：MD5、SHA-1、SHA-2、SHA-256 等

##### 数字签名

签名就是在信息的后面再加上一段内容（信息经过hash后的值），可以证明信息没有被修改过。hash值一般都会加密后（也就是签名）再和信息一起发送，以保证这个hash值不被修改。

#### 总结

综上所述，相比 HTTP 协议，HTTPS 协议增加了很多握手、加密解密等流程，虽然过程很复杂，但其可以保证数据传输的安全。所以在这个互联网膨胀的时代，其中隐藏着各种看不见的危机，为了保证数据的安全，维护网络稳定，建议大家多多推广HTTPS。

##### HTTP 传输面临的风险有：

（1） 窃听风险：黑客可以获知通信内容。

（2） 篡改风险：黑客可以修改通信内容。

（3） 冒充风险：黑客可以冒充他人身份参与通信。

##### HTTPS 缺点：

（1）SSL 证书费用很高，以及其在服务器上的部署、更新维护非常繁琐

（2）HTTPS 降低用户访问速度（多次握手）

（3）网站改用HTTPS 以后，由HTTP 跳转到 HTTPS 的方式增加了用户访问耗时（多数网站采用302跳转）

（4）HTTPS 涉及到的安全算法会消耗 CPU 资源，需要增加大量机器（https访问过程需要加解密）

来自——[HTTPS 原理详解]、[HTTP与HTTPS的区别]、[白话图解HTTPS原理]

[HTTPS 原理详解]:https://baijiahao.baidu.com/s?id=1570143475599137&amp;wfr=spider&amp;for=pc
[HTTP与HTTPS的区别]:https://www.cnblogs.com/wqhwe/p/5407468.html
[白话图解HTTPS原理]:https://www.cnblogs.com/ghjbk/p/6738069.html

### http缓存规则

HTTP缓存有多种规则，根据是否需要重新向服务器发起请求来分类，我将其分为两大类(强制缓存，对比缓存)

在详细介绍这两种规则之前，先通过时序图的方式，让大家对这两种规则有个简单了解。


#### 强制缓存

对于强制缓存来说，响应header中会有两个字段来标明失效规则（Expires/Cache-Control）。

##### Expires

Expires的值为服务端返回的到期时间，即下一次请求时，请求时间小于服务端返回的到期时间，直接使用缓存数据。


不过Expires 是HTTP 1.0的东西，现在默认浏览器均默认使用HTTP 1.1，所以它的作用基本忽略。

另一个问题是，到期时间是由服务端生成的，但是客户端时间可能跟服务端时间有误差，这就会导致缓存命中的误差。

所以HTTP 1.1 的版本，使用Cache-Control替代。

##### Cache-Control

Cache-Control 是最重要的规则。常见的取值有private、public、no-cache、max-age，no-store，默认为private。

- private:             客户端可以缓存
- public:              客户端和代理服务器都可缓存（前端的同学，可以认为- - public和private是一样的）
- max-age=xxx:   缓存的内容将在 xxx 秒后失效
- no-cache:          需要使用对比缓存来验证缓存数据（后面介绍）
- no-store:           所有内容都不会缓存，强制缓存，对比缓存都不会触发（对于前端开发来说，缓存越多越好，so...基本上和它说886）

![举个板栗](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%BC%BA%E5%88%B6%E7%BC%93%E5%AD%98.png)

图中Cache-Control仅指定了max-age，所以默认为private，缓存时间为31536000秒（365天）

也就是说，在365天内再次请求这条数据，都会直接获取缓存数据库中的数据，直接使用。

#### 对比缓存

对比缓存，顾名思义，需要进行比较判断是否可以使用缓存。

浏览器第一次请求数据时，服务器会将缓存标识与数据一起返回给客户端，客户端将二者备份至缓存数据库中。
再次请求数据时，客户端将备份的缓存标识发送给服务器，服务器根据缓存标识进行判断，判断成功后，返回304状态码，通知客户端比较成功，可以使用缓存数据。

![第一次访问](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E7%AC%AC%E4%B8%80%E6%AC%A1%E8%AE%BF%E9%97%AE.png)

![再次访问](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/%E5%86%8D%E6%AC%A1%E8%AE%BF%E9%97%AE.png)

通过两图的对比，我们可以很清楚的发现，在对比缓存生效时，状态码为304，并且报文大小和请求时间大大减少。

原因是，服务端在进行标识比较后，只返回header部分，通过状态码通知客户端使用缓存，不再需要将报文主体部分返回给客户端。

对于对比缓存来说，缓存标识的传递是我们着重需要理解的，它在请求header和响应header间进行传递，一共分为两种标识传递：

##### Last-Modified  /  If-Modified-Since

Last-Modified：服务器在响应请求时，告诉浏览器资源的最后修改时间。

![Last-Modified](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/Last-Modified.png)

If-Modified-Since：再次请求服务器时，通过此字段通知服务器上次请求时，服务器返回的资源最后修改时间。

- 服务器收到请求后发现有头If-Modified-Since 则与被请求资源的最后修改时间进行比对。
- 若资源的最后修改时间大于If-Modified-Since，说明资源又被改动过，则响应整片资源内容，返回状态码200；
- 若资源的最后修改时间小于或等于If-Modified-Since，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。

![If-Modified-Since](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/If-Modified-Since.png)

##### Etag  /  If-None-Match（优先级高于Last-Modified  /  If-Modified-Since）

Etag：服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器决定）。

![Etag](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/Etag.png)

If-None-Match：再次请求服务器时，通过此字段通知服务器客户段缓存数据的唯一标识。

- 服务器收到请求后发现有头If-None-Match 则与被请求资源的唯一标识进行比对，
- 不同，说明资源又被改动过，则响应整片资源内容，返回状态码200；
- 相同，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。

![If-None-Match](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-network/If-None-Match.png)

#### 总结

对于强制缓存，服务器通知浏览器一个缓存时间，在缓存时间内，下次请求，直接用缓存，不在时间内，执行比较缓存策略。
对于比较缓存，将缓存信息中的Etag和Last-Modified通过请求发送给服务器，由服务器校验，返回304状态码时，浏览器直接使用缓存。

浏览器第一次请求：

![第一次请求](https://graphbed.qiniu.songxingguo.com/%E7%AC%AC%E4%B8%80%E6%AC%A1%E8%AF%B7%E6%B1%82.png)

浏览器再次请求时：

![再次请求](https://graphbed.qiniu.songxingguo.com/%E7%AC%AC%E4%BA%8C%E6%AC%A1%E8%AF%B7%E6%B1%82.png)

来自——[彻底弄懂HTTP缓存机制及原理]、[http缓存浅谈]、[http协议缓存机制]、[HTTP缓存实现的原理]

[彻底弄懂HTTP缓存机制及原理]:https://www.cnblogs.com/chenqf/p/6386163.html

[http缓存浅谈]:https://www.cnblogs.com/chinajava/p/5705169.html

[http协议缓存机制]:https://segmentfault.com/a/1190000010690320

[HTTP缓存实现的原理]:https://www.cnblogs.com/zikai/p/4973355.html

### http性能优化


来自——[36条雅虎军规]、[http请求过程及性能优化分析]、[腾讯HTTPS性能优化实践]、[http性能优化的最佳实践]、[网络请求和性能优化]、[HTTP之2 HTTP优化(HTTP性能优化、安全的HTTP协议)]、[应用缓慢、卡顿千万不能忽视HTTP请求优化]

[36条雅虎军规]:http://www.mamicode.com/info-detail-139010.html
[http请求过程及性能优化分析]:https://blog.csdn.net/j_bleach/article/details/75215499
[腾讯HTTPS性能优化实践]:https://blog.csdn.net/suhuaiqiang_janlay/article/details/60962697
[http性能优化的最佳实践]:https://blog.csdn.net/Charles_Tian/article/details/80352118
[网络请求和性能优化]:https://www.jianshu.com/p/ab75b24a11e2
[HTTP之2 HTTP优化(HTTP性能优化、安全的HTTP协议)]:http://blog.51cto.com/jasonteach/1760468
[应用缓慢、卡顿千万不能忽视HTTP请求优化]:https://www.sohu.com/a/129178271_496760

### TCP 和 UDP

TCP是面向连接的协议，其显著的特征是在传输之前需要3次握手形成会话。

UDP又称用户数据包协议，与TCP一样同属于网络传输层。UDP与TCP最大的不同是UDP不是面向连接的。

来自——[TCP和UDP的优缺点及区别]、[TCP/IP和UDP的比较]、[TCP与UDP--图解TCP/IP读书笔记]、[TCP和UDP的最完整的区别]

[TCP和UDP的优缺点及区别]:https://www.cnblogs.com/xiaomayizoe/p/5258754.html
[TCP/IP和UDP的比较]:https://www.cnblogs.com/HPAHPA/p/7737641.html
[TCP与UDP--图解TCP/IP读书笔记]:https://blog.csdn.net/sinat_37138973/article/details/72822229
[TCP和UDP的最完整的区别]:https://blog.csdn.net/li_ning_/article/details/52117463

### AngularJS指令

### Websocket

### 面包屑导航

### 支付宝

### cookie 登录

### CDN

### 三次握手

### http和tcp有什么区别？

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
