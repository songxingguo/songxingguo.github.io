title: 《Angular 2开发实战》学习笔记-Angular应用程序的TypeScript
author: songxingguo
tags:
  - TypeScript
categories:
  - 读书笔记
date: 2018-07-30 03:00:00
---
### 作为Angular应用程序语言的TypeScript

- #### 不使用JavaScript

  为什么不使用JavaScript进行开发
 
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
  
- #### 使用TypeScript

  为什么使用TypeScript编写Angular应用程序
  
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
   
   - ##### TypeScript编译器
   
     安装并使用TypeScript编译器
     
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
     >**在Web浏览器中转码TypeScript**
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
  
 - #### JavaScript的超集
 
   TypeScript作为JavaScript的超集
   
   Typescript完全支持ES5和大多数ES6语法。只要将JavaScript代码文件的 **扩展名从.js改为.ts** ，它们就将变为有效的TypeScript代码。到目前为止，见过的仅有两个例外是 **处理可选的函数参数** ，以及 **将一个值赋给一个对象字面量** 。
   
   在JavaScript中，即使一个 **函数被声明具有两个参数** ，也 **可以通过提供仅提供一个参数来调用它** ；而在TypeScript中，需要为 **参数名称附加一个问号** ，以 **使其成为可选的** 。在JavaScript中，可以 **使用空的对象字面量来初始化变量** ，并 **使用点号立即附加属性** ；而在TypeScript中，**需要使用方括号** 。
   
   但这些差异很小 。更重要的是，因为是JavaScript的超集，TypeScript为JavaScript添加了许多有用的特性。
   
   >**提示**
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
   
     TypeScript函数（及函数表达式）与JavaScript函数类似，但可以显式地声明参数类型和返回值。我们来编写一个计算税款的JavaScript函数，它有三个参数，并将根据州（state）、收入（income）和家属（dependents）数量计算税款。对于每个家属，根据此人居住的州，给予$500或$300的税款减免。
    
      ```
      function calcTax(state, income, dependents) {
        if (state == 'NY') {
          return income * 0.06 - dependents * 500;
        }else if (state == 'NJ') {
          returnn income * 0.05 - dependents * 300;
        }
      }
      ```
     假如一个居住在新泽西（New Jersey）州的人，有￥50000的收入和两个家属。我们调用calcTax()：
    
      ```
      var tax = ccalcTax('NJ', 50000, 2);
      ```
     变量tax得到的值是1900,它是正确的。即使calcTax()没有为函数参数声明任何类型，也 **可以根据参数名称来猜测它们** 。
    
     现在我们来以错误的方式调用它，传入一个字符串值给家属（dependents）数量：
    
      ```
      var tax = calcTax('NJ', 50000, 'two');
      ```
     直到调用这个函数之后，你才知道问题。变量tax会有一个NaN（Not a Number）值。只是因为没有机会显示地指定参数类型，一个bug就偷偷溜了出来。下面用TypeScript重写这个函数，为参数和返回值声明类型。
    
      ```
      function calcTax(state: string, income: number, dependents: number) :number {
        if (state == 'NY') {
          return income * 0.06 - dependents * 500;
        }else if (state == 'NJ') {
          returnn income * 0.05 - dependents * 300;
        }
      }
      ```
     现在就没办法犯同样的错误了，**传一个字符串值给家属（dependents）数量** ：
    
      ```
      var tax: number = calcTax('NJ'， 50000， 'two');
      ```
     TypeScript编译器将显示一个错误: “Argument of type 'string' is not assignable to parameter of type 'number'。”此外，该函数的返回值被声明为number，这会阻止你犯下另一个错误， **将税款计算的结果赋值给一个非数值的变量** ：
    
      ```
      var tax：string = calcTax('NJ'， 50000， 'two');
      ```
     编译器将捕获到这一点，产生错误:“Argument of type 'string' is not assignable to type 'string': var tax: string。”在任何项目上，这种编译期间的类型检查可以节省大量的时间。
    
   - ##### 默认参数
   
     声明一个函数时，**可以指定默认的参数值** 。唯一的限制是带默认值的参数， **不能在必需（required）参数的前面** 。在上面代码中，要将NY作为state参数的默认值，就不能像下列声明一样：
     
     ```
     function calcTax(state: string = 'NY'， income: number, dependents: number): number {
       // the code goes here
     }
     ```
     **需要更改参数的顺序** ，以保证在默认参数之后没有必需的参数：
     
     ```
     function calcTax(income: number, dependents: number, state: string = 'NY'): number {
       //the code goes here
     }
     ```
     甚至需要更改calcTax()函数体中的一行代码，现在可以使用两个或三个参数自由地调用它：
     
     ```
     var tax: number = calcTax(50000, 2);
     //or 
     var tax： number = calcTax(5000, 2, 'NY');
     ```
     两次调用的结果将是一样的。
     
  - ##### 可选参数
    
    在TypeScript中，通过 **向参数名称附加一个问号** ，可以轻松地 **将函数参数标记为可选的** ，唯一的限制是 **可选的参数必须在函数声明的最后** 。当编写带可选参数的函数代码时， **需要提供应用程序逻辑来处理没有提供可选参数的情况** 。
    下面是修改税款计算函数：如果没有指定dependents,就不对计算的税款进行减免。
    ```
    function calcTax(income: number, state: string = 'NY', dependents?: number): number {
      var deduction: number;
      
      //处理dependents中的可选值
      if (dependents) {
        deduction = dependents * 500;
      } else {
        deduction = 0;
      }
      
      if (state = 'NY') {
        return income *  0.06 - deduction;
      } else if (state = 'NJ') {
        return income * 0.05 - deduction;
      }
    }
    
    var tax: number = calcTax(50000, 'NJ', 3);
    console.log("Your tax is" + tax);
    
    var tax: number = calcTax(50000);
    console.log("Your tax is" + tax);
    ```
    请注意dependents?: number中的问号。现在该函数会检查是否为dependents提供值。如果没有，就将0赋给变量deduction；否则，为每个家属（dependent）减免扣税$500。1000
    运行上面代码将产生以下输出：
    
    ```
    Your tax is 1000
    Your tax is 3000
    ```
    - ##### 箭头函数表达式
    
      TypeScript **支持在表达式中使用匿名函数的简化语法** 。不需要使用关键字function,宽箭头符号（=>）用于将参数和函数体分开。TypeScript支持箭头函数的ES6语法。在其他一些编程语言中，箭头函数被称为 **lambdda表达式** 。
      
      我们来看一个最简单的箭头函数的例子，函数体只有一行代码：
      
      ```
      var getName = () => 'John Smith';
      console.log(getName());
      ```
      空括号表示上述箭头函数没有参数。单行的箭头表达式不需要花括号或显式的return语句，而上述代码片段将在控制台打印“John Smith”。如果在TypeScript的playground中实验此代码，它们将被转换成以下ES5代码：
      
      ```
      var getName = function () { return 'John Smith'; };
      console.log(getName());
      ```
      如果箭头函数的函数体由多行构成，那么必须将它们括在大括号内并使用return语句。以下代码片段将硬编码的字符串值转换成大写形式，并在控制台打印“PETER LUGER”：
      
      ```
      var getNameSuper = () => {
        var name = 'Peter Luger'.toUpperCase();
        return name;
      }
      console.log(getNameUpper());
      ```
      除了提供较短的语法之外，箭头函数表达式也消除了this关键字臭名昭著的混乱。在JavaScript中，如果在一个函数中使用this关键字，它可能并不指向正在调用此函数的对象。者可能会导致运行时bug，并需要额外的调试时间。下面看一个例子。
      下面代码有两个函数：StockQuoteGeneratorArrow()和StockQuoteGeneratorAnonymous()。每隔一秒，这两个函数就会调用Math.random(),为被作为参数提供的股票号（symbol）生成一个随机的价格。在内部，StockQuoteGeneratorArrow()使用箭头函数语法，提供setInterval()的参数，而StockQuoteGeneratorAnonymous()使用匿名函数。
      
      ```
      function StockQuoteGeneratorArrow(symbol: string) {
        
        //将股票代号赋值给this.symbol
        this.symbol = symbol;
        
        setInterval(() => {
          console.log("StockQuoteGeneratorArrow. The price quote for " + this.symbol + " is " + Math.random());
        }, 1000);
      }
      var stockQuoteGeneratorArrow = new StockQuoteGeneratorArrow("IBM");
      
      function StockQuoteGeneratorAnonymous(symbol: string) {
        //将股票代码赋值给this.symbol
        this.symbol = symbol;
        
        setInterval(function() {
          console.log("StockQuoteGeneratorAnonymous. The price quote for " + this.symbol + " is " + Math.random());
        }, 1000);
      }
      
      var stockQuoteGeneratorAnonymous = new StockQuoteGeneratorAnonymous("IBM");
      ```
      这两种情况汇中，都将股票代号（“IBM”）赋给了对象this上的变量symbol。但是使用箭头函数，对构造函数StockQuoteGeneratorArrow()的实例引用，会被自动保存到单独的变量中；当从箭头函数中引用this.symbol时，会正确地找到它，并在控制台输出中使用“IBM”。
      但是当浏览器中调用匿名函数时，this指向全局的Window对象，该对象没有symbol属性。在Web浏览器中运行此代码，每隔一秒会打印下面这样的内容：
      
      ```
      StockQuoteGeneratorArrow. The price quote for IBM is 0.6888406401347633
      StockQuoteGeneratorAnonymous. The price quote for undefined is 0.7601010292924124
      ```
      如你所见，当使用箭头函数时，它将IBM识别为股票编代号，但在匿名函数中是undefined。
      
      >**注意**
      TypeScript **使用一个外部作用域的this引用，通过传入此引用，它会替换箭头函数表达式中的this** 。这就是为什么StockQuoteGeneratorArrow()中箭头内的代码能够正确地看到来自外部作用域的this.symbol的原因所在。
      
      >**函数重载**
      JavaScript **不支持函数重载** ，所以让几个函数同名但参数列表不同是不可能的。TypeScript的创建者引入了函数重载，但由于代码必须转码到单个JavaScript函数中，因此重载的语法不是很优雅。
      可以为仅有一个函数体的函数声明多个签名，（在此函数体中）需要检查参数的数量和类型，进而执行相应部分的代码：
      ```
      function attr(name: string): string;
      function attr(name: string, value: string): string;
      function attr(map: any): void;
      function attr(nameOrMap: any, value?: string): any {
        if (nameOrMap && typeof nameOrMap == 'string') {
          //handle string case
        } else {
          //handle map case
        }
          //handle value here
      }
      ```
 - #### 类
 
   如果有Java或C#开发经验，将会熟悉它们的经典形式中的类和继承的概念。在这些语言中，类的定义被作为单独的实体（像是蓝图）加载到内存中，并由该类的全部示例共享。如果一个类继承自另一个类，就使用两个类组合后的蓝图来实例化对象。
   
   TypeScrript是JavaScript的超集，JavaScript仅支持原型继承，可以通过 **将一个对象附加到另一个对象的prototype属性** 来创建继承层级结构。在这种情况下，对象的继承（或者更确切地说链接）是动态创建的。
   
   在TypeScript中，关键字class是简化编码的语法糖。最终，类会被转码为带有原型继承的JavaScript对象。在JavaScript中，可以声明一个构造函数并使用关键字new实例化它。在TypeScript中，也可以声明一个类并使用操作符new实例化它。
   
   类可以包括 **构造函数** 、 **字段**（也称属性）和 **方法** 。声明的属性和方法通常被称为 **类成员** 。
   
   我们创建一个简单的Person类，它包括四个属性用于存储姓、名、年龄和社会安全号码（Social Security number, 分配给每个美国合法居民的唯一标识符）。在下面的代码中可以看到声明并实例化Person类的TypeScript代码；
   
   ```
   class Person {
     firstName: string;
     lastName: string;
     age: number;
     ssn: string;
   }
   
   var p = new Person();
   
   p.firstName = "John";
   p.lastName = "Smith";
   p.age = 29;
   p.ssn = "123-90-4567";
   ```
   然后是一个由tsc编译器生成的JavaScript闭包。
   
   ```
   var Person = /** @class */ (function () {
    function Person() {
    }
    return Person;
	 }());
   var p = new Person();
   p.firstName = "John";
   p.lastName = "Smith";
   p.age = 29;
   p.ssn = "123-90-4567";
   ```
   通过为函数Person创建一个闭包，TypeScript编译器启用了暴露 或隐藏Person对象元素的机制。
   
   TypeScript也支持类的构造函数，它允许在实例化对象时初始化对象变量。类构造函数仅会在对象创建期间被调用一次。下面代码显示了Person类的下一版，它使用关键字constructor,并用传给构造函数的值实例化类的字段。
   
   ```
   class Person {
     firstName: string;
     lastName: string;
     age: number;
     ssn: string;
     
     constructor(firstName: string, lastName: string, age: number, ssn: string) {
       this.firstName = firstName;
       this.lastName = lastName;
       this.age = age;
       this.ssn = ssn;
     }
   }
   
   var p = new Person("John", "Smith", 29, "123-90-4567");
   ```
   生成的ES5版本在下面代码中显示。
   
   ```
   var Person = /** @class */ (function () {
    function Person(firstName, lastName, age, ssn) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.ssn = ssn;
    }
    return Person;
   }());
   var p = new Person("John", "Smith", 29, "123-90-4567");
   ```
   一些JavaScript开发者看不到使用类的价值，因为他们 **可以使用构造函数和闭包轻易地编写出相同的功能**  。但JavaScript初学者会发现，相比于构造函数和闭包， **类的语法更易于阅读和编写** 。
   
   - ##### 访问修饰符
   
     JavaScript无法将变量或方法声明为私有的（private, 对外部代码不可见）。要隐藏对象里的属性（或方法），需要创建闭包，这样既不会将属性附加到变量this，也不会在闭包return语句中返回。
     
     TypeScript提供了关键字 **public** 、**protected** 和 **private** ，以帮助在开发阶段控制对象成员的访问。默认情况下，**所有的类成员都具有public访问权限** ，并且它们 **从类的外部都是可见的** 。如果一个成员被声明带有protected修饰符，那么 **它在该类机器子类中是可见的** 。被声明为private的类成员 **仅在该类内部可见** 。
     
     我们使用关键字private来隐藏ssn属性的值，所以它无法从Person对象的外部被直接访问。我们将展示两个版本，它们都声明了具有使用访问修饰符的属性类。此类的较长版本看起来是这样的：
     
     ```
     class Person {
       public firstName: string;
       public lastName: string;
       public age: number;
       private _ssn: string;
       
       constructor(firstName: string, lastName: string, age: number, ssn: string) {
         this.firstName = firstName;
         this.lastName = lastName;
         this.age = age;
         this._ssn = ssn;
      }
     }
     
     var p = new Person("John", "Smith", 29, "123-90-4567");
     console.log("Last name: " + p.lastName + " SSN: " + p._ssn);
     ```
     请注意， **私有变量的名称以下画线开始**：_ssn。这只是私有属性的命名惯例。
     
     上面的代码的最后一行尝试从外部访问私有的属性_ssn，所以TypeScript分线器将给出如下编译错误：“Property '_ssn' is private and is only accessible in calss 'Person'”。但 **除非使用了编译器选项--noEmitOn-Error** ,否则错误的代码将被转码为JavaScript。
     
      ```
       var Person = /** @class */ (function () {
        function Person(firstName, lastName, age, ssn) {
            this.firstName = firstName;
            this.lastName = lastName;
            this.age = age;
            this._ssn = ssn;
        }
        return Person;
       }());
       var p = new Person("John", "Smith", 29, "123-90-4567");
       console.log("Last name: " + p.lastName + " SSN: " + p._ssn);
      ```
     **关键字private仅使其在TypeScript代码中是私有的** 。当尝试从外部访问一个对象的属性时，IDE将不会在上下文相关帮助中显示私有成员，但是， **生产（环境）中的JavaScript会将类的所有属性和方法视为公开的** 。
    
     TypeScript **允许使用构造函数的参数提供访问修饰符** ，例如以下Person类的简短版本所示：
    
      ```
      class Person {
        constructor(public firstName: string, public lastName: string, public age: number, private _ssn: string) {
        }
      }

      var p = new Person("John", "Smith", 29, "123-90-4567");
      ```
     当使用带有访问修饰符的构造函数时，**TypeScript编译器会将其作为一条指令** ，**创建并保留与构造的参数相匹配的类属性** 。 **不需要显式声明并初始化它们** 。Person类的长短两个版本都会生成相同的JavaScript。
     
     >**提示**
     访问修饰符有助于在开发期间控制对类成员的访问，但并不像Java和C#这样的语言那么严格。
    
   - ##### 方法
     
     当一个函数被声明在一个类中时，它被称为 **方法** 。在JavaScript中，需要在对象的原型上声明方法；但是使用类，可以通过指定一个后跟括号和大括号的名称来声明方法，就像在其他面向对象语言中一样。
     
     接下来的代码片段展示了如何声明并使用具有方法doSomething()的类MyClass，此方法有一个参数，没有返回值。
     
     ```
     class MyClass {
       doSomething(howManyTimes: number)： void {
        // do something here
       }
     }
     
     var mc =  new MyClass();
     mc.doSomething(5);
     ```
     >**静态成员和实例成员** 
     上面的代码中，首先创建了类的一个实例，然后使用一个指向此实例的引用变量来 访问它的成员：
     ```
     mc.doSomething(5)
     ```
     >如果使用 **关键字static** 声明一个类属性或方法，**它的值将被类的所有实例共享** ，而 **无需创建一个实例来访问静态成员** 。不是使用引用变量（例如mc）,而是 **使用类的名称** 。
     ```
     class MyClass {
       static doSomething(howManyTimes: number): void {
         // do something here
       }
     }
     
     MyClass.doSomething(5);
     ```
     >**如果实例化一个类** ，并且 **需要从声明在此类中的另一个方法中调用一个类方法** ，则 **必须** 使用 **关键字this**（例如。this.doSomething(5)）。在其他的编程语言中，在类的代码中使用this是可选的，但如果没有显式地使用this,TypeScript编译器将会抱怨服务找到该方法。
     
     我们将公共的设置器（setter）和访问器（getter）方法添加到Person类，以设置并获取_ssn的值。
     
     ```
     class Person {
       //在这一版中，构造函数的最后一个参数是可选的（_ssn?）
       constructor(public firstName: string, public lastName: string, public gae: number, private _ssn?: string) {
       }
       
       //访问器方法
       get ssn(): string {
         return this._ssn;
       }
       
       //设置器方法
       set ssn(value: string) {
         this._ssn = value;
       }
     }
     var p = new Person("John", "Smith", 29);
     // 创建Person对象的实例后，使用ssn设置器将该值赋给_ssn
     p.ssn = "456-70-1234";
     
     console.log("Last name: "  + p.lastName + "SSN: " + p.ssn);
     ```
     在上面代码中，访问器和设置器不包含任何应用程序逻辑；但是在实现的应用程序中，这些方法会执行验证。例如，访问器和设置器中的代码可以检查调用者是否被授权获取或设置_ssn值。
     
     >**注意**
     从ES5规范开始，JavaScript也支持访问器和设置器。
     
     请注意，在这些方法中 **使用了关键字this来访问对象的属性** 。 **这在TypeScript中是强制性的** 。
     
   - ##### 继承
   
     JavaScript **支持基于原型对象的继承**，其中一个对象可以使用另一个对象作为原型。像ES6及其他面向对象语言一样，TypeScrript具有用于类继承的关键字extends。但是，在转码为JavaScript的过程中，**生成的代码会使用原型继承的语法** 。下面代码显示了如何创建一个Employee类，它扩展了Person类。
     ```
     class Person {
       constructor(public firstName: string, public lastName: string,, public age： number, private _ssn: string) {
       }
     }
     
     class Employee extends Person {
     }
     ```
     下面代码可以看到转码后的JavaScript版本，它使用原型继承。
     ```
     var __extends = (this && this.__extends) || (function () {
       var extendStatics = Object.setPrototypeOf ||
        ({ __proto__: [] } instanceof Array && function (d, b) { d.__proto__ = b; }) ||
        function (d, b) { for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p]; };
       return function (d, b) {
        extendStatics(d, b);
        function __() { this.constructor = d; }
        d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
       };
     })();
     var Person = /** @class */ (function () {
        function Person(firstName, lastName, age, number, _ssn) {
          this.firstName = firstName;
          this.lastName = lastName;
          this.age = age;
          this._ssn = _ssn;
        }
        return Person;
     }());
     var Employee = /** @class */ (function (_super) {
        __extends(Employee, _super);
        function Employee() {
           return _super !== null && _super.apply(this, arguments) || this;
        }
        return Employee;
     }(Person));
     ```
     **此代码的TypeSccript版本更简洁且易于阅读。**
     
     我们来为Employee类添加一个构造函数和department属性。
     
     ```
     class Employee extends Person {
       
       //声明属性department
       department: string;
       
       //创建一个具有额外的department参数的构造函数
       constructor(firstName: string, lastName: string, age: number, _ssn: string, department: string) {
         //声明构造函数的子类必须调用父类的构造函数
         super(firstName, lastName, age, _ssn);
         
         this.department = department;
       }
     }
     ```
     如果要在子类类型的对象上，调用一个声明在父类中的方法，**可以使用此方法的名** ，**就像它被声明在子类中一样** 。但是，有时 **要专门调用父类的方法** ，这正是 **使用关键字super** 的时候。
     
     关键字super可以有两种使用方式。在 **派生类的构造函数中** ，**将其作为一个方法进行调用** 。还可以 **使用关键字super来专门调用父类的方法** 。这 **通常被用于方法重写**（method overriding）。例如，如果父类及子类都要doSomething()方法，则子类 **可复用编码在父类中功能** ，并且 **还可以添加其他的功能** ：
     
     ```
     doSomething() {
      super.doSomething();
      // Add more functionality here
     }
     ```
