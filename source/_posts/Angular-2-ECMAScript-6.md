title: 《Angular 2开发实战》学习笔记-ECMAScript 6概述
author: songxingguo
tags:
  - ES6
  - ''
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
      
      
      
      
      
      
    
    
   
     
     
     
     
     