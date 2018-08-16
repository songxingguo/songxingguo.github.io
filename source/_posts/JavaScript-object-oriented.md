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
  
### 原型模式

我们创建的 **每个函数都有一个 prototype （原型）属性** ，这个属性是 **一个指针** ，**指向一个对象** ，而这个对象的用途是 **包含可以由特定类型的所有实例共享的属性和方法** 。如果按照字面意思来理解，那么 **prototype 就是通过调用构造函数而创建的那个对象实例的原型对象** 。使用原型对象的好处是 **可以让所有对象实例共享它所包含的属性和方法** 。换句话说，**不必在构造函数中定义对象实例的信息** ，而是可以 **将这些信息直接添加到原型对象中** ，如下面的例子所示。

```js
function Person() {
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function() {
   alert(this.name);
}

var person1 = new Person();
person1.sayName();  // "Nicholas"

var person2 = new Person();
person2.sayName(); // "Nicholas"

alert(person1.sayName == person2.sayName); //true
```
在此，我们将 sayName() 方法和所有属性直接添加到了 Person 的 prototype 属性中，构造函数变成了空函数。即使如此，也仍然可以通过调用构造函数来创建新对象，而且新对象还会具有相同的属性和方法。但与构造函数模式不同的是，新对象的这些属性和方法是由所有实例共享的。换句话说， **person1 和 person2 访问的都是同一组属性和同一个 sayName() 函数** 。要理解原型模式的工作原理，必须先 **理解 ECMAScript 中原型对象的性质** 。

