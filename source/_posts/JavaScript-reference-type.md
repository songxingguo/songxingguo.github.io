title: 《JavaScript 高级程序设计》学习笔记-引用类型
author: songxingguo
tags:
  - JavaScript
categories:
  - 读书笔记
date: 2018-08-13 15:23:00
---
## 单体内置对象

### Global 对象

- #### eval() 方法

  > 整个 ECMAScript 语言中 **最强大的一个方法** ： **eval()** 。
 
  eval() 方法就像一个完整的 **ECMAScript 解析器** ，它只接受一个参数，即要 **执行的 ECMAScript（或 JavaScript）字符串** 。看下面的例子：
 
   ```js
   eval("alert('hi')");
   ```
   这行代码的作用等价于下面这行代码：

   ```js
   alert("hi");
   ```
   <!-- more -->

   当解析器发现代码中调用 eval() 方法时，它会 **将传入的参数当作实际的 ECMAScript 语句来解析，然后把执行结果插入到原位置** 。 **通过 eval() 执行的代码被认为是包含该次调用的执行环境的一部分，因此被执行的代码具有与该执行环境相同的作用域链** 。这意味着 **通过 eval() 执行代码可以引用在包含环境中定义的变量** ，举个例子：

   ```js
   var msg = "hello world";
   eval("alert(msg)"); //"hello world"
   ```
   可见，变量 msg 是在 eval() 调用环境之外定义的，但其调用的 alert() 仍然能够显示 "hello world"。 这是因为上面的第二行代码最终被替换成了一行真正的代码。同样地，我们 **可以在 eval() 调用中定义一个函数，然后在调用的外部代码中引用这个函数** ：

   ```js
   eval("function sayHIi() { alert ('hi'); }");
   sayHi();
   ```
   显然，函数 sayHi() 是在 eval() 内部定义的。但由于对 eval() 的调用最终会被替换成定义的实际代码，因此可以在下一行调用 sayHi()。**对于变量也一样** ：

   ```js
   eval("var msg =  'hello world'");
   alert(msg);
   ```
   在 eval() 中创建的 **变量或函数都不会被提升** ，因为在解析代码的时候，他们被包含在一个字符串中；它们 **只在 eval() 执行的时候创建** 。

  **严格模式** 下，**在外部访问不到 eval() 中创建的任何变量或函数** ，因此前面两个例子都会导致错误。同样，**严格模式** 下，**为 eval 赋值也会导致错误** ：
  
  ```js
  "use strict"
  evval = "hi"; //causes error
  ```
 > **能够解释代码字符串的能力非常强大，但也非常危险** 。因此在使用 eval() 时必须极为谨慎，特别是在用它执行用户输入数据的情况下。否则，可能会有恶意用户输入威胁你的站点或应用程序的安全的代码（即所谓的 **代码注入** ）。
 
