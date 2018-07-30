title: 《Angular 2开发实战》学习笔记-作为Angular应用程序语言的TypeScript
author: songxingguo
tags:
  - TypeScript
categories:
  - 前端技术
  - 读书笔记
date: 2018-07-30 03:00:00
---
### 作为Angular应用程序语言的TypeScript

- #### 为什么不使用JavaScript进行开发

  - JavaScript是宽容的，因为它是一种动态类型语言。JavaScript类型错误只能在运行时被捕获，而在编译的时候不会进行类型匹配。
  
  - JavaScript不支持静态类型。使得重构困难。
  
  目前有几十种语言可以编译成JavaScript（请参阅可编译为JavaScript的语言列表，网址为 http://mng.bz/vjzi ）。最受欢迎的是TypeScript(详见 www.typescriptlang.org )、CoffeeScript（详见 http://coffeescript.org ）和 Dart(详见 www.dartlang.org ）
  
  > **为什么使用Dart语言**
  - 与第三方JavaScript库的互操作性不是很好。
  - Dart中的开发只能在附带了Dart VM的Chrome浏览器的专用版本（Dartium）中完成。其他浏览器没有Dart VM。
  - 生成的JavaScript不易阅读。
  - Dart开发社区相当小。
  
  Angular框架是用TypeScript编写的。
  
- #### 为什么使用TypeScript编写Angular应用程序 

  可以使用ES6(甚至ES5)编写应用程序，但将TypeScipt作为编写JavaScript的一种更有成效的方法。以下是其原因：
  
  - **TypeScript支持类型。** 这就允许TypeScript编译器在开发过程中帮你找到并修复许多错误，甚至是在运行应用程序之前。
  
  - **极好的IDE支持。** IDE **提供了很好的上下文相关帮助** 。TypeScript代码可以 **由IDE重构** ，而JavaScript代码必须手动重构。IDE会 **提示可用的API** 。
  
  - Angular和类型定义文件被打包到一起，所以当使用Angular API时，IDE会执行 **类型检查** 并 **提供开箱即用的上下文相关帮助** 。
  
  - TypeScript **遵循ECMAScript 6和7规范** ，并向它们添加了 **类型** 、**接口** 、**装饰器** 、 **类成员变量** （字段）、 **泛型** 以及 关键字 **public** 和 **private** 。将来的TypeScript版本将支持缺少的ES6特性并实现ES7的特性（请参阅TypeScript的Roadmap, 在GitHub上，网址为 http://mng.bz/Ri29 ）
  
  - TypeScript接口允许声明将会在应用程序中用到的 **自定义类型** 。接口有助于避免在应用程序中使用错误类型的对象而引起的编译时错误。
  
  - 生成的JavaScript代码易于阅读，而且看起来像手写的代码。
  
  - Angular文档、文章和博客中的大多数代码示例都以TypeScript给出（详见http://angular.io/docs ）。
  
  
  