- #### 理解原型对象

  无论什么时候，只要 **创建一个新的函数** ，就会根据一组特定的规则为该函数创建一个 **prototype 属性** ，这个属性 **指向函数的原型对象** 。在默认情况下，**所有原型对象都会自动获得一个 constructor （构造函数）属性** ，这个属性是 **指向 prototype 属性所在函数的的指针** 。就拿前面的例子来说， **Peron.prototype.constructor 指向 Person** 。而 **通过这个构造函数** ，我们**还可继续为原型对象添加其他属性和方法** 。
  
  **创建了自定义的构造函数** 之后，其 **原型对象默认只会取得 constructor 属性** ；至于其他方法，则都是从 Object 继承而来的。当调用构造函数创建一个新实例后，该实例的 **内部将包含一个指针（内部属性）** ，**指向构造函数的原型对象** 。 ECMAScript-262 第 5 版中管这个指针叫 **[[Prototype]]**  。 虽然在脚本中没有标准的仿古式访问 [[Prototype]] ,但 Firefox 、Sofari 和 Chrome 在每个对象上都支持一个属性 _proto_; 而在其他实现中，这个属性对脚本则是完全不可见的。不过，要明确的真正重要的一点就是，这个 **连接存在于实例和构造函数的原型对象之间** ，而不是存在于实例和构造函数之间。 
  
  以前面使用 Person 构造函数和 Person.prototype 创建实例代码为例，下图展示了各个对象之间的关系。
  
  ![对象之间的关系](http://p9myzkds7.bkt.clouddn.com/%E5%AF%B9%E8%B1%A1%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB.jpg)
  
  上图展示了 Person 构造函数、Person 的原型属性以及 Person 现有的两个实例之间的关系。**prototype 指向了原型对象** ，而 **Person.prototype.contructor 又指回了 Person**  。原型对象中除了包含 constructor 属性之外，还包括后来添加的其他属性。Person 的每个实例—— person1 和 person2 都包含一个 **内部属性** ，该属性仅仅**指向了 Person.prototype** ；换句话说，**它们与构造函数没有直接的关系** 。此外，要格外注意的是，虽然这两个实例都不包含属性和方法，但我们却可以调用 person1.sayName() 。这是 **通过查找对象属性的过程来实现的** 。
  
  虽然在所有实现中都无法访问到 [[Prototype]],但是可以通过 isPrototypeOf() 方法来确定对象之间是否存在这种关系。从本质上讲，如果 [[Prototype]] 指向调用 isPrototypeOf() 方法的对象（Person.prototype）,那么这方法就返回 true，如下所示：
  
  ```js
  alert(Person.prototype.isPrototypeOf(person1)); //true
  alert(Person.prototype.isPrototypeOf(person2)); //true
  ```
  这里，我们用原型对象的 isPrototypeOf() 方法测试了 person1 和 person2。因为它们内部都有一个指向 Person.prototype 的指针，因此都返回了 true。
  
  ECMAScript 5 增加了一个新方法，叫 Object.getPrototypeOf(), 在所有支持的实现中，这个方法返回 [[Prototype]] 的值。例如：
  
  ```js
  alert(Object.getPrototypeOf(person1) == Person.prototype); //true
  alert(Object.getPrototypeOf(person1).name); // "Nicholas"
  ```
  这里的第一行代码只是确定 Object.getPrototypeOf() 返回的对象实际就是这个对象的原型。第二行代码取得了原型对象中 name 属性值，也就是 “Nicholas”。使用 **Object.getPrototypeOf()**  可以 **方便地取得一个对象的原型** ，而这个利用原型实现继承的情况下是非常重要的。 支持这方法的浏览器有 IE9+、 Firefox 3.5+、Safari 5+、Opera 12+ 和 Chrome。
  
  **每当代码读取某个对象属性时** ，都 **会执行一次搜索** ，**目标是具有给定名字的属性** 。搜索首先从对象实例本身开始。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。如果在原型对象中找到了这个属性，则返回该属性的值。也就是说，在我们调用 person1.sayName() 的时候，会先后执行两次搜索。**首先，解析器会问：“实例 person1 有 sayName 属性吗？”答：“没有。”然后，它继续搜索，再问：“person1 的原型有 sayName() 属性吗？”答：“有。”于是，它就读取那个保存在原型对象中的函数。** 当我们调用 person2.sayName() 时，将会重现相同的搜索过程，得到相同的结果。而正是多个对象实例共享原型所保存的属性和方法的基本原理。
  
  > 原型最初只包含 **constructor 属性** ，而该属性也是 **共享的**，因此可以通过对象实例访问。
  
  虽然 **可以通过对象实例访问保存在原型中的值** ，但却 **不能通过对象实例重写原型中的值** 。如果我们在实例中添加了一个属性，而该属性与实例原型中的一个属性同名，那我们就在实例中创建该属性，该属性将会屏蔽原型中的那个属性。来看下面的例子。
  
  ```js
  function Person() {
  }

  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function() {
     alert(this.name);
  }

  var person1 = new Person();
  var person2 = new Person();

  person1.name = "Greg";
  alert("person1.name"); // "Greg" —— 来自实例
  alert("person2.name"); //“Nicholas” —— 来自原型
  ```
  在这个实例中，person1 的 name 被一个新值给屏蔽了。但无论访问 person1.name 还是访问 person2.name 都能够正常地返回值，即分别是“Greg”（来自对象实例）和“Nicholas”（来自原型）。当在 alert() 中访问 person1.name 时，需要读取它的值，因此就会在这个实例上搜索一个名为 name 的属性。这个属性确实存在，于是就返回它的值不必在搜索原型了。当以同样的方式访问 person2.name 时，并没有在实例上发现该属性，因此就会继续搜索原型，结果在那里找到了 name 属性。
  
  当为对象实例添加一个属性时，这个属性就会屏蔽原型对象中保存的同名属性；换句话说，添加这个属性会阻止我们访问原型中的那个属性，但不会修改那个属性。即使将这个属性设置为 null，也只会在实例中设置这个属性，而不会恢复其指向原型的连接。不过，使用 delete 操作符则可以完全删除实例属性，从而让我们能够重新访问原型中的属性，如下所示。
  
   ```js
  function Person() {
  }

  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function() {
     alert(this.name);
  }

  var person1 = new Person();
  var person2 = new Person();

  person1.name = "Greg";
  alert("person1.name"); // "Greg" —— 来自实例
  alert("person2.name"); //“Nicholas” —— 来自原型
  
  delete person1.name ;
  alert(person1.name); // "Nicholas" —— 来自原型
  ```
  在这个修改后的例子中，我们使用 **delete 操作符** 删除了 person1.name，之前它保存的 “Greg”值屏蔽了同名的原型属性。把它删除以后，就恢复了对原型中 name 属性的连接。因此，接下来再调用 person1.name 时，返回的就是原型中的 name 属性的值了。
  
  使用 **hasOwnPropertoty()** 方法 **可以检测一个属性是存在与实例中** ，还是  **存在于原型中** 。这个方法（不要忘了它是从 Object 继承来的）只在给定属性存在于对象实例中时，才会返回 true。来看下面这个例子。
  
  ```js
  function Person() {
  }

  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function() {
     alert(this.name);
  }

  var person1 = new Person();
  var person2 = new Person();
  
  alert(person1.hasOwnProperty("name")); // false

  person1.name = "Greg";
  alert("person1.name"); // "Greg" —— 来自实例
  alert(person1.hasOwnProperty("name")); // true
  
  alert("person2.name"); //“Nicholas” —— 来自原型
  alert(person2.hasOwnProperty("name")); // false
  
  delete person1.name ;
  alert(person1.name); // "Nicholas" —— 来自原型
  alert(person1.hasOwnProperty("name")); // false
  ```
  通过使用 hasOwnProperty() 方法，，什么时候访问的是实例属性，什么时候访问的是原型属性就一清二楚了。调用 person1.hasOwnProperty("name") 时，只有当 person1 重写 name 属性后才会返回 true，因为只有这时候 name 才是一个实例属性，而非原型属性。下图展示了上面例子在不同情况下的实现与原型的关系（为了简单起见，图中省略了与 Person 构造函数的关系）。
  
  ![实现与原型的关系](http://p9myzkds7.bkt.clouddn.com/JavaScript-object-oriented/%E5%AE%9E%E7%8E%B0%E4%B8%8E%E5%8E%9F%E5%9E%8B%E7%9A%84%E5%85%B3%E7%B3%BB.jpg)
  
  > ECMAScript 5 的 Object.getOwnPropertyDescriptor() 方法只能 **用于实例属性** ，要取得原型属性的的描述符，必须直接在 **原型对象** 上调用 Object.getOwnPropertyDescriptor() 方法。
  
- #### 原型与 in 操作符

  有两种方式使用 in 操作符：**单独使用** 和 **在 for-in 循环中使用** 。在单独使用时，**in 操作符会在通过对象能够访问给的属性时返回 true** ，无论该属性存在于实例中还是原型中。看一看下面的例子。
  
  ```js 
  function Person() {
  }

  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function() {
     alert(this.name);
  }

  var person1 = new Person();
  var person2 = new Person();
  
  alert(person1.hasOwnProperty("name")); // false
  alert("name" in person1); // true

  person1.name = "Greg";
  alert("person1.name"); // "Greg" —— 来自实例
  alert(person1.hasOwnProperty("name")); // true
  alert("name" in person1); // true
  
  alert("person2.name"); //“Nicholas” —— 来自原型
  alert(person2.hasOwnProperty("name")); // false
  alert("name" in person2); // true
  
  
  delete person1.name ;
  alert(person1.name); // "Nicholas" —— 来自原型
  alert(person1.hasOwnProperty("name")); // false
  alert("name" in person1); // true
  ```
  在以上代码执行的整个过程中， name 属性要么是直接在对象上访问到的，要么是通过原型访问到的。因此，调用 “name” in person1 始终都返回 true，无论该属性存在于实例中还是存在于原型中。**同时使用 hasOwnProperty() 方法和 in 操作符** ，就可以 **确定该属性到底是存在于对象中** ，还是 **存在于原型中** ，如下所示。
  
  ```js
  function hasProperty(object, name) {
    return !object.hasOwnProperty(name) && (name in object);
  }
  ```
  由于 in 操作符只要通过对象能够访问属性就返回 true，hasOwnProperty() 只在属性存在于实例中时才返回 true，因此只要 in 操作符返回 true 而 hasOwnProperty() 返回 false，就可以确定属性是原型中的属性。下面来看一看上面定义的函数 hasPrototypeProperty() 的用法。
  
  ```js
  function Person() {
  }

  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function() {
     alert(this.name);
  }

  var person = new Person();
  alert(hasPrototypeProperty(person, "name")); // true
  
  person.name = "Greg";
  alert(hasPrototyProperty(person, "name")); //false
  ```
  在这里， name 属性先是存在于原型中，因此 hasPrototypeProperty() 返回 true 。当在实例中重写 name 属性后，该属性就存在于实例中了，因此 hasPrototypeProperty() 返回 false。即使原型中仍然有 name 属性，但由于现在实例中也有了这个属性，因此原型中的 name 属性就用不到了。
  
  在使用 for-in 循环时，返回的是所有能够通过对象访问的、可枚举的（enumerated）属性，其中既包括存在于实例中的属性，也包括存在于原型中的属性。屏蔽了原型中的不可枚举的属性（即将 [[Enumberable]] 标记为 flase 的属性）的实例属性也会在 for-in 循环中返回，因为根据规定，所有开发人员定义的属性都是可枚举的——只有在 iE8 及更早的版本中例外。
  
  **IE** 早期版本的实现中存在一个 **bug** ，即 **屏蔽不可枚举属性的实例属性不会出现在 for-in 循环中** 。例如：
  
  ```js
  var o = {
    toString: function() {
      return "My Object";
    } 
  };
  
  for (var prop in o) {
    if (prop == "toString") {
      alert(Found toString); // 在 IE 中不会显示
    }
  }
  ```
  当以上代码运行时， 应该会显示一个警告框，表明找到了 toString() 方法。这里的对象 o 定义了一个名为 toString() 的方法，该方法屏蔽了原型中（不可枚举）的 toString() 方法。在 IE 中，由于其实现认为原型的 toString() 方法被打上了值 false 的 [[Enumerable]] 标记，因此应该跳过该属性，结果我们就不会看到警示框。该 bug 会影响默认不可枚举的所有你属性和方法，包括：hasOwnProperty()、propertyIsEnumberable()、toLacaleString()、toString() 和 valueOf()。ECMAScript 5 也将 constructor 和 prototype 属性的 [[Enumerable]] 特性设置为 false，但并不是所有浏览器都照此实现。
  
  要取得对象上所有可枚举的实例属性，可以使用 ECMAScript 5 的 Object.keys() 方法。这个方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组。例如：
  
  ```js
  function Person() {
  }

  Person.prototype.name = "Nicholas";
  Person.prototype.age = 29;
  Person.prototype.job = "Software Engineer";
  Person.prototype.sayName = function() {
     alert(this.name);
  }
  
  var keys = Object.keys(Person, prototype);
  alert(keys); // "name, age, job, sayName"
  
  var p1 = new Person();
  p1.name = "Rob";
  p1.age = 31;
  var p1keys = Object.keys(p1);
  alert(p1keys); // "name, age"
  ```
  这里，**变量 keys 中将保存一个数组**，数组中是字符串“name”、“age”、“job” 和 “sayName”。**这个顺序也是它们在 for-in 循环中出现的顺序** 。如果是通过 Person 的实例调用，则 Object.keys() 返回的数组只包含 “name” 和 “age”这两个实例属性。
  
  如果你想要得到所有实例属性，无理论它是否可枚举，都可以使用 **Object.getOwnPropertyNames()** 方法。
  
  ```js
  var keys = Objects.getOwnPropertyNames(Person.proptotype);
  alert(keys); // "constructor、name、age、job、sayName"
  ```
  注意结果中包含了不可枚举的 constructor 属性。Object.keys() 和 Object.getOwnPropertyNames() 方法都可以用来替代 for0in 循环。支持这两个方法的浏览器有 IE9+、Firefox4+、Safari5+、Opera12+ 和 Chrome。
  
- #### 更简单的原型语法

  读者大概注意到了，前面的例子中每添加一个属性和方法就哟啊敲一遍 Person.prototype 。为减少不必要的输入，也为了从视觉上更好的封装原型的功能，更常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象，如下面的例子所示。
  
  ```js
  function Person() {
  }
  
  Person.prototype = {
    name: "Nicholas",
    age: 9,
    job: "Software Engineer",
    sayName: function() {
      alert(this.name);
    }
  };
  ```
  在上面的代码中，我们将 Person.prototype 设置为等于一个以对象字面量形式创建的新对象。最终结果相同，但有一个例外：**constructor 属性不再指向 Person 了** 。前面曾经介绍过，每创建一个函数，就会同时创建它的 prototype 对象，这个对象也会自动获得 constructor 属性。而我们子在这里使用的语法，**本质上完全重写了默认的 prototype 对象** ，因此 **constructor 属性也就变成了新对象的 constructor 属性**（指向 Object 构造函数），不再指向 Person 函数。此时，尽管 instanceof 操作符还能返回正确的结果，但通过 constructor 已经无法确定对象的类型了，如下所示。
  
  ```js
  var friend = new Person();
  
  alert(friend instanceof Object); //true
  alert(friend instanceif Person); //true
  alert(friend.constructor == Person); //false
  alert(friend.constructor == Object); //true
  ```
  在此，用 instanceof 操作符测试 Object 和 Person 仍然返回 true，但 constructor 属性则等于 Object 而不等于 Person 了。如果 constructor 的值真的很重要，可以像下面这样特意将它设置回适当的值。
  
  ```js
  function Person() {
  }
  
  Person.prototype = {
    constructor: Person,
    name: "Nicholas",
    age: 9,
    job: "Software Engineer",
    sayName: function() {
      alert(this.name);
    }
  };
  ```
  以上代码特意包含了一个 constructor 属性，并 **将它的值设置为 Person** ，**从而确保了通过该属性能访问到适当的值** 。
  
  注意，以上 **这种方式重设 constructoe 属性会导致它的 [[Enumberable]] 特性被设置为 true** 。默认情况下，**原生的 constructor 属性是不可枚举的** ，因此如果你使用兼容 ECMAScript 5 的 JavaScript 引擎，可以试一试 Object.definePeoperty()。
  
  ```js
  function Person() {
  }
  
  Person.prototype = {
    name: "Nicholas",
    age: 9,
    job: "Software Engineer",
    sayName: function() {
      alert(this.name);
    }
  };
  
  // 重设构造函数，只适用与于 ECMAScript 5 兼容的浏览器
  Object.definePeoperty(Person.prototype, "constructor", {
    enumerable: false,
    value: Person
  });
  ```
- #### 原型的动态性

  **由于原型中查找值的过程是一次搜索** ，因此我们 **对原型对象所做的任何修改都能够立即从实例上反映出来** ——即使是 **先创建了实例后修改原型也照样如此** 。请看下面的例子。
  
  ```js
  var friend = new Person();
  
  Person.prototype.saiHi = function() {
    alert("hi");
  }
  
  friend.sayHi();  //"hi"（没有问题！）
  ```
  以上代码先创建了 Person 的一个实例，并将其保存在 friend 。然后，下一条语句在 Person.prototype 中添加了一个方法 sayHi()。即使 friend 实例是在添加新方法之前创建的，但它仍然可u一访问这个新方法。其原因可以归结为 **实例与原型之间的松散连接关系** 。当我们调用 friend.sayHi() 时，首先会在实例中搜索名为 sayHi 的属性，在没有找到的情况下，会继续搜索原型。因为 **实例与原型之间的连接只不过是一个指针** ，而非副本，因此就可以在原型中找到新的 sayHi 属性并返回保存在那里的函数。
  
  尽管可以随时为原型添加属性和方法，并且修改能够立即在所有对象实例中反映出来，但如果是**重写整个原型对象** ，那么情况就不一样了。我们知道，**调用构造函数时** ，**会为实例添加一个指向最初原型的 [[Prototype]] 指针** ，而 **把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系** 。请记住： **实例中的指针仅指向原型** ，而 **不指向构造函数** 。看下面的例子。
  
  ```js
  function Person() {
  
  }
  
  var friend = new Person();
  
  Person.prototype = {
    constructor: Person,
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName : function () {
      alert(this.name);
    }
  };
  
  friend.sayName(); // error
  ```
  在这个例子中，我们先创建了 Person 的一个实例，然后又重写了其原型对象，然后在调用 friend.sayName() 时发生错误，因为 friend 指向原型中不包含以该名字命名的属性。下图展示了这个过程的内幕。
  
  ![过程的内幕](http://p9myzkds7.bkt.clouddn.com/JavaScript-object-oriented/%E8%BF%87%E7%A8%8B%E5%B1%95%E7%A4%BA.jpg)
  
  从上图可以看出，**重写原型对象切断了现有原型与任何之前已经存在的对象实例之间的联系** ；它们 **引用的仍然是最初的原型** 。
  
- #### 原生对象的原型

  原型模式的重要性 **不仅体现在创建自定义类型方面** ，就连 **所有原生的引用类型** ， **都采用这种模式创建的** 。所有原生引用类型（Object、Array、String，等等）都在其构造函数的原型上定义了方法。例如，在 Array.prototype 中可以找到 sort() 方法，而在 String.prototype 中可以找到 substring() 方法，如下所示。
  
  ```js
  alert(typeof Array.prototype.sort); // "function"
  alert(typeof String.prototype.substring()); // "function"
  ```
  通过原生对象的原型，不仅 **可以取得所有默认方法的引用** ，而且也 **可以定义新方法** 。**可以像修改自定义对象的原型一样修改原生对象的原型** ，因此 **可以随时添加方法** 。下面的代码就给基本包装类型 String 添加了一个名为 startsWith() 的方法。
  
  ```js
  String.prototype.startsWith = function (text) {
    return this.indexOf(text) == 0;
  }
  
  var msg = "Hello world!";
  alert(msg.startsWith("Hello")); //true
  ```
  这里新定义的 startsWith() 方法会在传入的文本位于一个字符串开始时返回 true。既然 **方法被天添加给了 String.prototype** ，那么 **当前环境中的所有字符串都可以调用它** 。由于 msg 是字符串，而且后台会调用 Sttring 基本包装函数创建这个字符串，因此通过 msg 就可以调用 sartsWith() 方法。
  
  > 尽管可以这样做，但我们 **不推荐在产品化的程序中修改原生对象的原型** 。如果因为某个实现中缺少某个方法，就在原生对象的原型中添加这个方法，那么当在另一个支持该方法的实现中运行代码时，就 **可能会导致命名冲突** 。而且，这样做也 **可能会意外重写原生方法** 。
  
- #### 原型对象问题

  原型模式也不是没有缺点。首先，它 **省略了为构造函数传递初始化参数这一环节** ，结果所有实例在 **默认情况下都将取得相同的属性值** 。虽然这会在某种程度上带来一些不方便，但还不是原型的最大问题。原型模式的最大问题是 **由其共享的本性所导致的** 。
  
  原型中所有属性是被很多实例共享的，这种共享 **对于函数非常适合** 。 **对于那些包含基本值的属性倒也说得过去** ，毕竟（如前面的例子所示），通过在实例上添加一个同名属性，可以隐藏原型中的对应属性。然而，**对于包含引用类型值的属性来说**  ，**问题就比较突出了** 。来看下面的例子。
  
  ```js
  function Person() {
  
  }
    
  Person.prototype = {
    constructor: Person,
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    friends: ["Shelby", "Court"],
    sayName : function () {
      alert(this.name);
    }
  };
  
  var person1 = new Person();
  var person2 = new Person();
  
  person1.friends.push("Van");
  
  alert(person1.friends); // "Shelby, Court, Van"
  alert(person2.friends); // "Shelby, Court, Van"
  alert(person1.friends == person2.friends); // ture
  ```
  在此，Person.prototype 对象有一个名为 friends 的属性，该属性包含一个字符串数组。然后，创建了 Person 的两个实例。接着，修改了 person1.friends 引用的数组，向数组中添加了一个字符串。**由于 friends 数组存在于 Person.prototype 而非 person1 中** ，所以刚刚提到的 **修改也会通过 person2.frends（与 person21.friends  指向同一个数组）反映出来** 。假如我们的 **初衷就是像这样在所有实例共享一个数组（静态方法或属性）** ，那么这个结果我没话可说。可是，**实例一般都是需要有属于自己的全部属性的** 。而这个问题正是我们 **很少看到有人单独使用原型模式的原因所在** 。
  
 ### 组合使用构造函数模式和原型模式
 
 创建自定义类型的最常见方式，就是组合使用构造函数模式和原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。另外，这种混成模式还支持向构造函数传递参数；可谓是集两种模式之长。下面的代码重写了前面的例子。
 
 ```js
 function Person(name, age, job) {
   this.name = name;
   this.age = age;
   this.job = job;
   this. friends = ["Shelby", "Court"];
 }
 
 Person.prototype = {
   constructor: Person,
   sayName: function() {
     alert(this.name);
   }
 }
 
 var person1 = new Person("Nicholas", 29, "Software Engineer");
 var person2 = new Person("Greg",  27, "Doctor");
 
 person1.friends.push("Van");
 alert(person1.friends); // "Shelby, Count, Van"
 alert(person2.friends); // "Shelby, Count"
 alert(person1.friends == person2.friends); // false
 alert(person1.sayName == person2.sayName); //true
 ```
 在这个例子中，**实例属性都是在构造函数中定义的** ，而由 **所有实例共享的属性 constructor 和 方法 sayName() 则是在原型中定义的** 。而修改了 person1.friends（向其中添加一个新字符串）， 并不会影响到 person2.friends, 因为它们分别 **引用了不同的数组** 。
 
 这种 **构造函数与原型模式混成的模式** ，是目前在 ECMAScript 中使用 **最广泛** 、**认同度最高**的 **一个创建自定义类型的方法** 。可以说，这是 **用来定义引用类型的一种默认模式** 。

### 动态原型模式

有其他 OO 语言经验的开发人员在看到独立的构造函数和原型时，很可能会感到非常困惑。动态原型模式正是致力于解决这个问题的一个方案，它 **把所有信息都封装在了构造函数中** ，而 **通过构造函数中初始化原型** （仅在必要的情况下），又 **保持了同时使用构造函数和原型的优点** 。换句话说，**可以通过检查某个应该存在的方法是否有效** ，**来决定是否需要初始化原型** 。来看一个例子。

```js
function Person(name, age,, job) {
  
  // 属性
  this.name = name;
  this.age = age;
  this.job = job;
  
  //方法
  if (typeof this.sayName != "function") {
    Person.prototype.sayName = function() {
      alert(this.name);
    };
  }
}

var friend =  new Person("Nicholas", 29, "Software Engineer");
friend.sayName();
```
注意构造函数代码中加粗的部分。这里 **只在 sayName() 方法不存在的情况下，才会将它添加到原型中** 。**这段代码只会在初次调用构造函数时才会执行** 。此后，原型已经完成初始化，不需要再做什么修改了。不过要记住，这里对原型所做的修改，能够立即在所有实例中得到反映。因此，这种方法确实可以说非常完美。其中，if 语句检查的可以是初始化之后应该存在的任何属性或方法—— **不必用一大堆 if 语句检查每个属性和每个方法** ；**只要检查其中一个即可** 。对于采用这种模式创建的对象，还 **可以用 instanceof 操作符确定它的类型** 。

> **使用动态原型模式时**，**不能使用对象字面量重写原型** 。前面解释过了，如果在 **已创建了实例的情况下** 重写原型，那么就 **会切断现有实例与原型之间的联系** 。


### 寄生构造函数模式

  通常，在前述的几种模式都不适用的情况下，可以使用 **寄生（parasitic）构造函数模式** 。这种模式的基本思想是 **创建一个函数** ，该函数的作用 **仅仅是封装创建对象的代码** ，然后 **再返回新创建的对象** ；但 **从表面上看，这个函数又很像是典型的构造函数** 。下面是一个例子。
 
 ```js
 function Person(name, age, job) {
   var o = new Object();
   o.name = name; 
   o.job = job;
   o.sayName = function() {
     alert(this.name);
   };
   return o;
 }
 
 var friend = new Person("NIcholas", 29, "Software Engineer");
 friend.sayName(); // "Nicholas"
 ```
 在这个例子中， Person 函数创建了一个新对象，并以相应的属性和方法初始化该对象，然后又返回了这个对象。除了 **使用 new 操作符并把使用的包装函数叫做构造函数之外** ，**这个模式跟工厂模式其实是一摸一样的** 。**构造函数在不返回值的情况下** ，**默认会返回新对象的实例** 。而通过 **在构造函数的末尾添加一个 return 语句** ，**可以重写调用构造函数时返回的值** 。
 
 这个模式 **可以在特殊的情况用来为对象创建构造函数** 。假设我们想 **创建一个具有额外方法的特殊数组** 。由于 **不能直接修改 Array 构造函数** ，因此可以使用这个模式。
 
 ```js
 function SpecialArray() {
   
   // 创建数组
   var values = new Array();
   
   // 添加值
   values.push.apply(values, arguments);
   
   values.toPipedString = function () {
     return this.join("|");
   };
   
   //返回值
   return values;
 }
 
 var colors = new SpecialArray("red", "blue", "green");
 alert(colors.toPipedString()); // "red|blue|green"
 ```
 在这个例子中，我们创建了一个名叫 SpecialArray 的构造函数。在这个函数内部，首先创建了一个数组，然后 **push() 方法（用构造函数接收到的所有参数） 初始化了数组的值** 。随后，又给数组实例添加了一个 toPipedString() 方法，该方法返回以竖线分隔的数组值。最后 ，将数组以函数值的形式返回。接着，我们调用了 SpecialArray 构造函数，向其中传入了用于初始化数组的值，此后又调用了 toPipedString() 方法。
 
  关于寄生构造函数模式，有一点需要说明：首先，**返回的对象与构造函数或者与构造函数的原型属性之间没有关系** ；也就是说，**构造函数返回的对象与构造函数外部创建的对象没有什么不同** 。为此，**不能依赖 instanceof 操作符来确定对象类型** 。由于 存在上述问题，**我们建议在可以使用其他模式的情况下，不要使用这种模式** 。
 
 
 ### 稳妥构造函数模式
 
 道格拉斯。格罗克福德（Douglas Crockford）发明了 JavaScript 中的 **稳妥对象** （durable objects ）这个概念。所谓稳妥对象，指的是 **没有公共属性** ，而且 **其他方法也不引用 this 的对象** 。**妥对象最适合在一些安全的环境中** （这些环境中会禁止使用 this 和 new）,或者 **防止数据被其他应用程序（如 Mashup 程序）改动时使用** 。**稳妥构造函数遵循与寄生构造函数类似的模式** ，但有两点不同：一是 **新创建对象的实例方法不引用 this** ; 二是 **不使用 new 操作符调用构造函数** 。按照稳妥构造函数的要求，可以将前面的 Person 构造函数重写如下。
 
 ```js
 function Person(name, age, job) {
   
   // 创建要返回的对象
   var o = new Object();
   
   //可以在这里定义私有变量和函数
   
   //添加方法
   o.sayName = function () {
     alert(name);
   };
   
   // 返回对象
   return o;
 }
 ```
 注意，在以这种模式创建的对象中，**除了使用 sayName() 方法之外没有其他方法可以访问 name 的值** 。可以像下面使用稳妥 Person 构造函数。
 
```js
var friend = Person("Nicholas", 29, "Software Engineer");
friend.sayName(); // "Nicholas"
```
这样，变量 friend 中保存的是一个稳妥对象，而除了调用 sayName() 之外，没有别的方式可以访问其数据成员。即使有其他代码会给这个对象添加方法或数据成员，但也不可能有别的办法访问传入构造函数中的原始数据。**稳妥构造函数模式提供的这种安全性** ，使得 **它非常适合在某些安全执行环境** —— 例如，ADsafe （www.adsafe.org）和 Caja
（ http://code.google.com/p/google-caja/ ）提供的环境——下使用。

> 与寄生构造函数模式类似，使用 **稳妥构造函数模式创建的对象与构造函数之间也没有什么关系** ，因此 **instanceof 操作符对这种对象也没有意义** 。

  
  
  
  


  
  
  
  






  