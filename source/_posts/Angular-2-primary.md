title: 《Angular 2开发实战》学习笔记-Angular入门
author: songxingguo
tags:
  - Angular 2
categories:
  - 前端技术
  - 读书笔记
date: 2018-07-25 11:57:00
---
### Angular入门
  
  代码示例，网址为 http://github.com/Farata/angular2typescript 。

  - #### 第一个Angular应用程序
  
     将展示使用TypeScript、ES5、ES6编写的Hello World应用程序的三个版本。
     
     - ##### TypeScript版本的Hello World
     
       此 应用程序包含两个文件：
       
       ```
       index.html
       main.ts
       ```
       **在HTML文件中加载Angular**
       
       Angular框架的代码由模块（每个模块一个文件）组成，它们被组合到库中，将库从逻辑上分组成包，例如@angular/core、@angular/common等。必须 **在应用程序代码之前加载所需的包** 。
       
       下面创建一个index.html文件，它首先加载必需的 **Angular脚本** 、**TypeScript编译器** 以及 **SystemJS模块加载器** 。以下代码从内容分发网络（Content Delivery Network，CDN）unpkg.com 载入这些脚本。
       
       `代码清单2.1 TypeScript index.html`
       ```
        <!DOCTYPE html>
        <html>
        <head>
          <script src="//unpkg.com/core-js/client/shim.min.js"></script>
          
          //Zone.js 是一个提供变更检测的库
          <script src="//unpkg.com/zone.js@0.8.4"></script>
          
          //TypeScript编译器在浏览器中将源代码编译成JavaScript
          <script src="//unpkg.com/typescript@2.0.0"></script>
          
          //SystemJS将应用程序代码动态加载到浏览器中。
          <script src="//unpkg.com/systemjs@0.19.37/dist/system.src.js"></script>
          <script>
            
            //配置SystemJS加载器，用于加载并编译TypeScript代码
            System.config({
              transpiler: 'typescript',
              typescriptOptions: {emitDecoratorMetadata: true},
              //将Angular模块的名称映射到它们的CDN位置
              map: {
                'rxjs': 'https://unpkg.com/rxjs@5.0.0-beta.12',
                '@angular/core'                    : 'https://unpkg.com/@angular/core@2.0.0',
                '@angular/common'                  : 'https://unpkg.com/@angular/common@2.0.0',
                '@angular/compiler'                : 'https://unpkg.com/@angular/compiler@2.0.0',
                '@angular/platform-browser'        : 'https://unpkg.com/@angular/platform-browser@2.0.0',
                '@angular/platform-browser-dynamic': 'https://unpkg.com/@angular/platform-browser-dynamic@2.0.0'
              },
              //为每个Angualr模块指定main脚本
              packages: {
                '@angular/core'                    : {main: 'index.js'},
                '@angular/common'                  : {main: 'index.js'},
                '@angular/compiler'                : {main: 'index.js'},
                '@angular/platform-browser'        : {main: 'index.js'},
                '@angular/platform-browser-dynamic': {main: 'index.js'}
              }
            });
            
            //指示Angular从main.ts文件加载主模块
            System.import('main.ts');
          </script>
        </head>
        <body>
          //自定义的HTML元素<hello-world></hello-world> 表示main.ts中实现的组件。
          <hello-world></hello-world>
        </body>
        </html>
       ```
       当此应用程序启动时， <hello-world>标签将由代码清单2.2中所示的@Component注解的模板内容替代。
       
       >提示
       如果使用的是IE（Internet Explorer）浏览器，那么可能需要添加额外的脚本 system-polyfills.js 。
       
       >内容分发网络（CDN）
       unpkg（ http://unpkg.com ）是发布到npm（ http://www.npmjs.com/ ）包管理器的注册表（软件）包的一个CDN。检查npmjs.com以查找特定包的最新版本。如果想要查看哪个其他版本的包可用，请运行 npm info packagename 命令。
       
       TypeScript文件
       
       现在创建main.ts文件，它具有TypeScript/Angular代码和以下三个部分：
        1. 声明Hello World组件
        2. 将其包组装成一个模块
        3. 加载该模块
        
        `代码清单2.2 TypeScript main.ts`
        ```
        //从相应的Angular包导入引导方法和@Component注解，使其可用于应用程序代码
        import {Component} from '@angular/core';
        import { NgModule }      from '@angular/core';
        import { BrowserModule } from '@angular/platform-browser';
        import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

        // Component
        @Component({
          //在HelloWorldComponent类之上的@Component注解将其转换为Angular组件
          selector: 'hello-world',
          
          //template属性定义了用于渲染此组件的HTML标记
          template: '<h1>Hello {{ name }}!</h1>'
        })
        
        注解过的HelloWorldCompoent类表示组件
        class HelloWorldComponent {
          //name属性被用于组件模板的数据绑定表达式中
          name: string;

          constructor() {
            //在构造函数中，使用值Angular 2初始化绑定到模板的name属性
            this.name = 'Angular';
          }
        }

        // Module 
        @NgModule({ //声明模块内容
          imports:      [ BrowserModule ],
          declarations: [ HelloWorldComponent ],
          bootstrap:    [ HelloWorldComponent ]
        })
        export class AppModule { } //声明表示模块的类

        // App bootstrap
        platformBrowserDynamic().bootstrapModule(AppModule); //加载此模块
        ```
        > 元数据
        **元数据是关于数据的附加信息。** 
        对于类，元数据 **是关于该类的附加信息** 。例如，@Component装饰器（有名注解）告诉Angular（元数据处理器）这 **不是一个常规类，而是一个组件** 。**Angular根据@Compnents装饰器的属性中提供的信息生成额外的JavaScript代码** 。@Component装饰器不会更改装饰的类，但**会添加一些描述该类的数据** ，因此Angular编译器可以在浏览器的内存（动态编译）或磁盘上的文件（静态编译）中正确生成组件的最终代码。
        
        通过使用与@Component注解的 **selector属性中的组件名称相匹配的标签** ，任何应用程序组件都可以包含在HTML文件（或其他组件的模板）中。 **组件选择器类似于CSS选择器** ，因此给定‘hello-world’选择器，就将使用名为<hello-world>的元素将这个组件渲染到HTML页面中。 **Angular会将此行转换成document.querySelctorAll(selector)** 。
        
        请注意，在代码清单2.2中整个模板都包含在 **反引号** 中，以 **将模板转换成一个字符串** 。这样，就可以在模板中 **使用单引号和双引号** ，并 **将器分成多行以进行更好的格式化** 。该模板包含 **数据绑定表达式{{name}}** ,而且在运行时，Angular将在组件上找到name属性，并 **用具体值替换大括号中的数据绑定表达式** 。
       
        
        
        
       
       