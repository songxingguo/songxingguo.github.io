title: Web前端面试题目及详解汇总-前端相关
author: songxingguo
tags:
  - ''
categories:
  - 找工作
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

### 常见调试方法

### 16、什么是线程？进程和线程的关系是什么？

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

F9    在你需要停下的地方设置断点 
F5    进入调试 
F10  单步运行 
F11  进入函数

### 前端性能优化

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