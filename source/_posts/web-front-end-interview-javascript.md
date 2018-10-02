title: Web前端面试题目及详解汇总-JavaScript部分
author: songxingguo
tags: []
categories:
  - 找工作
date: 2018-08-06 14:30:00
---
### 请指出document load和document ready的区别？
 
 共同点：这两种事件都代表的是页面文档加载时触发。

异同点：

ready 事件的触发，表示文档结构已经加载完成（不包含图片等非文字媒体文件）。

onload 事件的触发，表示页面包含图片等文件在内的所有元素都加载完成。

<!-- more -->

### JavaScript中如何检测一个变量是一个String类型？请写出函数实现。

方法一：typeof

```js
function isString(obj){
    return typeof(obj) === "string"? true: false;
    // returntypeof obj === "string"? true: false;
}
```

方法二：constructor

```js
function isString(obj){
    return obj.constructor === String? true: false;
}
```

方法三：call()
```js
function isString(obj){
     return Object.prototype.toString.call(obj) === "[object String]"?true:false;
}
如：
var isstring = isString('xiaoming');
console.log(isstring);  // true
```

### 请用js去除字符串空格？

方法一：使用replace正则匹配的方法

  去除所有空格: str = str.replace(/\s*/g,"");      

  去除两头空格: str = str.replace(/^\s*|\s*$/g,"");

  去除左空格： str = str.replace( /^\s*/, “”);

  去除右空格： str = str.replace(/(\s*$)/g, "");
  
str为要去除空格的字符串，实例如下：

 ```js
 var str = " 23 23 ";
 var str2 = str.replace(/\s*/g,"");
 console.log(str2); // 2323
 ```
 
 来自—— [2018年web前端经典面试题及答案]
 
 [2018年web前端经典面试题及答案]:https://www.cnblogs.com/wdlhao/p/8290436.html
 

### this 指向

1. this永远指向最后调用它的上级对象。

2. New 关键字可以改变 this 指向。

3. 自行改变 this 指向 call()、apply()、bind()。

4. 返回值是对象，this 指向对象，否则指向函数实例（null 也为对象）。

5. ES6 中箭头函数中 this 指向定义时所在对象。

> 严格模式中默认的 this 不再是 window，而是 undefined。

来自——[彻底理解js中this的指向，不必硬背。] 、 [浅析js之this --- 一次性搞懂this指向]

[彻底理解js中this的指向，不必硬背。]:https://www.cnblogs.com/pssp/p/5216085.html
[浅析js之this --- 一次性搞懂this指向]:https://www.cnblogs.com/cara-front-end/p/6762086.html

### 写出3个使用this的典型应用

- 在html元素事件属性中使用

  ```html
  <input type=”button” onclick=”showInfo(this);” value=”点击一下”/>
  ```

- 构造函数

  ```js
  function Animal(name, color) {
    this.name = name;
    this.color = color;
  }
  ```

- 访问当前事件

  ```html
  <input type=”button” id=”text” value=”点击一下”/>

  ```

  ```js
  Var btn = document.getElementById(“text”);

  Btn.onclick = function() {

    Alert(this.value); //此处的this是按钮元素

  }
  ```

- CSS expression表达式中使用this关键字

  ```
  <table width="100" height="100">
  <tr>
  <td>
  <div style="width: expression(this.parentElement.width); 
  height: expression(this.parentElement.height);">
  division element</div>
  </td>
  </tr>
  </table>
  ```
  这里的this指代div元素对象实例本身。
  
  
- apply()/call()求数组最值

  ```js
  var  numbers = [5, 458 , 120 , -215 ]; 
  var  maxInNumbers = Math.max.apply(this, numbers);  
  console.log(maxInNumbers);  // 458
  var maxInNumbers = Math.max.call(this,5, 458 , 120 , -215); 
  console.log(maxInNumbers);  // 458
  ```

来自——[四个使用this的典型应用]、[JavaScript中this的9种应用场景及三种复合应用场景]、[3个使用this的典型应用]

[四个使用this的典型应用]:http://www.cnblogs.com/kuangliu/p/3462727.html

[JavaScript中this的9种应用场景及三种复合应用场景]:https://www.jb51.net/article/72146.htm

[3个使用this的典型应用]:https://blog.csdn.net/cai181191/article/details/82718379


### 作用域和作用域链

#### 作用域

变量作用域： 全局作用域和局部作用域（在函数内部有用）。

全局作用域： function 声明的函数为全局的、不使用 var 声明的变量。

局部作用域：使用 var 声明的变量。

> JavaScript 没有块级作用域，JavaScript 作用域为函数作用域。

#### 作用域链

内部函数可以访问外部函数变量的机制，用链式查找决定哪些数据能被内部函数访问。

> 执行环境：js 的执行顺序是根据函数的调用来决定，函数被调用，函数环境变量对象就被压入到一个环境栈中，而函数执行之后，函数环境变量对象弹出，把控制权交给之前的执行环境变量对象。（保存在[Scope] 中）。

#### 如何理解闭包？

闭包： 内部函数的作用域链仍然保存这对父函数活动对象的引用。

闭包的作用：

- 可以读取自身函数外部的变量（沿着作用域链寻找）。 
- 让这些外部变量始终保存在内存中 。

> js函数内的变量值不是在编译的时候就确定的，而是等在运行时期再去寻找的。

来自——[JavaScript关于作用域、作用域链和闭包的理解]

[JavaScript关于作用域、作用域链和闭包的理解]:https://blog.csdn.net/whd526/article/details/70990994

### 你如何获取浏览器URL中查询字符串中的参数？

### js 字符串操作函数

### 怎样添加、移除、移动、复制、创建和查找节点？

### 比较typeof与instanceof？

### 什么是跨域？跨域请求资源的方法有哪些？

### 谈谈垃圾回收机制方式及内存管理

### 开发过程中遇到的内存泄露情况，如何解决的？

### javascript面向对象中继承实现？JS继承与原型问题

### JavaScript 数组(Array)对象

### 模块化编程

### 一个页面从URL到加载显示完成，都发生了什么？

### 队列、堆、栈的区别？