- #### 泛型
 
   TypeScript **支持参数化类型** ，也称为 **泛型**（generics） ，它可以用于各种场景。例如，可以创建一个函数，它 **能够使用任何类型的值** ，但 **在特定的上下文调用时** ，**能够明确地指定具体的类型** 。 
   
   举另一个例子：一个数组可以保存任何类型的对象，但是可以指定该数组中允许特定对象类型（例如,Person的实例）。如果试图添加不同类型的对象，TypeScript编译器将生成错误。
   以下代码片段声明了一个Person类，创建了它的两个实例，并将它们存储在使用泛型类型声明的数组workers中。通过将它们放在 **尖括号中** （例如，`<Person>` ）来表示泛型。
  
  ```
  class Person {
    name: string;
  }
  
  class Employee extends Person {
    department: number;
  }
  
  class Animal {
    breed: string;
  }
  
  var workers: Array<Person> = [];
  workers[0] = new Person();
  workers[1] = new Employee();
  workers[2] = new Animal(); //compile-time error
  ```
  在此代码片段中，声明了Person、Employee和Animal类，以及一个带有泛型类型<Person>的数组workers。通过这么做，表示计划 **仅存储Person类或其子类** 。尝试将一个Animal实例存进相同的数组，将导致编译时错误。
  
  如果在一个拥有动物员工（如警犬）的组织里工作，可以如下更改数组workers的声明：
  
  ```
  var workers: Array<any> = [];
  ```
   可以为任意对象或函数使用泛型类型吗？不行。对象或函数的创建者必须允许这一特性。如果在GitHub上（参见 http://mng.bz/I3V7 ），打开TypeScript的类型定义文件（lib.d.ts）,并搜索“interface Array”,将会看到Array的声明，如下所示。
   
   ```
        /////////////////////////////
    /// ECMAScript Array API (specially handled by compiler)
    /////////////////////////////
    interface Array<T> {
        /**
          * Gets or sets the length of the array. This is a number one higher than the highest element defined in an array.
          */
        length: number;
        /**
          * Returns a string representation of an array.
          */
        toString(): string;
        toLocaleString(): string;
        /**
          * Appends new elements to an array, and returns the new length of the array.
          * @param items New elements of the Array.
          */
        push(...items: T[]): number;
        /**
          * Removes the last element from an array and returns it.
          */
        pop(): T;
   ```
   第四行中的 `<T>` ，意味着 **TypeScript允许使用Array声明类型参数** ，并且编译器 **将检查程序中提供的特定类型** 。上面代码将泛型参数 `<T>` 指定为`<Person>` 。但是，由于 **ES6不支持泛型** ，因此在转码器 **生成的代码中将看不到它们** 。对于开发者，**它只是额外的编译时的安全网** 。
    
   在上面代码中，在第18行可以看到另一个T。当 **使用函数参数指定泛型类型** 时，**不需要尖括号** 。但在TypeScript中，实际上没有类型T。这里的T,意指方法push()可以将特定类型的对象推入一个数组中，如下所示：
    ```
    workers.push(new Person());
    ```
  在本节中，我们说明了使用泛型类型的一个用例，其中包含已经支持泛型的数组。
  也可以 **创建自己的支持泛型的类或函数** 。在代码中的某个地方，如果要尝试调用方法saySomething(),但提供了错误的类型，TypeScript编译器将给出错误。
    
      ```
      function saySomething<T>(data: T) {

      }

      //用一个字符串替换T
      saySomething<string>("Hello");

      //产生一个编译错误，因为123不是以字符串
      saySomething<string>(123);
      ```
  **生成的JavaScript将不会包含任何泛型信息** ，上面的代码将被转码成如下代码：
     
     ```
     function saySomething(data) {
     }
     saySomething("Hello");
     saySomething(123);
     ```
  如果想深入学习泛型，请参阅TypeScript手册（详见 http://mng.bz/447K ）的Generics部分。
  
