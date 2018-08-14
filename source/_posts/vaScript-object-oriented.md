title: 《JavaScript 高级程序设计》学习笔记-面向对象程序设计
author: songxingguo
tags:
  - JavaScript
categories:
  - 前端技术
  - 读书笔记
date: 2018-08-14 16:04:00
---
# 面向对象的程序设计

面向对象（Object-Oriented, OO）的语言有一个标志，那就是它们都有类的概念，而通过类可以创建任意多个具有相同属性和方法的对象。前面提到过，**ECMAScript 中没有类的概念** ，因此 **它的对象也与基于类的语言中的对象有所不同** 。

ECMA-262 把对象定义为：“**无序属性集合，其属性可以包含基本值、对象或者函数。** ”严格来讲，这就是相当于说 **对象是一组没有特定顺序的值** 。对象的每个属性或方法都有一个名字，而每个名字都映射到一个值。正因为这样（以及其他将要讨论的原因）， 我们可以把 ECMAScript 的对象想象成**散列表** ：**无非就是一组名值对** ，**其中值可以是数据和函数** 。

**每个对象** 都是 **基于一个引用类型创建的**，这个 **引用类型** 可以是 **原生类型** ，也可以是 **开发人员定义的类型**。

<!-- more -->

## 理解对象

  **创建自定义对象** 的最简单方式就是 **创建一个 Object 的实例** ，然后 **再为它添加属性和方法** ，如下所示。
  
  ```js
  var person = new Object();
  person.name = "Nicholas";
  person.age = 29;
  person.jop = "Software Engineer";
  
  person.sayName = function() {
    alert(this.name);
  }
  ```
  上面的例子创建了一个名为 person 的对象，并为它添加了三个属性（name、age 和 job）和一个方法（sayName()）。其中，sayName() 方法用于显示 this.name（将被解析为 person.name）的值。早期的 JavaScript 开发人员经常使用这个模式创建新对象。几年后， **对象字面量语法** 可以写成这样：
  
  ```js
  var person = {
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    
    sayName: function() {
      alert(this.name);
    }
  };
  ```
  这个例子中的 person 对象与前面例子中的 person 对象是一样的，都是相同是属性和方法。这些 **属性在创建时带有一些特征值**（characteristc），**JavaScript 通过这些特征值来定义它们的行为** 。
  
### 属性类型

  ECMA-262 第 5 版在定义 **只有内部才用的特性（attribute）时** ，描述了 **属性（property）的各种特征** 。ECMA-262 定义这些特征是为了实现 JavaScript 引擎用的，因此在 JavaScript 中不能直接访问它们。为了表示特性是内部值，该规范把它们放在了两对儿方括号中，例如 [[ Enumberable]] 。尽管 ECMA-262 第 3 版的定义有些不同，但本书只参考第 5 版的描述。
  
  ECMAScript 中有两种属性： **数据属性** 和 **访问器属性** 。
  
  - #### 数据属性 
  
    数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有 4 个描述其行为的特性。
    
    - 
  
  