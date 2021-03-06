title: Web前端面试题目及详解汇总-前端相关
author: songxingguo
tags:
  - 找工作
categories:
  - 备忘录
date: 2018-08-06 14:16:00
---
### 什么叫优雅降级和渐进增强？

- **渐进增强**（progressive enhancement）：

  > 针对 **低版本浏览器** 进行 **构建页面** ，保证 **最基本** 的功能，然后再针对 **高级浏览器** 进行 **效果** 、**交互** 等 **改进** 和 **追加功能** 达到 **更好的用户体验** 。
  
<!-- more -->

- **优雅降级**（graceful degradation）：

  > 一开始就构建 **完整** 的 **功能** ，然后再针对 **低版本浏览器** 进行 **兼容** 。

---
- **渐进增强和优雅降级的区别：**

   1. **优雅降级** 是从 **复杂** 的现状 **开始** ，并试图 **减少用户体验** 的供给。

   2. **渐进增强** 则是从一个 **非常基础** 的，**能够起作用** 的 **版本** 开始，并 **不断扩充** ，以 **适应** 未来环境的 **需要** 。

   3.  **降级**（功能衰减）意味着 **往回看** ；而 **渐进增强** 则意味着 **朝前看** ，同时 **保证其根基处于安全地带** 。

来自—— [什么叫优雅降级和渐进增强？]

### 浏览器的内核分别是什么?

> 浏览器的内核是分为两个部分的，一是 **渲染引擎** (layout engineer或Rendering Engine)，另一个是 **JS引擎** 。现在JS引擎比较独立，内核更加倾向于说 **渲染引擎** 。

**渲染引擎**

负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

**JS引擎则** 

 解析和执行javascript来实现网页的动态效果。最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。

- 主流浏览器内核及其内核

  ![主流浏览器内核]

- 五大流浏览器内核及其代表

  - Trident内核

    > 代表作品是 **IE** ，因 **IE捆绑在Windows中** ，所以占有极高的份额，又称为**IE内核** 或 **MSHTML** ，此内核 **只能用于 Windows 平台** ，且 **不是开源的** 。代表作品还有 **腾讯**、**Maxthon**（遨游）、**360浏览器** 等。存在很多的兼容性问题。

  - Gecko内核

    > 代表作品是 **Firefox** （即火狐浏览器）。因火狐是最多的用户，故常被称为  **firefox** 内核,它是 **开源的** ，最大优势是 **跨平台**，在 **Microsoft Windows** 、**Linux** 、**MacOs X** 等主要操作系统中使用。

  - Webkit内核

    > 代表作品是 **Safari**、曾经的 **Chrome**。开源的项目。

  - Presto内核：

    > 代表作品是 **Opera** ，**Presto** 是由 **Opera Software** 开发的 **浏览器排版引擎** ，它是世界公认 **最快的渲染速度的引擎** 。在13年之后，Opera宣布加入谷歌阵营，弃用了 **Presto** 。

  - Blink内核

    > 由 **Google** 和 **Opera Software** 开发的 **浏览器排版引擎** ，2013年4月发布。现在 **Chrome** 内核是 **Blink** 。谷歌还开发了自己的 **JS引擎** ，**V8** ，**使JS运行速度极大地提高了** 。
  
来自—— [浏览器的内核分别是什么？]、[五大流浏览器内核及其代表]

### Web前端性能优化

前端是庞大的，包括 HTML、 CSS、 Javascript、Image 、Flash等等各种各样的资源。前端优化是复杂的，针对方方面面的资源都有不同的方式。

#### 前端优化的目的是什么 ?

1. 从用户角度而言，优化能够**让页面加载得更快**、对用户的操作响应得更及时，能够**给用户提供更为友好的体验**。
2. 从服务商角度而言，优化能够 **减少页面请求数** 、或者 **减小请求所占带宽** ，能够节省可观的资源。

总之，恰当的优化不仅能够改善站点的用户体验并且能够节省相当的资源利用。

