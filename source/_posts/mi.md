title: 小米面试准备
author: songxingguo
tags: 
  - 找工作
categories:
  - 备忘录
date: 2018-10-16 08:54:00
---
# 用肯定自信的方式回答问题

尽可能的把自己知道的东西讲出来，尽可能的多表达。

尽最大努力去争取许。

## 一面

### 为什么来小米

#### 喜欢小米的产品，小米的产品都性价比很高。

#### 我很喜欢小米的 MIUI 系统，设计很人性化，比如：短信的验证码复制功能，动态音乐闹钟。

#### 我是一个米粉，我用过手环、小爱同学、耳机、手机，看小米的新平发布会。

#### 我看过《雷军传》、《互联网+：小米案例》，对雷总的经历很佩服，对小米的创立也很佩服，很想加入加入小米，也能做出有我参与的人性化产品。

<!-- more -->

### 为什么要做前端

  喜欢前端、可以展示自己、能将学到的东西学以致用，并能马上看到效果，很有成就感。

  对前端感兴趣。

  我很喜欢写博客，将重要的知识点记录下来，问题记录下来、或者自己的想法记录下来。

### 简单的自我介绍一下吧

我叫宋兴国，来自重庆理工大学软件工程系，家是重庆永川的。在2016年7月到2018年2月期间加入了专业实验室、曾做过一个以AngualrJS为前端框架的新能源物流车综合运营平台项目，离开实验室之后自己搭建了一个个人博客、独立设计和开发了一款微信小程序、以及了解了一下前端的框架 React、Angualr，打包工具 Webpack、ES6、Node（koa）。总的来说，具有一定的项目经验，属性项目开发的整个流程、具有一定的自学能力，以及对前端新技术有一定的敏感度。喜欢电影，打羽毛球。

很喜欢小米，是小米的忠实米粉。

我有什么需要提高、最希望提升的特质？ （比如：经验还不够丰富）。

说一说你做过的印象深刻的项目（着重说一个，你是怎么思考的，怎么解决了问题的，事后如何复盘的）。

优点：够努力。
缺点：犹豫、不够坚定。

熟悉HTML、CSS、JavaScript。

### 笔试题怎么想的

```
var fib = function(num) {
  if (num == 1 || num == 2) {
    return 1;
  } else {
    return arguments.callee(num - 1) + arguments.callee(num - 2);
  }
}

//		    var getRow = function(num) {
//		    	var  n = 1;
//		    	while(sumFib(n) <= num) {
//		    		n++;
//		    	}
//		    	return sumFib(n - 1);
//		    }
var sumFib = function(n) {
  var sum = 0;
  for (var i = 0; i < n; i++) {
    sum += fib(i);
  }
  return sum;
}

console.log(sumFib(2));
//          var  n = getRow(6);
//          console.log(n);
//		    console.log(sumFib(5));
```

### 正则表达式

