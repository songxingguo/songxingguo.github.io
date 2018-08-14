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
  
    数据属性包含一个数据值的位置。在这个位置可以读取和写入值。**数据属性** 有 4 个 **描述其行为的特性** 。
    
    - [[Configurable]]：表示 **能否通过 delete 删除属性从而重新定义属性** ，**能否修改属性的特性**，或者 **能否把属性修改为访问器属性** 。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。
    
    - [[Enumberable]]：表示 **能否通过 for-in 循环返回属性** 。像前面的例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。
    
    - [[Writable]]：表示 **能否修改属性的值** 。像前面的例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。
    
    - [[Value]]: 包含这个属性的 **数据值** 。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined。
    
    对于像前面例子中那样直接在对象上定义的属性，它们的 [[Configurable]]、[[Enumerable]] 和 [[Writable]] 特性都被设置为 true，而 [[Value]] 特性被设置为指定的值。例如：
    
    ```js
    var person = {
      name: "Nicholas"
    };
    ```
    这里创建了一个名为 name 的属性，为它指定的值是“Nicholas”。也就是说，[[Value]]特性将被设置为“Nicholas”，而对这个值的任何修改都将反映在这个位置。
    
    要 **修改属性默认的特性** ，必须使用 ECMAScript 5 的 Object.defineProperty() 方法。这个方法接受三个参数：**属性所在的对象** 、**属性的名字** 和 **一个描述符对象** 。其中，描述符（desccriptor）对象的属性必须是：configurable、enumberable、writable 和 value。 设置其中的一或多个值，可以修改对应的特性值。例如：
    
    ```js
    var personn = {};
    Object.defineProperty(person, "name", {
      writable: falase, 
      value: "Nicholas"
    });
    
    alert(person.name); //"Nicholas"
    person.name = "Greg";
    alert(person.name); //"Nicholas"
    ```
    这个例子创建了一个名为 name 的属性，它的值“Nicholas”是只读的。这个属性的值是不可以修改的，如果尝试为它指定新值，则在 **非严格模式** 下，**赋值操作将被忽略**；**严格模式** 下，**赋值操作将会导致抛出错误** 。
    
    类似的规则也适用于不可配置的属性。例如：
    
    ```js
    var person = {};
    Object.defineProperty(person, "name", {
      configurable: false;
      value: "Nicholas"
    });
    
    alert(person.name); //"Nicholas"
    delete person.name;
    alert(person.name); //"Nicholas"
    ```
    把 configurable 设置为 false，表示不能从对象周刊删除属性。如果对这个属性调用 delete ，则在 **非严格模式** 下 **什么也不会发生** ，而在 **严格模式** 下 **会导致错误** 。而且，**一旦把属性定义为不可配置的就不能把它变回可配置了**  。此时，再调用 Object.defineProperty() 方法修改 ** 除 writable 之外的特性都会导致错误**  ：
    
    ```js
    var person = {};
    Object.defineProperty(person, "name", {
      configurable: false;
      value: "Nicholas"
    });
    
    //抛出错误
    Object.defineProperty(person, "name", {
      configurable: true;
      value: "Nicholas"
    });
    ```
    也就是说，**可以多次调用 Object.defineProperty() 方法修改同一个属性，但在把 configurable 特性设置为 false 之后就会有限制了**  。
    
    **在调用 Object.defineProperty() 方法时**  ，如果 **不指定** ， configurable 、enumberable 和 writable 特性的 **默认值都是 false** 。多数情况下，可能没必要利用 Object.defineProperty() 方法提供的这些高级功能。不过，理解这些概念对理解 JavaScript 对象却非常有用。
    
    > IE8 是第一个实现 Object.defineProperty() 方法的浏览器版本。然而，这个版本实现存在诸多限制：只能在 DOM 对象上使用这个方法，而且只能创建访问器属性。由于实现不彻底，**建议读者不要在 IE8 中使用 Object.defineProperty() 方法** 。
    
- #### 访问器属性

  访问器属性不包含数据值，它们包含一对儿 **getter** 和 **setter** 函数（不过，**这两个函数都不是必需的** ）。在 **读取访问器属性** 时，**会调用 getter 函数** ，这个函数 **负责返回有效的值** ；在 **写入访问器属** 时，**会调用 setter 函数并传入新值** ，这个函数 **负责决定如何处理数据** 。访问器属性有如下 **4** 个特性。
  
  - [[Configurable]]：表示 **能否通过 delete 删除属性从而重新定义属性** ，**能否修改属性的特性** ，或者 **能否把属性修改为数据属性** 。对于直接在对象上定义的属性，这个特性的默认值为 true。
  
  - [[Enumerable]]： 表示 **能否通过 for-in 循环返回属性** 。对于直接在对象上定义的属性，这个特性的默认值为 true。
  
  - [[Get]]: **在读取属性时调用的函数** 。默认值为 undefined。
  
  - [[Set]]: **在写入属性时调用的函数** 。默认值为 undefined。
  
  **访问器属性不能直接定义** ，**必须使用 Object.defineProperty() 来定义** 。请看下面的例子。
  
  ```js
  var book = {
    _year: 2004,
    edition: 1
  };
  
  Object.defineProperty(book, "_year", {
    get: function() {
      return this._year;
    }
    set: function(newValue) {
      if (newValue > 2004) {
        this._year = newValue;
        this.edition += newValue - 2004;
      }
    }
  });
  
  book.year = 2005;
  alert(book.edition); //2
  ```
  以上代码创建了一个 book 对象，并给它定义两个默认的属性：_year 和 edition。**_year** 前面的 **下划线** 是一种常用的记号，用于 **表示只能通过对象方法访问的属性** 。而 **访问器属性 year** 则包含一个 **getter 函数** 和一个 **setter 函数** 。getter 函数返回 _year 的值，setter 函数通过计算来确定正确的版本。因此，把 year 属性修改为 2005 会导致 _year 变成 2005，而 edition 变为 2。**这是使用访问器属性的常见方式** ，即 **设置一个属性的值会导致其他属性发生变化** 。
  
  不一定非要同时指定 getter 和 setter 。 **只指定 getter 意味着属性是不能写** ，尝试写入属性会被忽略。在严格模式下，尝试写入只指定了 getter 函数的属性会抛出错误。类似地，**只指定 setter 函数的属性也不能读** ，否则在非严格模式下会返回 undefined，而在严格模式下会抛出错误。
  
  支持 ECMAScript 5 的这个方法的浏览器有 IE9+ （IE8 只是部分实现）、Firefox 4+、Opera 12+ 和 Chrome。**在这个方法之前，要创建访问器属性** ，一般都使用两个非标准的方法：**_defineGetter_()** 和**_defineSettrer_()** 。这两个方法最初是由 Firefox 引入的，后来 Sarfari 3 、Chrome 1、Opera 9.5 也给出了相同的实现。使用这两个遗留的方法，可以像下面这样重写前面的例子。
  
  ```js
  var book = {
    _year: 2004;
    edition: 1
  };
  
  //定义访问器的旧方法
  book._defineGetter_("year", function() {
    return this._year;
  });
  book._defineSetter_("year", function(newValue) {
    if (newValue > 2004) {
      this._year = newValue;
      this.edition += newValue - 2004;
    }
  });
  
  book.year = 2005;
  alert(book.edition); //2
  ```
  在 **不支持 Object.defineProperty() 方法的浏览器** 中 **不能修改**  **[[Configurable]]** 和 **[[Enumberable]] ** 。
  
  
    
    
    
    
    
  