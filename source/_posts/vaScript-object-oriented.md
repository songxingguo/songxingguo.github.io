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
  
### 定义多个属性

  由于为对象定义多个属性的可能性很大，ECMAScript 5 又定义了一个 Object.defineProperties() 方法。利用这个方法 **可以通过描述符一次定义多个属性** 。这个方法接收两个对象参数：第一个对象是 **要添加和修改其属性的对象** ，第二个对象属性 **与第一个对象中要添加或修改的属性一一对应** 。例如：
  
  ```js
  var book = {};
  
  Object.defineProperties(book, {
    _year: {
      writable: true,
      value: 2004
    },
    edition: {
      writable：true,
      value: 1
    },
    year: {
      get: function() {
        return this._year;
      },
      set: function(newValue) {
         if (newValue > 2004) {
           this._year = newValue;
           this.edition += newValue - 2004;
         }
      }
    }
  });
  ```
  以上代码在 book 对象上定义了两个数据属性（_year 和 edition）和 一个访问器属性（year）。最终对象与上一节中定义的对象相同。唯一的区别是这里的属性都是在同一时间创建的。
  
  支持 Object.defineProperties() 方法的浏览器有 IE9+、Firefox 4+、Safari 5+、Opera 12+ 和 Chrome。
  
### 读取属性的特性

使用ECMAScript 5 的Object.getOwnPropertyDescriptor() 方法，**可以取得给定属性描述符** 。这个方法接收两个参数：**属性所在的对象** 和 **要读取其他描述符的属性名称** 。返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable、enumberable、get 和 set；如果是数据属性，这个对象的属性有 configurable、enumerale、writable 和 value。例如：

```js
var book = {};

Object.defineProperties(book, {
  _year: {
    value: 2014
  },
  edition: {
    value: 1
  },
  year: {
    get: function() {
      return this._year;
    },
    set: function(newValue) {
      if (newValue > 2004) {
        this._year  = newValue;
        this.edition += newValue - 2004;
      }
    }
  }
});

var descritoion = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value); //2004
alert(descriptor.configurable); //false
alert(typeof descriptor.get); //"undefined"

var descriptor = Object.getOwnPropertyDescriptor(book，"year");
alert(descrptor.value); //undefined
alert(descriptor.enumberable); //false
alert(typeof descriptor.get); // "function"
```

对于数据属性 _year，value 等于最初的值，configurable 是 false，而 get 等于 undefined。对于访问器属性 year、value 等于 undefined, enumberable 是 flase，而   get 是一个指向 getter 函数的指针。

在 JavaScript 中，**可以针对任何对象** ——包括 **DOM** 和 **BOM** 对象，使用 Object.getOwnPropertyDescriptor() 方法。支持这个方法的浏览器有 IE9+、Firefox 4+、Safari 5+、opera 12+ 和 Chrome。

## 创建对象

虽然 Object 构造函数或对象字面量都可以用来创建单个对象，但这些方式有个明显的缺点：**使用同一个接口创建很多个对象** ，**会产生大量重复代码** 。为解决这个问题，**人们开始使用工厂模式的一种变体** 。

### 工厂模式

  工厂模式是软件工程领域 一种广为人知的设计模式，这种模式抽象了创建具体对象的过程。考虑到在 ECMAScript 中无法创建类，开发人员就发明了一种函数，**用这种函数来封装以特定接口创建对象的细节** ，如下面的例子所示。
  
  ```js
  function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = jon;
    o.sayName = function() {
      alert(this.name);
    }
    return o;
  }
  
  var person1 = createPerson("Nicholas", 29,  "Software Engineer");
  var person2 = createPerson("Greg", 27, "Doctor");
  ```
  函数 createPerson() 能够接受的参数来构建一个包含所有必要信息的Person对象。可以无数次地调用这个函数，而每次它都会返回一个包含三个属性一个方法的对象。工厂模式虽然 **解决了创建多个相似对象的问题** ，但却 **没有解决对象识别的问题** （即怎样知道一个对象的类型）。随着 JavaScript 的发展，又一个新模式出现了。
  
  
### 构造函数模式

  ECMAScript 中的构造函数可以用来创建特定类型的对象。像 Object 和 Array 这样的原生构造函数，在运行时会自动出现在执行环境中。此外，也可以创建自定义的构造函数，从而定义自定义对象的属性和方法。例如，可以使用构造函数模式将前面的例子重写如下。
  
  ```js
  function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function() {
      alert(this.name);
    }
  }
  
  var person1 = new Peron("Nicholas", 29,, "Software Engineer");
  var person2 = new Person("Greg", 27, "Doctor");
  ```
  在这个例子中，Person() 函数取代了 createPerson() 函数。我们注意到，Person() 中的代码除了与 createPerson() 中相同的部分外，还 **存在以下不同之处** ：
  
  - 没有显式地创建对象；
  
  - 直接将属性和方法赋给了 this 对象；
  
  - 没有 return 语句。
  
此外，还应该注意到函数名 Person 使用的是大写字母 P。 **按照惯例** ，**构造函数始终应该以一个大写字母开头** ，而 **非构造函数则应该以小写字母开头** 。这个做法借鉴自其他 OO 语言，主要是Wie了区别于 ECMAScript 中的其他函数；因为构造函数本身也是函数，只不过可以用来创建对象而已。
  
要创建 Person 的新实例，必须使用 new 操作符。以这种方式调用构造函数实际上会经历以下 4 个步骤：
  
  - 创建一个新对象；
  
  - 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
  
  - 执行构造函数中的代码（为这个新对象添加属性）；
  
  - 返回新对象。
  
