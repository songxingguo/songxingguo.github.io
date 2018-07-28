title: 《Angular 2开发实战》学习笔记-ECMAScript 6概述
author: songxingguo
tags:
  - ES6
categories:
  - 前端技术
  - 读书笔记
date: 2018-07-25 12:04:00
---
### ECMAScript 6概述


  - #### ECMASript的故事
  
    ![ECMASript的故事](http://p9myzkds7.bkt.clouddn.com/Angular/ECMAScript%E7%9A%84%E6%95%85%E4%BA%8B.png)
    <!-- more -->
   
    可以访问ECMAScript兼容性网站 http://mng.bz/ao59 **查看截至目前ES6的支持情况** 。
    
    使用转码器，比如 **Traceur** (详见 http://github.com/google/traceur-compiler ）或 **Babel**（详见 https://babeljs.io ）,将ES6转换成ES5的版本，所有浏览器都支持ES5标准。
    
    学习JavaScript的github地址为 http://mng.bz/EIH 。
    
    2016年发布了ES7标准（又称ES 2016）,这是一次小型发布会。ESMAScript 2017语言标准发布在 http://tc39.github.io/ecma262/ 上。
    
    ES6并 **不会废弃任何旧的语法** ，因此可以在最新的Web浏览器或独立的JavaScript引擎中安全地运行遗留的ES5或ES3。
    
  - #### 模板字面量
  
    ES6引入了一种新的语法，**用来处理包含了嵌入式语法的字符串字面量** ，此功能被称为  **字符串插值** （string interpolation）。
   
    在ES5中，可以使用 **字符串拼接** 创建一个包含字符串字面量与变量的混合字符串。
   
     ```
     var customerName = "John Smith";
     console.log("Hello" + customerName);
     ```
    在ES6中，**模板字面量被反引号包围**。可以将 **表达式嵌入文字** 中，将其 **放在以美元符号为前缀的大括号之间**。在下面代码片段中，变量custimerName的值被嵌入到字符串字面量中：
   
     ```
     var customerName = "John Smith";
     console.lgo(`Hello ${customerNamr}`);
     
     function getCustomer() {
       return "Allan Lou";
     }
     console.log(`Hello ${getCustomer()}`);
     ```
     上述代码的输出为：
     ```
     Hello John Smith
     Hello Allan Lou
     ```
     **在大括号之间可以使用任何有效的JavaScript表达式。**
     
    - ##### 多行字符串
  
      在代码中字符串可以被写成多行。**使用反引号可以编写多行字符串** ，而无须把它们拼接在一起或者使用反斜杠：
    
      ```
      var message = `Please enter a password that
                has at least 8 characters and
                includes a capital letter`;

      console.log(message);
      ```

      生成的字符串会把所有空格也视为字符串内容的一部分，因此输出如下所示：

      ```
      Please enter a password that 
                has at least 8  characters and 
                includes a capital letter
      ```
    - ##### 带标签的模板字符串
      
      如果一个 **模板字符串紧跟在一个函数名称引用后面** ，那么这个字符串首先 **会被计算** ，之后 **被传入到函数中做进一步处理** 。**模板字符串部分会被组成一个数组** ，在模板中 **经过计算的表达式会被当成独立的参数** 传入到函数中。这种语法看起来有些不同，因为在常规的函数调用中不会使用括号：
      
      ``` 
      mytag`Hello ${name}`;
      ```
      下面介绍 **如何根据region变量打印出带有货币符号的金额** 。如果region变量的值为1，那么金额不变，在金额前面加一个美元符号。如果region变量的值为2，则需要转换金额，以汇率计算，并在前面加一个欧元符号。模板字符串看起来如下所示：
      
      ```
      `You've earned ${region} ${amount}!`
      ```
      调用currencyAdjustment()函数，带标签的模板字符串看起来如下所示：
      
      ```
      currencyAdjustment`You've earned ${region} ${amount}!`
      ```
      currencyAdjustment()函数接收三个参数： 第一个参数是 **模板字符串中所有的字符串** ，第二个参数获得 **region的值** ，第三个参数获得 **amount的值** 。**第一参数之后的参数的数量可以是任意** 。完整的示例如下面的代码清单所示：
      
      ```
      function currencyAdjustment(stringParts, region, amount) {
        console.log(stringParts);
        console.lgo(region);
        console.log(amount);
        
        var sign;
        if(sign==1) {
          sign = "$"
        } else {
          sign = '\u20AC';
          amount = 0.9*amount;
        }
        return `${stringParts[0]} ${sign}${amount}${stringParts[2]}`;
      }
      
      var amount = 100;
      var region = 2;
      
      var message = currencyAdjustment`You've earned ${region} ${amount}!`
      console.log(message);
      ```
      currencyAdjustment()函数接收一个内嵌了region和amount的字符串，并解析这模板，**将字符串的部分与这些值分开（空格也被认为是字符串的一部分）** 。为了方便说明在程序开始会打印模板解析的结果，之后函数将会检查region，开始货币转换，返回一个新的字符串模板。运行代码会产生如下输出：
      
      ```
      ["You've earned "," ","!"]
      2
      100
      You've earned €90!
      ```
      有关带标签的模板字符串的更多详细信息，请参阅由AxelRauschmayer编写的Exploring ES6一书中的Template Literals章节，可从 http://exploringjs.com 获取。
      
  - #### 可选参数和默认值
  
     在ES6中，可以 **为函数参数指定默认值** ，这对于当函数被调用却没有传参数的情况是非常有用的。假设有一个计算税费的函数，它有两个参数：一个是全年的收入（income）;另一个是居住的州（state）。如果没有传state的值，你希望能够使用Florida。
     在ES5中，需要在函数体开始的时候检查是否提供了state值，如果没有，则使用Florida：
     
     ```
     function calcTaxES5(income, state) {
     
       state = state || "Florida";
       
       console.log("ES5. Calculating tax for the resident of " + state +
                                "with the income " + income);
     }
     
     calcTaxES5（50000);
     ```
     下面是代码的输出结果：
     
     ```
     "ES5. Calculating tax for the resident if Florida with the income 50000"
     ```
     在ES6中，需要 **在函数签名中指定默认值** ：
     
     ```
     function calcTaxES6(income, state = "Florida") {
      console.log("ES6. Calculating tax for the resident of " + state + 
                               "with the income " + income);
     }
     
     calcTaxES6(50000);
     ```
     输出与上面看起来是一样的：
     
     ```
     "ES6. Calculating tax for the resident if Florida with the income 50000"
     ```
     除了能够为可选参数提供硬编码的默认值之外，甚至可以 **把函数的返回值作为默认值** ：
     
     ```
     function calcTaxES6(income, state = getDefaultState()) {
       console.log("ES6. Calculating tax for the resident of " + state +
                                "with the income " + income);
     }
     
     function getDefaultState() {
       return "Florida";
     }
     ```
     请记住，每次调用calcTaxES6()同样 **也会调用getDefaultState()函数** ，这  **可能会带来性能问题** 。这个可选参数的新语法能够让你 **编写更少的代码** ，并  **使代码更好理解** 。
     
  - #### 变量的作用域
  
    ES5中作用域机制相当混乱。无论在哪里使用var声明的变量，**声明都会被移动到作用域的顶部** ，这被称为 **变量提升**（hoisting）。 this 关键字的使用也不像Java或C#语言那么简单。
    
    ES6通过引入 **关键字let** 来 **消除变量提升带来的混乱** ，通过 **箭头函数** 来 **解决this带来的混乱** 。
    
    - ##### 变量提升
    
      在JavaScript中，**所有的变量声明都会被移到顶部** ，并且 **没有块级作用域** 。看看下面的简单示例，其中 **在for循环中声明了变量i** ,但是 **在循环外面仍然可以使用**。
      
      ```
      function foo() {
        for (var i = 0; i < 10; i++) {
          
        }
        
        console.log("i=" + i);
      }
      
      foo();
      ```
      运行代码后会打印i=10。变量i在循环外仍然可以被访问，尽管它看起来似乎只能在循环内部调用。**JavaScript自动把变量的声明提升到顶部** 。
      
      在这个例子中，因为只有一个变量被命名为i，提升不会引起任何问题。如果在函数内部和外部同时声明名字一样的两个变量，就 **可能会造成代码逻辑上的混乱** 。思考一下下面代码，在 **全局作用域内声明了变量customer** 。之后在 **本地作用域内同样引入一个customer变量** ，但是 **会把引入的这个变量注释掉** 。
      
      ```
      <!DOCTYPE html>
      <html>
      <head>
        <title>hoisting.html</title>
      </head>
      <body>
      
      <script>
        "use strict";
        
        var customer = "Joe";
        
        (function() {
          console.log("The name of the customer inside the function is " +                 customer);
          
          /* if(2 > 1) {
            var customer = "Mary";
          }*/
        })();
        
        console.log("The name of the customer outside the function is " +               customer);
      </script>
      
      </body>
      </html>
      ```
      使用Chrome浏览器打开这个文件，查看Chrome Developer Tools中控制台输出。正如我们所期望的，全局变量customer在函数的内部和外部都可见的,如下图所示。
      
      ![变量声明被提升](http://p9myzkds7.bkt.clouddn.com/Angular/%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E%E8%A2%AB%E6%8F%90%E5%8D%87.png)
      
      **取消对if语句的注释**，在大括号中初始化customer变量。现在，有两个名字一样的变量，其中 **一个是全局变量** ，而 **另一个变量则在函数的作用域内** 。刷新浏览器页面，控制台的输出与之前不同了，函数中的customer变量为undefined,如下图所示。
      
      ![变量初始化未被提升](http://p9myzkds7.bkt.clouddn.com/Angular/%E5%8F%98%E9%87%8F%E5%88%9D%E5%A7%8B%E5%8C%96%E6%9C%AA%E8%A2%AB%E6%8F%90%E5%8D%87.png)
      
      这是因为在ES5中，**变量声明** 总是 **被提升到作用域顶部** ，但是 **变量并不会被初始化** 。因此 **第二个没有初始化的变量被提升到了函数顶部** ，console.log()打印的值为 **函数内容部定义的变量的值** ，，而不是全局变量customer的值。
      
      **函数的声明同样会被提升** ，因此 **可以在声明一个函数之前调用它** ：
      
      ```
      doSomething();
      
      function doSomething() {
        console.log("I'm doing something");
      }
      ```
      另一方面，**函数表达式被认为是变量初始化** ，因此 **不会被提升**。在下面的代码片段中，**变量doSomething()的值为undefined** :
      
      ```
      doSomething();
      
      var doSomething = function() {
        console.log("I'm doing something");
      }
      ```
    - ##### let和const
    
      使用ES6关键字let代替var声明变量，能够 **让变量拥有块级作用域** 。下面是一个示例：
      
      ```
      <!DOCTYPE html>
      <html>
      <head>
        <title>let.html</title>
      </head>
      <body>
      
      <script>
        "use strict";
        
        let customer = "Joe";
        
        (function() {
          console.log("The name of the customer inside the function is " +                 customer);
          
          if(2 > 1) {
            let customer = "Mary";
            console.log("The name of the customer inside the block is " +                 customer);
          }
        })();
        
        for (let i =0; i < 5; i++) {
          console.log("i=" + i);
        }
        
        console.log("i=" + i); //prints Uncaught Reference Error: i is not defined
      </script>
      
      </body>
      </html>
       ```
       现在两个customer变量有不同的作用域和值，如下图所示。
       
       ![let的块级作用域](http://p9myzkds7.bkt.clouddn.com/Angular/let%E7%9A%84%E5%9D%97%E7%BA%A7%E4%BD%9C%E7%94%A8%E5%9F%9F.png)
       
       如果循环中声明一个变量，它只是循环中可用：
       
       ```
       for (let i =0; i < 5; i++) {
          console.log("i=" + i);
       }
        
       console.log("i=" + i); //Reference Error: i is not defined
       ```
       在Traceur REPL中测试let关键字，访问Traceur的Transcoding Demo页面（ http://mng.bz/b191 ）,把输入的ES6语法已交互的方式转码为ES5。
       
       简而言之，如果正在开发新的应用程序，请不要使用var,而是使用let。**let关键字允许为变量无限制地分配值** 。
       
       如果要声明一个变量，使其 **在分配了值之后不会被重新分配** ，可以使用const关键词。**常量同样支持块级作用域** 。
       
       let和const的唯一区别是：**const不允许改变已经被分配的值** 。程序中 **优先推荐使用const** ;如果 **变量需要被改变** ，那么 **使用let替换const**。
       
    - ##### 函数的块级作用域
    
      如果在一个块（一对大括号）中声明了一个函数，那么在快外，该函数是不可见的。下面的代码将抛出错误： “doSomething is note defined”。
      
      ```
      {
        function doSomething() {
          console.log("In doSomething");
        }
      }
      
      doSomething();
      ```
      在ES5中，doSomething()声明将会被提升，并打印“In doSomething”。在ES5中并不推荐在块中声明一个函数（参加 http://mng.bz/Bvym 中的 “ES5 Implementtation Best Practice”）,这是因为 **不同浏览器会以不同的方式解析此语法** ，产生不一致的结果。
   
  - #### 箭头函数、this和that
   
     ES6引入了箭头函数表达式，**为匿名函数提供了更短的写法** ，**为this变量添加了词法作用域** 。在其他编程语言（如C#和Java）中，类似的语法被称为lambda表达式。
     
     箭头函数表达式的语法包括 **参数** 、**箭头符号**（=>）、以及 **函数体** 。如果 **函数体只有一个表达式**， **可以不写大括号** 。如果 **一个单一表达式函数返回值** ，那么 **可以省略return语句**，结果被隐式地返回：
     
     ```
     let sun = (arg1, arg2) => arg1 + arg2;
     ```
     如果 **函数体是多行的箭头函数表达式** ，那么 **需要使用大括号闭合函数体** ，并 **使用显式的return语句** ：
     
     ```
     (arg1, arg2) => {
       //do something
       return someResult;
     }
     ```
     如果箭头函数 **没有任何参数** ，**使用空括号** ：
     
     ```
     () => {
       // do something 
       return someResult;
     }
     ```
     如果箭头函数 **只有一个参数** ，**括号不是必需的** ：
     
     ```
     arg1 => {
       //do something
     }
     ```
     在下面的代码片段中，箭头函数作为参数传入reduce()方法用来计算合计，另一个箭头函数被传入filter()方法用来打印偶数：
     
     ```
     var myArray = [1, 2, 3, 4, 5];
     console.log("The sum if myArray elements is " + 
                myArray.reduce((a,b) = > a + b); //pints 15
     console.lgo("The even numbers in myArray are " +
                myArray.filter(value => value % 2 == 0); prints 2 4
     ```
     在ES5中，搞清楚关键字this是那个对象的引用并不是简单的任务。在线搜索“JavaScript this and that”,会发现很多类似的文章，人们都在抱怨this指向了“错误”的对象。基于函数如何被调用以及是否严格模式（参见 http://mng.bz/VNVL 上有关Mozila Developer NetNork的“Strict Mode”）, **this 引用可以有不同的值** 。
     
     考虑thisAndThat.html文件中的代码，它会每秒调用一次getQuote()函数。getQuote()函数会在StockQuoteGenerator()构造函数中为股票代码打印随机生成的价格。
     
      ```
      <!DOCTYPE html>
      <html>
      <head>
        <title>thisAndThat.html</title>
      </head>
      <body>
      
      <script>
        function StockQuoteGenerator(symbol) {
          // this.symbol = symbol; //is undefined inside getQuote()
          
          var that = this;
          that.symbol = symbol;
          
          setInterval(function getQuote() {
            console.log("The price quote for " + that.symbol  
                  + " is " + Math.random());
          },1000);
        }
        
        var stockQuoteGenerator = new StockQuoteGenerator("IBM");

      </script>
      
      </body>
      </html>
       ```
       上述代码中被注释部分，演示了this的错误用法。当函数需要一个值时，虽然看起来是同一个this引用，但实际上并不是，。如果 **没有把this变量的值保存到that中** ，使用setInterval()或回调函数调用getQuote()函数 **得到的this.symbol的值将是undefined** 。在getQuote()中， **this指向全局对象** ，与StockQuoteGenerator构造函数中定义的this不一致。
       
       另一种可能的解决方式是 **使用JavaScriptcall()、apply()或bind()函数** ，以 **确保函数运行在指定的this对象中** 。
       
       **提示：** 如果不了解JavaScript中this问题，详见Richard Bovell的文章“Understand JavaScript s 'this' with Clarity and Master It”（网址为 http://mng.bz/ZQfz ）
       
       fatArrow.html文件演示了箭头函数的解决方案，无须像thisAndThat.html中所做的那样，在that中存储this。
       
      ```
      <!DOCTYPE html>
      <html>
      <head>
        <title>fatArrow.html</title>
      </head>
      <body>
      
      <script>
      
        "use strict";
        
        function StockQuoteGenerator(symbol) {
        
          this.symbol = symbol; 
         
          setInterval(() => {
            console.log("The price quote for " + that.symbol  
                  + " is " + Math.random());
          },1000);
        }
        
        var stockQuoteGenerator = new StockQuoteGenerator("IBM");

      </script>
      
      </body>
      </html>
       ```
       箭头函数被当成参数传入setInterval()中，它 **使用了封闭上下文中的this值** ，因此可以识别this.symbol的值为IBM。
       
     - ##### rest和扩展运算符
       
        在ES5中， **编写可变长度参数** 的函数需要 **使用特殊的arguments对象** 。这个 **对象类似于数组** ，其中包含了对应传递给函数的参数。隐式的arguments变量 **在任何函数中都可以被视为局部变量** 。
        
         ES6有 **rest** 和 **扩展运算符** ，都 **用三个点（...）来表示** 。rest操作符被 **用于为函数传递可变长度参数** ，该操作符必须是 **参数列表中最后一个参数** 。如果函数参数的名字 **以三个点开始** ，函数将会 **以数组的形式得到参数的剩余部分** 。例如，在rest操作符的作用下只需使用一个变量名便可以向函数传递多个customers:
        
         ```
          function processCustomers(...customers) {
            //implementation of the function goes here
          }
         ```
         在这个函数中，可以 **像处理任何数组一样处理customers数据**  。
        
         想象一下，需要编写一个用来计算税费的函数，第一个参数是income，后面根据客户数量，可以又任意数量的参数来表示顾客的名字。下面代码显示了分别使用老语法和新语法处理可变参数。
      
           ```
            <!DOCTYPE html>
            <html>
            <head>
              <title>rest.html</title>
            </head>
            <body>

            <script>

              "use strict";

               //ES5 and arguments object
               function calcTaxES5() {
                 //income是第一个参数
                 console.log("ES5. Calculating tax for customers with the income ", 
                                 arguments[0]);

                 //从第二个元素起提取出一个数组
                 var customers = [].slice.call(arguments, 1);

                 customers.forEach(function(customer) {
                   console.log("Processing ", customer);
                 });
               }

               calcTaxES5(50000, "Smith", "Johnson", "McDonald");
               calcTaxES5(750000, "Smith", "Olson", "Clinton");


               //ES6 and rest operator
               function calcTaxdES6(income, ...customers) {
                 console.log("ES6. Calculating tax for customers with the income ", 
                    income);

                 customers.forEach(function(customer) {
                    console.log("Processing", customer);
                 });
               }

               calcTaxdES6(50000, "Smith", "Johnson", "McDonald");
               calcTaxdES6(750000, "Olson", "Clinton");

            </script>

            </body>
            </html>
           ```
         calcTaxEs5()和calcTaxEs6()函数产生相同的结果：
          
           ```
             ES5. Calculating tax for customers with the income  50000
             Processing  Smith
             Processing  Johnson
             Processing  McDonald
             ES5. Calculating tax for customers with the income  750000
             Processing  Smith
             Processing  Olson
             Processing  Clinton
             ES6. Calculating tax for customers with the income  50000
             Processing Smith
             Processing Johnson
             Processing McDonald
             ES6. Calculating tax for customers with the income  750000
             Processing Olson
             Processing Clinton
          ```
          不过，它们在处理customers时还是有差异的，因为arguments对象并不是一个真正的数组，在ES5中不得不使用slice()和call()方法 **创建一个数组** ，并从arguments中的第二个元素开始提取顾客姓名，放入到新创建的数组中。
          
          ES6则不需要使用这些技巧，rest操作符能够返回一个正常的customers数组。rest参数能 **让代码更简单** ，**可读性更强** 。
          
          如果 **rest操作符** 能够把 **变长参数转换为数组** ，那么 **扩展运算符** 则执行相反的操作； **把一个数组分解到参数列表中** 。假设需要编写一个函数，计算给定收入的三名顾客的税费。这一次，**参数的长度是固定的** ，但是顾客被存放到要给数组中。可以使用扩展运算符——三个点（...）, **把数组分解到独立参数的列表中** 。
         
          ```
            <!DOCTYPE html>
            <html>
            <head>
              <title>spread.html</title>
            </head>
            <body>

            <script>

              "use strict";

               function calcTaxSpread(customer1, customer2, customer3, income) {
                 console.log("ES6. Caclulating tax for customers with the income ", income);
                 console.log("Processing", customer1, customer2, customer3);
               }
               
               var customers = ["Smith", "Johnson", "McDonald"];
               
               //扩展运算符
               calcTaxSpread(...customers, 50000);
            </script>

            </body>
            </html>
           ```
           这个示例中，并不会从customers数组中提取值，然后把这些值作为函数的参数，而是使用扩展运算符处理数组，就好像在对函数说：“你需要三个参数，而我只会给你一个数组，你自己把它们提取出来吧”。注意，作为rest运算符的反向操作，  **扩展运算符不一定必须是参数列表中的最后一个参数** 。
           
     - ##### generator函数
        
        当浏览器执行一个 **常规的JavaScript函数** 时，它 **将会从头一直运行到结尾** ，不会被打断。但是 **generator函数** 执行过程 **可以被多次暂停和恢复** 。generator函数可以控制运行在同一线程中的脚本调用。一旦generator函数中的代码 **执行到关键字yield** ，**函数将会被挂起** ，通过 **调用generator的next()** 能够 **恢复函数的执行** 。
        
        为 **把一个常规函数转换成generator函数** ，需要 **在关键词function和函数名之间添加一个星号** 。下面是一个示例：
        
        ```
        function* doSomething() {
          console.log("Started processing");
          yield;
          console.log("Resumed processing");
        }
        ```
        当调用这个函数时，它并 **不会立即执行函数代码** ，而是会 **返回一个特殊的generator对象** ，它 **可以被认为是一个 迭代器** 。下面的代码 **不会打印任何东西** ：
        
        ```
        var iterator = dosomething();
        ```
        为了开始执行函数体，需要 **调用generator的next()方法** ：
        
        ```
        iterator.next();
        ```
        在这行代码执行后，doSomething()函数将会 **打印“Started processing”** ,并且由于 **执行到yield操作符** ， **函数将暂停执行** 。再次 **调用next()方法** 将会 **打印“Resumed peocessing”** 。
        
        当需要 **编写一个产生数据流的函数** 时，generator函数是非常有用的。想象一下，需要一个函数能够检索和生成指定代码（IBM、MSFT等）的股票价格。如果股票价格第一指定的价格（限价），那么购买这只股票。
        
        下面的generator函数getStockPrice()模拟了这种场景。为了简单起见，它并不会从股票交易所检索价格，而是采用由Math。random()生成的随机数代替。
        
        ```
        function* getStockPrice(symbol) {
          
          while(true) {
            yield Math.random() * 100;
            
            console.log(`resuming for ${symbol}`);
          }
        }
        ```
        如果在yield之后的值，那么它应该被返回给调用函数，但是这个函数并没有执行完。尽管getStockPrice()中有一个无限循环，但只有当调用getStockPrice()的脚本在这个genernator上调用next()时，才会产生（返回）价格，如下所示：
        
        ```
        //创建Generator对象，但是不执行getStockPrice()函数体，getStockPrice将会提供IBM价格的数据流。
        let priceGenerator = getStockPrice("IBM");
        
        //设置限价为$15初始化价格为$100
        const limitPrice = 15;
        let price = 100;
        
        //请求股票价格，直到价格低于$15
        while(price > limitPrice) {
          
          //请求价格并打印到控制台
          price = priceGenerator.next().value;
          console.log(`The generator returned ${price}`);
        }
        
        //如果价格低于$15,循环结束，打印一条关于购买的股票以及购买价格的消息
        console.log(`buying at ${price} !!!`);
        ```
        运行代码，在浏览器的控制台将会打印类似于下面的输出：
        
        ```
         The generator returned 83.18050163853641
         resuming for IBM
         The generator returned 88.87640154512837
         resuming for IBM
         The generator returned 46.90606158470856
         resuming for IBM
         The generator returned 1.1542147358593269
         buying at 1.1542147358593269 !!!
        ```
        注意消息的顺序。当调用priceGenerator的next()方法时，被暂停的getStockPrice方法会被恢复，yield下面的代码会打印出“resuming for IBM”，即使控制流退出函数，之后再次进入函数，getStockPrice()也仍然记录symbol的值为“IBM”。当yield操作符把控制权还给外部脚本后，它将 **会创建堆栈的快照** ，**以便能够记录所有本地变量值** ，即便generator函数恢复，这些值也不会丢。
        
        利用generator函数，可以 **分离某些操作执行 （如获取报价）以及这些操作产生数据的消耗** 。数据使用者可以 **惰性求值** 并且 **决定是否需要请求更多的数据** 。
        
     - ##### 解构
      
       **创建对象** 意味着 **在内存中构造它们** 。**解构**（Destructuring）意味着** 将对象分解** 。在ES5中可以 **编写一个方法来解构任何对象和数组** 。ES6引入了 **解构赋值语法** ，允许通过 **指定匹配模式**（matching pattern），**利用简单的表达式从一个对象的属性或一个数组中提取数据** 。
       
       解构表达式由 **匹配模式** 、**等号**  以及需要分解的 **对象** 或 **数组** 组成。用示例说明会更容易理解。下面会有一个具体示例：
       
       - ###### 解构对象
         
         假设getStock()函数返回一个Stock对象，其中包括symbol和price属性。在ES5中，如过想要把这些属性的值分配给不同的变量，首先需要 **创建一个变量来存储Stock对象**，之后编写两条语句， **把对象属性分配到对应的变量中** ：
         
         ```
         var stock = getStock();
         var symbol = stock.symbol;
         var price = stock.price;
         ```
         在ES6中，只需要在等号左侧 **写一个匹配模式**，并 **把Stock对象分配给它** ：
         ```
         let {symbol, price}= getStock();
         ```
         在等号左侧，你会看到大括号与平时的用法有些不太一样，这是 **匹配表达式语法** 的一部分。当在左侧看到的大括号时，应该把它们认为是 **代码块** 而 **不是对象字面量** 。
         下面的代码演示了如何从getStock()函数中得到Stock对象，并把其解构到两个变量中。
         
         ```
         function getStock() {
           return {
             symbol: "IBM",
             price: 100.00
           }
         }
         
         let  {symbol, price} = getStock();
         
         concole.log(`The price of ${symbol} is ${price}`);
         ```
         运行上面的代码将会打印如下输出：
         
         ```
         The price of IBM is 100
         ```
         换句话说，通过一个 **赋值表达式** ，将 **一组数据**（在本例中是对象属性）绑定到**另一组变量** （symbol和price）中。即使Stock对象除了这两个属性外 **还有其他属性** ，结构表达式也 **仍然可以工作** ，因为symbol和price **满足匹配模式** 。**匹配表达式** 仅列出 **希望提取** 的对象属性对应的变量。
         
         上面代码，只有在变量名称与Stock对象属性名称一致时才会工作。让我们把symbol改变称sym:
        
          ```
          let {sym, price} = getStock();
          ```
         这样输出结果将会发生变化，因为JavaScript并不知道要把对象的symbol属性赋给sym变量：
        
          ```
          The price of undefined is 100
          ```
         这是一个错误的模式匹配示例。如果确实需要用变量sym映射到symbol属性，那么需要  **为symbol分配一个别名** ：
        
          ```
          let {symnol: sym, price} = getStock();
          ```
         如果等号左侧变量的 **数量超过对象属性的数量** ， **额外的变量将会被分配值undefined**。
         如果在等号左侧 **添加了一个StockExchange变量** ，它 **将会被初始化为undefined** ,这是因为getStock()返回的对象中没有同名的属性：
        
        	```
        	let {sym, price, stockExchange} = getStock();
        	console..log(`The price of ${symbol} is ${price} ${stockExchange}`);
        	```
         如果将上面的结构表达式应用到同一个Stock对象上，控制台输出将如下所示：
        
        	```
        	The price of IBM is 100 undefined
        	```
         如果希望stockExchange变量有一个 **默认值** ，比如“NASDAQ”,可以重写解构表达式，如下所示：
        
         ```
         let {sym, price, stockExchange="NASDAQ"} = getStock();
         ```
         **嵌套对象也可以解构。** 在下面代码中创建了一个嵌套对象用来表示MSFT股票，并把其传递给printStockInfo()函数，该函数从对象中提取股票代码这证券交易所的名称。
        
          ```
          let msft = {symbol: "MSFT"，
              lastPrice: 50。00，
              exchange: {
                name: "NASDAQ",
                tradingHours: "9:30am-4pm",
              }
           }

           function printStockInfo(stock) {
             let {symbol, exchange: {name}} = stock;
             console.log(`The ${symbol} stock is traded at ${name}`);
           }

           printStockInfo(msft);
          ```
         运行这个脚本将会打印如下输出：
        
          ```
          The MSFT stock if traded at NASDAQ
          ```
       - ###### 解构数组
        
         **数组解构** 与对象解构 **类似** ，但是会用 **方括号** 代替大括号。当 **解构对象*8 时，需要 **声明匹配属性的变量** ；而当 **解构数组** 时，则需要 **声明匹配索引的变量** 。下面的代码会从数组中提取两个数组元素并复制给两个变量：
         
         ```
         let [name1, name2] = ["Smith", "Clinton"];
         console.log(`name1 = ${name1}, name2 = ${name2}`);
         ```
         输出如下所示：
         
         ```
         name1 = Smith, name2 = Clinton
         ```
         如果仅需要 **提取数组的第二个元素** ，匹配模式应该这么写：
         
         ```
         let [, name2] = ["Smith", "Clinton"];
         ```
         如果 **函数返回一个数组** ，结构语法会将其 **转换为一个返回多个数据的函数** ，如下面的getCustomers()函数所示：
         
         ```
         function getCustomers() {
           return ["Smith", , , "Gonzales"];
         }
         
         let [firstCustomer, , , lastCustomer] = getCustomers();
         console.log(`The first customer is ${firstCustomer} and the last one is ${lastCustomer}`);
         ```
         现在我们可以 **将rest参数和数据结构组合在一起使用** 。假设有一个数组，其中包含了若干名顾客，但是只需处理最开始的两个元素。下面的代码片段展示了如何实现：
         
         ```
         let customers = ["Smith", "Clinton", "Lou", "Gonzles"];
         
         let [firstCust, secondCust, ...otherCust] = customers;
         
         console.log(`The first customer is ${firstCust} and the second one is ${secondCust}`);
         console.log(`Other customers are ${otherCust}`);
         ```
         代码在控制台产生的输出如下所示：
         
         ```
         The first customer is Smith and the second one is Clinton
         Other customers are Lou,Gonzles
         ```
         另外一种用法，可以匹配模式以及rest参数一起传递给函数：
         
         ```
         var customers = ["Smith", "Clinton", "Lou", "Gonzles"];
         
         function processFirstTwoCustomers([firstCust, secondCust, ...otherCust]) {
           console.log(`The first customer is ${firstCust} and the second one is ${secondCust}`);
           console.log(`Other customers are ${otherCust}`);
         }
         
         processFirstTwoCustomers(customers);
         ```
         输出与上次执行是一致的：
         ```
         The first customer is Smith and the second one is Clinton
         Other customers are Lou,Gonzles
         ```
         总而言之，解构的好处是当 **需要从对象属性或数组中初始一些变量** 时，可以 **写更少的代码** 。
         
   - #### 用forEach()、for-in和for-of进行迭代
   
     可以使用不同的JavaScript关键字和API对对象集合进行遍历。在本节中，将会展示如何使用新的for-of循环，将会对for-of、for-in以及forEach()方法进行比较。
    
     - ##### 使用forEach()方法
     
       考虑下面的代码，对一个包含了4个数字的数组进行迭代。该数组还有一个额外的desciption属性，这个 **属性会被forEach()忽略** ：
       
       ```
       var numbersArray = [1, 2, 3, 4];
       numbersArray.description = "four numbers";
       
       numbersArray.forEach(forEach(n) => console.log(n));
       ```
       脚本输出如下所示：
       ```
       1
       2
       3
       4
       ```
       forEach()方法将一个函数作为参数，并从数组中正确的打印四个数字，忽略了description属性。forEach()的另一个限制是 **无法提前打断循环** 。可以使用every()方法代替forEach()或借助其他一些hack手段来实现这个功能。
       
     - ##### 使用for-in循环
     
        for-in能够 **循环遍历对象的属性以及集合数据** 。在JavaScript中，任何一个对象都是 **键值对集合** 。键对应的是属性名称，值是属性的值。数组有五个属性：numbers Array其中有四个为数字，一个是description属性。让我们遍历这数组的属性：
        
        ```
        var numbersArray = [1, 2, 3, 4];
        numbersArray.description = "four numbers";
        
        for(let n in numbersArray) {
           console.log(n);
        }
        ```
        上面代码的输出如下所示：
        
        ```
        0
        1
        2
        3
        description
        ```
        通过调试器运行此代码，显示每个属性都是字符串。为了 **查看属性实际的值** ，可以使用numbersArray[n]打印数组元素：
        
        ```
        var numbersArray = [1, 2, 3, 4];
        numbersArray.description = "four numbers";
        
        for(let n in numbersArray) {
           console.log(numbersArray[n]);
        }
        ```
        输出如下所示：
        
        ```
        1
        2
        3
        4
        four numbers
        ```
        正如你看到的，for-in循环 **遍历的是所有属性** ，而 **不仅仅是数据** ，这个可能并非预期效果。
        
     - ##### 使用for-of循环
     
        ES6引入了for-of循环，能够做到 **只遍历数据** 而 **不读取数据集合中的其他属性** 。可以使用break关键字打断循环：
        
        ```
        var numbersArray = [1, 2, 3, 4];
        numbersArray.description = "four numbers";
        
        console.log("Running for of  for the entire array");
        for (let n of numbersArray) {
          console.log(n);
        }
        
        console.log("Running for of with a break");
        for (let n of numbersArray) {
          if (n > 2) break;
          console.log(n);
        }
        ```
        输出如下所示：
        
        ```
         Running for of  for the entire array
         1
         2
         3
         4
         Running for of with a break
         1
         2
        ```
        for-of 可以遍历 **任何一个可被迭代的对象** ，包括Array、Map、Set以及其他一些对象。字符串同样是可迭代的。下面的代码将会打印字符串“John”,一次一个字母。
        
        ```
        for (let char of "John") {
          console.log(char);
        }
        ```
   - #### 类与继承
   
     ES3和ES5都支持 **面向对象编程** 以及 **继承** ,但是使用ES6的 **类** 够更容易地 **开发代码** 和 **理解代码** 。
     
     在ES5中，既可以 **创建一个全新的对象** ，也可以 **从其他对象继承得到一个对象** 。默认所有JavaScript对象 **继承自Object类** 。对象的继承是由一个名为 **prototype的特殊属性** 实现的，该属性 **指向对象的父级** ，这被称为 **原型继承** （prototypal inheritance）。例如，为了创建继承自对象Tax的NJTax对象，需要这么写：
     
     ```
     function Tax() {
       //The code of the tax object goes here
     }
     
     function NJTax() {
       //The code of New Jersey tax object goes here 
     }
     
     //NJTax继承自Tax
     NJTax.prototype = new Tax();
     var njTax = new NJTax();
     ```
     ES6新引入关键字 **class** 和 **extends** ,使其在语法与其他面向对象编程语言（如Java和C#）保持一致。上面的代码按照ES6的写法如下所示：
     
     ```
     class Tax {
       //The code of the tax class goes here
     }
     
     class NJTax extends Tax {
       // The code of New Jersey tax object goes here
     }
     
     var njTax = new NJTax();
     ```
     Tax是父级或者称为超类，NJTax是子级或者称为子类。可以认为NJTax类与Tax类是“is a”的关系。换句话说，NJTax是一个Tax。可以在NJTax中 **实现额外的功能** ，但NJTax始终是一个（“is a”）Tax,或者说是一种（“is a kind of”） Tax。同样，如果创建一个继承自Person的类Employee,那么Employee是Person。
     可以创建一个或多个对象，如下所示：
     
     ```
     //Tax对象的第一个实例
     var tax1 = new Tax();
     //Tax对象的第二个实例
     var tax2 = new Tax();
     ```
     **提示：** 类声明是不会被提升的。在使用类之前需要首先声明它。
     
     这些对象 **都具有Tax类的属性和方法** ，但是它们 **保持不同的状态** 。例如，第一个实例可以创建一名年收入$50 000的客户，而第二个实例可以创建为一名年收入$75 000的客户。每一个实例 **共享同一份Tax中声明的方法的拷贝** ，因此 **不会有重复的代码存在** 。
     
     在ES5中，为了 **避免代码重复** ，可以通过不在对象中声明方法而是在 **属性中声明方法** 来解决：
     
     ```
     function Tax() {
       //The code of the tax object goes here
     }
     
     
     Tax.prototype = {
      calcTax: function() {
        // code to calculate tax goes here
      }
     }
     ```
     JavaScript依旧是 **原型继承的语言** ，但是ES6能够令开发者 **编写更优雅的代码** ：
     
     ```
     class Tax {
     
       calcTax() {
         // code to calculate tax goes here
       }
     }
     ```
     **不支持成员变量：** ES6语法不支持声明Java、C#或TypeScript中类似的类成员变量。下面的语法是不受支持的：
     
     ```
     class Tax {
       var income;
     }
     ```
     - ##### 构造函数
     
       在实例化的过程中，类会执行一些特使方法中的代码，这些方法被称为 **构造函数** 。在Java和C#这类开发语言中，构造函数的名称必须与类的名称保持一致；但在ES6中，使用**constructor关键字** 声明类的构造函数：
       
       ```
       class Tax {
         constructor (income) {
           this.income = income;
         }
       }
       
       var myTax = new Tax(50000);
       ```
       构造函数是 **一类特殊的方法** ，当对象 **创建时只会被执行一次** 。如果对Java或C#语言熟悉的话，就会发现上面的代码看起来有些不同；并**没有** 单独 **声明一个类级别的income变量** ，而是 **动态的创建在this对象上** ，使用构造函数的参数来初始化this.income。**this变量** 指向 **当前对象的实例** 。
       
       下面的示例将会展示如何创建子类NJTax的实例，并把50 000传入到它的构造函数中：
       
       ```
       class Tax {
         constructor(income) {
           this.income = income;
         }
       }
       
       class NJTax extends Tax {
         // The code of New Jersey tax object goes here
       }
       
       var njTax = new NJTax(50000);
       
       console.log(`The income in njTax instance is ${njTax.income}`);
       ```
       上面代码片段的输出如下所示：
       
       ```
       The income in njTax instance is 50000
       ```
       由于子类NJTax并没有定义自己的构造函数，因此当初始化NJTax时， **父类Tax的构造函数会自动被调用** 。
       
       注意通过njTax引用变量可以类的外部访问到income变量。
       
     - ##### 静态变量
     
       如果一个类的属性希望能够 **被它的多个实例共享** ，那么需要在类声明的 **外部创建这个属性** 。在下面的示例中，counter变量被对象A的两个实例所共享：
       
       ```
       class A {
       }
       
       A.counter =  0;
       
       var a1  = new A();
       A.counter++;
       console.log(A.counter);
       
       var a2  = new A();
       A.counter++;
       console.log(A.counter);
       ```
       执行代码后，输出如下：
       
       ```
       1
       2
       ```
     - ##### 访问器、设置器以及方法定义
     
       对象的访问器（getter）和设置器（setter）方法并不是ES6的新语法，在介绍新的方法定义语法之前，先回顾一下。设置器和生成器把函数 **绑定到对象的属性中** 。
       
       
      
      
       
       
       
       
       
       
       
       
       
       
       
       
       
       
       
       
       