- #### 接口

  JavaScript **不支持接口的概念** ，在其他的面向对象语言中，接口被 **用来引入API必须准守的代码契约**（code contract）。契约的一个例子可以是，类X声明它实现了接口Y。 **如果类X不包括接口Y中声明的一个方法是实现，被认为是违反了契约** ，并且不会被编译。
  
  TypeScript包含关键字 **interface** 和用于支持接口的 **implements**,但 **接口不会转码为JavaScript代码** 。它们 **只是帮助你避免在开发过程中使用错误的类型** 。
  
  在TypeScript中，使用接口的模式有两种：
  
  - **声明一个接口，它定义了一个包含一些属性的自定义类型。** 然后，声明一个具有这种类型的参数的方法。当此方法被调用时，编译器将检查作为参数给出的对象是否包含所有在该接口中声明的属性。
  
  - **声明一个包含（未实现的）抽象方法的接口。** 当一个类声明它implements此接口时，该类必须提供所有抽象方法的实现。
  
  - ##### 声明自定义类型
    
    使用接口声明自定义类型
    
    当使用JavaScript框架时，可能会遇到需要某中配置对象作为函数参数的API。要弄明白此配置对象中的哪个属性必须提供，需要打开此API的文档或者阅读该框架的源码。在TypeScript中，可以 **声明一个接口** ，**它包含了配置对象中必须存在的所有属性及其类型** 。
    
    我们看看如何在Person类中完成 这些，它包含一个构造函数，有四个参数：firstName、lastName、age和ssn。这一次，将声明一个包含四个成员的接口IPerson，并且将修改Person类的构造函数，以将此自定义类型的对象用作参数。
    
    ```
    interface IPerson {
      firstName: string;
      lastName: string;
      age: number;
      
      //声明接口IPerson,将ssn作为可选成员（注意问号）
      ssn?: string;
    }
    
    class Person {
      
      //类Person有一个构造函数，它带有一个类型为IPerson的参数
      constructor(public config: IPerson) {
      }
    }
    
    //创建一个成员与IPerson兼容的对象字面量aPerson
    var aPerson: IPerson = {
      firstName: "John",
      lastName: "Smith",
      age: 29
    }
    
    //实例化Person对象，提供一个IPerson类型的对象作为参数
    var p = new Person(aPerson);
    console.log("Last name: " + p.config.lastName);
    ```
    TypeScript **具有结构化的类型系统** ，这意味着如果两个不同的类型包含相同的成员，这些类型将被认为是兼容的。拥有相同的成员，意思是这些成员具有相同的名称和类型。在上面代码中，即使不指定变量aPerson的类型，它也仍会被认为是与Person兼容的，而且当实例化Person对象时，可以被用作构造函数参数。
    
    如果改变了IPerson中一个成员的名称或类型，TypeScript将会报错。另一方面，如果尝试实例化一个Person，其中包括一个对象，它拥有IPerson所有必需的成员和一些其他的成员，则不会引发红色标识（错误）。可以将以下对象作为Person的构造函数的一个参数：
    ```
    var anEmployee: IPerson = {
      firstName: "John",
      lastName: "Smith",
      age: 29,
      department: "HR"
    }
    ```
    在接口IPerson中，没有定义成员department。但是，只要该对象拥有接口中列出的其他全部成员，就满足契约条款。
    
    接口IPerson没有定义任何方法，但是，TypeScript接口可以包括没有实现的方法签名。
    
  - ##### 关键字implements
  
     使用关键字implements
  
     **关键字implements与类声明一起使用** ，以声明该类将实现的特定的接口。假如接口IPayable的声明如下：
     
     ```
     interface IPayable {
       increase_cap: number;
       
       increasePay(percent: number): boolean
     }
     ```
     现在，类Employee可以声明它实现了IPayable:
     
     ```
     class Employee implementsd IPayable {
       // The implementation goes here
     }
     ```
     在进入实现细节之前，我们来回答下面这个问题：“为什么不把所有必需的代码都写在类中，而将一部分代码隔离到接口中？”我们假设，要编写一个应用程序，它可以让你为你的组织的雇员增加薪水。可以创建一个Employee类（扩展自Person类），并包含increaseSalary()方法，然后，业务分析人员可能要求新增为外包人员增加工资的功能，他们为你们公司工作。但是，外包人员用他们自己公司名称和ID表示，他们没有工资的概念，并且按小时付费。
     
     可以创建另一个类Contractor（不是继承自Person类），其中包括一些属性和increaseHourlyRate()方法 。现在，你有了两个不同的API：一个用于增加员工的工资，另一个用于增加外包人员的工资。更好的解决方案是：**创建一个通用得我接口IPayable,并让Employee和Contractor类为各自提供不同的IPayable实现** ，如下所示：
     
     ```
     interface IPayable {
       //接口IPayable包括方法increasePay()的签名，它将被Employee和Contractor实现
       increasePay(percent: number): boolean
     }
     
     class Person {
       //Person类作为Employee的基类
       // properties are omitted tor brevity 
       constructor() {
       }
     }
     
     //Employee类继承自Person类，并实现了接口IPayable。一个类可以实现多个接口
     class Employee extends Person implements IPayable {
     
       //Employee 类实现了increasePay()方法。员工的工资可以增加任意金额。所以此方法只是在控制台打印消息，并返回true。（允许增加）
       increasePay(percent: number): boolean {
         console.log("Increasing salary by " + percent);
         return true;
       }
     }
     
     class Contractor implements IPayable {
      
       //Contractor类包含一个属性，它将加薪的上限设置为20%
       increaseCap: number = 20;
       
       increasePay(percent: number): boolean {
         
         //Contractor类中increasePay()的实现有所不同。使用大于20的参数调用increasePay()，返回消息“Sorry”和返回值false
         if (percent < this.increaseCap) {
           console.log("Increasing hourly rate by " + percent);
           return true;
         } else {
           console.log("Sorry, the increase cap for contractors is ", this.increaseCap);
           return false;
         }
       }
     }
     
     //声明一个带有泛型<IPayable>的数组，允许放置IPayable类型的任何对象（但请参阅下方注解）
     var workers : Array<IPayable> = [];
     workers[0] = new Employee();
     workers[1] = new Contractor();
     
     //现在，可以在数组workers中任何对象上调用increasePay()。请注意，不要对带有单个参数的worker的箭头函数表达式使用括号
     workers.forEach(worker => worker.increasePay(30));
     ```
     运行上面代码，会在浏览器的控制台产生如下输出：
     
     ```
     Increasing salary by 30
     Sorry, the increase cap for contractors is  20
     ```
     >**为什么要使用关键字inplements声明类？**
     上面代码说明了 **TypeScript的结构化的子类型** 。如果从Employee或Contractor的声明中删除implements IPayable，代码仍然可以工作，而且编译器不会对将这些添加到workers数组的几行代码报错。编译器足够聪明地看到，虽然该类没有显示地声明implements IPayable，但它正确地实现了increasePay()。
     但是，如果删除了implements IPayable，并尝试修改任何一个类中increasePay()方法的签名，就不能将这个对象放到workers数组中，因为对象不再是IPayable类型。此外，没有关键字implements,IDE支持（例如重构）将会被破坏。
     
 - ##### 使用可调用接口
   
   TypeScript具有一个有趣的特性，叫做 **可调用接口**（callable interface）,它包含一个 **裸函数签名** （bare function signature, 不带函数名称的签名）。以下示例显示了一个裸函数签名，它接收一个number类型的参数并返回一个boolean值：
   
   ```
   (percent: number): boolean;
   ```
   该裸函数签名表示 **此接口的实例是可调用的** 。咋下面代码中，将展示声明IPayable的不同版本，其中包含一个裸函数签名。为了简洁起见，为此例中删除了继承。将会声明独立的函数，用于实现员工及外包人员的工资增加规则。这些函数将作为参数传递，并会被Person类的构造函数调用。
   
   ```
   //包含一个裸函数签名的可调用接口
   interface IPayable {
     (percent: number): boolean;
   }
   
   class Person {
    
    //Person类的构造函数，将可调用接口IPable的一个实现作为一个参数
    constructor(private validator: IPayable) {
    }
    
    //increasePay()方法调用传入的IPayable实现上的裸函数，提供用于验证的工资增加值
    increasePay(percent: number): boolean {
      return this.validator(percent);
    }
   }
   
   //通过使用箭头函数表达式，实现了员工工资增加规则
   var forEmployees: IPayable = (percent) => {
     console.log("Increasing salary by ", percent);
     return true;
   }
   
   //通过使用箭头函数表达式，实现了外包人员工资的增加规则
   var forContractors: IPayable = (percent) => {
     var increaseCap: number = 20;
     
     if (percent < increaseCap) {
       console.log("Increasing hourly rate by " + percent);
       return true;
     } else {
       console.log("Sorry, the increase cap for contractors is ", increaseCap);
       return false;
     }
   }
   
   var workers : Array<Person> = [];
   
   //实例化两个Person对象，传入不同的加薪规则
   workers[0] = new Person(forEmployees);
   workers[1] = new Person(forContractors);

   //调用每个实例上的increasePay()，验证加薪30%
   workers.forEach(worker => worker.increasePay(30));
   ```
   运行上面代码，将会在浏览器的控制台生成以下输出：
   
   ```
   Increasing salary by  30
   Sorry, the increase cap for contractors is  20
   ```
     >**将类作为接口**
     在TypeScript中，可以将任何类看作接口。如果声明了类A{}和B{},写作class A implements B {}是完全合法的。
     当转码成JavaScript时，**TypeScript接口不会生成任何输出** 。而且如果在一个单独的文件（例如iplayable.ts）中放入一个接口声明，并使用tsc编译它，将生成要给空的ipayable.js文件。如果使用SystemJS从文件（例如ipayable.js）导入此接口，将会报错，因为不能导入空文件。需要让SystemJS知道，必须将IPayable作为模块，并在全局的System注册表中注册它。这可以通过在配置SystemJS时使用meta注解来完成，如下所示：
     ```
     Symtem.config({
       transpiler: 'typescript',
       typescriptOPtions: {emitDecoratorMetadata: true},
       packages: {app: {defaultExtension: 'ts'}}
       meta: {
         'app/ipayable.ts': {
          format: 'es6'
         }
       }
     });
     ```
     接口机制提供了创建自定义类型的一种方式，并最小化了类型相关错误数量。此外，它极大地简化了依赖注入设计模式的实现。
     
     可以在TypeScript手册（详见 http://mng.bz/spm7 ）的“Interfaces”小节找到更多详情。
     
     >**注意**
     实用工具TypeDoc是一个便捷的工具，用于根据TypeScript代码中的注释生成程序文档，可以从 www.npmjs.com/package/typedoc 获得。
     
