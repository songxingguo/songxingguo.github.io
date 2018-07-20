title: 《Angular 2开发实战》-Angular 2介绍
author: songxingguo
tags:
  - Angular 2
categories:
  - 前端技术
  - 读书笔记
date: 2018-07-20 12:20:00
---
### Angular 2介绍

 Angular 2（以下简称Angular）是一个由Google维护的开源JavaScript框架，它完全重写了备受欢迎的AngularJs（Angular 1.x版本）。

 - #### JavaScript框架和库
  
  框架能够让你完全控制自己应用程序中的架构、设计模式和代码风格。
  
  Angular是用于开发Web应用程序的众多框架之一。
  
  框架为代码提供了一个结构，并强制按照特定方式编写代码。库通常会提供大量的组件和API接口，它们可以在任何代码中被调用。换句话说，与库相比，框架对应用程序的设计更有用。
 <!-- more -->
  
  - ##### 重量级框架
  
    重量级框架包含了开发一个Web应用程序需要的所有东西。这将为你的代码强加一个结构，并配套UI组件和工具以便开发和部署应用程序。
    Ext JS是一个由Sencha创建和维护的全能型框架。它配备了一套优秀的UI框架，其中包括一个高级的数据表格和图表控件，这对于开发后台企业级应用程序是至关重要的。如果一个应用程序采用Ext JS框架，那么它的大小不会小于1MB。Ext JS是入侵式框架，一旦采用，并不能很容易切换到其他框架。
   
    Sencha同时推出了Sencha Touch框架，主要用于开发面向移动设备的Web应用程序。
    
  - ##### 轻量级框架
  
    请量级框架为Web应用程序添加结构，提供一种在不同视图之间切换的导航方式，通常会把应用程序拆分成不同的层，实现模型-视图-控制器（Model-View-Controller,MVC）设计模式。还有一类轻量级框架，专门用于测试用JavaScript开发的应用程序。
    
    Angular是用于开发Web应用程序的开源框架。使用Angular更容易创建那些能够被插入到HTML文档中并实现了应用逻辑的自定义组件。Angular大量地使用数据绑定，包含该一个依赖注入模块，支持模块化，并提供路由机制。Angular并不像Angular那样基于MVC的，整个框架并不包括UI组件。
    
    Ember.js是用于开发Web应用程序的开源框架。它包括路由机制，支持双向数据绑定。Ember.js使用大量的代码约定，从而提高了软件开发人员的生成效率。
    
    Jasmine是一个JavaScript开源测试框架。Jasmine不需要任何一个Dom对象，它包括一系列方法用来测试应用程序中某些行为是否符合预期。Jasmine经常与Karma一起使用，Karma是一个测试运行器，能运行在不同的浏览器中。
    
   - ##### 库
   
     jQuery是流行的JavaScript。它的用法简单，不需要大幅改变自己的Web编程方式。jQuery用于查找和操作Dom元素，处理浏览器事件以及浏览器兼容性问题。jQuery是一个可扩展库，全世界的开发者为其开发了数以千计的插件。如果找不到符合自己要求的插件，还可以自行开发一个。
     
     Bootstrap是一个由Twitter开发的开源UI组件库。使用响应式Web设计原则构建组件。
    
     Google基于一系列设计原则开发了一套UI组件库，叫做Material Design,它可能会成为Bootstrap替代品。Material Design对跨屏幕展示做了优化，并搭配了一套漂亮的UI组件。
     
     React是一个由Facebook开发的开源用户界面库。React作为MVC模式中的V(View)层，是非侵入式的，可以与其他任何库和框架结合使用。React创建自有的虚拟DOM对象，最大限度地减少对浏览器DOM的访问，从而达到更好的性能。对于内容的渲染，React引入了JSX格式，这是一个对JavaScript的扩展，看起来像是XML。React建议使用JSX，但这并不是必需的。
     
     Polymer是一个由Google开放的基于Web Components标准构建自定义组件库。它搭配了一套非常漂亮的可定制的UI组件，可以作为标签在HTML页面中被引用。Pilymer还为应用程序提供离线工作模块以及使用GoogleAPI的组件（如日历，地图等）。
     
     RxJS是一组使用了可观察集合并组合异步请求和基于事件编程的库。它允许应用程序处理异步数据流。使用RxJS，数据流会被表示为一个可观察队列。RxJS可以单独使用，也可以与其他JavaScript框架结合使用。
     
     查看顶级网站使用了什么JavaScript框架和库，可以访问“JavaScript开发网站的使用的使用统计情况”页面，网址为 http://trends.builtwith.com/javascript 。
     
  - ##### 什么是Node.js
  
    Node.js（或者称为Node）不仅仅是一个框架或库，它还是一个运行时环境。
    
    Node.js框架可以用于开发浏览器之外运行的JavaScript程序。可以使用JavaScript或TypeScript开发Web应用程序的服务器端程序；Google为Chrome浏览器开发了一款高性能V8 JavaScript引擎，可用于运行使用Node.jsAPI编写的代码。Node.js包括一个API，具有操作文件系统、访问数据库、监听HTTP请求的功能。
    
    JavaScript社区成员们已经构建了大量的工具用于开发Web应用程序，借助于Node的JavaScript引擎，可以在命令行中运行这些工具。
    
    