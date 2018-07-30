title: 《Angular 2开发实战》学习笔记-Angular应用程序的TypeScript
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
  
 - #### TypeScript作为JavaScript的超集
 
   Typescript完全支持ES5和大多数ES6语法。只要将JavaScript代码文件的 **扩展名从.js改为.ts** ，它们就将变为有效的TypeScript代码。到目前为止，见过的仅有两个例外是 **处理可选的函数参数** ，以及 **将一个值赋给一个对象字面量** 。
   
   在JavaScript中，即使一个 **函数被声明具有两个参数** ，也 **可以通过提供仅提供一个参数来调用它** ；而在TypeScript中，需要为 **参数名称附加一个问号** ，以 **使其成为可选的** 。在JavaScript中，可以 **使用空的对象字面量来初始化变量** ，并 **使用点号立即附加属性** ；而在TypeScript中，**需要使用方括号** 。
   
   但这些差异很小 。更重要的是，因为是JavaScript的超集，TypeScript为JavaScript添加了许多有用的特性。
   
   >提示
   如果正在 **将JavaScript项目转换为TypeScript版本** ，可以 **使用tsc编译器的--allowJs选项** 。TypeScript编译器将检查输入的.js文件的语法错误，并根据tsc的--target和--module选项发出有效的输出。此输出还可以与其他的.ts文件相结合。就像.ts文件一样，仍会为.js文件生成source map。
   
 
 - #### 可选类型
 
   可声明变量，并为它们全部或其中的一些提供类型。以下两行是有效的TypeScript语法：
   
   ```
   var name1 = 'John Smith';
   var name2: string = 'John Smith';
   ```
   如果使用类型，TypeScript转码器可以在开发期间 **检测不匹配的类型** ，而且IDE将会提供 **代码补全** 和 **重构支持** 。在任何大小合适的项目上，这有助于提高生产力。即使不在声明中使用类型，TypeScript也会 **根据赋值猜测类型** ，并且之后 **仍然会进行类型检查** ，这被称为 **类型推断**（type inference)。
   
   以下TypeScript代码片段，不能将一个数字赋给本来是一个字符串的变量name1,即使它最初没有类型（JavaScript语法）声明。使用字符串初始化此变量之后，推断类型将不允许把数字值赋给name1。同样的规则适用于带有显式类型声明的变量name2:
   
   ```
   var name1 = 'John Smith';
   //JavaScript中，将类型不同的一个赋值给一个变量是有效的，但由于类型推断，这在TypeScript中是无效的
   name1 = 123;
   
   var name2: string = 'John Somith';
   //JavaScript中，将类型不同的一个赋值给一个变量是有效的，但由于显示的类型声明，这在TypeScript中是无效的
   name2 = 123;
   ```
   在TypeScript中，可以声明有类型的 **变量**、 **函数参数** 和 **返回值** 。有四个关键字用于声明 **基本类型** ： **number** 、 **boolean** 、 **string** 和 **void** 。**void** 在函数声明中 **表示没有返回值** 。与JavaScript类似，变量可以有 **null** 或 **undefined** 类型的值。
   
   以下是带有显式类型声明变量的示例：
   
   ```
   var salary： number;
   var name: string = 'Alex';
   var isValid: boolean;
   var customerName: string = null;
   ```
   所有这些类型都是 **any类型的子类型** 。如果在 **声明一个变量或函数参数时没有指定类型** ，TypeScript编译器将 **假设它具有any类型** ，这将 **允许给此变量后函数参数赋任何值** 。也可以 **显示地声明一个变量** ，**指定它的类型为any** 。这种情况下，不会应用推断类型。这两种声明都是有效的：
   
   ```
   var name2: any = 'John Smith';
   name2 = 123;
   ```
   如果使用显式的类型声明变量，编译器将检查他们的值以保证它们与声明相匹配。TypeScript包括用于与Web浏览器交互的其他类型，例如HTMLElement和Document。
   
   **如果定义一个类或接口，它可以在变量声明中用作自定义类型。**
   
   - ##### 函数
   
    
   
   
   
   
   
   
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     