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
  
  <!-- more -->
  
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
  
 - #### 转码器的角色
 
   除了JavaScript语言，Web浏览器不懂任何语言。如果源代码用TypeScript编写，那么在能够在浏览器或独立的JavaScript引擎中运行它们之前，它们必须转码为JavaScript。
   
   转码意味着 **将某种语言的源代码转换成另一种语言的源代码** 。许多开发者喜欢使用单词“编译”（compiling）来描述，所以诸如“TypeScript编译器”和“将TypeScript编译成JavaScript”也是有效的。
   
   Angular与静态类型的TypeScript的结合 **简化了大中型Web应用程序的开发** 。**良好的工具** 及 **静态类型分析器** 会大幅度 **减少运行时的错误数** ，并将缩短上市时间。当完成时，Angular应用程序将会有大量的JavaScript代码；而且虽然用TypeScript进行开发需要编写更多的代码，但将通过 **节省测试和重构时间** 以及 **运行时的错误数量来获益** 。
   
 - #### TypeScript入门
   
   **微软** 开放了 **TypeScript源代码** ，它把TypeScript的代码仓库托管在GitHUub上（详见 http://mng.bz/Ri29 ）。可以使用 **npm安装TypeScript编译器** ，或者 **从 www.typescriptlang.org 下载它** 。TypeScript网站还有 **一个托管在Web上的TypeScript编译器** （一个Palyground,详见 http://www.typescriptlang.org/play/index.html ），可以在那里输入TypeScript代码并通过交互将其编译成JavaScript。
   
   - ##### 安装并使用TypeScript编译器
   
     TypeScript编译器本身就是用TypeScript编写的。可以使用Node.js的npm包管理器安装此编译器。
     
     **全局安装TypeScript编译器** ，在命名窗口或终端窗口中运行以下npm命令：
     
     ```
     npm install -g typescript
     ```
     **检查TypeScript编译器的版本** ，请运行以下命令：
     
     ```
     tsc --version
     ```
     TypeScript代码被保存在带有 **.ts扩展名** 的文件中。可以使用以下命令将TypeScript脚本文件 **main.ts转码成main.js** :
     
     ```
     tsc main.ts
     ```
     还可以 **生成source map文件** ， **将TypeScript源代码映射到生成的JavaScript中** 。 **使用source map** ,当浏览器中运行时， **可以在TypeScript中设置断点** ，即使执行的是JavaScript。要 **将main.ts编译成main.js** ，同时还 **生成source map文件main.map** ,运行以下命令：
     
     ```
     tsc --sourcemap main.ts
     ```
     在编译期间，TypeScript编译器会 **从代码中删除所有的TypeScript类型、接口和关键字** ，以 **生成有效的JavaScript** 。通过提供编译器选项，可以生成与ES3、ES5或ES6语法兼容的JavaScript。目前默认是ES3。以下是 **将代码转码成兼容ES5语法命令** ：
     
     ```
     tsc --t ES5 main.ts
     ```
     >在Web浏览器中转码TypeScript
     在开发过程中，我们使用本地安装的tsc。但转码也可以（在服务器上） **在部署过程中完成** ，或者 **当Web浏览器加载应用程序时即时完成** 。使用SystemJS库，它 **内部使用tsc转码并动态加载应用程序模块** 。
     请记住，在浏览器中即时转码，可能导致在用户设备上显示应用程序内容的延迟。如果使用SystemJS加载并在浏览器中将代码转码，**默认会生成source map** 。
     
     **在内存中编译代码** ，而 **不生成输出.js文件** ，可使用--noEmit选项运行tsc:
     
     ```
     tsc --noEmit mian.ts
     ```
     可以通过提供-w选项，以监控（watch）模式启动TypeScript编译器。在此模式下，无论何时修改并保存代码，都会将其自动转码到相应的JavaScript文件中。 **要编译并监控全部的.ts文件** ，运行以下命令：
     
     ```
     tsc -w *.ts
     ```
     编译器将会编译所有的TypeScript文件，在控制台打印错误消息（如果有的话），并继续监控改动的文件。一旦文件改动了，tsc将立即重新编译它。
     
     >注意
     通常，我们 **不使用IDE来编译TypeScript** 。我们用 **SystemJS及其浏览器内置（in-browser)的编译器** ，或者使用 **bundler(Webpack)** ,它会使用一个特殊的用于编译的TypeScript加载器。我们使用由IDE提供的 **TypeScript代码分析器来高亮显示错误** ，并使用 **浏览器来调试TypeScript** 。
     
     TypeScrpt编译器允许配置编译的过程（指定源目录和目标目录、source map生成等待）。项目目录中配置文件tsconfig.json的存在，意味着可以 **在命令行中输入tsc** ,而编译器将从tsconfig.json中读取所有的选项。下面显示了一个示例的tsconfig.json文件。
     
     ```
     {
       "compilerOptions": {
         "target": "es5",
         "module": "commonjs",
         "emitDecoratorMetadata": true,
         "experimentaldecorators": true,
         "rootDir": ".",
         "outDir":"./js"
       }
     }
     ```
     该配置文件指示tsc将代码转码为ES5语法。生成的JavaScript文件将位于js目录中。tsconfig.json文件 **可能包含files部分** ，它 **列出了必须由TypeScript编译的文件** 。因为使用rootDir选项，要求从项目的根目录开始所有文件的编译。
     
     如果要将项目的某些文件从编译中排除，请将exclude属性添加到tsconfig.json中。以下演示了如排除node_modules目录的全部内容：
     
     ```
     "exclude" : [
        "node_modules"
      ]
     ```
     可以在TypeScript文档（详见 http://mng.bz/rf14 ）中阅读有关 **配置编译过程** 以及 **TypeScript编译器选项的更多信息** 。
     
     
     
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  
  
  