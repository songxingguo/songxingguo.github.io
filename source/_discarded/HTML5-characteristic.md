title: HTML5的新特性
author: songxingguo
date: 2018-08-02 05:03:40
tags:
---

20、说说你对HTML5认识?（是什么,为什么）
HTML5指的是包括 HTML 、 CSS 和 JavaScript 在内的一套技术组合。 HTML5 是 HTML 最新版本， 2014 年 10 月由万维网联盟（ W3C ）完成标准制定。

HTML4陈旧不能满足日益发展的互联网需要，特别是移动互联网。为了增强浏览器功能 Flash 被广泛使用，但安全与稳定堪忧，不适合在移动端使用（耗电、触摸、不开放）。
HTML5增强了浏览器的原生功能，符合 HTML5 规范的浏览器功能将更加强大，减少了 Web 应用对插件的依赖，让用户体验更好，让开发更加方便，另外 W3C 从推出 HTML4.0 到 5.0 之间共经历了 17 年， HTML 的变化很小，这并不符合一个好产品的演进规则。

作者：毕安
链接：https://www.jianshu.com/p/0e9a0d460f64
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

html5有哪些新特性？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？　　

    (Q1)

　　HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。

　　(1)绘画 canvas;

　　(2)用于媒介回放的 video 和 audio 元素;

　　(3)本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;

　　(4)sessionStorage 的数据在浏览器关闭后自动删除;

　　(5)语意化更好的内容元素，比如 article、footer、header、nav、section;

　　(6)表单控件，calendar、date、time、email、url、search;

　　(7)新的技术webworker, websocket, Geolocation;

　(Q2)

　　IE8/IE7/IE6支持通过document.createElement方法产生的标签，

　　可以利用这一特性让这些浏览器支持HTML5新标签，

　　浏览器支持新标签后，还需要添加标签默认的样式。

　　当然也可以直接使用成熟的框架、比如html5shim，