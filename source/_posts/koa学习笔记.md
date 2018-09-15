title: koa学习笔记
author: songxingguo
tags:
  - koa
categories:
  - 后端技术
date: 2018-06-30 08:09:00
---
## 安装

> Koa 依赖 node v7.6.0 或 ES2015及更高版本和 async 方法支持.

你可以使用自己喜欢的版本管理器快速安装支持的 node 版本：

```
 nvm install 7
 npm i koa
 node app.js
```

<!-- more -->

### 使用 Babel 实现 Async 方法

  > 要在 node < 7.6 版本的 Koa 中使用 async 方法, 我们推荐使用 [babel's require hook](https://babel.bootcss.com/docs/usage/babel-register/).
  
  ```
  require('babel-register');
  // 应用的其余 require 需要被放到 hook 后面
  const app = require('./app');
  ```
  
  > 要解析和编译 async 方法, 你至少应该有 [transform-async-to-generator](https://babel.bootcss.com/docs/plugins/transform-async-to-generator/) 或 [transform-async-to-module-method](https://babel.bootcss.com/docs/plugins/transform-async-to-module-method/) 插件.
  
  新建 .babelrc 文件：
  
  ```
  {
    "plugins": ["transform-async-to-generator"]
  }
  ```
  
  你也可以用 [env preset](https://babel.bootcss.com/docs/plugins/preset-env/) 的 target 参数 "node": "current" 替代.

## 应用程序

> Koa 应用程序是 **一个包含一组中间件函数的对象** ，它是 **按照类似堆栈的方式组织和执行的** 。 Koa 类似于你可能遇到过的许多其他中间件系统，例如 Ruby 的 Rack ，Connect 等，然而，一个关键的设计点是 **在其低级中间件层中提供高级“语法糖”** 。 这提高了 **互操作性** ，**稳健性** ，并使 **书写中间件更加愉快** 。
这包括诸如 **内容协商** ，**缓存清理** ，**代理支持** 和 **重定向** 等常见任务的方法。 尽管 **提供了相当多的有用的方法 Koa 仍保持了一个很小的体积** ，因为没有捆绑中间件。

第一个Koa应用 hello world :

```
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

### 级联

 > Koa 中间件以更传统的方式级联，您可能习惯使用类似的工具 - 之前难以让用户友好地使用 node 的回调。然而，使用 async 功能，我们可以实现 “真实” 的中间件。对比 Connect 的实现，通过一系列功能直接传递控制，直到一个返回，Koa 调用“下游”，然后控制流回“上游”。
 
 下面以 “Hello World” 的响应作为示例，当请求开始时首先 **请求流** 通过 **x-response-time** 和 **logging**  中间件，然后继续移交控制给 **response 中间件** 。 当一个中间件调用 **next() ** 则该函数 **暂停并将控制传递给定义的下一个中间件** 。 当在下游没有更多的中间件执行后，**堆栈将展开并且每个中间件恢复执行其上游行为** 。
 
 ```
  const Koa = require('koa');
  const app = new Koa();

  // logger

  app.use(async (ctx, next) => {
    await next();
    const rt = ctx.response.get('X-Response-Time');
    console.log(`${ctx.method} ${ctx.url} - ${rt}`);
  });

  // x-response-time

  app.use(async (ctx, next) => {
    const start = Date.now();
    await next();
    const ms = Date.now() - start;
    ctx.set('X-Response-Time', `${ms}ms`);
  });

  // response

  app.use(async ctx => {
    ctx.body = 'Hello World';
  });

  app.listen(3000);
 ```
 
### 设置


应用程序设置是 app 实例上的属性，目前支持如下：

app.env 默认是 NODE_ENV 或 "development"
app.proxy 当真正的代理头字段将被信任时
app.subdomainOffset 对于要忽略的 .subdomains 偏移[2]

#### app.listen(...)

Koa 应用程序不是 HTTP 服务器的1对1展现。 可以将一个或多个 Koa 应用程序安装在一起以形成具有单个HTTP服务器的更大应用程序。

创建并返回 HTTP 服务器，将给定的参数传递给 Server#listen()。这些内容都记录在 nodejs.org.

以下是一个无作用的 Koa 应用程序被绑定到 3000 端口：

```
const Koa = require('koa');
const app = new Koa();
app.listen(3000);
```
这里的 app.listen(...) 方法只是以下方法的语法糖:

```
const http = require('http');
const Koa = require('koa');
const app = new Koa();
http.createServer(app.callback()).listen(3000);
```
这意味着您可以将同一个应用程序同时作为 HTTP 和 HTTPS 或多个地址：

```
const http = require('http');
const https = require('https');
const Koa = require('koa');
const app = new Koa();
http.createServer(app.callback()).listen(3000);
https.createServer(app.callback()).listen(3001);
```

#### app.callback()

返回适用于 http.createServer() 方法的回调函数来处理请求。你也可以使用此回调函数将 koa 应用程序挂载到 Connect/Express 应用程序中。

#### app.use(function)

将给定的中间件方法添加到此应用程序。参阅 [Middleware](https://github.com/koajs/koa/wiki#middleware) 获取更多信息.

app.keys=
设置签名的 Cookie 密钥。

这些被传递给 [KeyGrip](https://github.com/crypto-utils/keygrip)，但是你也可以传递你自己的 KeyGrip 实例。

例如，以下是可以接受的：

```
app.keys = ['im a newer secret', 'i like turtle'];
app.keys = new KeyGrip(['im a newer secret', 'i like turtle'], 'sha256');
```
这些密钥可以倒换，并在使用 { signed: true } 参数签名 Cookie 时使用。

```
ctx.cookies.set('name', 'tobi', { signed: true });
```

#### app.context

app.context 是从其创建 ctx 的原型。您可以通过编辑 app.context 为 ctx 添加其他属性。这对于将 ctx 添加到整个应用程序中使用的属性或方法非常有用，这可能会更加有效（不需要中间件）和/或 更简单（更少的 require()），而更多地依赖于ctx，这可以被认为是一种反模式。

例如，要从 ctx 添加对数据库的引用：

```
app.context.db = db();

app.use(async ctx => {
  console.log(ctx.db);
});
```

> **注意:** 
ctx 上的许多属性都是使用 getter ，setter 和 Object.defineProperty() 定义的。你只能通过在 app.context 上使用 Object.defineProperty() 来编辑这些属性（不推荐）。查阅  https://github.com/koajs/koa/issues/652.
安装的应用程序目前使用其父级的 ctx 和设置。 因此，安装的应用程序只是一组中间件。

#### 错误处理

默认情况下，将所有错误输出到 stderr，除非 app.silent 为 true。 当 err.status 是 404 或 err.expose 是 true 时默认错误处理程序也不会输出错误。 要执行自定义错误处理逻辑，如集中式日志记录，您可以添加一个 “error” 事件侦听器：

```
app.on('error', err => {
  log.error('server error', err)
});
```
如果 req/res 期间出现错误，并且 _无法_ 响应客户端，Context实例仍然被传递：

```
app.on('error', (err, ctx) => {
  log.error('server error', err, ctx)
});
```
当发生错误 _并且_ 仍然可以响应客户端时，也没有数据被写入 socket 中，Koa 将用一个 500 “内部服务器错误” 进行适当的响应。在任一情况下，为了记录目的，都会发出应用级 “错误”。