title: JavaScript 原型链和继承
author: songxingguo
tags:
  - JavaScript
categories:
  - 前端技术
date: 2018-12-22 11:01:00
---
## 原型链

### 构造函数、原型对象和实例三者之间的关系：

![关系图](https://graphbed.qiniu.songxingguo.com/prototype-and-inherit/1544147270338.png)

<!-- more -->

三个概念：

- 构造函数：被 new 操作符操作的普通函数。

- 实例：

- 原型对象：

三个属性： 

- __proto__

- prototype

- constuctor

对象间的关系：

![三者关系](https://graphbed.qiniu.songxingguo.com/prototype-and-inherit/%E4%B8%89%E8%80%85%E7%9A%84%E5%85%B3%E7%B3%BB.jpg)


> 抓住 new 运算符，然后向上梳理。

**控制台输出原型链** ：

![原型链](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-html-css/%E5%8E%9F%E5%9E%8B%E9%93%BE.png)

### new 运算符

[new 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)

![new 运算符](https://graphbed.qiniu.songxingguo.com/prototype-and-inherit/new%E6%93%8D%E4%BD%9C%E7%AC%A6)

重点：

- 如果构造函数没有返回对象，才会返回创建的新对象。

## 继承

### 原型链

> 原型对象将包含指向另一个原型的指针,另一个原型中也包含着一个指向另一个构造函数的指针,如此层层递进 ，就构成了实例与原型的链条。

```js
function SuperType() {
  this.property = true;
}

SuperType.prototype.getSuperValue = function() {
  return this.property;
}

function SubType() {
  this.subproperty = false;
}

//继承了 SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function() {
  return this.subproperty;
}

var instance = new SubType();
alert(instance.getSuperValue()); // true
```
实例以及构造函数和原型之间的关系：

![实例以及构造函数和原型之间的关系](https://graphbed.qiniu.songxingguo.com/JavaScript-object-oriented/%E5%AE%9E%E4%BE%8B%E4%BB%A5%E5%8F%8A%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E5%92%8C%E5%8E%9F%E5%9E%8B%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB%20.png)

完整的原型链：

> 所有引用类型默认都继承了 Object 。

![完整的原型链](https://graphbed.qiniu.songxingguo.com/JavaScript-object-oriented/%E5%AE%8C%E6%95%B4%E7%9A%84%E5%8E%9F%E5%9E%8B%E9%93%BE.png)

#### 确定原型和实例的关系

- 使用 instanceof 操作符

  > instanceof 可以判断实例对象的 `__proto__` 属性是否与构造函数的 prototype 属性指向同一地址。
  
  > instanceof 左边是实例，右边是构造函数。

  > 只要用这个操作符来测试实例与原型链中出现过的构造函数，结果就会返回 true。

  ```js
  alert(instance instanceof Object); //true
  alert(instance instanceof SuperType); //true
  alert(isntance instanceof SubType); //true
  ```
- 使用 isPrototypeOf()

  > 只要是原型链中出现过的原型 ，都可以说是该原型链所派生的实例的原型 ，因此 isPrototypeOf() 方法会返回 true 。

  ```js
  alert(Object.prototype.isPrototypeOf(instance)); //true
  alert(SuperType.prototype.isPrototype(instance)); //true
  alert(SubType.prototype.isPrototype(instance)); //true
  ```
#### 谨慎地定义方法

子类型有时候需要覆盖超类型中的某个方法 ，或者 需要添加超类型中不存在的某个方法 。但不管怎样，给原型添加方法代码一定要放在替换原型的语句之后 。

```js
function SuperType() {
  this.property = true;
}

SuperType.prototype.getSuperValue = function() {
  return this.property;
}

function SubType() {
  this.subproperty = false;
}

//继承了 SuperType
SubType.prototype = new SuperType();

// 添加新方法
SubType.prototype.getSubValue = function() {
  return this.subproperty;
}

// 重写超类型中方法
SubType.prototype.getSuperValue = function() {
  return false;
}

var instance = new SubType();
alert(instance.getSuperValue()); // false
```

通过原型链实现继承时 ，不能使用对象字面量创建原型方法 。因为 这样做就会重写原型链 ，如下面的例子所示。

```js
function SuperType() {
  this.property = true;
}

SuperType.prototype.getSuperValue = function() {
  return this.property;
}

function SubType() {
  this.subproperty = false;
}

SubType.prototype = {
  getSubValue: function() {
    return this.subproperty;
  },
  
  someSotherMethod: function () {
    return false;
  }
};

var instance = new SubType();
alert(instance.getSuperValue()); // error
```
#### 原型链的问题

- 包含引用类型值的原型属性会被所有实例共享。

  ```js
  function SuperType() {
    this.colors = ["red", "blue", "green"];
  }

  function SubType() {
  }

  //继承了 SuperType
  SubType.prototype = new SuperType();

  var instance1 = new SubType();
  instance1.colors.push("black");
  alert(instance1.colors); // "red、blue、green、black"

  var instance2 = new SubType();
  alert(instance2.colors); // "red、blue、green、black"
  ```
- 在创建子类型的实例时，不能向超类型的构造函数中传递参数 。

### 借用构造函数

> 在子类型构造函数的内部调用超类型构造函数。

**函数只不过是时在特定环境中执行代码的对象** ，因此通过使用 apply() 和 call() 方法也可以在（将来）新创建的对象上执行构造函数，如下所示：

```js
function SubType() {
  this.colors = ["red", "blue", "green"];
}

function SubType() {
  // 继承了 SuperType 
  SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push("blackk");
alert(instance1.colors); // "red, blue, green, black"

var istance2 = new SubType();
alert(instance2.colors); // "red，blue,green"
```
在（未来将要）新创建的 SubType 实例的环境下调用了 SuperType 构造函数。

**借用构造函数的优势**

可以在子类型构造函数中向超类型构造函数传递参数。

```js
function SuperType(name) {
  this.name = name;
}

function SubType() {
  // 继承了 SuperType，同时还传递了参数
  SuperType.call(this, "Nicholas");
  
  // 实例属性
  this.age = 29;
}

var instance = new SubType();
alert(instance.name); // "Nicholas"
alert(instance.age); //29
```

**借用构造函数的问题**

- 方法都在构造函数中定义，因此函数复用就无从谈起了。
- 在超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型只能构造函数模式。

### 组合继承

> **使用原型链实现对原型属性和方法的继承** ，而 **通过借用构造函数来实现对实例属性的继承** 。

```js
function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
  alert(this.name);
};

function SubType(name, age) {
  // 继承属性
  SuperType.call(this，name);
  
  this.age = age;
}

//继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function() {
  alert(this.name);
}

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); // "red, blue, green, black"
instance1.sayName(); // "Nicholas"
instance1.sayAge(); //29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors); // "red, blue, green"
instance2.sayName();  //"Greg"
instance2.sayAge()； // 27
```
让两个不同的 SubType 实例既分别拥有自己属性 —— 包括 colors 属性，又可以使用相同的方法了。

**组合继承的问题**

- 无论什么情况下，都会调用两次超类型构造函数；一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。

- 子类型最终会包含超类型对象的全部实例属性，但我们不得不在调用子类型构造时重写这些属性。

组合继承

```js
function superType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function () {
  alert(this.name);
}

function SubType(name, age) {
  SuperType.call(this, name); // 第二次调用 SuperType()
  
  this.age = age;
}

SubType.prototype = new SuperType();  //第一次调用 SuperType()
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function() {
  alert(this.name);
}
```
组合继承调用过程

![组合继承调用过程](https://graphbed.qiniu.songxingguo.com/JavaScript-object-oriented/%E7%BB%84%E5%90%88%E7%BB%A7%E6%89%BF%E8%B0%83%E7%94%A8%E8%BF%87%E7%A8%8B.png)

### 原型式继承

> 借助原型可以基于已有对象创建新对象，同时还不必因此创建自定义类型。

在传入一个参数的情况下，Object.create() 与 object() 方法的行为相同。

```js
var person =  {
  name: "Nicholas",
  friends: ["Shrlby", "Court", "Van"]
};

var anotherPeron = object.create(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson object.create(person);
yetAnotherPerson.name = "Linda";
yetAntherPerson.friends.push("Barbie");

alert(person.friends); //"Shelby, Court, Van, Rob, Barbie"
```
Object.creat() 方法的第二个参数与 Object.defineProperties() 方法的第二个参数格式相同：每个属性都是通过自己描述符定义的。以这种方式指定的任何属性都会覆盖原型对象上的同名属性。

```js
var person =  {
  name: "Nicholas",
  friends: ["Shrlby", "Court", "Van"]
};

var anotherPeron = object.create(person, {
  name: {
    value: "Greg"
  }
});

alert(anotherPerson.name); //"Greg"
```
**原型式继承的问题**

- 只是让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的。

### 寄生式继承

> 创建一个仅用于封装继承过程的函数，该函数之内以某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象。

```
function createAnother(original) {
  var name = object(original); //通过调用函数创建一个新对象
  clone.sayHi = function() {  //以某种方式来增强这个对象
    alert("Hi");
  };
  return clone; //返回这个对象
}

var person =  {
  name: "Nicholas",
  friends: ["Shrlby", "Court", "Van"]
};

var anotherPeron = object.createAnother(person);
anotherPeson.sayHi(); //"hi"
```

**寄生式继承的问题**

- 使用寄生式继承来为对象添加函数，会由于不能做到函数复用而降低效率。

### 寄生组合式继承

> 即通过借用构造函数来继承属性，通过原型链的混成形式继承方法。使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

```js
function inheritPrototype(subType, superType) {
  var prototype = Object(superType.prototype);  //创建对象
  prototype.constructor = subType;  // 增强对象
  subType.prototype = prototype;   // 指定对象
}

function superType(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function () {
  alert(this.name);
}

function SubType(name, age) {
  SuperType.call(this, name);
  
  this.age = age;
}

inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function() {
  alert(this.name);
}
```

## apply()、 call() 以及 bind()


### 相同之处

- call()、apply()、bind() 都是用来重定义 this ，改变 this 指向。

- 每个函数都包含两个非继承而来的方法： apply()、 call() 和 bind()。

### 不同之处

**返回值的区别**

![返回值的区别](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-html-css/call%28%29%E3%80%81apply%28%29%E3%80%81bind%28%29%20%E4%B8%89%E8%80%85%E7%9A%84%E5%8C%BA%E5%88%AB.png)

```js
obj.myFun.call(db)；　　　  //德玛年龄99
obj.myFun.apply(db);　　　 //德玛年龄99
obj.myFun.bind(db)();　　　//德玛年龄99
```
> bind() 返回的是一个新的函数，你必须调用它才会被执行。

**参数的区别**

![返回值的区别](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-html-css/call%28%29%E3%80%81apply%28%29%E3%80%81bind%28%29%20%E4%B8%89%E8%80%85%E7%9A%84%E5%8C%BA%E5%88%AB.png)

```js
obj.myFun.call(db,'成都','上海')；　　　　 //德玛 年龄 99  来自 成都去往上海
obj.myFun.apply(db,['成都','上海']);        //德玛 年龄 99  来自 成都去往上海  
obj.myFun.bind(db,'成都','上海')();         //德玛 年龄 99  来自 成都去往上海
obj.myFun.bind(db,['成都','上海'])();　　 //德玛 年龄 99  来自 成都,上海去往undefined
```

> 第一个参数都是 this 的指向对象。

> 第二个参数 apply() 只需要传入数组，而在使用 bind() 和 call() 方法时，传递给函数的参数必须逐个列举出来。

### 作用

- 在特定的作用域中调用函数 ，实际上等于 **设置函数体内 this 对象的值** 。

- 它们真正强大的地方是 **能够扩充函数赖以运行的作用域** 。

- 对象不需要与方法有任何耦合关系 。

### 传参与返回值

**apply()**

接收两个参数：一个是 在其中运行函数的作用域 ，另一个是 **参数数组** 。

**call()**

接收两个参数：一个是 在其中运行函数的作用域 ，另一个是 **参数列表** 。

**bind()** 

接收两个参数：一个是 在其中运行函数的作用域 ，另一个是 **参数列表** ,并 **返回函数**。

> 创建一个函数的实例 ，其 this 值会被绑定到传给 bind() 函数的值 。

来自—— [javascript中call()、apply()、bind()的用法终于理解](https://www.cnblogs.com/Shd-Study/archive/2017/03/16/6560808.html)