[正则表达式](http://www.runoob.com/regexp/regexp-syntax.html)

[JavaScript 正则表达式](http://www.runoob.com/js/js-regexp.html)

[正则表达式基本语法](https://www.cnblogs.com/ldq2016/p/5528177.html)

[正则表达式简单语法及常用正则表达式](https://blog.csdn.net/u010760374/article/details/79974586)

### html5用过哪些标签？

header、footer、main、nav、video 、audio、canvas、section、article

### css3用过什么？

2D/3D转换、动画、Flex布局

### css选择器

元素选择器、ID选择器、类选择器、后代选择器、属性选择器、子元素选择器()、相邻兄弟选择器、[伪类](http://www.w3school.com.cn/css/css_pseudo_classes.asp)、伪元素(CSS 伪元素用于向某些选择器设置特殊效果。)

在支持 CSS 的浏览器中，链接的不同状态都可以不同的方式显示，这些状态包括：活动状态，已被访问状态，未被访问状态，和鼠标悬停状态。

提示：在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

提示：在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

提示：伪类名称对大小写不敏感。

[CSS3 选择器](http://www.w3school.com.cn/cssref/css_selectors.asp)

### webstorage和cookie有什么区别？

![session和cookie之间的区别](https://graphbed.qiniu.songxingguo.com/web-interview/cookie%E3%80%81sessionStorage%E4%B8%8ElacalStrage%E7%9A%84%E5%8C%BA%E5%88%AB.png)

[session和cookie之间的区别](https://www.songxingguo.com/2018/08/06/web-front-end-interview-network/)

### 你知道有哪些js的继承方式

JavaScript 主要通过原型链实现继承。原型链的构建是通过将一个类型的实例赋值给另一个构造函数的原型实现的。这样，子类型就能够访问超类型的所有属性和方法，这一点与基于类的继承很相似。原型链的问题是 **对对象实例共享所有继承的属性和方法**  ，因此 **不适宜单独使用** 。解决这个问题的技术是 **借用构造函数** ，即 **在子类型构造函数的内部调用超类型构造函数** 。这样就 **可以做到每个实例都具有自己的属性，同时还能保证只使用构造函数模式来定义类型** 。使用最多的继承模式是 **组合继承** ，这种模式 **使用原型链继承共享的属性和方法** ，而 **通过借用构造函数继承实例属性** 。

此外，还存在下列可供选择的继承模式。

**原型式继承** ，可以在不必预先定义构造函数的情况下实现继承，其本质是 **执行对给定对象的浅复制** 。而复制得到的副本还可以得到进一步改造。

**寄生式继承** ，与 **原型式继承非常相似，也是基于某个对象或某些信息创建一个对象，然后增强对象，最后返回对象** 。为了解决组合继承模式由于多次调用超类型构造函数而导致的低效率问题，可以将这个模式与组合继承一起使用。

**寄生组合式继承** ，**集寄生式继承和组合继承优点与一身** ，，是 **实现基于类型继承的最有效方式** 。

### git pull 和 git fetch区别

1. git fetch：相当于是从远程获取最新版本到本地，不会自动merge

2. git pull：相当于是从远程获取最新版本并merge到本地

   上述命令其实相当于git fetch 和 git merge

   在实际使用中，git fetch更安全一些

   因为在merge前，我们可以查看更新情况，然后再决定是否合并

### 设计一个团队开发流程的git工作流，要求每天发布一个版本,重点就是发布到主分支还是每天的分支上，应该是发布到主分支

![常用 命令流程图](https://graphbed.qiniu.songxingguo.com/mi/git%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

![发布流程](https://graphbed.qiniu.songxingguo.com/mi/%E5%8F%91%E5%B8%83%E6%B5%81%E7%A8%8B.png)

### this的描述和绑定规则，我回答的是按照《你不知道的js》

- this永远指向最后调用它的上级对象。

- New 关键字可以改变 this 指向。

- 自行改变 this 指向 call()、apply()、bind()。

- 返回值是对象，this 指向对象，否则指向函数实例（null 也为对象）。

- ES6 中箭头函数中 this 指向定义时所在对象。

严格模式中默认的 this 不再是 window，而是 undefined。

### 函数作用域，有哪些改变方式apply call，怎样不在运行时才决定this（用箭头函数）

每个函数都包含两个非继承而来的方法： apply() 和 call() 。这两个方法的用途都是 在特定的作用域中调用函数 ，实际上等于 设置函数体内 this 对象的值 。首先， apply() 方法 接收两个参数：一个是 在其中 **运行函数的作用域**  ，另一个是 参数数组 。其中，第二个参数可以是 **Array 的实例** ，也可以是 **arguments 对象** 。

call() 方法 与 apply() 方法的作用相同，它们的区别仅在于接收参数的方式不同。对于 call()方法而言，第一个参数是 this 值没有变化 ，变化的是其余参数都直接传递给函数 。换句话说，在使用 call() 方法时，**传递给函数的参数必须逐个列举出来**。 

使用 call() （或 apply() ）来扩充作用域的最大好处，就是 对象不需要与方法有任何耦合关系 。

ECMAScript 5 还定义了一个方法： bind() 。这个方法会 创建一个函数的实例 ，其 this 值会被绑定到传给 bind() 函数的值 。

### es6了解吗？了解一些

[去哪儿网面试准备](https://www.songxingguo.com/2018/10/10/qunar/)

### 说一说你觉得es6高效好用的地方？promise

箭头函数（简化函数定义）、扩展运算符（减少传参）、结构运算符、模块化（import、export、default export）、promise（解决了回调地域）

### promise有几种状态？分别是什么？

```js
function getProductDetails() {
  setTimeout(function(){
    console.log('Getting customers');
    setTimeout(function(){
      console.log('Getting orders');
      setTimeout(function() {
        console.log('Getting products');
        setTimeout(function() {
          console.log('Getting products details');
        }, 1000);
      }, 1000);
    }, 1000);
    
  }, 1000);
}

getProductDetails();
```

ES6引入了promise，在 保持与回调相同功能 的同时，**消除回调嵌套并令代码更易阅读** 。Promise对象 等待并监听异步操作的结果 ，通知代码执行是否执行成功或失败 ,以便能够相应地处理下一步操作。Promise对象 表示一个未来结果的操作 ，可能是以下状态之一：

- Fulfilled: 操作成功完成。
- Rejected: 操作失败并返回一个错误。
- Pending: 操作正在处理中，既没有fulfilled,也没有rejected。


#### promise.then返回的是什么？

Deferred对象的用途是与Promise对象相关联，提供一些API处理成功或失败的任务状态，具有如下方法和属性：

- resolve(value)：用于标识当前任务状态成功，可以继续链式调用then方法。
- reject(reason)：用于标识当前任务状态失败，会跳转到catch方法，类似于编程语言中的try…catch语法。
- notify(value)：用于更新Promise的执行状态，在Promise被处理或被拒绝之前可能会被调用多次。
- promise - {Promise}：和Deferred对象相关联的Promise对象。

Promise对象具有then、catch、finally三个方法，每个方法的调用都返回一个Promise对象，这为链式调用提供了支撑，finally方法和编程语言中的try…catch…finally语句类似，一定会被执行到。

### promise 原理


### 箭头函数用过吗？用过,箭头函数的优点是什么？

- 为匿名函数提供了更短的写法。

- 为this变量添加了词法作用域。

### for-of 循环

- every() ：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true ，则返回 true 。
- filter() ：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
- forEach() ：对数组中的每一项运行给定函数。这个方法没有返回值。
- map() ：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
- some() ：对数组中的每一项运行给定函数，如果该函数对任一项返回 true ，则返回 true 。

- 使用forEach()方法

  考虑下面的代码，对一个包含了4个数字的数组进行迭代。
  
  forEach()方法将一个函数作为参数，并从数组中正确的打印四个数字，忽略了description属性。forEach()的另一个限制是 无法提前打断循环 。可以使用every()方法代替forEach()或借助其他一些hack手段来实现这个功能。

- for-in能够 循环遍历对象的属性以及集合数据 。

  正如你看到的，for-in循环 遍历的是所有属性 ，而 不仅仅是数据 ，这个可能并非预期效果。

- ES6引入了for-of循环，能够做到 只遍历数据 而 不读取数据集合中的其他属性 。可以使用break关键字打断循环。

  for-of 可以遍历 任何一个可被迭代的对象 ，包括Array、Map、Set以及其他一些对象

### 浏览器跨域了解吗？具体怎么实现？ 答的是document.domain和jsonp

#### 什么是跨域

- 域名地址的组成

  ![域名地址的组成](https://graphbed.qiniu.songxingguo.com/cross-domain/%E8%B7%A8%E5%9F%9F.jpg)

- 跨域错误代码

   ![跨域错误代码](https://graphbed.qiniu.songxingguo.com/cross-domain/%E8%B7%A8%E5%9F%9F%E9%94%99%E8%AF%AF%E4%BB%A3%E7%A0%81.jpg)

- 跨域的定义

   跨域是JavaScript出于安全方面的考虑，不允许跨域调用其他页面对象。是JavaScript同源策略的限制。

- 常见跨域场景

   ![常见跨域场景](https://graphbed.qiniu.songxingguo.com/cross-domain/%E8%B7%A8%E5%9F%9F%E7%9A%84%E7%B1%BB%E5%9E%8B.jpg)

#### 处理跨域的方法

##### 代理

服务器后台做代理，a.com 调用 b.com的服务时不直接调用，而是通过a.com的服务器后台去调用b.com的服务。

##### JSONP

###### jsonp的原理

ajax请求受同源策略影响，不允许进行跨域请求，而script标签src属性中的链接却可以访问跨域的js脚本，利用这个特性，服务端不再返回JSON格式的数据，而是返回一段调用某个函数的js代码，在src中进行了调用，这样实现了跨域。

[简单透彻理解JSONP原理及使用](https://blog.csdn.net/u011897301/article/details/52679486)

###### 缺点：

- 用JSONP定义参数名称，只支持GET请求。

- 在登录模块中需要用到session来判断当前用户的登录状态,这时候由于是跨域的原因,前后台的取到的session是不一样的,那么就不能就行session来判断.

- 由于jsonp存在安全性问题

#####  XHR2

跨域资源共享（CORS）

普通跨域请求：只服务端设置Access-Control-Allow-Origin即可，前端无须设置。
带cookie请求：前后端都需要设置字段，另外需注意：所带cookie为跨域请求接口所在域的cookie，而非当前页。

目前，所有浏览器都支持该功能(IE8+：IE8/9需要使用XDomainRequest对象来支持CORS）)，CORS也已经成为主流的跨域解决方案。

#####  nginx代理

设置监听域和转发域。

#### nodejs中间件

node中间件实现跨域代理，原理大致与nginx相同，都是通过启一个代理服务器，实现数据的转发。

##### WebSocket

WebSocket protocol是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现。

[跨域问题](https://www.songxingguo.com/2018/07/15/cross-domain/)

### 标签语义化的理解

#### 什么是语义元素呢？

语义元素 为它们标注的内容赋予额外的含义 ，让网页的结构更加清晰 ，语义元素都有一个显著的特点： 不真正做任何事 。

#### 容易修改和维护

通过 HTML5的语义元素 ，通过标记就可以传达出 额外的结构化信息 。

#### 无障碍性

现代Web设计的一个重要主题，就是 让任何人都能无障碍的访问网页 。换句话说，要让使用屏幕阅读器和其他辅助工具的人都能够在页面中自由导航。

#### 搜索引擎优化

如果搜索引擎能更好的理解你的站点，那 搜索者的查询就会越容易与你的内容匹配 ，因而你的网站列在搜索结果中的可能性也就越大。

#### 未来功能

新浏览器和 Web编辑器工具 一定会 利用语义元素 。

最关键的问题在于，如果正确地使用了语义元素，就能够 创建更加清晰的页面结构 ，就能 适应未来的浏览器和Web设计工具的发展趋势 。

### 了解doctype么？html5的和html4的有什么区别（我当时以为要问声明html5以下的doctype怎么写，吓的，其实是想考察doctype对盒模型的影响）

HTML5的文档声明不包括官方规范的版本号。事实上，这个声明只表示当前页面是HTML页面。这与HTML5是一门活着的语言的远见是分不开的。换句话说，只要有新功能添加到HTML语言中，你在页面中都可以使用它们，而不必为此修改文档类型声明。

##### 不同html版本采用不同的渲染模式

##### HTML5只有一种DTD模型： 

HTML4有三种DTD模型：严格、过渡（最多）、框架 
过渡 DOCTYPE 的目的是帮助开发人员从老版本迁移到新版本

##### 对于 HTML 4.01 文档 

包含严格 DTD 的 DOCTYPE 常常导致页面以标准模式呈现。
包含过渡 DTD 和 URI 的 DOCTYPE 也导致页面以标准模式呈现。
有过渡 DTD 而没有 URI 会导致页面以混杂模式呈现。
DOCTYPE 不存在或形式不正确会导致 HTML 和 XHTML 文档以混杂模式呈现。
对于HTML5文档 

HTML5 没有 DTD ，（ HTML5 没有标准和怪异之）

DTTD: 包含严格 DTD（文档类型定义(Document Type Definition)是一套为了进行程序间的数据交换而建立的关于标记符的语法规则） 的 DOCTYPE 常常导致页面以标准模式呈现。

[dtd](https://baike.baidu.com/item/dtd/2661276?fr=aladdin)

[DOCTYPE声明、显示模式（标准模式、怪异模式）、盒子模型](https://blog.csdn.net/qq_35936643/article/details/79126826)

### 盒模型（这里好像因为太愉悦了没深思熟虑所以好像说反了）

盒模型的两种标准： 标准模型，IE模型。

两种标准的区别：

在标准模型中，盒模型的 宽高 只是 内容（content）的宽高 。

在IE模型中盒模型的 宽高 是 内容(content)+填充(padding)+边框(border)的总宽高 
。

```css
/* 标准模型 */
box-sizing: content-box;

/*IE模型*/
box-sizing: border-box;

/*继承父元素 box-sizing 属性的值*/
box-sizing: inherit;
```

### 知道BFC么？怎么生成BFC（这个不会）

#### BFC 是什么呢？

BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。

具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

#### 触发 BFC

只要元素满足下面任一条件即可触发 BFC 特性(脱离文档流就会触发)：

- body 根元素

- 浮动元素：float 除 none 以外的值

- 绝对定位元素：position (absolute、fixed)

- display 为 inline-block、table-cells、flex

- overflow 除了 visible 以外的值 (hidden、auto、scroll)

#### BFC 特性及应用

##### 同一个 BFC 下外边距会发生折叠；

这不是 CSS 的 bug，我们可以理解为一种规范，如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。

##### BFC 可以包含浮动的元素（清除浮动）；

浮动的元素会脱离普通文档流，由于容器内元素浮动，脱离了文档流，所以容器只剩下 2px 的边距高度。如果使触发容器的 BFC，那么容器将会包裹着浮动元素。

##### BFC 可以阻止元素被浮动元素覆盖；

这个方法可以用来实现两列自适应布局，效果不错，这时候左边的宽度固定，右边的内容自适应宽度(去掉上面右边内容的宽度)。

BFC的区域不会与float重叠。

##### 内部的盒子会在垂直方向，一个个地放置；

##### 每个元素的左边，与包含的盒子的左边相接触，即使存在浮动也是如此；

##### BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也如此；

##### 计算BFC的高度时，浮动元素也参与计算。

### 说说你知道的css单位（这个只说了一下px，rem，em，vh，vw，%）

px，rem，em，vh，vw，%,rpx

### 知道引入css的方式有哪几种？

内联样式、内部样式、外部样式。

### 了解js在处理不同数据类型相加时怎么做隐性转化的么？比如1+‘1’=‘11’

如果两个操作符都是数值，执行常规的加法计算，然后根据下列规则返回结果：

- 如果有一个操作数是 NaN ，则结果是 NaN ；
- 如果是 Infinity 加 Infinity ，则结果是 Infinity ；
- 如果是 -Infinity 加 -Infinity ，则结果是 -Infinity ；
- 如果是 Infinity 加 -Infinity ，则结果是 NaN ；
- 如果是+0 加+0，则结果是+0；
- 如果是-0 加-0，则结果是-0；
- 如果是+0 加-0，则结果是+0。

不过，如果有一个操作数是字符串，那么就要应用如下规则：

- 如果两个操作数都是字符串，则将第二个操作数与第一个操作数拼接起来；

- 如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来。

- 如果有一个操作数是对象、数值或布尔值，则调用它们的 toString() 方法取得相应的字符串值，然后再应用前面关于字符串的规则。对于 undefined 和 null ，则分别调用 String() 函数并取得字符串 “undefined” 和 “null” 。

#### 数值转换

有 3 个函数可以 把非数值转换为数值 ： Number() 、 parseInt() 和 parseFloat() 。第一个函数，即转型函数 Number() 可以用于任何数据类型，而另两个函数则专门用于把字符串转换成数值。这 3个函数对于同样的输入会有返回不同的结果。

  Number() 函数的转换规则如下。

  - 如果是 Boolean 值， true 和 false 将分别被转换为 1 和 0。
  - 如果是数字值，只是简单的传入和返回。
  - 如果是 null 值，返回 0。
  - 如果是 undefined ，返回 NaN 。
  - 如果是字符串，遵循下列规则：
    - 如果字符串中只包含数字（包括前面带正号或负号的情况），则将其转换为十进制数值，即 "1"会变成 1， "123" 会变成 123，而 "011" 会变成 11（注意：前导的零被忽略了）；
    - 如果字符串中包含有效的浮点格式，如 "1.1" ，则将其转换为对应的浮点数值（同样，也会忽略前导零）；
    - 如果字符串中包含有效的十六进制格式，例如 "0xf" ，则将其转换为相同大小的十进制整数值；
    - 如果字符串是空的（不包含任何字符），则将其转换为 0；
    - 如果字符串中包含除上述格式之外的字符，则将其转换为 NaN 。
  - 如果是对象，则调用对象的 valueOf() 方法，然后依照前面的规则转换返回的值。如果转换的结果是 NaN ，则调用对象的 toString() 方法，然后再次依照前面的规则转换返回的字符串值。

#### typeof 操作符

检测给定变量的数据类型。

- “undefined” ——如果这个值未定义；
- “boolean” ——如果这个值是布尔值；
- “string” ——如果这个值是字符串；
- “number” ——如果这个值是数值；
- “object” ——如果这个值是对象或 null ；
- “function” ——如果这个值是函数。

#### | 和 &

二进制操作，也是 | 一真则真，&一假则假。

```js
var result = 25 | 3;
alert(result); //27
```
25 与 3 按位或的结果是 27：

```
25 = 0000 0000 0000 0000 0000 0000 0001 1001
3 = 0000 0000 0000 0000 0000 0000 0000 0011
--------------------------------------------
OR = 0000 0000 0000 0000 0000 0000 0001 1011

```
[按位与（AND）](https://www.songxingguo.com/2018/08/30/JavaScript-basic-concepts/)

### new的过程中发生了什么

要创建 Person 的新实例，必须使用 new 操作符。以这种方式调用构造函数实际上会经历以下 4个步骤：

(1) 创建一个新对象；
(2) 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象） ；
(3) 执行构造函数中的代码（为这个新对象添加属性） ；
(4) 返回新对象。

[js中的new()到底做了些什么？？](https://www.cnblogs.com/faith3/p/6209741.html)

### 输入url的过程中发生了什么

![输入url的过程中发生了什么](http://upload-images.jianshu.io/upload_images/2075673-3afda32a13a68c6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[浏览器中输入url后发生了什么](https://blog.csdn.net/nicexibeidage/article/details/78048773)

可以分为这几个大的过程：

- DNS解析
- TCP连接
- 客户端发送HTTP请求
- 服务器处理请求并返回HTTP报文
- 浏览器解析渲染页面
- 结束

[经典面试题: 从输入URL到页面加载的过程发生了什么?](https://www.cnblogs.com/zgx123/p/7379993.html)

### tcp三次握手和四次挥手

#### 三次握手

发送端首先发送一个带SYN（synchronize）标志的数据包给接收方，接收方收到后，回传一个带有SYN/ACK(acknowledegment)标志的数据包以示传达确认信息。最后发送方再回传一个带ACK标志的数据包，代表握手结束。在这过程中若出现问题中断，TCP会再次发送相同的数据包。


#### 四次挥手

由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。这原则是当一方完成它的数据发送任务后就能发送一个FIN来终止这个方向的连接。收到一个 FIN只意味着这一方向上没有数据流动，一个TCP连接在收到一个FIN后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。
（1） TCP客户端发送一个FIN，用来关闭客户到服务器的数据传送。
（2） 服务器收到这个FIN，它发回一个ACK，确认序号为收到的序号加1。和SYN一样，一个FIN将占用一个序号。
（3） 服务器关闭客户端的连接，发送一个FIN给客户端。
（4） 客户端发回ACK报文确认，并将确认序号设置为收到序号加1。

[四次挥手](https://baike.baidu.com/item/%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B/7794287?fr=aladdin)

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


#### 对比缓存

对比缓存，顾名思义，需要进行比较判断是否可以使用缓存。

浏览器第一次请求数据时，服务器会将缓存标识与数据一起返回给客户端，客户端将二者备份至缓存数据库中。
再次请求数据时，客户端将备份的缓存标识发送给服务器，服务器根据缓存标识进行判断，判断成功后，返回304状态码，通知客户端比较成功，可以使用缓存数据。

#### 总结

对于强制缓存，服务器通知浏览器一个缓存时间，在缓存时间内，下次请求，直接用缓存，不在时间内，执行比较缓存策略。
对于比较缓存，将缓存信息中的Etag和Last-Modified通过请求发送给服务器，由服务器校验，返回304状态码时，浏览器直接使用缓存。

[彻底弄懂HTTP缓存机制及原理](https://www.cnblogs.com/chenqf/p/6386163.html)

### 清除浮动

#### 对父级设置合适的CSS高度。

  浮动 导致 父级元素不能被撑开 。

#### 使用 clear: both 清除浮动。

  clear属性规定元素的哪一侧不允许其他的浮动元素。如果声明左侧或者右侧不允许浮动，会使元素的上下边框边界刚好在该边上浮动元素的下外边距边距之下。

#### 父级 div 定义： overflow： hidden 。

#### 在浮动元素后面增加 <br/> 标签。

#### 在父级元素加上 .clearfix 类（常用）  

其 原理 就是 使用 CSS 伪类 ，在 元素末尾 加 一个带有浮动功能的标签 。

```
.clearfix:after {
  visibility: hidden;
  display: block;
  font-size: 0;
  content: "";
  clear: both;
  height: 0;
}
.clearfix {
  display: inline-table;
}

/* Hides from IE-mac \*/
*html .clearfix {
  height: 1%;
}
.clearfix {
  dispaly: block;
}
/* End hide from IE-mac */
```

### 在白板上写出水平垂直居中的方法，一般三种就差不多了

### 水平

-  text-align: center;

- margin: 0 auto;

- table, margin: 0 auto;

- dispay:inline;text-align: center;

- position: relative; left:50%;margin-left: -200rpx;

### 垂直

- padding: 20px 0;

- height: 100px; line-height: 100px;

- vertical-align:middle;

[CSS实现居中](https://www.songxingguo.com/2018/08/06/web-front-end-interview-html-css/)

### WebSocket

[WebSocket双向通信原理初探](https://blog.csdn.net/liuxiaoyi216/article/details/79630294)

[看完让你彻底搞懂Websocket原理](https://www.cnblogs.com/fuqiang88/p/5956363.html)

### 面包屑导航

[AngularJS(九)面包屑导航](https://blog.csdn.net/houysx/article/details/80455226)

### 支付宝支付

- 订单签名

- 支付宝接口

- 验证结果（返回结果）

- 异步返回结果

![支付宝支付](https://gw.alipayobjects.com/os/skylark-tools/public/files/81fdbf664f654970835e5426b55959f6)

[支付宝支付](https://docs.open.alipay.com/204)

### 三栏布局的方法，这里很尴尬，手误在双飞翼模型，大家还是小心点吧。。。中间还提了一个**column-count**这个属性，我完全没印象

 圣杯布局和双飞翼布局基本上是一致的，都是两边固定宽度，中间自适应的三栏布局，其中，中间栏放到文档流前面，保证先行渲染。解决方案大体相同，都是三栏全部float:left浮动，区别在于解决中间栏div的内容不被遮挡上，圣杯布局是中间栏在添加相对定位，并配合left和right属性，效果上表现为三栏是单独分开的（如果可以看到空隙的话），而双飞翼布局是在中间栏的div中嵌套一个div，内容写在嵌套的div里，然后对嵌套的div设置margin-left和margin-right，效果上表现为左右两栏在中间栏的上面，中间栏还是100%宽度，只不过中间栏的内容通过margin的值显示在中间。

[圣杯布局、双飞翼布局、Flex布局和绝对定位布局的几种经典布局的具体实现示例](https://blog.csdn.net/wangchengiii/article/details/77926868)

[圣杯布局和双飞翼布局的理解和区别]

### CDN

[什么是 CDN](https://blog.csdn.net/lu_embedded/article/details/80519898)

[CDN是什么？](https://www.cnblogs.com/tinywan/p/6067126.html)

### 然后我主动讲了栅格系统

### 抛开vue，自己设计双向绑定会怎么实现，还是用的脏检测

### React比jquery高效在哪里？

### 手写“模拟洗牌”的代码

### 手撕字符串反转 单词位置反转有空格

### 关于个人情况：是大四吗是否可以全职实习等。

### 谈了谈可以提前去实习的问题

### 职业规划？

#### 最近准备做什么？

### 然后和面试官握了手

### 还有什么要问我的？

您决定我今天回答和简历有些什么需要改进的地方吗？

面试官也挺有趣我问他有没有什么有趣的书推荐几本。

## 二面

### js的运行环境（果然还是躲不过node），简单说了说浏览器环境和node的区别

一、全局环境下this的指向

　　在node中this指向global而在浏览器中this指向window，这就是为什么underscore中一上来就定义了一 root；

　而且在浏览器中的window下封装了不少的API 比如 alert 、document、location、history 等等还有很多。我们就不能在node环境中xxx();或window.xxx();了。因为这些API是浏览器级别的封装，纯javascript中是没有的。当然node中也提供了不少node特有的API。

二、js引擎

　　在浏览器中不同的浏览器厂商提供了不同的浏览器内核，浏览器依赖这些内核解释折我们编写的js。但是考虑到不同内核的少量差异，我们需要考虑浏览器兼容性。好在有一些优秀的库帮助我们处理这个问题，比如jquery、underscore等等。

　　NodeJS是基于Chrome's JavaScript runtime，也就是说，实际上它是对GoogleV8引擎（应用于Google Chrome浏览器）进行了封装。V8引 擎执行Javascript的速度非常快，性能非常好。

      NodeJS并不是提供简单的封装，然后提供API调用，如果是这样的话那么它就不会有现在这么火了。Node对一些特殊用例进行了优化，提供了替代的API，使得V8在非浏览器环境下运行得更好。例如，在服务器环境中，处理二进制数据通常是必不可少的，但Javascript对此支持不足，因此，V8.Node增加了Buffer类，方便并且高效地 处理二进制数据。因此，Node不仅仅简单的使用了V8,还对其进行了优化，使其在各环境下更加给力。

　　js引擎都固定了，还对应神马兼容性。

三、DOM操作

　　浏览器中的js大多数情况下是在直接或间接（一些虚拟DOM的库和框架）的操作DOM。因为浏览器中的代码主要是在表现层工作。但是node是一门服务端技术。没有一个前台页面，所以我门不会在node中操作DOM。

四、I/O读写

　　与浏览器不同，我们需要像其他服务端技术一样读写文件，nodejs提供了比较方便的组件。而浏览器（确保兼容性的）想在页面中直接打开一个本地的图片就麻烦了好多（别和我说这还不简单，相对路径。。。。。。试试就知道了要么找个库要么二进制流，要么上传上去有了网络地址在显示。不然人家为什么要搞一个js库呢），而这一切node都用一个组件搞定了。

五、模块加载

　　javascript有个特点，就是原生没提供包引用的API一次性把要加载的东西全执行一遍，这里就要看各位闭包的功力了。所用东西都在一起，没有分而治之，搞的特别没有逻辑性和复用性。如果页面简单或网站当然我们可以通过一些AMD、CMD的js库（比如requireJS 和 seaJS）搞定事实上很多大型网站都是这么干的。

　　在nodeJS中提供了CMD的模块加载的API，如果你用过seaJS，那么应该上手很快。

　　node还提供了npm 这种包管理工具，能更有效方便的管理我们饮用的库

　　当然浏览器这边ES6也有这方面的补充，相信未来会更好。。。

### 前端调试，devTools断点的使用

[20个Chrome DevTools调试技巧](http://www.cnblogs.com/fundebug/p/20_chrome_devtools_debugging_tips.html)

[快捷键大全](https://www.cnblogs.com/lishanyang/p/7767135.html)

### 字符串的方法

[String 类型](https://www.songxingguo.com/2018/08/13/JavaScript-reference-type/)

### 数组的方法

[Array 类型](https://www.songxingguo.com/2018/08/13/JavaScript-reference-type/)

### mvvm和mvp模式的简单描述，很简单的描述了下，3分钟左右

ViewModel, 然视图和数据不直接交互。

### 原生ajax的使用步骤，面试官当时的意思应该是讲讲过程就好

### Js的垃圾回收机制。 

### 登录流程

[用户登录验证流程图](https://blog.csdn.net/chenmiaoqin950606/article/details/79693309)

### 浏览器事件循环。 

### Node.js事件循环。

### setTimeout 和 Interval

### tcp握手，以及和udp的应用场景

### 二叉树高度，，手写代码。

### Webpack

### 反转一个字符串以及优化

### 说出什么是AVL（二叉反转树）

### 说出你知道的排序算法（冒泡排序的过程以及优化(这里就是检查一下是否已有序，没反应过来)），以及堆排序的原理，没答出来

### OSI七层模型你知道吗？不知道

### 反转链表，没答出来。。。

### setter和getter

### vue的响应式原理：数据劫持 观察者模式，以及vue的组件化理解

### vuex的作用，使用，以及个人的理解

### 缓存中有last-modified规则了，为什么还要有etag。面试官告诉我是这样一种情况：当一个文件被加了一个空格，l-m

就被修改了，然后再把那个空格删掉，此时文件还是原来那个文件，但l-m就不一样了，所以这就是etag的作用。

### jQuery里使用$.attr('class')方法，那么原生对应的是什么obj.classList。面试官告诉我的

### setTimeout 0 和Promise.then谁先执行 Vue 的nextTick如何实现。

### for of for of能拿来遍历对象吗。

### 遍历器接口。 

### Symbol的应用场景。 

#### 写一个call方法。 

### vue双向绑定和依赖收集怎么实现

### 组件是怎么封装的

### 有封装过哪些组件

### 给定一个长为n数组,里面存储着n个无序的正整数，求出数组中两个相差绝对值最小数的索引，，，说思路

### 逻辑题，共26个硬币，其中有一个是假硬币，假硬币较真硬币略轻。问用一个天平最少称几次可以找出假硬币。

### 一个是轮播图点击小图换大图，另一个是传入一个图片的url会返回该图片颜色的rgba

### 讲一下什么是B 树

## HR面

### 问了下怎么学前端的和其他offer和薪资

### 自我感觉如何

### 有没有offer

### 哪里人啊

#### 意向工作城市
