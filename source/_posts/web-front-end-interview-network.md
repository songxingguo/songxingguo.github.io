title: Web前端面试题目及详解汇总-网络部分
author: songxingguo
tags: []
categories:
  - 找工作
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

![5 个状态](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/5%20%E4%B8%AA%E7%8A%B6%E6%80%81.png)

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

![四层的体系结构](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E5%9B%9B%E5%B1%82%E7%9A%84%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.png)

![四层的体系结构](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E5%9B%9B%E5%B1%82%E7%9A%84%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%842.png)

#### 五层体系结构

五层体系结构包括：应用层、运输层、网络层、数据链路层和物理层。 

五层协议只是OSI和TCP/IP的综合，实际应用还是TCP/IP的四层结构。为了方便可以把下两层称为网络接口层。

三种模型结构： 

![三种模型结构](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%9E%8B%E7%BB%93%E6%9E%84%EF%BC%9A%20.png)

![三种模型结构](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E4%B8%89%E7%A7%8D%E6%A8%A1%E5%9E%8B%E7%BB%93%E6%9E%842.png)

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

![基于TCP的应用层协议](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E5%9F%BA%E4%BA%8ETCP%E7%9A%84%E5%BA%94%E7%94%A8%E5%B1%82%E5%8D%8F%E8%AE%AE.png)

##### 基于UDP的应用层协议

![基于UDP的应用层协议](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E5%9F%BA%E4%BA%8EUDP%E7%9A%84%E5%BA%94%E7%94%A8%E5%B1%82%E5%8D%8F%E8%AE%AE.png)


![应用层协议](http://p9myzkds7.bkt.clouddn.com/web-front-end-interview-network/%E5%BA%94%E7%94%A8%E5%B1%82%E5%B8%B8%E8%A7%81%E7%9A%84%E5%8D%8F%E8%AE%AE)

来自——[应用层常见的协议及对应的端口号]、[常见应用层协议端口号]、[]

[应用层常见的协议及对应的端口号]:https://blog.csdn.net/mellymengyan/article/details/51115521

[常见应用层协议端口号]:https://blog.csdn.net/sdnu111111111/article/details/38129109

[TCP/IP协议-应用层协议端口号及各层协议数据单元]:https://blog.csdn.net/baidu_35692628/article/details/77747223

### AngularJS指令

### Websocket

### 面包屑导航

### 支付宝

### cookie 登录

### CDN

### 三次握手

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