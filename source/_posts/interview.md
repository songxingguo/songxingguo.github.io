title: 面试考点
author: songxingguo
tags: []
categories:
  - 找工作
date: 2018-10-01 13:54:00
---
## 自我介绍

我叫宋兴国，来自重庆理工大学软件工程系，家是重庆永川的。在2016年7月到2018年2月期间加入了专业实验室、曾做过一个以AngualrJS为前端框架的新能源物流车综合运营平台项目，离开实验室之后自己搭建了一个个人博客、独立设计和开发了一款微信小程序、以及了解了一下前端的框架 React、Angualr，打包工具 Webpack、ES6、Node（koa）。总的来说，具有一定的项目经验，属性项目开发的整个流程、具有一定的自学能力，以及对前端新技术有一定的敏感度。喜欢电影，打羽毛球。

优点：够努力。
缺点：犹豫、不够坚定。

<!-- more -->

熟悉HTML、CSS、JavaScript。

## HTTP 协议

### HTTP1.0 HTTP 1.1 HTTP 2.0主要区别

#### HTTP 1.0 与 HTTP 1.1 的区别

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


[HTTP1.0 HTTP 1.1 HTTP 2.0主要区别](https://blog.csdn.net/linsongbin1/article/details/54980801/)

[深入研究：HTTP2 的真正性能到底如何](https://segmentfault.com/a/1190000007219256)

[HTTP,HTTP2.0,SPDY,HTTPS你应该知道的一些事](http://www.alloyteam.com/2016/07/httphttp2-0spdyhttps-reading-this-is-enough/)

[HTTP2.0的奇妙日常](http://www.alloyteam.com/2015/03/http2-0-di-qi-miao-ri-chang/)

[一分钟预览 HTTP2 特性和抓包分析](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651551351&idx=2&sn=a56ff090060f97e11e856aef2622a717&chksm=8025a1b6b75228a0080fa971222b3cb7c3179ba5474028b8fa4656619073c4c14d76cf83cd86&scene=0#wechat_redirect)

### HTTPS

![http和https的区别](http://p9myzkds7.bkt.clouddn.com/interview/http%E5%92%8Chttps.JPG)

如上图所示 HTTPS 相比 HTTP 多了一层 SSL/TLS

[HTTPS 原理详解](https://baijiahao.baidu.com/s?id=1570143475599137&wfr=spider&for=pc)

[HTTP与HTTPS的区别](https://www.cnblogs.com/wqhwe/p/5407468.html)

[白话图解HTTPS原理](https://www.cnblogs.com/ghjbk/p/6738069.html)

### HTTP缓存

[彻底弄懂HTTP缓存机制及原理](https://www.cnblogs.com/chenqf/p/6386163.html)

[http缓存浅谈](https://www.cnblogs.com/chinajava/p/5705169.html)

[http协议缓存机制](https://segmentfault.com/a/1190000010690320)

[HTTP缓存实现的原理](https://www.cnblogs.com/zikai/p/4973355.html)

### http性能优化

[36条雅虎军规](http://www.mamicode.com/info-detail-139010.html)

[http请求过程及性能优化分析](https://blog.csdn.net/j_bleach/article/details/75215499)

[腾讯HTTPS性能优化实践](https://blog.csdn.net/suhuaiqiang_janlay/article/details/60962697)

[http性能优化的最佳实践](https://blog.csdn.net/Charles_Tian/article/details/80352118)

[网络请求和性能优化](https://www.jianshu.com/p/ab75b24a11e2)

[HTTP之2 HTTP优化(HTTP性能优化、安全的HTTP协议)](http://blog.51cto.com/jasonteach/1760468)

[应用缓慢、卡顿千万不能忽视HTTP请求优化](https://www.sohu.com/a/129178271_496760)

### TCP 和 UDP

[TCP和UDP的优缺点及区别](https://www.cnblogs.com/xiaomayizoe/p/5258754.html)

[TCP/IP和UDP的比较](https://www.cnblogs.com/HPAHPA/p/7737641.html)

[TCP与UDP--图解TCP/IP读书笔记](https://blog.csdn.net/sinat_37138973/article/details/72822229)

[TCP和UDP的最完整的区别](https://blog.csdn.net/li_ning_/article/details/52117463)


## CSS

### CSS 性能

[css优化，js优化以及web性能优化](https://blog.csdn.net/lululul123/article/details/76167861)

[提高 web 应用性能之 CSS 性能调优](https://www.ibm.com/developerworks/cn/web/1109_zhouxiang_optcss/index.html)

[前端开发规范(三、CSS性能优化)](https://baijiahao.baidu.com/s?id=1603253323416106525&wfr=spider&for=pc)


### CSS3 动画（性能、3D硬件加速）

[css3动画性能优化](https://www.cnblogs.com/sowhite/p/6524689.html)

[GPU（图形处理器）](https://baike.baidu.com/item/%E5%9B%BE%E5%BD%A2%E5%A4%84%E7%90%86%E5%99%A8/8694767?fromtitle=gpu&fromid=105524&fr=aladdin)

### css3动画与js动画的区别

[css3动画与js动画的区别](https://www.cnblogs.com/shuaishuaidejun/p/7444711.html)

[前端制作动画的几种方式（css3，js）](https://www.cnblogs.com/zhangwenjiajessy/p/6154703.html)

[css3动画和JS+DOM动画和JS+canvas动画比较](https://www.cnblogs.com/ricoliu/p/6361018.html)

[Javascript&CSS3动画](https://www.cnblogs.com/coco1s/category/807452.html)


### 重排和重绘

[前端知识点分析——重排和重绘](https://blog.csdn.net/sophia_little/article/details/79613990)

[重绘和重排](https://blog.csdn.net/baidu_28479651/article/details/76228216)

### 浏览器渲染页面

[浏览器渲染原理与过程](https://www.imooc.com/article/40004)

[浏览器如何渲染页面？](https://blog.csdn.net/jnshu_it/article/details/77150545)

[浏览器渲染页面过程与页面优化](https://segmentfault.com/a/1190000010298038)

### css布局（圣杯布局、双飞翼布局、垂直居中、水平居中）

[六种CSS经典三栏布局方案](http://www.php.cn/css-tutorial-382041.html)

[CSS布局说——可能是最全的](https://segmentfault.com/a/1190000011358507)

[CSS经典布局](https://blog.csdn.net/Sourcemyx/article/details/79126069)

[CSS布局十八般武艺都在这里了](https://segmentfault.com/a/1190000008789039)

[史上最全Html和CSS布局技巧](https://blog.csdn.net/qq_33834489/article/details/79111172)

[CSS 常见布局方式](https://www.sohu.com/a/168143624_274163)

### 移动端适配 

[H5移动端适配总结](https://www.jianshu.com/p/64877ce6e893)

[移动端前端适配方案对比](https://blog.csdn.net/sinat_17775997/article/details/62416605)

[前端移动端适配总结](https://segmentfault.com/a/1190000011586301)

### CSS 响应式设计

[响应式 Web 设计-菜鸟教程](http://www.runoob.com/css/css-rwd-viewport.html)

### CSS 属性

- box-sizing（content-box、border-box、inerit）
- display （none、inline、inline-block、block）
- position（relative、absoulate、fixed、static、sticky）

### CSS3新特

### less、sass 预处理器

### Webpack 4.0 （简化配置）、打包流程、order和progan
### 前端安全 XXS
### 闭包内存（内存泄漏如何处理）
### 垃圾回收机制（JC机制、V8）
### 冒泡，事件模型
### 异步和同步（JS为单线程）
### ES6 Prommise（手撕源码）
### html 行内、块级
### BFC（独立的布局空间）
### html5

## 前端框架
### AngularJs

[不要被惯性思维骗了，AngularJS真的那么完美？](https://blog.csdn.net/guoshengkai373/article/details/78874935/)

### 框架原理 React Angular AngularJS（双向绑定）


### MCV和MVM 
### jQuery extend fn
### SEO 搜索引擎优化、爬虫
### 排序算法、树、队列、栈
### JavaScript API
### 项目（数据请求优化）