大概有如下优化方法：

![优化方法](https://graphbed.qiniu.songxingguo.com/%E4%BC%98%E5%8C%96%E6%96%B9%E6%B3%95.png)

#### 页面级优化

##### 减少 HTTP请求数

- 每个请求都是有成本的，既包含时间成本也包含资源成本。一个完整的请求都需要经过 **DNS寻址、与服务器建立连接、发送数据、等待服务器响应、接收数据这样一个 “漫长” 而复杂的过程** 。

- 时间成本就是用户需要看到或者 “感受” 到这个资源是必须要等待这个过程结束的，资源上由于每个请求都需要携带数据，因此每个请求都需要占用带宽。

- 另外，由于浏览器进行并发请求的请求数是有上限的 (具体参见此处 )，因此请求数多了以后，浏览器需要分批进行请求，因此会增加用户的等待时间，会给用户造成站点速度慢这样一个印象，即使可能用户能看到的第一屏的资源都已经请求完了，但是浏览器的进度条会一直存在。

**减少 HTTP请求数的主要途径包括：**

###### 从设计实现层面简化页面。

保持页面简洁、减少资源的使用时最直接的。

###### 合理设置 HTTP缓存

缓存原则是：**能缓存越多越好，能缓存越久越好**。

>例如，很少变化的图片资源可以直接通过 HTTP Header中的Expires设置一个很长的过期头 ;变化不频繁而又可能会变的资源可以使用 Last-Modifed来做请求验证。尽可能的让资源能够在缓存中待得更久。

###### 资源合并与压缩

- 尽可能的将外部的脚本、样式进行合并，多个合为一个。
- 另外， CSS、 Javascript、Image 都可以用相应的工具进行压缩，压缩后往往能省下不少空间。

###### CSS Sprites

合并 CSS图片，减少请求数的又一个好办法。

###### Inline Images

通过编码的字符串将图片内嵌到网页文本中。

```css
.sample-inline-png {
    padding-left: 20px;
    background: white url('data:image/png;base64,iVBORw0KGgoAA
AANSUhEUgAAABAAAAAQAQMAAAAlPW0iAAAABlBMVEUAAAD///+l2Z/dAAAAM0l
EQVR4nGP4/5/h/1+G/58ZDrAz3D/McH8yw83NDDeNGe4Ug9C9zwz3gVLMDA/A6
P9/AFGGFyjOXZtQAAAAAElFTkSuQmCC') no-repeat scroll left top;
}
```
>如果不考虑资源管理上的问题的话，不失为一个好办法。如果是嵌入页面的话换来的是增大了页面的体积，而且无法利用浏览器缓存。使用在 CSS中的图片则更为理想一些。

###### Lazy Load Images

这条策略实际上并不一定能减少 HTTP请求数，但是却能在某些条件下或者页面刚加载时减少 HTTP请求数。

对于图片而言，在页面刚加载的时候可以只加载第一屏，当用户继续往后滚屏的时候才加载后续的图片。

######  将外部脚本置底（将脚本内容在页面信息内容加载后再加载）

浏览器是可以并发请求的，这一特点使得其能够更快的加载资源，然而外链脚本在加载时却会阻塞其他资源，例如在脚本加载完成之前，它后面的图片、样式以及其他脚本都处于阻塞状态，直到脚本加载完成后才会开始加载。

###### 异步执行 inline脚本(其实原理和上面是一样，保证脚本在页面内容后面加载。)

inline脚本对性能的影响与外部脚本相比，是有过之而无不及。

inline脚本在执行的时候，页面处于空白状态。

鉴于以上两点原因，建议将执行时间较长的 inline脚本异步执行，异步的方式有很多种，例如使用 **script元素的defer 属性** (存在兼容性问题和其他一些问题，例如不能使用 document.write)、**使用setTimeout** ，此外，在HTML5中引入了 **Web Workers的机制** ，恰恰可以解决此类问题。

######  Lazy Load Javascript（只有在需要加载的时候加载，在一般情况下并不加载信息内容。）

一个框架往往包括了很多的功能实现，这些功能并不是每一个页面都需要的，如果下载了不需要的脚本则算得上是一种资源浪费 -既浪费了带宽又浪费了执行花费的时间。

目前的做法大概有两种:

- 一种是为那些流量特别大的页面专门定制一个专用的 mini版框架，另一种则是 Lazy Load。

- YUI 则使用了第二种方式，在 YUI的实现中，最初只加载核心模块，其他模块可以等到需要使用的时候才加载。

######  将 CSS放在 HEAD中

如果将 CSS放在其他地方比如 BODY中，则浏览器有可能还未下载和解析到 CSS就已经开始渲染页面了，这就导致页面由无 CSS状态跳转到 CSS状态，用户体验比较糟糕。

##### 异步请求 Callback（就是将一些行为样式提取出来，慢慢的加载信息的内容）

在某些页面中可能存在这样一种需求，需要使用 script标签来异步的请求数据。

直接在页面上写 `<script>`对页面的性能也是有影响的，即增加了页面首次加载的负担，推迟了 DOMLoaded和window.onload 事件的触发时机。如果时效性允许的话，可以考虑在 DOMLoaded事件触发的时候加载，或者使用 setTimeout方式来灵活的控制加载的时机。

##### 减少不必要的 HTTP跳转

对于以目录形式访问的 HTTP链接，很多人都会忽略链接最后是否带 ’/'，假如你的服务器对此是区别对待的话，那么你也需要注意，这其中很可能隐藏了 301跳转，增加了多余请求。具体参见下图，其中第一个链接是以无 ’/'结尾的方式访问的，于是服务器有了一次跳转。

来自——[常见的web性能优化方法]

[常见的web性能优化方法]:https://blog.csdn.net/daimomo000/article/details/72897436

### 前端网页性能最佳实践

![优化方法](https://graphbed.qiniu.songxingguo.com/%E4%BC%98%E5%8C%96%E6%96%B9%E6%B3%95.png)

#### 网页内容

##### 减少http请求次数

- 资源合并与压缩：将多个脚本文件捆绑成一个文件，将多个样式表文件捆绑成一个文件，以此来减少文件的下载次数。

- CSS Sprites：把多个图片拼成一副图片，然后通过CSS来控制在什么地方具体显示这整张图片的什么位置

-  Image Maps：也是将多幅图拼在一起，然后通过坐标来控制显示导航。

- Inline images: 通过编码的字符串将图片内嵌到网页文本中。

##### 减少DNS查询次数

DNS查询也消耗响应时间，如果我们的网页内容来自各个不同的domain (比如嵌入了开放广告，引用了外部图片或脚本)，那么客户端首次解析这些domain也需要消耗一定的时间。

DNS查询结果缓存在本地系统和浏览器中一段时间，所以 **DNS查询一般是对首次访问响应速度有所影响。**

##### 避免页面跳转

当客户端收到服务器的跳转回复时，客户端再次根据服务器回复中的location指定的地址再次发送请求，例如以下跳转回复。

```
HTTP/1.1 301 Moved Permanently
Location: http://example.com/newuri
Content-Type: text/html
```

##### 缓存Ajax

Ajax可以帮助我们异步的下载网页内容，但是有些网页内容即使是异步的，用户还是在等待它的返回结果。例如ajax的返回是用户联系人的下拉列表。

注意尽量应用以下规则提高ajax的响应速度。

- 添加Expires 或 Cache-Control报文头使回复可以被客户端缓存
- 压缩回复内容
- 减少dns查询
- 精简javascript
- 避免跳转
- 配置Etags

##### 延迟加载

延迟加载需要我们知道我们的网页最初加载需要的最小内容集是什么。剩下的内容就可以推到延迟加载的集合中。

Javascript是典型的可以延迟加载内容。

一个比较激进的做法是开发网页时先确保网页在没有Javascript的时候也可以基本工作，然后通过延迟加载脚本来完成一些高级的功能。

##### 提前加载

与延迟加载目的相反，提前加载的是为了提前加载接下来网页中访问的资源。

无条件提前加载：当前网页加载完成后，马上去下载一些其他的内容。

有条件加载：根据用户的输入推断需要加载的内容。

有预期的的加载：这种情况一般发生在网页重新设计时，由于用户经常访问旧网页，本地对旧的网页内容缓存充分从而显得旧网页速度很快，而新的网页内容却没有缓存，设计者可以在旧网页的内容中预先加载一些新网页中可能用到的内容，这样新的网页就会生下来一些需要下载的资源。

##### 减少DOM元素数量

网页中元素过多对网页的加载和脚本的执行都是沉重的负担。

想知道你的网页中有多少元素，通过在浏览器中的一条简单命令就可以算出，

```js
document.getElementsByTagName('*').length
```

##### 根据域名划分内容

**浏览器一般对同一个域的下载连接数有所限制** ，**按照域名划分下载内容可以浏览器增大并行下载连接** ，但是 **注意控制域名使用在2-4个之间，不然dns查询也是个问题。**

一般网站规划会将静态资源放在类似于static.example.com，动态内容放在www.example.com上。这样做还有一个好处是可以在静态的域名上避免使用cookie。

##### 减少iframe数量

优点

- 可以用来加载速度较慢的内容，例如广告。
- 安全沙箱保护。浏览器会对iframe中的内容进行安全控制。
- 脚本可以并行下载

缺点

- 即使iframe内容为空也消耗加载时间
- 会阻止页面加载
- 没有语义

##### 避免404

404我们都不陌生，代表服务器没有找到资源。

我们要特别要注意404的情况不要在我们提供的网页资源上，客户端发送一个请求但是服务器却返回一个无用的结果，时间浪费掉了。

更糟糕的是我们网页中需要加载一个外部脚本，结果返回一个404，不仅阻塞了其他脚本下载，下载回来的内容(404)客户端还会将其当成Javascript去解析。

#### 服务器

##### 使用CDN

再次强调第一条黄金定律，减少网页内容的下载时间。

提高下载速度还可以通过CDN(内容分发网络)来提升。CDN通过部署在不同地区的服务器来提高客户的下载速度。如果你的网站上有大量的静态内容，世界各地的用户都在访问，我说的是youtube么？那CDN是必不可少的。

##### 添加Expires 或Cache-Control报文头

- 对于静态内容添加Expires，将静态内容设为永不过期，或者很长时间以后。

- 对于动态内容应用合适的Cache-Control，让浏览器根据条件来发送请求。

##### Gzip压缩传输文件

Gzip通常可以减少70%网页内容的大小，包括脚本、样式表、图片等文件。Gzip比deflate更高效，主流服务器都有相应的压缩支持模块。

>值得注意的是pdf文件可以从需要被压缩的类型中剔除，因为pdf文件本身已经压缩，gzip对其效果不大，而且会浪费CPU。

##### 配置ETags

虽然标题叫配制ETags，但是这里你要根据具体情况进行一些判断。首先Etag简单来说是通过一个文件版本标识使得服务器可以轻松判断该请求的内容是否有所更新，如果没有就回复304 (not modified)，从而避免下载整个文件。

如果你遇到这样的问题，IIS 7中可以通过如下方法将Etag去掉，使用URL Rewrite，然后在web.config中添加如下配制:

```html
<rewrite>
   <outboundRules>
      <rule name="Remove ETag">
         <match serverVariable="RESPONSE_ETag" pattern=".+" />
         <action type="Rewrite" value="" />
      </rule>
   </outboundRules>
</rewrite>
```
IIS8里提供了一个简单配制来直接关闭Etag：

```html
<element name="clientCache">
   <attribute name="cacheControlMode" type="enum" defaultValue="NoControl">
          <enum name="NoControl" value="0" />
          <enum name="DisableCache" value="1" />
          <enum name="UseMaxAge" value="2" />
          <enum name="UseExpires" value="3" />
  </attribute>
  <attribute name="cacheControlMaxAge" type="timeSpan" defaultValue="1.00:00:00" />
  <attribute name="httpExpires" type="string" />
  <attribute name="cacheControlCustom" type="string" />
  
<attribute name="setEtag" type="bool" defaultValue="false" />
</element>
```

##### 尽早flush输出

网页后台程序中我们知道有个方法叫Response.Flush()，一般我们调用它都是在程序末尾，但注意这个方法可以被调用多次。目的是可以将现有的缓存中的回复内容先发给客户端，让客户端“有活干”。

那在什么时候调用这个方法比较好呢？**一般情况下我们可以在对于需要加载比较多外部脚本或者样式表时可以提前调用一次** ，客户端收到了关于脚本或其他外部资源的链接可以并行的先发请求去下载，服务器接下来把后续的处理结果发给客户端。

##### 使用GET Ajax请求

浏览器在实现XMLHttpRequest POST的时候分成两步，先发header，然后发送数据。而GET却可以用一个TCP报文完成请求。另外GET从语义上来讲是去服务器取数据，而POST则是向服务器发送数据，所以我们 **使用Ajax请求数据的时候尽量通过GET来完成** 。

##### 避免空的图片src

空的图片src仍然会使浏览器发送请求到服务器，这样完全是浪费时间，而且浪费服务器的资源。尤其是你的网站每天被很多人访问的时候，这种空请求造成的伤害不容忽略。

浏览器如此实现也是根据RFC 3986 - Uniform Resource Identifiers标准，空的src被定义为当前页面。

所以注意我们的网页中是否存在这样的代码

straight HTML 

```html
<img src="">
```
JavaScript 

```js
var img = new Image(); 
img.src = "";
```
#### Cookie

##### 减少Cookie大小

Cookie被用来做认证或个性化设置，其信息被包含在http报文头中，对于cookie我们要注意以下几点，来提高请求的响应速度，

- 去除没有必要的cookie，如果网页不需要cookie就完全禁掉。
- 将cookie的大小减到最小
- 注意cookie设置的domain级别，没有必要情况下不要影响到sub-domain
- 设置合适的过期时间，比较长的过期时间可以提高响应速度。

##### 页面内容使用无cookie域名

**大多数网站的静态资源都没必要cookie，我们可以采用不同的domain来单独存放这些静态文件** ，这样做不仅可以减少cookie大小从而提高响应速度，还有一个好处是有些proxy拒绝缓存带有cookie的内容，如果能将这些静态资源cookie去除，那就可以得到这些proxy的缓存支持。

常见的划分domain的方式是将静态文件放在static.example.com，动态内容放在www.example.com。

也有一些网站需要在二级域名上应用cookie，所有的子域都会继承，这种情况下一般会再购买一个专门的域名来存放cookie-free的静态资源。例如Yahoo!的yimg.com，YouTube的ytimg.com等。

#### CSS

##### 将样式表置顶

经样式表(css)放在网页的HEAD中会让网页显得加载速度更快，因为这样做可以使浏览器逐步加载已将下载的网页内容。

##### 避免CSS表达式

CSS表达式可以动态的设置CSS属性，在IE5-IE8中支持，其他浏览器中表达式会被忽略。

CSS表达式的问题在于它被重新计算的次数远比我们想象的要多，不仅在网页绘制或大小改变时计算，即使我们滚动屏幕或者移动鼠标的时候也在计算，因此我们还是尽量避免使用它来防止使用不当而造成的性能损耗。

如果想达到类似的效果我们可以通过简单的脚本做到。

##### 用`<link>`代替@import

避免使用@import的原因很简单，因为它相当于将css放在网页内容底部。

##### 避免使用Filters

滤镜的使用会导致图片在下载的时候阻塞网页绘制，另外使用这种滤镜会导致内存使用量的问题。

#### Javascript

##### 将脚本置底

因此对于脚本提速，我们可以考虑以下方式，

- 把脚本置底，这样可以让网页渲染所需要的内容尽快加载显示给用户。
- 现在主流浏览器都支持defer关键字，可以指定脚本在文档加载后执行。
- HTML5中新加了async关键字，可以让脚本异步执行。

##### 使用外部Javascirpt和CSS文件

- 使用外部Javascript和CSS文件可以使这些文件被浏览器缓存，从而在不同的请求内容之间重用。

- 同时将Javascript和CSS从inline变为external也减小了网页内容的大小。

##### 精简Javascript和CSS

精简就是将Javascript或CSS中的空格和注释全去掉。

JS compressors:

- Packer
- JSMin
- Closure compiler
- YUICompressor (also does CSS)
- AjaxMin (also does CSS)

CSS compressors:

- CSSTidy
- Minify
- YUICompressor (also does JS)
- AjaxMin (also does JS)
- CSSCompressor

与VS集成比较好的工具如下.

- YUICompressor - 编译集成，包含在NuGet.
- AjaxMin  - 编译集成

##### 去除重复脚本

重复的脚本不仅浪费浏览器的下载时间，而且浪费解析和执行时间。一般用来避免引入重复脚本的做法是使用统一的脚本管理模块，这样不仅可以避免重复脚本引入，还可以兼顾脚本依赖管理和版本管理。

##### 减少DOM访问

通过Javascript访问DOM元素没有我们想象中快，元素多的网页尤其慢，对于Javascript对DOM的访问我们要注意

- 缓存已经访问过的元素
- ffline更新节点然后再加回DOM Tree
- 避免通过Javascript修复layout

##### 使用智能事件处理

通过不同的方式尽量少去触发事件，如果必要就尽早的去处理事件。

#### 图片

##### 优化图像

当美工完成了网站的图片设计后，我们可以在上传图片之前对其做以下优化

- 检查GIF图片中图像颜色的数量是否和调色板规格一致。如果你发现图片中只用到了4种颜色，而在调色板的中显示的256色的颜色槽，那么这张图片就还有压缩的空间。可以使用imagemagick检查： 

  ```
  identify -verbose image.gif
  ```
- 尝试把GIF格式转换成PNG格式，看看是否节省空间。大多数情况下是可以压缩的。下面这条简单的命令可以安全地把GIF格式转换为PNG格式： 

  ```
  convert image.gif image.png
  ```
- 在所有的PNG图片上运行pngcrush（或者其它PNG优化工具）。例如：

  ```
  pngcrush image.png -rem alla -reduce -brute result.png
  ```
- 在所有的JPEG图片上运行jpegtran。这个工具可以对图片中的出现的锯齿等做无损操作，同时它还可以用于优化和清除图片中的注释以及其它无用信息 

  ```
  jpegtran -copy none -optimize -perfect src.jpg dest.jpg
  ```
##### 优化CSS Sprite

- Spirite中水平排列图片，垂直排列会增加文件大小；
- Spirite中把颜色较近的组合在一起可以降低颜色数，理想状况是低于256色以便适用PNG8格式；
- 不要在Spirite的图像中间留有较大空隙。这虽然不大会增加文件大小,但对于用户代理来说它需要更少的内存来把图片解压为像素地图。100×100的图片为1万像素，1000×1000就是100万像素。

##### 不要在HTML中缩放图片

不要通过图片缩放来适应页面，如果你需要小图片，就直接使用小图片吧。

##### 使用小且可缓存的favicon.ico

网站图标文件favicon.ico，不管你服务器有还是没有，浏览器都会去尝试请求这个图标。所以我们要确保这个图标

- 存在
- 文件尽量小，最好小于1k
- 设置一个长的过期时间

#### 移动客户端

##### 保持单个内容小于25KB

这限制是因为iphone，他只能缓存小于25K，注意这是解压后的大小。所以单纯gzip不一定够用，精简文件工具要用上了。

##### 打包组建成符合文档

把页面内容打包成复合文本就如同带有多附件的Email，它能够使你在一个HTTP请求中取得多个组建。当你使用这条规则时，首先要确定用户代理是否支持（iPhone不支持）。

来自——[毫秒必争，前端网页性能最佳实践]

[毫秒必争，前端网页性能最佳实践]:http://www.cnblogs.com/developersupport/p/webpage-performance-best-practices.html

### Webpack 

[入门 Webpack，看这篇就够了](https://segmentfault.com/a/1190000006178770)

[Webpack是什么](https://www.cnblogs.com/wymbk/p/6172208.html)

[Webpack 配置详解（含 4）——关注细节](https://segmentfault.com/a/1190000014685887)

[Webpack基本配置介绍](https://blog.csdn.net/mutouafangzi/article/details/79377917)

#### loader 和 plugins

Loaders是webpack提供的最激动人心的功能之一了。通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理。

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。

#### 打包流程

[细说webpack之流程篇](https://www.cnblogs.com/yxy99/p/5852987.html)

[Webpack打包流程细节源码解析（P2）](https://www.jianshu.com/p/05222b9f8a8b)

#### Webpack 4.0 （简化配置）

[Webpack 4.0发布了！！](https://www.jianshu.com/p/3a13f1b37300)

[Webpack4.0初体验](https://blog.csdn.net/liwusen/article/details/79424118)

### MCV和MVM 
### jQuery extend fn
### SEO 搜索引擎优化、爬虫

### 常见调试方法

### 什么是线程？进程和线程的关系是什么？

### 怎么样从web前端方面优化性能？至少列举5点？

###  如果制作一个访问量很大的网站，对css，js和图片应该怎么处理?

### 31.写出几种IE6 BUG的解决方法？

### 浏览器标准模式和怪异模式之间的区别是什么？

### 你如何对网站的文件和资源进行优化？期待的解决方案包括：

### 经常遇到的浏览器的兼容性有哪些？原因，解决方法是什么，常用hack的技巧？6、前台兼容性问题有哪些？

### 平时如何管理项目？

### 工作中用过什么构建工具？

### 谈谈你对模块化的理解？

### 平时都用什么第三方框架？

### 10、谈谈你对预加载的理解？
11、缓存和预加载的区别是什么？
9、如何使用缓存？

### 12、图片如何压缩？

### 13、压缩文件有哪些方法？

### 14、如何区分静态页面和动态页面？


### 18、内存泄漏怎么理解？

### 19、微格式到底是做啥用？


### 、如何缓存整个页面，在没有网络的时候可以来回的跳转？（访问缓存）

### 22、CDN是啥？

### 23、浏览器一次可以从一个域名下请求多少资源？

### 24、什么是垃圾回收机制（GC）？
36、请描述你熟悉的语言的垃圾回收(GC)机制，他们对循环引用是如何处理的？如何查找内存泄漏(MemoryLeak)?

### 25、image和canvas在处理图片的时候有什么区别？

image是通过对象的形式描述图片的

canvas通过专门的API将图片绘制在画布上

### 26、简述移动开发的注意点,如何做好不同手机的适配,你以前的项目是怎么做的?


### 35、设计模式有哪些？列举你在前端开发工作中自己应用到或者了解到其他框架所用到的设计模式？

单例、工厂、观察者、适配器、代理模式

### 19、什么是MVVM框架？
[来源](https://www.jianshu.com/p/0e9a0d460f64)

### 调试按钮

F9   在你需要停下的地方设置断点 
F5   进入调试 
F10  单步运行 
F11  进入函数


[同步请求和异步请求的区别]: https://blog.csdn.net/goodshot/article/details/7244053
[简述同步和异步的区别]:https://www.cnblogs.com/Renyi-Fan/p/7808756.html#_label0_5
[阻塞与非阻塞的区别]:https://www.cnblogs.com/orez88/articles/2513460.html
[HTML文档的根元素是 html 元素]:https://blog.csdn.net/ixygj197875/article/details/79737953
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