在前面例子的最后，person1 和 person2 分别保存着 Person 的一个不同的实例。这两个对象都有一个 constructor （构造函数）属性，该属性指向 Person，如下所示。
  
  ```js
  alert(person1.constructor == Person); //true
  alert(person2.constructor == Person); //true
  ```
对象的 constructor 属性最初是 **用来标识对象类型的** 。但是，提到 **检查对象类型** ，还是 **instanceof 操作符** 更可靠一些。我们在这个例子中创建的所有对象既是 Object 实例，同时也是 Person 的实例，这一点通过 instanceof 操作符可以得到验证。

```js
alert(person1 instanceof Object); //true
alert(person1 instanceof Person); //true
alert(person2 instanceof Object); //true
alert(person2 instanceof Person); //true
```
创建自定义的构造函数意味着 **将来可以将它的实例标识为一种特定的类型** ；而这正是构造函数模式胜过工厂模式的地方。在这个例子中，person1 和 person2 之所以同时是 Object 的实例，是因为 **所有对象均继承自 Object ** 。

> 以这种方式定义的构造函数是定义在 Global 对象（在浏览器中是 window 对象）中的。

- #### 将构造函数当作函数

  **构造函数** 与 **其他函数** 的 **唯一区别**，就在于 **调用它们的方式不同。** 不过，构造函数毕竟也是函数，不存在定义构造函数的特殊语法。**任何函数** ，只要 **通过 new 操作符来调用** ，那 **它就可以作为构造函数** ；而 **任何函数** ，如果 **不通过 new 操作符来调用** ，那  **它就跟普通函数也不会有什么两样** 。例如，前面例子中定义的 Person() 函数可以通过下列任何一种方式来调用。
  
  ```js
  
  // 当作构造函数使用
  var person = new Person("Nicholas", 29, "Software Engineer");
  person.sayName(); // "Nicholas"
  
  // 作为普通函数使用
  Person("Greg", 27, "Doctor"); //添加到 window
  window.sayName(); // "Greg"
  
  
  //在另一个对象的作用域中调用
  var o = new Object();
  Person.call(o, "Krosten", 25, "Nurse");
  o.sayName(); // "Kristen"
  ```
  这个例子中的前面两行代码展示了构造函数的典型用法，即使用 new 操作符来创建一个新对象。接下来的两行代码展示了不使用 new 操作符调用 Person() 会出现什么结果：**属性和方法被添加给 window 对象了** 。有读者可能还记得，**当在全局作用域周刊调用一个函数时，this 对象总是指向 Global 对象** （在浏览器中就是 window 对象）。因此，在调用完函数之后，通过 window 对象来调用 sayName() 方法，并且还返回了 “Greg”。 最后，也可以 **使用 call() (或者 apply() ) 在某个特殊对象的作用域中调用 Person() 函数** 。这里是在对象 o 的作用域中调用的，因此 **调用后 o 拥有了所有属性和 sayName() 方法** 。
  
- #### 构造函数的问题

  构造函数模式虽然好用，但也并非没有缺点。使用构造函数的主要问题，就是  **每个方法都要在每个实例上重新创建一遍** 。在前面的例子中，person1 和 person2 都有一个名为 sayName() 的方法，但那两个方法不是同一个 Function 的实例。不要忘了—— ECMAScript 中的 **函数是对象** ，因此每定义一个函数，也就实例化了一个对象。从逻辑角度讲，此时的构造函数也可以这样定义。
  
  ```js
  function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = new Function("alert(this.name)"); //与声明函数在逻辑上是等价的
  }
  ```
  从这个角度上来构造函数，更容易明白每个 Person 实例都是包含一个不同的 Function 实例（以显示 name 属性）的本质。说明白些，以这种方式创建函数，**会导致不同作用域链和标识符解析** ，但 **创建 Function 新实例的机制仍然是相同的** 。因此，**不同实例上的同名函数是不相等的** ，以下代码可以证明这一点。
  
  ```js
  alert("person1.sayName == person2.sayName"); // false
  ```
  然而，创建两个完成同样任务的 Function 实例的确没有必要；**况且有 this 对象在** ，**根本不用在执行代码之前就把函数绑定到特定对象上面** 。因此，大可像下面这样，通过 **把函数定义转移到构造函数外部** 来解决这个问题。
  
  ```js
  function Person(name, age, job) {
    this.name = name;
    this. age = age;
    this.job = job;
    this.sayName = sayName;
  }
  
  function sayName() {
    alert(this.name);
  }
  
  var person1 = new Person ("Nicholas", 29, "Software Engineer");
  var person2 = new Person("Greg", 27, "Doctor");
  ```
  在这个例子中，我们把 sayName() 函数定义转移到了构造函数外部。而在构造函数内部，我们将 sayName() 属性设置成等于全局的 sayName 函数。这样一来，**由于 sayName 包含的是一个指向函数的指针** ，因此 **person1 和 person2  对象共享了在全局作用域中定义的同一个 sayName() 函数** 。这样做确实解决了两个函数做同一件事的问题，可是新问题又来了：在全局作用域中定义的函数实际上 **只能被某个对象调用** ，这让全局作用域有点名不副实。而让人无法接受的是：如果对象需要定义很多方法，那么就要定义很多个全局函数，于是我们 **这个定义的引用类型就丝毫没有封装可言了** 。好在，这些问题可以 **通过使用原型模式来解决** 。


  