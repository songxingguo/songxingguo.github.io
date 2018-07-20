title: 《Angular 2 开发实战》学习笔记-前言
tags:
  - Angular
categories:
  - 前端技术
  - 读书笔记
author: songxingguo
date: 2018-07-20 11:15:00
---
### 译者序

 与Angular 1.x 相比，Angular 2继承了前者的整合性，延续了“All in One” 的设计理念，并在变更检测性能、指令简化、以及移动前端开发等方面做了很大改进，开发语言层面全面拥抱了TypeScript。
 
 需要注意的是，Angualr 2 并不兼容Angular 1.x。
 
### 序言
  
 Angular 2和TypeSript是用于Web应用开发的正确工具，主要原因有下面这些：
 
 - 组UI和应用程序逻辑分隔干净
  
  渲染UI的代码和实现业务逻辑的代码之间得以干净地分隔。UI不是必须要用HTML渲染，并且已经又支持渲染iOS和Android原生UI的产品了。
  
 <!-- more -->
  
 - 模块化
 
  又用于应用程序模块化的简单机制，支持模块的延迟加载。
  
 - 导航支持
 
  路由支持单页面应用程序中复杂的导航场景。
 
 - 低耦合
  
  依赖注入提供了一种在组件和服务之间实现低耦合的干净方式。绑定和事件允许创建可复用且低耦合的组件。
 
 - 组件生命周期
 
  每个组件都经过一个明确定义的生命周期，并且有用于拦截重要的组件事件的钩子，供应用程序开发者使用。
 
 - 变更检测
  
  自动（快速）的更改检测机制手动强制UI更新，同时也提供了一种微调此过程的方法。
 
 - 没有回调地狱
 
  Anugular 2 附带了RxJs库，它允许安排基于订阅的异步数据处理，从而消除回调地狱。
  
 - 表单及验证
 
  设计良好的表单及自定义验证支持。可以通过验证支持。可以通过编程或为目标中的from元素添加指令的方式创建表单。
  
 - 测试
 
  对单元测试和端到端测试支持良好，并且可以将测试集成到自动化的构建过程中。
  
 - Webpack打包及优化
 
  使用Webpack（及其各种插件）打包并优化代码，可将部署的应用程序小型化。
  
 - 工具
 
  工具支持和Java及.NET平台一样好。TypeScript代码分析器会在输入时警告出现的错误，脚手架和部署工具（Angualr CLI）会帮你编写样板代码和配置脚本。
 
 - 代码简洁
  
  使用TypeScript类和皆接口，使代码更简洁并易于阅读和编写。
  
 - 编译器
  
  TypeScript会生成人类可以阅读的JavaScript。TypeScript代码可以编译成ES3、ES5和ES6版本的JavaScript。特定于Angualr代码的（不要与TypeScript编译器混淆）提前（Aot, Ahead-of-time）编译，消除了将编译器与应用程序打包在一起的必要性，这进一步降低了框架的开销。
 
 - 服务器渲染
  
  在线下的一个构建步骤中，Angular Universal会将应用程序转换成HTML,这可以用于服务器的渲染，从而大大提高搜索引擎索引和SEO。
 
 - 现代UI组件
 
  一个现代风格的UI库（Angular Material 2）正处在开发过程中。
  
### 前言

 Angular 2应用程序能够支持使用两种JavaScript语法（ES5和ES6）进行开发，同样也支持使用Dart和TypeScript进行开发。框架本身使用TypeScript开发。
 
 TypeScript是JavaScript的一个超集。
 
 本书示例源代码可以从网站 https://www.manning.com/books/angular-2-development-with-typescript 下载。