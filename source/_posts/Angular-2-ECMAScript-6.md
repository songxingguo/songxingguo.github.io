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
      
      
      
      
    
      
    
    
    
     
     
   
   