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
  
 - #### TypeScript作为JavaScript的超集
 
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
  
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  