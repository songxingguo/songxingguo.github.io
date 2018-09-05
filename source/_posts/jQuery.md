title: jQuery教程
author: songxingguo
tags:
  - jQuery
categories:
  - 读书笔记
  - ''
date: 2018-08-14 11:24:00
---
## jQuery 简介

> jQuery 库可以通过一行简单的标记被添加到网页中。

### 什么是 jQuery ？

jQuery是一个JavaScript函数库。

jQuery是一个轻量级的"写的少，做的多"的JavaScript库。

<!-- more -->

jQuery库包含以下功能：

  - HTML 元素选取
  - HTML 元素操作
  - CSS 操作
  - HTML 事件函数
  - JavaScript 特效和动画
  - HTML DOM 遍历和修改
  - AJAX
  - Utilities

提示： 除此之外，Jquery还提供了大量的插件。

### 为什么使用 jQuery ？

目前网络上有大量开源的 JS 框架, 但是 jQuery 是目前最流行的 JS 框架，而且提供了大量的扩展。

## jQuery 语法

> 通过 jQuery，您可以选取（查询，query） HTML 元素，并对它们执行"操作"（actions）。

### jQuery 语法

jQuery 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。

基础语法： $(selector).action()

- 美元符号定义 jQuery
- 选择符（selector）"查询"和"查找" HTML 元素
- jQuery 的 action() 执行对元素的操作

实例:

- $(this).hide() - 隐藏当前元素

- $("p").hide() - 隐藏所有 `<p>` 元素

- $("p.test").hide() - 隐藏所有 class="test" 的 `<p>` 元素

$("#test").hide() - 隐藏所有 id="test" 的元素

### 文档就绪事件

您也许已经注意到在我们的实例中的所有 jQuery 函数位于一个 document ready 函数中：

```js
$(document).ready(function(){
 
   // 开始写 jQuery 代码...
 
});
```
这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码，即在 DOM 加载完成后才可以对 DOM 进行操作。

如果在文档没有完全加载之前就运行函数，操作可能失败。下面是两个具体的例子：

- 试图隐藏一个不存在的元素
- 获得未完全加载的图像的大小

提示：**简洁写法**（与以上写法效果相同）:

```js
$(function(){
 
   // 开始写 jQuery 代码...
 
});
```
以上两种方式你可以选择你喜欢的方式实现文档就绪后执行 jQuery 方法。

## jQuery 选择器

> jQuery 选择器允许您对 HTML 元素组或单个元素进行操作。

jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 CSS 选择器，除此之外，它还有一些自定义的选择器。

jQuery 中所有选择器都以美元符号开头：$()。

### 元素选择器

> jQuery 元素选择器基于元素名选取元素。

在页面中选取所有 `<p>` 元素:

```js
$("p")
```
用户点击按钮后，所有 `<p>` 元素都隐藏：

```js
$(document).ready(function(){
  $("button").click(function(){
    $("p").hide();
  });
});
```
### #id 选择器

> jQuery #id 选择器通过 HTML 元素的 id 属性选取指定的元素。

页面中元素的 id 应该是唯一的，所以您要在页面中选取唯一的元素需要通过 #id 选择器。

通过 id 选取元素语法如下：

```js
$("#test")
```
当用户点击按钮后，有 id="test" 属性的元素将被隐藏：

```js
$(document).ready(function(){
  $("button").click(function(){
    $("#test").hide();
  });
});
```

### .class 选择器

> jQuery 类选择器可以通过指定的 class 查找元素。

语法如下：

```js
$(".test")
```

用户点击按钮后所有带有 class="test" 属性的元素都隐藏：

```js
$(document).ready(function(){
  $("button").click(function(){
    $(".test").hide();
  });
});
```
### 更多实例