- #### 使用注解添加类元数据

  术语 **元数据** （metadata）有着不同的定义。流行的定义是，元数据是 **有关数据的数据** 。我们认为 **** 元数据是描述代码的数据** 。TypeScript装饰器提供了一种为代码添加元数据的方式。特别地，要 **将TypeScript类转换为Angular组件**，可以 **使用元数据注解它** 。注解以@符号开头。
  
  要将TypeScript类转换为Angualr UI组件，需要使用注解@Component装饰它。**Angular将内部解析注解并生成代码** ， 这会将所需的行为添加到此TypeScript类：
  
  ```
  @Component({
    //这里包含选择器名称，以在HTML文档中识别组件
    
    //提供具有HTML片段的template属性来渲染组件。组件的样式也在这里
    //
    //
  })
  
  class HelloWordComponent {
    //实现组件的应用程序逻辑的代码在这里
    //
  }
  ```
  当使用注解时，应该有一个 **注解处理器** ，它可以 **解析注解的内容** ，并将其 **转换成运行时（浏览器的JavaScript引擎）可以理解的代码** 。**Angular编译器ngc** 执行注解处理器的职责。
  
  **要使用Angular支持注解** ，**需要将它们的实现导入到应用程序中** 。例如，需要从Angular模块带入@Component注解。
  
  ```
  import { Component } from 'Angular 2/core'
  ```
  虽然这些注解的实现是在Angular中完成的，但可能需要一个标注化的机制来创建自己的注解。这就要用到 **TypeScript装饰器** 。Angular提供了它的注解，用于装饰代码，但 **TypeScript允许在装饰器的支持下创建自己的注解** 。
  
