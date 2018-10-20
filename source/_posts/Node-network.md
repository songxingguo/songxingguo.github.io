title: 《深入浅出Node.js》-读书笔记-网络编程
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
date: 2018-10-06 00:09:00
---
## 网络编程

Node是一个面向网络而生的平台，它具有事件驱动、无阻塞、单线程等特性，具备良好的可伸缩性，使得它十分轻量，适合在分布式网络中扮演各种各样的角色。同时Node提供的API十分贴合网络，适合用它基础的API构建灵活的网络服务。从本章起，我将介绍Node在网络服务器方面的具体能力。

利用Node可以十分方便地搭建网络服务器。在Web领域，大多数的编程语言需要专门的Web服务器作为容器，如ASP、ASP.NET需要IIS作为服务器，PHP需要搭载Apache或Nginx环境等，JSP需要Tomcat服务器等。但对于Node而言，只需要几行代码即可构建服务器，无需额外的容器。

Node提供了net、dgram、http、https这4个模块，分别用于处理TCP、UDP、HTTP、HTTPS，适用于服务器端和客户端。

<!-- more -->

### 构建TCP服务

TCP服务在网络应用中十分常见，目前大多数的应用都是基于TCP搭建而成的。

#### TCP

TCP全名为传输控制协议，在OSI模型（由七层组成，分别为物理层、数据链结层、网络层、传输层、会话层、表示层、应用层）中属于传输层协议。

许多应用层协议基于TCP构建，典型的是HTTP、SMTP、IMAP等协议。七层协议示意图如下图所示。

