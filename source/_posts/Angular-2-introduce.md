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
   
- #### AngularJS高级概述

  - ##### Angular的故事
    ![Angular的故事](http://p9myzkds7.bkt.clouddn.com/Angular/Angular%20%E6%95%85%E4%BA%8B.png)
 
  - ##### AngularJS流行原因
 
    - AngularJS具有利用指令概念创建自定义HTML标签和属性的机制，允许根据自己应哟程序的需要扩展HTML标签。

    - AngularJS是入侵式的，但不会产生过多的干扰。可以把ng-app属性添加到任何一个 `<div>` 标签上，只有这个 `<div>` 标签里面的内容会被影响，页面中的其他部分仍然是纯粹的HTML和JavaScript。

    - AngularJS令你能够轻松地把数据绑定到视图上，更改数据将会触发相应视图元素自动更新，反之亦然。

    - AngularJS配套有一个可配置的路由，使你能够对应用程序的组件与URL模式进行映射，当URL变化时会导致页面对应的视图发生变化。
    
    - 在控制器中定义应用程序的数据流，数据流是JavaScript对象类型，其中包括属性和方法。
    
    - AngularJS应用程序使用有层级关系的作用域来存储由控制器和视图分享的数据。
    
    - AngularJS包含了一个依赖注入模块，使你能够以解耦方式开发应用程序。
  
   与jQuery简化DOM操作相反，AngularJS允许开发者以MVC模式设计应用程序，从而把逻辑从UI层分离出来。下图描述了一个用于处理产品的AngularJS应用程序的工作流示例。
   
    ![AngularJS应用程序架构示例](http://p9myzkds7.bkt.clouddn.com/Angular/%E5%B7%A5%E4%BD%9C%E6%B5%81%E5%9B%BE.jpg)
    `AngularJS应用程序架构示例`
    
   一旦模式中保持的数据发生变化，AngularJS就会自动更新视图。如果用户修改了视图中输入框里的数据，那么UI变化也会传递给模型。这个双向更新机制被称为双向数据绑定。如下图所示。
  
   双向绑定意味着其中一个会自动更新另一个，因此在AngularJS中，模型和视图是紧密绑定在一起的。
  
    ![双向绑定](http://p9myzkds7.bkt.clouddn.com/Angular/%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A.jpg)
    `双向绑定`
    
   模式数据保存在一个特定$scope对象的上下文中，AngularJS作用域是一个有层级结构的对象。$rootScope是为整个应用程序创建的。控制器和指令（自定义组件）有自己的$scope对象。
 
   可以通过创建和加载模块对象来实现模块化。当一个特定模块依赖于其他对象（如控制器、模块或服务）时，Angular的注入机制将会创建这些对象的实例。下面代码片段展示了AngularJS将对象注入其他对象的一种方法：
   ![依赖注入](http://p9myzkds7.bkt.clouddn.com/Angular/%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%20.jpg)

   AngularJS经常被用来创建单页面应用程序，用户操作或服务器返回数据只会更新页面中的指定部分（子视图）。

   通过配置ng-route路由组件来设置AngularJS中视图之间的导航。可以根据URL模式指定多个.when选项将应用程序导航到相应的视图。下面代码示例将会演示路由的使用。
   ![路由使用](http://p9myzkds7.bkt.clouddn.com/Angular/%E8%B7%AF%E7%94%B1%E4%BD%BF%E7%94%A8%20.jpg)

   AngularJS支持深度链接，当把页面加入到书签中时，不仅可以收藏整个网页，还可以收藏页面中的某个特定状态。
   
- #### Angular高级概述

  与AngularJS相比，Angular在很多方面的表现都会更好。Angular更容易学习，应用程序的框架的结构也被简化了，并且代码更容易书写。
  
  - ##### 简化代码
  
   Angular应用程序支持ECMAScript 6(ES6)中的标准模块、异步模块定义（Asynchronous Module Definition ,AMD）以及CommonJS格式。通常一个模块一个文件。使用通用的模块加载器SystemJS，并添加import语句。
   
   应用程序的着陆页面HTML文件中包含了Angular模块以及它们的依赖。应用程序的代码通过加载自己的根模块进行引导。所有必需的组件和服务将会根据模块中声明和导入语句进行加载。
   
   ![index.html文件]()
   
   每个组件的HTML片段都可以在组件内部（template属性）或者通过templateURL属性从组件引用的文件中内联得到。
   
   组件是Angular新架构的核心内容。如下图展示了一个由4个组件和2个服务组成的示例Angular应用程序的示意图。所有这些都会被封装到一个模块中。
   
    ![Angular应用程序的示例框架]()
   
   声明一个组件的简单方式就是用TypeScript写一个类(当然，也可以使用ES5、ES6或Dart)。
   
   - 如果为TypeScript的类前缀添加了一个@NgModule元数据注解，那么表示它是一个模块。
   
   - 如果为类前缀添加了一个@Component元数据注解，那么表示它是一个组件。@Component注解（又被称为装饰器）包含了template属性，声明了一个用于浏览器渲染HTML片段。元数据注解允许在设计阶段修改组件的属性。HTML模板可能包含被双大括号包围的数据绑定表达式。事件绑定也在@Component注解的template属性中定义，并且在类中作为方法被实现。
   
   - 另一个元数据注解是@Injectable,它表示创建的组件会被DI模块处理。
   
   @Component注解还包含了selector属性，用来声明能够在HTML文档中使用的自定义标签。当Angular发现一个匹配selector属性值的HTML元素时，就会知道是哪个组件实现了这个元素。
        
   ```
    <body>
      <auction-application>
        <search-product [productID] = "123"></search-product>
      </auction-aplication>
    </body>
   ```
   父组件通过为子组件绑定输入属性来向其传递数据（注意上面代码中的方括号），而子元素通过经过它们的输出属性触发事件来实现与父组件的通信。
   
   下面的代码显示了一个搜索组件SearchComponent。它的selector属性被声明为search-product，因此可以在HTML文档中通过 `<search-product>` 标签来引用它：、
   
    ```
     @Component({
      selector: 'search-product',
      template: 
        `<from>
           <div>
             <input id="prodToFind" #prod>
             <button (click)="findProduct(prod, value)">Find Product</button>
             Product name: {{product.name}}
           </div>
         </from>
        `
     })
     class SearchComponent {
       @Input( ) productID: number;
       
       product: Product;
       
       findProduct(productName: string) {
         // Implementation of the click hander goes here
       }
       // Other code can go here
     }
    ```
   下图为组件内部工作原理。
   
    ![组件内部实现]()
   
   组件从服务中获取数据并用于渲染，这些数据用类的方式来定义。在TypeScript中Product类可以是下面的结构：
   
    ```
     class Product {
       id：number;
       name: string;
       description: string;
       bid: number;
       price:number;

       // construct and other methods go here
     }
    ```
    注意，TypeScript允许在声明类变量的同时指定类型。为让UI组件SearchComponent掌握自己的数据，可以声明一个类变量，如product:
  
      ```
       @Component ({ /* code omitted for brevity */ })
       class SearchComponent {
         product: Product;

         findProduct(productID) {
           // The implementation of the click handler
           // for the Find components button goes here
         }
       }
      ```
    如果SearchComponent想要返回多个产品，可以声明一个数组来存储产品：
  
      ```
      products: Array<Product>;
      ```
    在上面代码中 `<Product>` 使用了泛型符号，表示TypeScript编译器中只有Product类型的对象被允许存储在这个数组中。
   
    Angular不是一个MVC框架，
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  
  