- #### Global 对象属性

  下表列出了 Global 对象的所有属性。
  
  ![Global 对象的所有属性](http://p9myzkds7.bkt.clouddn.com/JavaScript-reference-typeGlobal%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%89%80%E6%9C%89%E5%B1%9E%E6%80%A7.png)
  
  <!-- | 属性           | 说明                    |
  | -------------- | ----------------------- |
  | undefined      | 特殊值 undefined        |
  | NaN            | 特殊值 NaN              |
  | Infinity       | 特殊值 Infinity         |
  | Object         | 构造函数 Object         |
  | Array          | 构造函数 Array          |
  | Function       | 构造函数 Function       |
  | Boolean        | 构造函数 Boolean        |
  | String         | 构造函数 String         |
  | Number         | 构造函数 Number         |
  | Date           | 构造函数 Date           |
  | RegExp         | 构造函数 RegExp         |
  | Error          | 构造函数 Error          |
  | EvalError      | 构造函数 EvalError      |
  | RangeError     | 构造函数 RangeError     |
  | ReferenceError | 构造函数 ReferenceError |
  | SyntaxError    | 构造函数 SyntaxError    |
  | Type Error     | 构造函数 TypeError      |
  | URIError       | 构造函数 URIError       | -->
  
  ECMAScript 5 明确禁止给 undefined 、NaN 和 Infinity 赋值，这样做即使在非严格模式下也会导致错误。
 
- #### window 对象

  ECMAaScript 虽然没有指出如何直接访问 Global 对象，但 Web 浏览器都是 **将这个全局对象作为 window 对象的一部分加以实现的** 。因此，在全局作用域中声明的所有变量和函数，就都 **成为了 window 对象的属性** 。来看一个例子。
  
  ```js
  var color = "red";
   
  function sayColor() {
    alert(window.color);
  }
  
  window.sayColor(); // "red"
  ```
  这里定义了一个名为 color 的全局变量和一个名为 sayColor() 的全局函数。在 sayColor() 内部我们通过 window.color 来访问 color 变量，以说明全局变量是 window 对象的属性。然后，又使用 window.sayColor() 来直接通过 window 对象调用这个函数，结果显示在了警告框中。
  
  > JavaScript 中的 window 对象除了扮演 ECMAScript 规定的 Global 对象的角色外，还承担的了很多别的任务。
  
  另一种 **取得 Global 对象的方法** 是使用以下代码：
  
  ```js
  var global = function() {
    return this;
  }
  ```
  以上代码创建了一个立即调用的函数表达式，返回 this 的值。如前所述，**在没有给函数明确指定 this 值的情况下** （无论是通过 **将函数添加为对象的方法** ，还是通过 **call()** 或 **apply()** ）, **this 值等于 Global 对象** 。而像这样通过简单的返回 this 来取得 Global 对象，**在任何执行环境下都是可行的** 。
  
### Math 对象

  ECMAScript 还为保存数学公式和信息提供了一个公共位置，即 **Math 对象** 。与我们在 JavaScript 直接编写的计算功能相比， **Math 对象提供的计算功能执行起来要快得多** 。Math 对象中还 **提供了辅助完成这些计算的属性和方法** 。
  
  - #### Math 对象的属性
    
    Math 对象包含的 **属性大多数是** 数学计算中可能会用到的一些 **特殊值** 。下表列出了这些属性。
    
    ![Math 对象的属性](http://p9myzkds7.bkt.clouddn.com/JavaScript-reference-typeMath%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%B1%9E%E6%80%A7%E5%80%BC.png)
    
    <!-- | 属性         | 说明                                |
| ------------ | ----------------------------------- |
| Math.E       | 自然对数的底数，即常量 e 的值       |
| Math.LN1.0   | 10 的自然对数                       |
| Math.LN2     | 2 的自然对数                        |
| Math.LOG2E   | 以 2 为底 e 的对数                  |
| Math.LOG10E  | 以 10 为底 e 的对数                 |
| Math.PI      | π 的值                              |
| Math.SQRT1_2 | 1/2 的平方根（即 2 的平方根的倒数） |
| Math.SQRT2   | 2 的平方根                          | -->
   
- #### min() 和 max() 方法
  
    min() 和 max() 方法 **用于确定一组数值中的最小值和最大值** 。这两个方法都可以 **接受任意多个数值参数** ，如下面的例子所示。
    
    ```js
    var max = Math.max(3, 54, 32, 16);
    alert(max); //54
    
    var min = Math.min(3, 54, 32, 16);
    alert(min); //3
    ```
    对于 3、34、32 和 16， Math.max() 返回 54， 而 Math.min() 返回 3。 这两个方法经常被用于避免多余的循环和在 if 语句中确定一组数的最大值。
    
    **要找到数值中的最大或最小值** ，可以像下面这样使用 apply() 方法。
    
    ```
    var values = [1, 2, 3, 4, 5, 6, 7, 8];
    var max = Math.max.apply(Math, values);
    ```
    这个技巧的关键是把 **Math 对象** 作为 **apply() 的第一个参数** ，从而 **正确地设置 this 值** 。然后，可以将 **任何数值** 作为 **第二个参数**。

- #### 舍入方法

  下面将介绍  **将小数值舍入为整数** 的几个方法： **Math.ceil()** 、 **Math.floor()** 和 **Math.round()** 。这三个方法分别遵循下列舍入规则：
  
  - Math.ceil() 执行 **向上舍入**  ，即它总是 **将数值向上舍入为最接近的整数** 。
  
  - Math.floor() 执行 **向下舍入**，即它总是 **将数值向下舍入为最接近的整数** 。
  
  - Math.round() 执行 **标准舍入** ，即它总是 **将数值四舍五入为最接近的整数** （这也是我我们在数学课上学到的舍入规则）。
  
  下面是使用这些方法的示例：
  
  ```js
  alert(Math.ceil(25.9));  //26
  alert(Math.ceil(25.5));  //26
  alert(Math.ceil(25.1));  //26
  
  alert(Math.round(25.9)); //26
  alert(Math.round(25.5)); //26
  alert(Math.round(25.1)); //25
  
  alert(Math.floor(25.9)); //25
  alert(Math.floor(25.5)); //25
  alert(Math.floor(25.1)); //25
  ```
  对于介于 25 和 26 之间的数值， Math.ceil() 始终返回 26，因为它执行的是向上舍入。Math.round() 方法只在大于等于 25.5 时返回 26；否则返回 25。最后， Math.floor() 对介于25 和 26 之间的数值都返回 25。
  
- #### random() 方法
 
  Math.random() 方法 **返回大于等于 0 小于 1 的一个随机数** 。对于某些站点来说，这个方法非常实用，因为可以利用它来随机显示名人名言和新闻事件。套用下面的公式，就 **可以利用 Math.random() 从某个整数范围内随机选择一个值** 。
 
   ```
   值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能的值)
   ```
  公式中用到了 Math.floor() 方法，这是因为 Math.random() 总是返回一个小数值。而用这个小数值乘以一个整数，然后在加上一个整数，最终结果仍然还是一个小数。举例来说，如果你想 **选择一个 1 到 10 之间的数值** ，可以像下面这样编写代码：
  
  ```js
  var num = Math.floor(Math.random() * 10 + 1);
  ```
  总共 10 个可能的值（1 到 10）， 而第一个可能的值是 1.如果想要选择一个介于 2 到 10 之间的值，就应该将上面的代码改成这样：
  
  ```js
  var num = Math.floor(Math.random() * 9 + 2);
  ```
  从 2 数到 10 要数 9 个数，因此可能值的总数就是 9，而第一个可能的值就是 2.多数情况下，其实都 **可以通过一个函数来计算可能的总数和第一个可能的值** ，例如：
  
  ```js
  function selectFrom(lowerValue, upperValue) {
    var choices = upperValue - lowerValue + 1;
    return Math.floor(Math.random() * choices + lowerValue);
  } 
  
  var num = selectFrom(2, 10);
  alert(num); //介于 2 和 10 之间（包括 2 和 10）的一个数值
  ```
  函数 seclectFrom() 接受两个参数：应该返回的最小值和最大值。而用最大值减最小值再加上 1 得到了可能的总数，然后它又把这些数值套用到了前面的公式中。这样，通过调用 selectFrom(2, 10) 就可以得到一个介于 2 和 10 之间（包括 2 和 10）的数值了。利用这个函数，可以方便 **从数值中随机取出一项** ，例如：
  
  ```js
  var colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"];
  var color = colors[selectFrom(0, colors.length - 1)];
  alert(color); //可能是数组中包含的任何一个字符串
  ```
  在这个例子中，传递给 selectFrom() 的第二个参数是数组的长度减 1，也就是数组中最后一项的位置。
  
- #### 其他方法

  下面我们给出一个表格，其中列出了这些没有介绍到的 Math 对象的方法。
  ![ Math 对象的方法](http://p9myzkds7.bkt.clouddn.com/JavaScript-reference-type/Math%20%E5%AF%B9%E8%B1%A1%E6%96%B9%E6%B3%95.png)
  
  <!-- | 方法                 | 说明                    |
| -------------------- | ----------------------- |
| Math.abs(num)        | 返回 num 的绝对值       |
| Math.exp(num)        | 返回 Math.E 的 num 次幂 |
| Math.log(num)        | 返回 num 的自然对数     |
| Math.pow(num, power) | 返回 num 的 power 次幂  |
| Math.sqrt(num)       | 返回 num 的平方根       |
| Math.acos(x)         | 返回 x 的反余弦值       |
| Math.asin(x)         | 返回 x 的反正弦值       |
| Math.atan(x)         | 返回 x 的反正切值       |
| Math.atan2(y, x)     | 返回 y/x 的反正切值     |
| Math.cos(x)          | 返回 x  的余弦值        |
| Math.sin(x)          | 返回 x 的正弦值         |
| Math.tan(x)          | 返回 x 的正切值         | -->

  虽然 ECMA-262 规定了这些方法，但不同实现可能会对方法采用不同的算法。毕竟，计算某个值的正弦、余弦和正切的方式多种多样。也正是因为如此，**这些方法在不同的实现中可能会有不同的精度** 。
  
## 小结
  
  **对象** 在 JavaScript 中被称为 **引用类型的值** ，而且 **有一些内置的引用类型可以用来创建特定的对象** ，现简要总结如下：
  
  - 引用类型与传统面向对象程序设计中的类 **相似** ，但 **实现不同** ；
  
  - Object 是一个基础类型，其他 **所有类型都从 Object 继承了基本的行为** ；
  
  - Array 类型是 **一组值的有序列表** ，同时提供了 **操作** 和 转换这些值的功能** ；
  
  - Date 类型提供了有关 **日期** 和 **时间** 信息，包括 **当前日期和时间** 以及 **相关的计算功能** ；
  
  - RegExp 类型是 **ECMAScript 支持正则表达式的一个接口** ，提供了 **最基本的和一些高级的正则表达式功能** ；
  
函数实际上是 Function 类型的实例，因此 **函数也是对象** ；而这一点正是 JavaScript 最有特色的地方。由于函数是对象，所以 **函数也拥有方法** ，**可以用来增强其行为** 。
 
因为有了 **基本包装类型** ，所以 JavaScript 中 **基本类型值可以被当作对象来访问** 。三种基本包装类型分别是： **Boolean** 、 **Number** 和 **String** 。以下是它们共同的特征：
 
 - 每个包装类型都映射到 **同名的基本类型** ；
 
 - 在 **读取模式** 下访问 **基本类型值** 时，就 **会创建对应的基本包装类型的一个对象** ，从而方便了数据操作；
 
 - 操作基本数据类型值的语句一经执行完毕，就会 **立即销毁新创建的包装对象** 。
 
在 **所有代码执行之前** ，作用域中已经存在两个内置对象：**Global** 和 **Math** 。在大多数 ECMAScript 实现中都不能直接访问 Global 对象；不过，Web 浏览器实现了承担该角色的 **Window 对象** 。**全局变量和函数** 都是 **Global 对象属性** 。Math 对象提供了很多属性和方法，用来完成复杂的数学计算任务。