- #### 类型定义文件
   
   几年来，TypeScript定义文件的一个大型代码库—— **DefinitelyTped**  ，一直是 **新的ECNAScript API** 以及 **数百个用JavaScript编写的流行框架和库** 的唯一来源。这些（定义）文件的目的，是 **让TypeScript编译器知道这些库的API所期望的类型** 。虽然代码仓库definitelytyped.org仍然存在，但是npmjs.org成为类型定义文件的新的代码仓库。而且我们在所有代码示例中都使用了它。
   
   任何定义文件名称的后缀都是d.ts，而且在运行npm install之后，可以在node_modules/@Angular的子目录中找到定义文件。
   
   所有必需的*.d.ts文件，都与Angular的npm包打包在一起，并且不需要单独安装它们。项目中定义文件的存在，**将允许TypeScript编译器确保代码在调用Angular API时，使用正确的类型** 。
   
   例如，通过调用bootstrapModule()方法启动Angular应用程序，会将应用程序的根模块作为参数传给它。文件application_ref.d.ts包含此方法的如下定义：
   
   ```
   bootstrapModule<M>(moduleType: ConcreteType<M>),
   compilerOptions?: CompilerOPtions | CompilerOptions[]): Promise<NgModuleRef<M>>;
   ```
   通过阅读此定义，你和编译器tsc了解到，可以使用一个ConcreteType类型的必选参数和一个可选的编译器选项数组来调用该方法。如果application_ref.d.ts文件不是项目的一部分，TypeScript可能会允许使用错误的参数类型调用bootstrapModule()方法，或者根本不带任何参数，这将会导致运行时错误。但是由于application_def.d.ts存在，因此TypeScript会生成编译时错误提示“Supplied parameters do not match any signature of call target”。当编写调用Angular函数或为对象属性赋值的代码时，类型定义文件还允许IDE显示上下文相关的帮助。
   
  - ##### 安装类型定义文件
    
    要为使用JavaSript编写的库或框架安装类型定义文件，开发者需要使用类型定义管理器：tsd和Typings。前者被弃用了，因为它仅允许从definitelytyped.org获取*.d.ts文件。在TypeScrit2.0发布之前，使用Typings(详见 https://github.com/typeings/typeings ),它允许从任意的代码仓库引入类型定义（文件）。
    
    随着TypeScript2.0的发布，**不再需要为基于npm的项目使用类型定义管理器了** 。现在，npmjs.org的npm仓库包含了一个@types组织，里面存储着所有流行的JavaScript库的类型定义（文件）。来自definitelytyped.org的所有库都发布在那里。
    
    假设需要为jQuery安装类型定义文件。运行以下命令，将在目录node_modules/@types中安装类型定义（文件），并将此依赖保存到项目的package.json文件中：
    
    ```
    npm install @types/jquery --save-dev
    ```
    在许多项目中都将使用类是命令安装类型定义（文件）。例如，ES6为数组引入find()方法，但如果TypeScript项目将ES5配置成编译目标，IDE将会用红色高亮显示find()方法，因为ES5不支持它。安装es6-shim类型定义文件将消除IDE中的红色（提示）：
    ```
    npm i @types/es6-shim --save-dev
    ```
    >**如果tsc找不到类型定义文件怎么办？**
    在TypeScript2.0时，tsc有可能找不到位于目录node_modules/@types中的类型定义文件。如果遇到这个问题，请在tsconfig.json文件的types部分添加所需的文件。以下是一个例子：
    ```
    "compilerOptions": {
      ...
      "types": ["es6-shim", "jasmine"],
    }
    ```
    >**模块解析及引用标签**
    除非使用CommonJS模块，否则需要显式地为代码添加要给引用，以指向所需的类型定义（文件），如下所示：
    ```
    /// <reference types="typings/jquery.d.ts" />
    ```
    >使用CommonJS模块作为一个tsc选项，而且每个项目的tsconfig.json文件包含如下选项：
    ```
    "module": "commonjs"
    ```
    >当tsc看到一条指向某个模块的import语句时，它会自动尝试在node_modules目录中寻找 `<module-name>`d.ts文件。如果没有找到，它会向上一级重复此过程。可以在TypeScript手册（参见 http://mng.bz/ih4z）的“Typings for npm Modules”小节，阅读更多这方面的信息。在即将发布的tsc版本中，将会实现相同的策略，用于MD模块解析。
    
    Angular包含所有需要的类型定义文件，并且不需要使用类型定义管理器，除非应用程序使用了其他第三方的JavaScript库。在这种情况下，就需要手动安装它们的（类型）定义文件， **以获得IDE的上下文帮助** 
    
    Angular在其d.ts文件中使用了ES6语法，而且对于大多数模块，可以使用以下导入语法：import {Component} from 'Angular 2/core'。你将会找到Component类的定义，并且将导入其他的Angular模块和组件。
    
  - ##### TSlint控制代码风格
  
    使用TSlint控制代码风格
  
    TSLint是一个工具，可以 **用来保证程序的编写符合指定的规则和代码风格**。可以配置TSLint来 **检查项目中的TypeScript代码** ，**检查是否正确对齐和缩进** ，**所有的接口名称是否都是以大写的I开头** ，以及 **类名是否都使用CameCase表示法** ，等等。
    
    可以使用如下命令全局安装TSLint：
    
    ```
    npm install tslint -g
    ```
    要在项目目录中安装TSLint，请使用以下命令：
    
    ```
    npm install tslint
    ```
    要应用于代码的规则可以在配置文件tslint.json中指定。TSLint附带一个示例的规则文件，该文件的名称是sample.tslint.json,它位于docs目录中。可以根据需要打开或关闭具体的规则。
    
    有关使用TSLint的详细信息，请访问 www.npmjs.com/package/tslint 。IDE可能支持开箱即用。
    
    >**IDE**
    有几个IDE支持TypeScript，最流行的是 **WebStorm** 、**Visual Studio Code** 、 **Sublime Text** 和 **Atom** 。所有这些IDE和编辑器都剋在Windows、Mac OS和Linux（环境）工作。如果是在Windows电脑上开发TypeScript/Angular应用程序，可以使用 **Visual Studio 2015** 。
    
- #### 开发流程概述

  TypeScript/Angular开发流程概述

  TypeScript/Angular应用程序的开发和部署流程由多个步骤组成，它们应该能够自动化。有多种方法可以做到这一点，以下步骤列表可以用来创建Angular应用程序。
  
  1. 为项目创建一个目录
  
  2. 创建一个package.json文件，其中列出了应用程序所有的依赖项，例如Angular包、测试框架Jasmine，等等。
  
  3. 使用npm install 命令，安装所有在package.json中列出的包和库。
  
  4. 编写应用程序代码。
  
  5. 通过SystemJS加载器的帮助，将应用程序加载到浏览器中，此加载器不仅可以加载应用程序，还可以在浏览器中将TypeScript转换成JavaScript。
  
  6. 在Webpack及其他插件的帮助下，压缩并打包代码和资源。
  
  7. 使用npm脚本将所有的文件复制到分发目录中。
  
  >**注意** 
  Angular CLI是一个命令行实用工具，可以作为项目的骨架，生成组件和服务，并准备构建。
  
  >**注意**
  由于TypeScript是JavaScript的超集，因此错误处理（的方式）与JavaScript相同。在Mozilla Developer Network （参见 http://mng.bz/FwfO ）上，可以在Error构造函数相关的JavaScriptTeference文章中，阅读有关不同的错误类型的信息。
  
  
    
   
    
  
    
     
   
   
   
     
     
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  