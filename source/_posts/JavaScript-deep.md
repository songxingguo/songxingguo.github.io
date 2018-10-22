title: 你所不知道的JavaScript
author: songxingguo
tags: []
categories:
  - 前端技术
date: 2018-10-22 15:19:00
---
### 0.1 + 0.2 == 0.30000000000000004

```js
console.log(0.1 + 0.2 == 0.3); // false
```
大家可以看到对于上面的这个计算我们很容易得出答案是 0.3，然而当输入到计算机的却发现这个等式是不成立。

关于浮点数值计算会产生舍入误差的问题，有一点需要明确：这是使用基于 **IEEE754** 数值的浮点计算的通病 ，ECMAScript 并非独此一家；其他使用相同数值格式的语言也存在这个问题。

<!-- more -->

#### 什么是IEEE754

[百度百科]：IEEE二进制浮点数算术标准（IEEE 754）是20世纪80年代以来最广泛使用的浮点数运算标准，为许多CPU与浮点运算器所采用。

[百度百科]:https://baike.baidu.com/item/IEEE%20754/3869922?fr=aladdin

### 逻辑非、逻辑与和 逻辑或

```
( window.foo || ( window.foo = "bar" ) ); //bar
```
根据逻辑与的规则：如果第一个操作数是对象，则返回第二个操作数。上面代码中 window.foo 是一个对象，所以就会直接返回第二个操作数。

参考地址：[布尔操作符]

[布尔操作符]:http://localhost:4000/2018/08/30/JavaScript-basic-concepts/


### “连等赋值”问题

```js
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
console.log(foo.x); // undefined
```

![连等赋值](http://p9myzkds7.bkt.clouddn.com/JavaScript-deep/%E8%BF%9E%E7%AD%89%E8%B5%8B%E5%80%BC.png)

如上图所看到的“连等赋值”分为两步：

1. 确定指针
2. 赋值

首先是在内存空间中添加一个 `foo.x`的指针。

然后将 `{n: 2}` 赋值给foo,接着又将 `{n: 2}` 赋值给 `foo.x`的指针。

最后 foo 指针指向的是 `{n: 2}`。
 
所以 `foo.x` 为 undefined。

参考地址：[深入理解“连等赋值”问题]、[关于js连等赋值的问题]

[关于js连等赋值的问题]:https://blog.csdn.net/sinat_36598441/article/details/53384567
[深入理解“连等赋值”问题]:https://segmentfault.com/a/1190000004224719

### 异步执行

```
for (var i = 0; i < 2; i++) {
    console.log(i);  // 0 1
}

for (var i = 0; i < 2; i++) {
    setTimeout(() => console.log(i)); // 2 2
}

for (let i = 0; i < 2; i++) {
    setTimeout(() => console.log(i)); // 0 1
}
```
上面三个例子中，应该第二个是最不好理解的，所以重点说说第二个。

根据事件循环的策略，JavaScript会将同步任务和异步任务分别放入执行栈和任务队列中。

并且执行栈中的代码（同步任务），总是在读取”任务队列”（异步任务）之前执行。

setTimeout属于异步任务，所以 setTimeout 是在等循环执行完之后才执行的，所以最终的输出是 2 2。

我们要如何改变这个问题呢？我可以使用即时执行函数对它进行改造，让它在每次循环的时候就执行代码。

```
for (var i = 0; i < 2; i++) {
   (function(i) {
      setTimeout(() => console.log(i)); // 2 2
   })(i)
}
```
### z-index 失效

> z-index 只对定位元素有效。

z-index失效的情况:

1、父标签 position属性为relative

2、问题标签无position属性

3、问题标签含有浮动(float)属性

4、问题标签的祖先标签的z-index值比较小

解决方法:

第一种: position:relative改为position:absolute；

第二种:添加position属性（如relative，absolute等）；

第三种:去除浮动。

第四种:提高父标签的z-index值