![更多实例](http://p9myzkds7.bkt.clouddn.com/jquery/%E6%9B%B4%E5%A4%9A%E5%AE%9E%E4%BE%8B.png)

## jQuery 事件

### 什么是事件？

> 页面对不同访问者的响应叫做事件。

事件处理程序指的是当 HTML 中发生某些事件时所调用的方法。

实例：

- 在元素上移动鼠标。
- 选取单选按钮
- 点击元素

在事件中经常使用术语"触发"（或"激发"）例如： "当您按下按键时触发 keypress 事件"。

常见 DOM 事件：

![常见 DOM 事件](http://p9myzkds7.bkt.clouddn.com/jQuery/%E5%B8%B8%E8%A7%81%E4%BA%8B%E4%BB%B6.png)

### jQuery 事件方法语法

在 jQuery 中，大多数 DOM 事件都有一个等效的 jQuery 方法。

页面中指定一个点击事件：

```js
$("p").click();
```
下一步是定义什么时间触发事件。您可以通过一个事件函数实现：

```js
$("p").click(function(){
    // 动作触发后执行常用的 jQuery 事件方法的代码!!
});
```
### 常用的 jQuery 事件方法

- #### $(document).ready()

  $(document).ready() 方法允许我们在文档完全加载完后执行函数。

- #### click()

  click() 方法是当按钮点击事件被触发时会调用一个函数。

  该函数在用户点击 HTML 元素时执行。

  在下面的实例中，当点击事件在某个 `<p> `元素上触发时，隐藏当前的 <p> 元素：

  ```js
  $("p").click(function(){
    $(this).hide();
  });
  ```
-  #### dblclick()

  当双击元素时，会发生 dblclick 事件。

  dblclick() 方法触发 dblclick 事件，或规定当发生 dblclick 事件时运行的函数：

  ```js
  $("p").dblclick(function(){
    $(this).hide();
  });
  ```
- #### mouseenter()

  当鼠标指针穿过元素时，会发生 mouseenter 事件。

  mouseenter() 方法触发 mouseenter 事件，或规定当发生 mouseenter 事件时运行的函数：

  ```js
  $("#p1").mouseenter(function(){
      alert('您的鼠标移到了 id="p1" 的元素上!');
  });
  ```
- #### mouseleave()

  当鼠标指针离开元素时，会发生 mouseleave 事件。

  mouseleave() 方法触发 mouseleave 事件，或规定当发生 mouseleave 事件时运行的函数：

  ```js
  $("#p1").mouseleave(function(){
      alert("再见，您的鼠标离开了该段落。");
  });
  ```
- #### mousedown()

  当鼠标指针移动到元素上方，并按下鼠标按键时，会发生 mousedown 事件。

  mousedown() 方法触发 mousedown 事件，或规定当发生 mousedown 事件时运行的函数：

  ```js
  $("#p1").mousedown(function(){
      alert("鼠标在该段落上按下！");
  });
  ```
- #### mouseup()

  当在元素上松开鼠标按钮时，会发生 mouseup 事件。

  mouseup() 方法触发 mouseup 事件，或规定当发生 mouseup 事件时运行的函数：

  ```js
  $("#p1").mouseup(function(){
      alert("鼠标在段落上松开。");
  });
  ```
- #### hover()

  hover()方法用于模拟光标悬停事件。

  当鼠标移动到元素上时，会触发指定的第一个函数(mouseenter);当鼠标移出这个元素时，会触发指定的第二个函数(mouseleave)。

  ```js
  $("#p1").hover(
      function(){
          alert("你进入了 p1!");
      },
      function(){
          alert("拜拜! 现在你离开了 p1!");
      }
  );
  ```
- #### focus()

  当元素获得焦点时，发生 focus 事件。

  当通过鼠标点击选中元素或通过 tab 键定位到元素时，该元素就会获得焦点。

  focus() 方法触发 focus 事件，或规定当发生 focus 事件时运行的函数：

  ```js
  $("input").focus(function(){
    $(this).css("background-color","#cccccc");
  });
  ```
- #### blur()

  当元素失去焦点时，发生 blur 事件。

  blur() 方法触发 blur 事件，或规定当发生 blur 事件时运行的函数：

  ```js
  $("input").blur(function(){
    $(this).css("background-color","#ffffff");
  });
  ```
  
## jQuery 效果

> 隐藏、显示、切换，滑动，淡入淡出，以及动画，哇哦！

### 隐藏和显示

- #### jQuery hide() 和 show()

  通过 jQuery，您可以使用 hide() 和 show() 方法来隐藏和显示 HTML 元素：

  ```js
  $("#hide").click(function(){
    $("p").hide();
  });
  ```
  ```js
  $("#show").click(function(){
    $("p").show();
  });
  ```

  语法:

  ```js
  $(selector).hide(speed,callback);
  ```
  ```js
  $(selector).show(speed,callback);
  ```
  可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是隐藏或显示完成后所执行的函数名称。

  下面的例子演示了带有 speed 参数的 hide() 方法：

  ```js
  $("button").click(function(){
    $("p").hide(1000);
  });
  ```
- #### jQuery toggle()

  通过 jQuery，您可以使用 toggle() 方法来切换 hide() 和 show() 方法。

  显示被隐藏的元素，并隐藏已显示的元素：

  ```js
  $("button").click(function(){
    $("p").toggle();
  });
  ```
  语法:

  ```js
  $(selector).toggle(speed,callback);
  ```
  可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是隐藏或显示完成后所执行的函数名称。
  
### 淡入淡出

jQuery Fading 方法，通过 jQuery，您可以实现元素的淡入淡出效果。

jQuery 拥有下面四种 fade 方法：

```
fadeIn()
fadeOut()
fadeToggle()
fadeTo()
```
- #### jQuery fadeIn()

  jQuery fadeIn() 用于淡入已隐藏的元素。

  语法:

  ```js
  $(selector).fadeIn(speed,callback);
  ```
  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。.

  可选的 callback 参数是 fading 完成后所执行的函数名称。

  下面的例子演示了带有不同参数的 fadeIn() 方法：

  ```js
  $("button").click(function(){
    $("#div1").fadeIn();
    $("#div2").fadeIn("slow");
    $("#div3").fadeIn(3000);
  });
  ```
- #### jQuery fadeOut()

  jQuery fadeOut() 方法用于淡出可见元素。

  语法:

  ```js
  $(selector).fadeOut(speed,callback);
  ```

  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是 fading 完成后所执行的函数名称。

  下面的例子演示了带有不同参数的 fadeOut() 方法：

  ```js
  $("button").click(function(){
    $("#div1").fadeOut();
    $("#div2").fadeOut("slow");
    $("#div3").fadeOut(3000);
  });
  ```
- #### jQuery fadeToggle()

  jQuery fadeToggle() 方法可以在 fadeIn() 与 fadeOut() 方法之间进行切换。

  如果元素已淡出，则 fadeToggle() 会向元素添加淡入效果。

  如果元素已淡入，则 fadeToggle() 会向元素添加淡出效果。

  语法:

  ```js
  $(selector).fadeToggle(speed,callback);
  ````
  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是 fading 完成后所执行的函数名称。

  下面的例子演示了带有不同参数的 fadeToggle() 方法：

  ```js
  $("button").click(function(){
    $("#div1").fadeToggle();
    $("#div2").fadeToggle("slow");
    $("#div3").fadeToggle(3000);
  });
  ```
- #### jQuery fadeTo()

  jQuery fadeTo() 方法允许渐变为给定的不透明度（值介于 0 与 1 之间）。

  语法:

  ```js
  $(selector).fadeTo(speed,opacity,callback);
  ```
  必需的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  fadeTo() 方法中必需的 opacity 参数将淡入淡出效果设置为给定的不透明度（值介于 0 与 1 之间）。

  可选的 callback 参数是该函数完成后所执行的函数名称。

  下面的例子演示了带有不同参数的 fadeTo() 方法：

  ```js
  $("button").click(function(){
    $("#div1").fadeTo("slow",0.15);
    $("#div2").fadeTo("slow",0.4);
    $("#div3").fadeTo("slow",0.7);
  });
  ```

## 滑动

jQuery 滑动方法，通过 jQuery，您可以在元素上创建滑动效果。

jQuery 拥有以下滑动方法：

```
slideDown()
slideUp()
slideToggle()
```
- #### jQuery slideDown()

  jQuery slideDown() 方法用于向下滑动元素。

  语法:

  ```js
  $(selector).slideDown(speed,callback);
  ```
  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是滑动完成后所执行的函数名称。

  下面的例子演示了 slideDown() 方法：

  ```js
  $("#flip").click(function(){
    $("#panel").slideDown();
  });
  ```
- #### jQuery slideUp()

  jQuery slideUp() 方法用于向上滑动元素。

  语法:

  ```js
  $(selector).slideUp(speed,callback);
  ```
  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是滑动完成后所执行的函数名称。

  下面的例子演示了 slideUp() 方法：

  ```js
  $("#flip").click(function(){
    $("#panel").slideUp();
  });
  ```
- #### jQuery slideToggle()

  jQuery slideToggle() 方法可以在 slideDown() 与 slideUp() 方法之间进行切换。

  如果元素向下滑动，则 slideToggle() 可向上滑动它们。

  如果元素向上滑动，则 slideToggle() 可向下滑动它们。

  ```js
  $(selector).slideToggle(speed,callback);
  ```
  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是滑动完成后所执行的函数名称。

  下面的例子演示了 slideToggle() 方法：

  ```js
  $("#flip").click(function(){
    $("#panel").slideToggle();
  });
  ```
### 动画

- #### animate()

  jQuery animate() 方法用于创建自定义动画。

  语法：

  ```js
  $(selector).animate({params},speed,callback);
  ```
  必需的 params 参数定义形成动画的 CSS 属性。

  可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

  可选的 callback 参数是动画完成后所执行的函数名称。

  下面的例子演示 animate() 方法的简单应用。它把 <div> 元素往右边移动了 250 像素：

  ```js
  $("button").click(function(){
    $("div").animate({left:'250px'});
  });
  ```
  > 默认情况下，所有 HTML 元素都有一个静态位置，且无法移动。如需对位置进行操作，要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute！

- #### 操作多个属性

  请注意，生成动画的过程中可同时使用多个属性：

  ```js
  $("button").click(function(){
    $("div").animate({
      left:'250px',
      opacity:'0.5',
      height:'150px',
      width:'150px'
    });
  });
  ```
  > 如果需要生成颜色动画，您需要从 [jquery.com](http://jquery.com/download/) 下载 颜色动画 插件。

- #### 使用相对值

  也可以定义相对值（该值相对于元素的当前值）。需要在值的前面加上 += 或 -=：

  ```js
  $("button").click(function(){
    $("div").animate({
      left:'250px',
      height:'+=150px',
      width:'+=150px'
    });
  });
  ```
- #### 使用预定义的值

  您甚至可以把属性的动画值设置为 "show"、"hide" 或 "toggle"：

  ```js
  $("button").click(function(){
    $("div").animate({
      height:'toggle'
    });
  });
  ```
- #### 使用队列功能

  默认地，jQuery 提供针对动画的队列功能。

  这意味着如果您在彼此之后编写多个 animate() 调用，jQuery 会创建包含这些方法调用的"内部"队列。然后逐一运行这些 animate 调用。

  ```js
  $("button").click(function(){
    var div=$("div");
    div.animate({height:'300px',opacity:'0.4'},"slow");
    div.animate({width:'300px',opacity:'0.8'},"slow");
    div.animate({height:'100px',opacity:'0.4'},"slow");
    div.animate({width:'100px',opacity:'0.8'},"slow");
  });
  ```
  下面的例子把 `<div>` 元素往右边移动了 100 像素，然后增加文本的字号：

  ```js
  $("button").click(function(){
    var div=$("div");
    div.animate({left:'100px'},"slow");
    div.animate({fontSize:'3em'},"slow");
  });
  ```
### 停止动画

- #### jQuery stop()

  jQuery stop() 方法用于停止动画或效果，在它们完成之前。

  stop() 方法适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。

  语法:

  ```js
  $(selector).stop(stopAll,goToEnd);
  ```
  可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。

  可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。

  因此，默认地，stop() 会清除在被选元素上指定的当前动画。

  下面的例子演示 stop() 方法，不带参数：

  ```js
  $("#stop").click(function(){
    $("#panel").stop();
  });
  ```
  
### Callback 方法

  - #### jQuery 动画的问题

  许多 jQuery 函数涉及动画。这些函数也许会将 speed 或 duration 作为可选参数。

  ```js
  $("p").hide("slow")
  ```
  speed 或 duration 参数可以设置许多不同的值，比如 "slow", "fast", "normal" 或毫秒。

  以下实例在隐藏效果完全实现后回调函数:

  ```js
  // 使用 callback 实例
  $("button").click(function(){
    $("p").hide("slow",function(){
      alert("段落现在被隐藏了");
    });
  });
  ```
  以下实例没有回调函数，警告框会在隐藏效果完成前弹出：

  ```js
  // 没有 callback(回调)
  $("button").click(function(){
    $("p").hide(1000);
    alert("段落现在被隐藏了");
  });
  ```
### 链(Chaining)


通过 jQuery，可以把动作/方法链接在一起。

Chaining 允许我们在一条语句中运行多个 jQuery 方法（在相同的元素上）。

- #### jQuery 方法链接

  直到现在，我们都是一次写一条 jQuery 语句（一条接着另一条）。

  不过，有一种名为链接（chaining）的技术，允许我们在相同的元素上运行多条 jQuery 命令，一条接着另一条。

  提示： 这样的话，浏览器就不必多次查找相同的元素。

  如需链接一个动作，您只需简单地把该动作追加到之前的动作上。

  下面的例子把 css()、slideUp() 和 slideDown() 链接在一起。"p1" 元素首先会变为红色，然后向上滑动，再然后向下滑动：

  ```js
  $("#p1").css("color","red").slideUp(2000).slideDown(2000);
  ```
  如果需要，我们也可以添加多个方法调用。

  提示：当进行链接时，代码行会变得很长。不过，jQuery 语法不是很严格；您可以按照希望的格式来写，包含换行和缩进。

  如下书写也可以很好地运行：

  ```js
  $("#p1").css("color","red")
    .slideUp(2000)
    .slideDown(2000);
  ```
  jQuery 会抛掉多余的空格，并当成一行长代码来执行上面的代码行。
