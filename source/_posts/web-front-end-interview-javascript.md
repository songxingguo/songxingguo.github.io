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

### 你如何获取浏览器URL中查询字符串中的参数？

### js 字符串操作函数

### 怎样添加、移除、移动、复制、创建和查找节点？

### 写出3个使用this的典型应用

### 比较typeof与instanceof？

### 如何理解闭包？

### 什么是跨域？跨域请求资源的方法有哪些？

### 谈谈垃圾回收机制方式及内存管理

### 开发过程中遇到的内存泄露情况，如何解决的？

### javascript面向对象中继承实现？JS继承与原型问题

### JavaScript 数组(Array)对象

### 模块化编程

### 一个页面从URL到加载显示完成，都发生了什么？

### 队列、堆、栈的区别？