![七层协议示意图](http://p9myzkds7.bkt.clouddn.com/Node-network/%E4%B8%83%E5%B1%82%E5%8D%8F%E8%AE%AE%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

TCP是面向连接的协议，其显著的特征是在传输之前需要3次握手形成会话，如下图所示。

![TCP在传输之前的3次握手](http://p9myzkds7.bkt.clouddn.com/Node-network/TCP%E5%9C%A8%E4%BC%A0%E8%BE%93%E4%B9%8B%E5%89%8D%E7%9A%843%E6%AC%A1%E6%8F%A1%E6%89%8B.jpg)

只有会话形成之后，服务器端和客户端之间才能互相发送数据。在创建会话的过程中，服务器端和客户端分别提供一个套接字，这两个套接字共同形成一个连接。服务器端与客户端则通过套接字实现两者之间连接的操作。

#### 创建TCP服务器端

在基本了解TCP的工作原理之后，我们可以开始创建一个TCP服务器端来接受网络请求，代码如下：

```js
var net = require('net');
var server = net.createServer(function (socket) { 
    // 新的连接
    socket.on('data', function (data) {
        socket.write("你好");
    });
    socket.on('end', function () {
        console.log('连接断开');
    });
    socket.write("欢迎光临《深入浅出Node.js》示例：\n");
});
server.listen(8124, function () {
    console.log('server bound');
});
```
我们通过net.createServer(listener)即可创建一个TCP服务器，listener是连接事件connection的侦听器，也可以采用如下的方式进行侦听：

```js
var server = net.createServer();
server.on('connection', function (socket) { 
  // 新的连接
});
server.listen(8124);
```
我们可以利用Telnet工具作为客户端对刚才创建的简单服务器进行会话交流，相关代码如下所示：

```js
$ telnet 127.0.0.1 8124
Trying 127.0.0.1...Connected to localhost.
Escape character is '^]'.
欢迎光临《深入浅出Node.js》示例：
hi你好
```
除了端口外，同样我们也可以对Domain Socket进行监听，代码如下：

```js
server.listen('/tmp/echo.sock');
```
通过nc工具进行会话，测试上面构建的TCP服务的代码如下所示：

```
$ nc -U /tmp/echo.sock
欢迎光临《深入浅出Node.js》示例：
hi你好
```
通过net模块自行构造客户端进行会话，测试上面构建的TCP服务的代码如下所示：

```js
var net = require('net');
var client = net.connect({port: 8124}, function () { 
    //'connect' listener 
    console.log('client connected');
    client.write('world!\r\n');
});
client.on('data', function (data) {
    console.log(data.toString());
    client.end();
});
client.on('end', function () {
    console.log('client disconnected');
});
```
将以上客户端代码存为client.js并执行，如下所示：

```
$ node client.jsclient connected
欢迎光临《深入浅出Node.js》示例：
你好client disconnected
```
其结果与使用Telnet和nc的会话结果并无差别。如果是Domain Socket，在填写选项时，填写path即可，代码如下：

```js
var client = net.connect({path: '/tmp/echo.sock'});
```

#### TCP服务的事件

在上述的示例中，代码分为服务器事件和连接事件。

1. 服务器事件

对于通过net.createServer()创建的服务器而言，它是一个EventEmitter实例，它的自定义事件有如下几种。

- listening：在调用server.listen()绑定端口或者Domain Socket后触发，简洁写法为server.listen(port,listeningListener)，通过listen()方法的第二个参数传入。
- connection：每个客户端套接字连接到服务器端时触发，简洁写法为通过net.create-Server()，最后一个参数传递。
- close：当服务器关闭时触发，在调用server.close()后，服务器将停止接受新的套接字连接，但保持当前存在的连接，等待所有连接都断开后，会触发该事件。
- error：当服务器发生异常时，将会触发该事件。比如侦听一个使用中的端口，将会触发一个异常，如果不侦听error事件，服务器将会抛出异常。

2. 连接事件

服务器可以同时与多个客户端保持连接，对于每个连接而言是典型的可写可读Stream对象。Stream对象可以用于服务器端和客户端之间的通信，既可以通过data事件从一端读取另一端发来的数据，也可以通过write()方法从一端向另一端发送数据。它具有如下自定义事件。

- data：当一端调用write()发送数据时，另一端会触发data事件，事件传递的数据即是write()发送的数据。
- end：当连接中的任意一端发送了FIN数据时，将会触发该事件。
- connect：该事件用于客户端，当套接字与服务器端连接成功时会被触发。
- drain：当任意一端调用write()发送数据时，当前这端会触发该事件。
- error：当异常发生时，触发该事件。
- close：当套接字完全关闭时，触发该事件。
- timeout：当一定时间后连接不再活跃时，该事件将会被触发，通知用户当前该连接已经被闲置了。

另外，由于TCP套接字是可写可读的Stream对象，可以利用pipe()方法巧妙地实现管道操作，如下代码实现了一个echo服务器：

```js
var net = require('net');
var server = net.createServer(function (socket) { socket.write('Echo server\r\n');
socket.pipe(socket);});
server.listen(1337, '127.0.0.1');
```
值得注意的是，TCP针对网络中的小数据包有一定的优化策略：Nagle算法。如果每次只发送一个字节的内容而不优化，网络中将充满只有极少数有效数据的数据包，将十分浪费网络资源。Nagle算法针对这种情况，要求缓冲区的数据达到一定数量或者一定时间后才将其发出，所以小数据包将会被Nagle算法合并，以此来优化网络。这种优化虽然使网络带宽被有效地使用，但是数据有可能被延迟发送。

在Node中，由于TCP默认启用了Nagle算法，可以调用socket.setNoDelay(true)去掉Nagle算法，使得write()可以立即发送数据到网络中。

另一个需要注意的是，尽管在网络的一端调用write()会触发另一端的data事件，但是并不意味着每次write()都会触发一次data事件，在关闭掉Nagle算法后，另一端可能会将接收到的多个小数据包合并，然后只触发一次data事件。

### 构建UDP服务

UDP又称用户数据包协议，与TCP一样同属于网络传输层。UDP与TCP最大的不同是UDP不是面向连接的。TCP中连接一旦建立，所有的会话都基于连接完成，客户端如果要与另一个TCP服务通信，需要另创建一个套接字来完成连接。但在UDP中，一个套接字可以与多个UDP服务通信，它虽然提供面向事务的简单不可靠信息传输服务，在网络差的情况下存在丢包严重的问题，但是由于它无须连接，资源消耗低，处理快速且灵活，所以常常应用在那种偶尔丢一两个数据包也不会产生重大影响的场景，比如音频、视频等。UDP目前应用很广泛，DNS服务即是基于它实现的。

#### 创建UDP套接字

创建UDP套接字十分简单，UDP套接字一旦创建，既可以作为客户端发送数据，也可以作为服务器端接收数据。下面的代码创建了一个UDP套接字：

```js
var dgram = require('dgram');var socket = dgram.createSocket("udp4");
```
#### 创建UDP服务器端

若想让UDP套接字接收网络消息，只要调用dgram.bind(port, [address])方法对网卡和端口进行绑定即可。以下为一个完整的服务器端示例：

```js
var dgram = require("dgram");
var server = dgram.createSocket("udp4");
server.on("message", function (msg, rinfo) {
    console.log("server got: " + msg + " from " +
        rinfo.address + ":" + rinfo.port);
});
server.on("listening", function () {
    var address = server.address();
    console.log("server listening " +
        address.address + ":" + address.port);
});
server.bind(41234);
```
该套接字将接收所有网卡上41234端口上的消息。在绑定完成后，将触发listening事件。

#### 创建UDP客户端

接下来我们创建一个客户端与服务器端进行对话，代码如下：

```js
var dgram = require('dgram');
var message = new Buffer("深入浅出Node.js");
var client = dgram.createSocket("udp4");
client.send(message, 0, message.length, 41234, "localhost", function (err, bytes) {
    client.close();
});
```
保存为client.js并执行，服务器端的命令行将会有如下输出：

```
$ node server.js
server listening 0.0.0.0:41234
server got: 深入浅出Node.js from 127.0.0.1:58682
```
当套接字对象用在客户端时，可以调用send()方法发送消息到网络中。send()方法的参数如下：

```js
socket.send(buf, offset, length, port, address, [callback])
```
这些参数分别为要发送的Buffer、Buffer的偏移、Buffer的长度、目标端口、目标地址、发送完成后的回调。与TCP套接字的write()相比，send()方法的参数列表相对复杂，但是它更灵活的地方在于可以随意发送数据到网络中的服务器端，而TCP如果要发送数据给另一个服务器端，则需要重新通过套接字构造新的连接。

#### UDP套接字事件

UDP套接字相对TCP套接字使用起来更简单，它只是一个EventEmitter的实例，而非Stream的实例。它具备如下自定义事件。

- message：当UDP套接字侦听网卡端口后，接收到消息时触发该事件，触发携带的数据为消息Buffer对象和一个远程地址信息。
- listening：当UDP套接字开始侦听时触发该事件。
- close：调用close()方法时触发该事件，并不再触发message事件。如需再次触发- - 
- message事件，重新绑定即可。
- error：当异常发生时触发该事件，如果不侦听，异常将直接抛出，使进程退出。

### 构建HTTP服务

TCP与UDP都属于网络传输层协议，如果要构造高效的网络应用，就应该从传输层进行着手。但是对于经典的应用场景，则无须从传输层协议入手构造自己的应用，比如HTTP或SMTP等，这些经典的应用层协议对于普通应用而言绰绰有余。Node提供了基本的http和https模块用于HTTP和HTTPS的封装，对于其他应用层协议的封装，也能从社区中轻松找到其实现。

在Node中构建HTTP服务极其容易，Node官网上的经典例子就展示了如何用寥寥几行代码实现一个HTTP服务器，代码如下：

```js
var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, {
        'Content-Type': 'text/plain'
    });
    res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```
尽管这个HTTP服务器简单到只能回复Hello World，但是它能维持的并发量和QPS都是不容小觑的，其背后的原因在第3章中有叙述，此处我们不再探讨。这里我们抛开性能，只对其HTTP服务在应用层的实现原理进行展开、讨论和研究。

#### HTTP

1. 初识HTTP

HTTP的全称是超文本传输协议，英文写作HyperText Transfer Protocol。欲了解Web，先了解HTTP将会极大地提高我们对Web的认知。HTTP构建在TCP之上，属于应用层协议。在HTTP的两端是服务器和浏览器，即著名的B/S模式，如今精彩纷呈的Web即是HTTP的应用。HTTP得以发展是W3C和IETF两个组织合作的结果，他们最终发布了一系列RFC标准，目前最知名的HTTP标准为RFC 2616。

2. HTTP报文

为了详细解释HTTP的报文，在启动上述服务器端代码后，我们对经典示例代码进行一次报文的获取，这里采用的工具是curl，通过-v选项，可以显示这次网络通信的所有报文信息，如下所示：

```
$ curl -v http://127.0.0.1:1337
* About to connect() to 127.0.0.1 port 1337 (#0)
* Trying 127.0.0.1...
* connected
* Connected to 127.0.0.1 (127.0.0.1) port 1337 (#0)
> GET / HTTP/1.1
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
> Host: 127.0.0.1:1337
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: text/plain< Date: Sat, 06 Apr 2013 08:01:44 GMT
< Connection: keep-alive< Transfer-Encoding: chunked
<
Hello World
* Connection #0 to host 127.0.0.1 left intact
* Closing connection #0
```
从上述信息中我们可以看到这次网络通信的报文信息分为几个部分，第一部分内容为经典的TCP的3次握手过程，如下所示：

```
* About to connect() to 127.0.0.1 port 1337 (#0)
* Trying 127.0.0.1...
* connected
* Connected to 127.0.0.1 (127.0.0.1) port 1337 (#0)
```
第二部分是在完成握手之后，客户端向服务器端发送请求报文，如下所示：

```
> GET / HTTP/1.1
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
> Host: 127.0.0.1:1337
> Accept: */*
>
```
第三部分是服务器端完成处理后，向客户端发送响应内容，包括响应头和响应体，如下所示：

```
< HTTP/1.1 200 OK
< Content-Type: text/plain
< Date: Sat, 06 Apr 2013 08:01:44 GMT
< Connection: keep-alive
< Transfer-Encoding: chunked<
Hello World
```
最后部分是结束会话的信息，如下所示：

```
* Connection #0 to host 127.0.0.1 left intact
* Closing connection #0
```
从上述的报文信息中可以看出HTTP的特点，它是基于请求响应式的，以一问一答的方式实现服务，虽然基于TCP会话，但是本身却并无会话的特点。

从协议的角度来说，现在的应用，如浏览器，其实是一个HTTP的代理，用户的行为将会通过它转化为HTTP请求报文发送给服务器端，服务器端在处理请求后，发送响应报文给代理，代理在解析报文后，将用户需要的内容呈现在界面上。以浏览器打开一张图片地址为例：首先，浏览器构造HTTP报文发向图片服务器端；然后，服务器端判断报文中的要请求的地址，将磁盘中的图片文件以报文的形式发送给浏览器；浏览器接收完图片后，调用渲染引擎将其显示给用户。简而言之，HTTP服务只做两件事情：处理HTTP请求和发送HTTP响应。

无论是HTTP请求报文还是HTTP响应报文，报文内容都包含两个部分：报文头和报文体。

上文的报文代码中>和<部分属于报文的头部，由于是GET请求，请求报文中没有包含报文体，响应报文中的Hello World即是报文体。

#### http模块

Node的http模块包含对HTTP处理的封装。在Node中，HTTP服务继承自TCP服务器（net模块），它能够与多个客户端保持连接，由于其采用事件驱动的形式，并不为每一个连接创建额外的线程或进程，保持很低的内存占用，所以能实现高并发。HTTP服务与TCP服务模型有区别的地方在于，在开启keepalive后，一个TCP会话可以用于多次请求和响应。TCP服务以connection为单位进行服务，HTTP服务以request为单位进行服务。http模块即是将connection到request的过程进行了封装，示意图如下图所示。

![http模块即是将connection到request的过程进行了封装](http://p9myzkds7.bkt.clouddn.com/Node-network/http%E6%A8%A1%E5%9D%97%E5%8D%B3%E6%98%AF%E5%B0%86connection%E5%88%B0request%E7%9A%84%E8%BF%87%E7%A8%8B%E8%BF%9B%E8%A1%8C%E4%BA%86%E5%B0%81%E8%A3%85.jpg)

除此之外，http模块将连接所用套接字的读写抽象为ServerRequest和ServerResponse对象，它们分别对应请求和响应操作。在请求产生的过程中，http模块拿到连接中传来的数据，调用二进制模块http_parser进行解析，在解析完请求报文的报头后，触发request事件，调用用户的业务逻辑。该流程的示意图如下图所示。

![http模块产生的请求的流程](http://p9myzkds7.bkt.clouddn.com/Node-network/http%E6%A8%A1%E5%9D%97%E4%BA%A7%E7%94%9F%E7%9A%84%E8%AF%B7%E6%B1%82%E7%9A%84%E6%B5%81%E7%A8%8B.jpg)

上图中的处理程序对应到示例中的代码就是响应Hello World这部分，代码如下：

```js
function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World\n');
}
```
1. HTTP请求

对于TCP连接的读操作，http模块将其封装为ServerRequest对象。让我们再次查看前面的请求报文，报文头部将会通过http_parser进行解析。请求报文的代码如下所示：

```
> GET / HTTP/1.1
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
> Host: 127.0.0.1:1337
> Accept: */*
>
```
报文头第一行GET / HTTP/1.1被解析之后分解为如下属性。

- req.method属性：值为GET，是为请求方法，常见的请求方法有GET、POST、DELETE、PUT、CONNECT等几种。
- req.url属性：值为/。
- req.httpVersion属性：值为1.1。

其余报头是很规律的Key: Value格式，被解析后放置在req.headers属性上传递给业务逻辑以供调用，如下所示：

```
headers:
{ 'user-agent': 'curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5', host: '127.0.0.1:1337',
accept: '*/*' },
```
报文体部分则抽象为一个只读流对象，如果业务逻辑需要读取报文体中的数据，则要在这个数据流结束后才能进行操作，如下所示：

```js
function(req, res) { 
  // console.log(req.headers);
  var buffers = [];
  req.on('data',
  function(trunk) {
    buffers.push(trunk);
  }).on('end',
  function() {
    var buffer = Buffer.concat(buffers); 
    // TODO
    res.end('Hello world');
  });
}
```
HTTP请求对象和HTTP响应对象是相对较底层的封装，现行的Web框架如Connect和Express都是在这两个对象的基础上进行高层封装完成的。

2. HTTP响应

再来看看HTTP响应对象。HTTP响应相对简单一些，它封装了对底层连接的写操作，可以将其看成一个可写的流对象。它影响响应报文头部信息的API为res.setHeader()和res.writeHead()。在上述示例中：

```js
res.writeHead(200, {'Content-Type': 'text/plain'});
```
其分为setHeader()和writeHead()两个步骤。它在http模块的封装下，实际生成如下报文：

```
< HTTP/1.1 200 OK
< Content-Type: text/plain
```
我们可以调用setHeader进行多次设置，但只有调用writeHead后，报头才会写入到连接中。除此之外，http模块会自动帮你设置一些头信息，如下所示：

```
< Date: Sat, 06 Apr 2013 08:01:44 GMT
< Connection: keep-alive
< Transfer-Encoding: chunked
<
```
报文体部分则是调用res.write()和res.end()方法实现，后者与前者的差别在于res.end()会先调用write()发送数据，然后发送信号告知服务器这次响应结束，响应结果如下所示：

```
Hello World
```
响应结束后，HTTP服务器可能会将当前的连接用于下一个请求，或者关闭连接。值得注意的是，报头是在报文体发送前发送的，一旦开始了数据的发送，writeHead()和setHeader()将不再生效。这由协议的特性决定。另外，无论服务器端在处理业务逻辑时是否发生异常，务必在结束时调用res.end()结束请求，否则客户端将一直处于等待的状态。当然，也可以通过延迟res.end()的方式实现客户端与服务器端之间的长连接，但结束时务必关闭连接。

3. HTTP服务的事件

如同TCP服务一样，HTTP服务器也抽象了一些事件，以供应用层使用，同样典型的是，服务器也是一个EventEmitter实例。

- connection事件：在开始HTTP请求和响应前，客户端与服务器端需要建立底层的TCP连接，这个连接可能因为开启了keep-alive，可以在多次请求响应之间使用；当这个连接建立时，服务器触发一次connection事件。
- request事件：建立TCP连接后，http模块底层将在数据流中抽象出HTTP请求和HTTP响应，当请求数据发送到服务器端，在解析出HTTP请求头后，将会触发该事件；在res.end()后，TCP连接可能将用于下一次请求响应。
- close事件：与TCP服务器的行为一致，调用server.close()方法停止接受新的连接，当已有的连接都断开时，触发该事件；可以给server.close()传递一个回调函数来快速注册该事件。
- checkContinue事件：某些客户端在发送较大的数据时，并不会将数据直接发送，而是先发送一个头部带Expect: 100-continue的请求到服务器，服务器将会触发checkContinue事件；如果没有为服务器监听这个事件，服务器将会自动响应客户端100 Continue的状态码，表示接受数据上传；如果不接受数据的较多时，响应客户端400 Bad Request拒绝客户端继续发送数据即可。需要注意的是，当该事件发生时不会触发request事件，两个事件之间互斥。当客户端收到100 Continue后重新发起请求时，才会触发request事件。
- connect事件：当客户端发起CONNECT请求时触发，而发起CONNECT请求通常在HTTP代理时出现；如果不监听该事件，发起该请求的连接将会关闭。
- upgrade事件：当客户端要求升级连接的协议时，需要和服务器端协商，客户端会在请求头中带上Upgrade字段，服务器端会在接收到这样的请求时触发该事件。这在后文的WebSocket部分有详细流程的介绍。如果不监听该事件，发起该请求的连接将会关闭。- 
- clientError事件：连接的客户端触发error事件时，这个错误会传递到服务器端，此时触发该事件。

#### HTTP客户端

在对服务器端的实现进行了描述后，HTTP客户端的原理几乎不用再描述，因为它就是服务器端服务模型的另一部分，处于HTTP的另一端，在整个报文的参与中，报文头和报文体由它产生。同时http模块提供了一个底层API：http.request(options, connect)，用于构造HTTP客户端。

下面的示例与上文的curl命令大致相同：

```js
var options = {
  hostname: '127.0.0.1',
  port: 1334,
  path: '/',
  method: 'GET'
};
var req = http.request(options,
function(res) {
  console.log('STATUS: ' + res.statusCode);
  console.log('HEADERS: ' + JSON.stringify(res.headers));
  res.setEncoding('utf8');
  res.on('data',
  function(chunk) {
    console.log(chunk);
  });
});
req.end();
```
执行上述代码得到以下输出：

```
$ node client.js
STATUS: 200 
HEADERS: {
  "date": "Sat, 06 Apr 2013 11:08:01 GMT",
  "connection": "keep-alive",
  "transfer-encoding": "chunked"
}
Hello World
```
其中options参数决定了这个HTTP请求头中的内容，它的选项有如下这些。

- host：服务器的域名或IP地址，默认为localhost。
- hostname：服务器名称。port：服务器端口，默认为80。
- localAddress：建立网络连接的本地网卡。socketPath：Domain套接字路径。
- method：HTTP请求方法，默认为GET。path：请求路径，默认为/。
- headers：请求头对象。auth：Basic认证，这个值将被计算成请求头中的Authorization部分。

报文体的内容由请求对象的write()和end()方法实现：通过write()方法向连接中写入数据，通过end()方法告知报文结束。它与浏览器中的Ajax调用几近相同，Ajax的实质就是一个异步的网络HTTP请求。

1. HTTP响应

HTTP客户端的响应对象与服务器端较为类似，在ClientRequest对象中，它的事件叫做response。ClientRequest在解析响应报文时，一解析完响应头就触发response事件，同时传递一个响应对象以供操作ClientResponse。后续响应报文体以只读流的方式提供，如下所示：

```js
function(res) {
  console.log('STATUS: ' + res.statusCode);
  console.log('HEADERS: ' + JSON.stringify(res.headers));
  res.setEncoding('utf8');
  res.on('data',
  function(chunk) {
    console.log(chunk);
  });
}
```
由于从响应读取数据与服务器端ServerRequest读取数据的行为较为类似，此处不再赘述。

2. HTTP代理

如同服务器端的实现一般，http提供的ClientRequest对象也是基于TCP层实现的，在keepalive的情况下，一个底层会话连接可以多次用于请求。为了重用TCP连接，http模块包含一个默认的客户端代理对象http.globalAgent。它对每个服务器端（host + port）创建的连接进行了管理，默认情况下，通过ClientRequest对象对同一个服务器端发起的HTTP请求最多可以创建5个连接。它的实质是一个连接池，示意图如下图所示。

![HTTP代理对服务器端创建的连接进行管理](http://p9myzkds7.bkt.clouddn.com/Node-network/HTTP%E4%BB%A3%E7%90%86%E5%AF%B9%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E5%88%9B%E5%BB%BA%E7%9A%84%E8%BF%9E%E6%8E%A5%E8%BF%9B%E8%A1%8C%E7%AE%A1%E7%90%86.jpg)

调用HTTP客户端同时对一个服务器发起10次HTTP请求时，其实质只有5个请求处于并发状态，后续的请求需要等待某个请求完成服务后才真正发出。这与浏览器对同一个域名有下载连接数的限制是相同的行为。

如果你在服务器端通过ClientRequest调用网络中的其他HTTP服务，记得关注代理对象对网络请求的限制。一旦请求量过大，连接限制将会限制服务性能。如需要改变，可以在options中传递agent选项。默认情况下，请求会采用全局的代理对象，默认连接数限制的为5。

我们既可以自行构造代理对象，代码如下：

```js
var agent = new http.Agent({
  maxSockets: 10
});
var options = {
  hostname: '127.0.0.1',
  port: 1334,
  path: '/',
  method: 'GET',
  agent: agent
};
```
也可以设置agent选项为false值，以脱离连接池的管理，使得请求不受并发的限制。

Agent对象的sockets和requests属性分别表示当前连接池中使用中的连接数和处于等待状态的请求数，在业务中监视这两个值有助于发现业务状态的繁忙程度。

3. HTTP客户端事件

与服务器端对应的，HTTP客户端也有相应的事件。

- response：与服务器端的request事件对应的客户端在请求发出后得到服务器端响应时，会触发该事件。
- socket：当底层连接池中建立的连接分配给当前请求对象时，触发该事件。
- connect：当客户端向服务器端发起CONNECT请求时，如果服务器端响应了200状态码，客户端将会触发该事件。
- upgrade：客户端向服务器端发起Upgrade请求时，如果服务器端响应了101 Switching Protocols状态，客户端将会触发该事件。
- continue：客户端向服务器端发起Expect: 100-continue头信息，以试图发送较大数据量，如果服务器端响应100 Continue状态，客户端将触发该事件。

### 构建WebSocket服务

提到Node，不能错过的是WebSocket协议。它与Node之间的配合堪称完美，其理由有两条。

- WebSocket客户端基于事件的编程模型与Node中自定义事件相差无几。
- WebSocket实现了客户端与服务器端之间的长连接，而Node事件驱动的方式十分擅长与大量的客户端保持高并发连接。

除此之外，WebSocket与传统HTTP有如下好处。

- 客户端与服务器端只建立一个TCP连接，可以使用更少的连接。
- WebSocket服务器端可以推送数据到客户端，这远比HTTP请求响应模式更灵活、更高效。
- 有更轻量级的协议头，减少数据传送量。

WebSocket最早是作为HTML5重要特性而出现的，最终在W3C和IETF的推动下，形成RFC 6455规范。现代浏览器大多都支持WebSocket协议，接下来我们用一段代码来展现WebSocket在客户端的应用示例：

```js
var socket = new WebSocket('ws://127.0.0.1:12010/updates');
socket.onopen = function() {
  setInterval(function() {
    if (socket.bufferedAmount == 0) 
      socket.send(getUpdateData());
  },50);
};
socket.onmessage = function(event) {
  // TODO：event.data
};  
```
上述代码中，浏览器与服务器端创建WebSocket协议请求，在请求完成后连接打开，每50毫秒向服务器端发送一次数据，同时可以通过onmessage()方法接收服务器端传来的数据。这行为与TCP客户端十分相似，相较于HTTP，它能够双向通信。浏览器一旦能够使用WebSocket，可以想象应用的使用空间极大。

在WebSocket之前，网页客户端与服务器端进行通信最高效的是Comet技术。实现Comet技术的细节是采用长轮询（long-polling）或iframe流。长轮询的原理是客户端向服务器端发起请求，服务器端只在超时或有数据响应时断开连接（res.end()）；客户端在收到数据或者超时后重新发起请求。这个请求行为拖着长长的尾巴，是故用Comet（彗星）来命名它。

使用WebSocket的话，网页客户端只需一个TCP连接即可完成双向通信，在服务器端与客户端频繁通信时，无须频繁断开连接和重发请求。连接可以得到高效应用，编程模型也十分简洁。

前文也或多或少提到了WebSocket与HTTP的区别，相比HTTP，WebSocket更接近于传输层协议，它并没有在HTTP的基础上模拟服务器端的推送，而是在TCP上定义独立的协议。让人迷惑的部分在于WebSocket的握手部分是由HTTP完成的，使人觉得它可能是基于HTTP实现的。

WebSocket协议主要分为两个部分：握手和数据传输。下面我们来详细说一说这两个部分。

#### WebSocket握手

客户端建立连接时，通过HTTP发起请求报文，如下所示：

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
```
与普通的HTTP请求协议略有区别的部分在于如下这些协议头：

```
Upgrade: websocket
Connection: Upgrade
```
上述两个字段表示请求服务器端升级协议为WebSocket。其中Sec-WebSocket-Key用于安全校验：

```
Sec-WebSocket-Key: 
dGhlIHNhbXBsZSBub25jZQ==
```
Sec-WebSocket-Key的值是随机生成的Base64编码的字符串。服务器端接收到之后将其与字符串258EAFA5-E914-47DA-95CA-C5AB0DC85B11相连，形成字符串dGhlIHNhbXBsZSBub25jZQ==258EAFA5-E914-47DA-95CA-C5AB0DC85B11，然后通过sha1安全散列算法计算出结果后，再进行Base64编码，最后返回给客户端。这个算法如下所示：

```js
var crypto = require('crypto');
var val = crypto.createHash('sha1').update(key).digest('base64');
```
另外，下面两个字段指定子协议和版本号：

```
Sec-WebSocket-Protocol: chat, 
superchat
Sec-WebSocket-Version: 13
```
服务器端在处理完请求后，响应如下报文：

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
Sec-WebSocket-Protocol: chat
```
上面的报文告之客户端正在更换协议，更新应用层协议为WebSocket协议，并在当前的套接字连接上应用新协议。剩余的字段分别表示服务器端基于Sec-WebSocket-Key生成的字符串和选中的子协议。客户端将会校验Sec-WebSocket-Accept的值，如果成功，将开始接下来的数据传输。

这里我们用Node模拟浏览器发起协议切换的行为，代码如下：

```js
var WebSocket = function(url) { 
   // 伪代码，解析ws://127.0.0.1:12010/updates，用于请求
  this.options = parseUrl(url);
  this.connect();
};
WebSocket.prototype.onopen = function() {
  // TODO
};
WebSocket.prototype.setSocket = function(socket) {
  this.socket = socket;
};
WebSocket.prototype.connect = function() {
  var this = that;
  var key = new Buffer(this.options.protocolVersion + '-' + Date.now()).toString('base64');
  var shasum = crypto.createHash('sha1');
  var expected = shasum.update(key + '258EAFA5-E914-47DA-95CA-C5AB0DC85B11').digest('base64');
  var options = {
    port: this.options.port,
    // 12010 
    host: this.options.hostname,
    // 127.0.0.1
    headers: {
      'Connection': 'Upgrade',
      'Upgrade': 'websocket',
      'Sec-WebSocket-Version': this.options.protocolVersion,
      'Sec-WebSocket-Key': key
    }
  };
  var req = http.request(options);
  req.end();
  req.on('upgrade',
  function(res, socket, upgradeHead) { // 连接成功
    that.setSocket(socket); // 触发open事件
    that.onopen();
  });
};
```
下面是服务器端的响应行为：

```js
var server = http.createServer(function(req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  });
  res.end('Hello World\n');
});
server.listen(12010);
// 在收到upgrade请求后，告之客户端允许切换协议
server.on('upgrade',
function(req, socket, upgradeHead) {
  var head = new Buffer(upgradeHead.length);
  upgradeHead.copy(head);
  var key = req.headers['sec-websocket-key'];
  var shasum = crypto.createHash('sha1');
  key = shasum.update(key + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11").digest('base64');
  var headers = ['HTTP/1.1 101 Switching Protocols', 'Upgrade: websocket', 'Connection: Upgrade', 'Sec-WebSocket-Accept: ' + key, 'Sec-WebSocket-Protocol: ' + protocol];
  // 让数据立即发送 
  socket.setNoDelay(true);
  socket.write(headers.concat('', '').join('\r\n')); // 建立服务器端WebSocket连接
  var websocket = new WebSocket();
  websocket.setSocket(socket);
});
```
一旦WebSocket握手成功，服务器端与客户端将会呈现对等的效果，都能接收和发送消息。

#### WebSocket数据传输

在握手顺利完成后，当前连接将不再进行HTTP的交互，而是开始WebSocket的数据帧协议，实现客户端与服务器端的数据交换。下图为协议升级过程示意图。

![协议升级过程示意图](http://p9myzkds7.bkt.clouddn.com/Node-network/%E5%8D%8F%E8%AE%AE%E5%8D%87%E7%BA%A7%E8%BF%87%E7%A8%8B%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

握手完成后，客户端的onopen()将会被触发执行，代码如下：

```js
socket.onopen = function () { 
  // TODO: opened()
};
```
服务器端则没有onopen()方法可言。为了完成TCP套接字事件到WebSocket事件的封装，需要在接收数据时进行处理，WebSocket的数据帧协议即是在底层data事件上封装完成的，代码如下：

```js
WebSocket.prototype.setSocket = function(socket) {
  this.socket = socket;
  this.socket.on('data', this.receiver);
};
```
同样的数据发送时，也需要做封装操作，代码如下：

```js
WebSocket.prototype.send = function(data) {
  this._send(data);
};
```
当客户端调用send()发送数据时，服务器端触发onmessage()；当服务器端调用send()发送数据时，客户端的onmessage()触发。当我们调用send()发送一条数据时，协议可能将这个数据封装为一帧或多帧数据，然后逐帧发送。

为了安全考虑，客户端需要对发送的数据帧进行掩码处理，服务器一旦收到无掩码帧（比如中间拦截破坏），连接将关闭。而服务器发送到客户端的数据帧则无须做掩码处理，同样，如果客户端收到带掩码的数据帧，连接也将关闭。

我们以客户端发送hello world!到服务器端，服务器端回以yakexi作为一个流程来研究数据帧协议的实现过程。

下图中为WebSocket数据帧的定义，每8位为一列，也即1个字节。其中每一位都有它的意义。

![WebSocket数据帧的定义](http://p9myzkds7.bkt.clouddn.com/Node-network/WebSocket%E6%95%B0%E6%8D%AE%E5%B8%A7%E7%9A%84%E5%AE%9A%E4%B9%89.jpg)

- fin：如果这个数据帧是最后一帧，这个fin位为1，其余情况为0。当一个数据没有被分为多帧时，它既是第一帧也是最后一帧。
- rsv1、rsv2、rsv3：各为1位长，3个标识用于扩展，当有已协商的扩展时，这些值可能为1，其余情况为0。
- opcode：长为4位的操作码，可以用来表示0到15的值，用于解释当前数据帧。0表示附加数据帧，1表示文本数据帧，2表示二进制数据帧，8表示发送一个连接关闭的数据帧，9表示ping数据帧，10表示pong数据帧，其余值暂时没有定义。ping数据帧和pong数据帧用于心跳检测，当一端发送ping数据帧时，另一端必须发送pong数据帧作为响应，告知对方这一端仍然处于响应状态。
- masked：表示是否进行掩码处理，长度为1。客户端发送给服务器端时为1，服务器端发送给客户端时为0。
- payload length：一个7、7+16或7+64位长的数据位，标识数据的长度，如果值在0~125之间，那么该值就是数据的真实长度；如果值是126，则后面16位的值是数据的真实长度；如果值是127，则后面64位的值是数据的真实长度。
- masking key：当masked为1时存在，是一个32位长的数据位，用于解密数据。
- payload data：我们的目标数据，位数为8的倍数。

客户端发送消息时，需要构造一个或多个数据帧协议报文。由于hello world!较短，不存在分割为多个数据帧的情况，又由于hello world!会以文本的方式发送，它的payload length长度为96（12字节×8位/字节），二进制表示为1100000。所以报文应当如下：

```
fin(1) + res(000) + opcode(0001) + masked(1) + payload 
length(1100000) + masking key(32位) + payload 
data(hello world!加密后的二进制)
```
当以文本方式发送时，文本的编码为UTF-8，由于这里发送的不存在中文，所以一个字符占一个字节，即8位。

客户端发送消息后，服务器端在data事件中接收到这些编码数据，然后解析为相应的数据帧，再以数据帧的格式，通过掩码将真正的数据解密出来，然后触发onmessage()执行，如下所示：

```
socket.onmessage = function (event) { 
  // TODO: event.data 
};
```
服务器端再回复yakexi的时候，剩下的事情就是无须掩码，其余相同，如下所示：

```
fin(1) + res(000) + opcode(0001) + masked(0) + payload 
length(1100000) + payload data(yakexi的二进制)
```
这里的行为与纯TCP连接的行为十分类似，近似地可以理解为TCP客户端套接字的connect事件和data事件。

至此，WebSocket的原理介绍完毕，具体如何解析数据帧和触发onmessage()，请参考ws模块的实现，由于其有过多细节，这里不再展开描述。

#### 小结

在所有的WebSocket服务器端实现中，没有比Node更贴近WebSocket的使用方式了。它们的共性有以下内容。

- 基于事件的编程接口。
- 基于JavaScript，以封装良好的WebSocket实现，API与客户端可以高度相似。

另外，Node基于事件驱动的方式使得它应对WebSocket这类长连接的应用场景可以轻松地处理大量并发请求。尽管Node没有内置WebSocket的库，但是社区的ws模块封装了WebSocket的底层实现。socket.io即是在它的基础上构建实现的。

### 网络安全服务

在网络中，数据在服务器端和客户端之间传递，由于是明文传递的内容，一旦在网络被人监控，数据就可能一览无余地展现在中间的窃听者面前。为此我们需要将数据加密后再进行网络传输，这样即使数据被截获和窃听，窃听者也无法知道数据的真实内容是什么。但是对于我们的应用层协议而言，如HTTP、FTP等，我们仍然希望能够透明地处理数据，而无须操心网络传输过程中的安全问题。在网景公司的NetScape浏览器推出之初就提出了SSL（Secure Sockets Layer，安全套接层）。SSL作为一种安全协议，它在传输层提供对网络连接加密的功能。对于应用层而言，它是透明的，数据在传递到应用层之前就已经完成了加密和解密的过程。最初的SSL应用在Web上，被服务器端和浏览器端同时支持，随后IETF将其标准化，称为TLS（Transport Layer Security，安全传输层协议）。

Node在网络安全上提供了3个模块，分别为crypto、tls、https。其中crypto主要用于加密解密，SHA1、MD5等加密算法都在其中有体现，在这里我们不用再提。真正用于网络的是另外两个模块，tls模块提供了与net模块类似的功能，区别在于它建立在TLS/SSL加密的TCP连接上。对于https而言，它完全与http模块接口一致，区别也仅在于它建立于安全的连接之上。

#### TLS/SSL

1. 密钥

TLS/SSL是一个公钥/私钥的结构，它是一个非对称的结构，每个服务器端和客户端都有自己的公私钥。公钥用来加密要传输的数据，私钥用来解密接收到的数据。公钥和私钥是配对的，通过公钥加密的数据，只有通过私钥才能解密，所以在建立安全传输之前，客户端和服务器端之间需要互换公钥。客户端发送数据时要通过服务器端的公钥进行加密，服务器端发送数据时则需要客户端的公钥进行加密，如此才能完成加密解密的过程，如下图所示。

![客户端和服务器交换密钥](http://p9myzkds7.bkt.clouddn.com/Node-network/%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%92%8C%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%BA%A4%E6%8D%A2%E5%AF%86%E9%92%A5.jpg)

Node在底层采用的是openssl实现TLS/SSL的，为此要生成公钥和私钥可以通过openssl完成。我们分别为服务器端和客户端生成私钥，如下所示：

```js
// 生成服务器端私钥 
$ openssl genrsa -out server.key 1024 
// 生成客户端私钥 
$ openssl genrsa -out client.key 1024
```
上述命令生成了两个1024位长的RSA私钥文件，我们可以通过它继续生成公钥，如下所示：

```
$ openssl rsa -in server.key -pubout -out server.pem 
$ openssl rsa -in client.key -pubout -out client.pem
```
公私钥的非对称加密虽好，但是网络中依然可能存在窃听的情况，典型的例子是中间人攻击。客户端和服务器端在交换公钥的过程中，中间人对客户端扮演服务器端的角色，对服务器端扮演客户端的角色，因此客户端和服务器端几乎感受不到中间人的存在。为了解决这种问题，数据传输过程中还需要对得到的公钥进行认证，以确认得到的公钥是出自目标服务器。如果不能保证这种认证，中间人可能会将伪造的站点响应给用户，从而造成经济损失。下图是中间人攻击的示意图。

![中间人攻击的示意图](http://p9myzkds7.bkt.clouddn.com/Node-network/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

为了解决这个问题，TLS/SSL引入了数字证书来进行认证。与直接用公钥不同，数字证书中包含了服务器的名称和主机名、服务器的公钥、签名颁发机构的名称、来自签名颁发机构的签名。在连接建立前，会通过证书中的签名确认收到的公钥是来自目标服务器的，从而产生信任关系。


2. 数字证书

为了确保我们的数据安全，现在我们引入了一个第三方：CA（Certificate Authority，数字证书认证中心）。CA的作用是为站点颁发证书，且这个证书中具有CA通过自己的公钥和私钥实现的签名。

为了得到签名证书，服务器端需要通过自己的私钥生成CSR（Certificate Signing Request，证书签名请求）文件。CA机构将通过这个文件颁发属于该服务器端的签名证书，只要通过CA机构就能验证证书是否合法。

通过CA机构颁发证书通常是一个烦琐的过程，需要付出一定的精力和费用。对于中小型企业而言，多半是采用自签名证书来构建安全网络的。所谓自签名证书，就是自己扮演CA机构，给自己的服务器端颁发签名证书。以下为生成私钥、生成CSR文件、通过私钥自签名生成证书的过程：

```js
$ openssl genrsa -out ca.key 1024 
$ openssl req -new -key ca.key -out ca.csr 
$ openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
```
其流程如下图所示。

![生成自签名证书示意图](http://p9myzkds7.bkt.clouddn.com/Node-network/%E7%94%9F%E6%88%90%E8%87%AA%E7%AD%BE%E5%90%8D%E8%AF%81%E4%B9%A6%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

上述步骤完成了扮演CA角色需要的文件。接下来回到服务器端，服务器端需要向CA机构申请签名证书。在申请签名证书之前依然是要创建自己的CSR文件。值得注意的是，这个过程中的Common Name要匹配服务器域名，否则在后续的认证过程中会出错。如下是生成CSR文件所用的命令：

```
$ openssl req -new -key 
server.key -out server.csr
```
得到CSR文件后，向我们自己的CA机构申请签名吧。签名过程需要CA的证书和私钥参与，最终颁发一个带有CA签名的证书，如下所示：

```
$ openssl x509 -req -CA ca.crt -
CAkey ca.key -CAcreateserial -in 
server.csr -out server.crt
```
客户端在发起安全连接前会去获取服务器端的证书，并通过CA的证书验证服务器端证书的真伪。除了验证真伪外，通常还含有对服务器名称、IP地址等进行验证的过程。这个验证过程如下图所示。

![客户端通过CA验证服务器端的证书真伪过程示意图](http://p9myzkds7.bkt.clouddn.com/Node-network/%E5%AE%A2%E6%88%B7%E7%AB%AF%E9%80%9A%E8%BF%87CA%E9%AA%8C%E8%AF%81%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E7%9A%84%E8%AF%81%E4%B9%A6%E7%9C%9F%E4%BC%AA%E8%BF%87%E7%A8%8B%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

CA机构将证书颁发给服务器端后，证书在请求的过程中会被发送给客户端，客户端需要通过CA的证书验证真伪。如果是知名的CA机构，它们的证书一般预装在浏览器中。如果是自己扮演CA机构，颁发自有签名证书则不能享受这个福利，客户端需要获取到CA的证书才能进行验证。

上述的过程中可以看出，签名证书是一环一环地颁发的，但是在CA那里的证书是不需要上级证书参与签名的，这个证书我们通常称为根证书。

#### TLS服务

1. 创建服务器端

将构建服务所需要的证书都备齐之后，我们通过Node的tls模块来创建一个安全的TCP服务，这个服务是一个简单的echo服务，代码如下：

```js
var tls = require('tls');
var fs = require('fs');
var options = {
  key: fs.readFileSync('./keys/server.key'),
  cert: fs.readFileSync('./keys/server.crt'),
  requestCert: true,
  ca: [fs.readFileSync('./keys/ca.crt')]
};
var server = tls.createServer(options,function(stream) {
  console.log('server connected', stream.authorized ? 'authorized': 'unauthorized');
  stream.write("welcome!\n");
  stream.setEncoding('utf8');
  stream.pipe(stream);
});
server.listen(8000,
function() {
  console.log('server bound');
});
```
启动上述服务后，通过下面的命令可以测试证书是否正常：

```
$ openssl s_client -connect 127.0.0.1:8000
```
2. TLS客户端

为了完善整个体系，接下来我们用Node来模拟客户端，如同net模块一样，tls模块也提供了connect()方法来构建客户端。在构建我们的客户端之前，需要为客户端生成属于自己的私钥和签名，代码如下：

```
// 创建私钥 
$ openssl genrsa -out client.key 1024 
// 生成CSR 
$ openssl req -new -key client.key -out client.csr 
// 生成签名证书 
$ openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt
```
并创建客户端，代码如下：

```js
var tls = require('tls');
var fs = require('fs');
var options = {
  key: fs.readFileSync('./keys/client.key'),
  cert: fs.readFileSync('./keys/client.crt'),
  ca: [fs.readFileSync('./keys/ca.crt')]
};
var stream = tls.connect(8000, options,function() {
  console.log('client connected', stream.authorized ? 'authorized': 'unauthorized');
  process.stdin.pipe(stream);
});
stream.setEncoding('utf8');
stream.on('data',function(data) {
  console.log(data);
});
stream.on('end',function() {
  server.close();
});
```
启动客户端的过程中，用到了为客户端生成的私钥、证书、CA证书。客户端启动之后可以在输入流中输入数据，服务器端将会回应相同的数据。

至此我们完成了TLS的服务器端和客户端的创建。与普通的TCP服务器端和客户端相比，TLS的服务器端和客户端仅仅只在证书的配置上有差别，其余部分基本相同。

#### HTTPS服务

HTTPS服务就是工作在TLS/SSL上的HTTP。在了解了TLS服务后，创建HTTPS服务是再简单不过的事情。

1. 准备证书

HTTPS服务需要用到私钥和签名证书，我们可以直接用上文生成的私钥和证书。

2. 创建HTTPS服务

创建HTTPS服务只比HTTP服务多一个选项配置，其余地方几乎相同，代码如下：

```js
var https = require('https');
var fs = require('fs');
var options = {
  key: fs.readFileSync('./keys/server.key'),
  cert: fs.readFileSync('./keys/server.crt')
};
https.createServer(options,
function(req, res) {
  res.writeHead(200);
  res.end("hello world\n");
}).listen(8000);
```
启动之后通过curl进行测试，相关代码如下所示：

```
$ curl https://localhost:8000/ curl: (60) SSL certificate problem, verify that the CA cert is OK. Details: 
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed More details here: http://curl.haxx.se/docs/sslcerts.html 
curl performs SSL certificate verification by default, using a "bundle" 
of Certificate Authority (CA) public keys (CA certs). 
If the default bundle file isn't adequate, you can specify an alternate file 
using the --cacert option. 
If this HTTPS server uses a certificate signed by a CA represented in 
the bundle, the certificate verification probably failed due to a problem with the certificate (it might be expired, or the name might 
not match the domain name in the URL). 
If you'd like to turn off curl's verification of the certificate, use 
the -k (or --insecure) option.
```
由于是自签名的证书，curl工具无法验证服务器端证书是否正确，所以出现了上述的抛错，要解决上面的问题有两种方式。一种是加-k选项，让curl工具忽略掉证书的验证，这样的结果是数据依然会通过公钥加密传输，但是无法保证对方是可靠的，会存在中间人攻击的潜在风险。其结果如下所示：

```
$ curl -k 
https://localhost:8000/ 
hello world
```
另一种解决的方式是给curl设置--cacert选项，告知CA证书使之完成对服务器证书的验证，如下所示：

```
$ curl --cacert keys/ca.crt 
https://localhost:8000/ 
hello world
```
3. HTTPS客户端

对应的，我们也会用Node来实现HTTPS的客户端，与HTTP的客户端相差不大，除了指定证书相关的参数外，如下所示：

```js
var https = require('https');
var fs = require('fs');
var options = {
  hostname: 'localhost',
  port: 8000,
  path: '/',
  method: 'GET',
  key: fs.readFileSync('./keys/client.key'),
  cert: fs.readFileSync('./keys/client.crt'),
  ca: [fs.readFileSync('./keys/ca.crt')]
};
options.agent = new https.Agent(options);
var req = https.request(options,function(res) {
  res.setEncoding('utf-8');
  res.on('data',
  function(d) {
    console.log(d);
  });
});
req.end();
req.on('error',function(e) {
  console.log(e);
});
```
执行上面的操作得到以下输出：

```
$ node client.js 
hello world
```
如果不设置ca选项，将会得到如下异常：

```
[Error: UNABLE_TO_VERIFY_LEAF_SIGNATURE]
```
解决该异常的方案是添加选项属性rejectUnauthorized为false，它的效果与curl工具加-k一样，都会在数据传输过程中会加密，但是无法保证服务器端的证书不是伪造的。

### 总结

Node基于事件驱动和非阻塞设计，在分布式环境中尤其能发挥出它的特长，基于事件驱动可以实现与大量的客户端进行连接，非阻塞设计则让它可以更好地提升网络的响应吞吐。Node提供了相对底层的网络调用，以及基于事件的编程接口，使得开发者在这些模块上十分轻松地构建网络应用。下一章我们将在本章的基础上探讨具体的Web应用。