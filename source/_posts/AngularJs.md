title: 《AngularJs 入门与进阶》-读书笔记
author: songxingguo
tags:
  - AngularJs
categories:
  - 读书笔记
date: 2018-10-02 16:04:00
---
# AngularJs入门与进阶


## 前言

AngularJS是Google公司开发的一款Web前端框架。AngularJS提供了一些优秀的特性，例如双向数据绑定、MVC架构模式、指令等，能够在很大程度上降低Web前端开发的难度，因此深受广大Web前端开发人员的喜爱。

AngularJS框架功能虽然强大，但是对于初学者来说入门比较困难，主要是因为AngularJS有别于传统的Web前端框架，指令、路由、服务等概念都是其他前端框架所不具备的。

<!-- more -->


## 走进AngularJS世界

### AngularJS简介

AngularJS是Google工程师研发的一款开放源代码JavaScript框架，官方文档中的描述是：完全使用JavaScript编写客户端的技术，同其他历史悠久的Web前端技术（例如HTML、CSS等）配合使用，使得Web开发变得更简单、更高效。它是一款比较有特色的框架，和其他知名度较高的前端框架（例如jQuery、Dojo等）有些不同，AngularJS以HTML作为模板语言并扩展HTML元素及属性，使得应用组件开发保持高度清晰、一致。

> HTML标签中尖括号的英文为angular brackets，这便是AngularJS名称的来源。AngularJS源码托管在Github上，源码地址为 https://github.com/angular/angular.js/ 。

### 搭建AngularJS开发环境

#### 选择集成开发工具

1. ##### WebStorm

    WebStorm是JetBrains公司旗下一款Web前端开发工具，也是目前最受欢迎的前端开发工具之一，不论哪种IDE，它们的主要作用无非是代码补全提示、代码重构、代码定位、代码调试。与其他工具相比，WebStorm的主要优势如下：
    - 智能的代码补全，支持JavaScript及HTML补全。代码补全包含了所有流行的JavaScript库，例如jQuery、YUI、Dojo、Prototype、Mootools、AngularJS等。
    
    - 代码重构，支持重命名、提取变量／函数、内联变量／函数、移动／复制、安全删除、包裹或者去掉外围代码等。
    
    - 代码定位与错误修复，只需按住Ctrl键单击方法或者变量名，即可跳转到定义处，可以全项目查找方法或者变量并高亮显示，能够快速找到代码中的错误或者需要优化的地方，并给出修改意见。

    WebStorm官方网站下载地址为 http://www.jetbrains.com/webstorm/ 。

2. ##### Sublime Text

    Sublime Text是一款跨平台的代码编辑器，提供了目前主流操作系统（例如Windows、UNIX/Linux、OS X等）的Release版本，支持多种编程语言的语法高亮，拥有优秀的代码自动补全功能，界面简约美观（软件界面如图1.2所示），深受开发者喜爱。

    Sublime Text的强大之处在于插件扩展功能。**它拥有大量的第三方扩展插件，用户可以根据个人喜好通过插件对编辑器功能进行定制**。例如，安装了AngularJS代码提示插件后，可以对AngularJS应用中的关键字、内置指令、服务等进行代码补全。目前最新版本为Sublime Text 3，也是一款收费软件，但是可以无期限试用，且试用版无功能限制。

    Sublime Text 3的官方下载地址为http://www.sublimetext.com/3 。

3. ##### Brackets

    Brackets是大名鼎鼎的Adobe公司发行的一款免费、开源且跨平台的Web前端开发工具，支持Windows、Linux以及OS X等主流平台。

    Brackets的特点是简约、优雅、快捷！它没有很多的视图或者面板，也没有太多花哨的功能，核心目标是减少在开发过程中那些效率低下的重复性工作，具有自动刷新浏览器、修改元素的样式后实时预览、强大的代码搜索功能等特性。和Sublime Text不同的是，Sublime Text可支持多种语言，而Brackets专为Web前端开发而生。

    开源产品一直比较受大众青睐，因此本书将采用Brackets作为代码编辑器，对案例进行演示。读者可以从Brackets官方网站下载Brackets的安装包，官方网站地址为 http://brackets.io/ 。
    
     Brackets除了具有上述优点外，更加让人兴奋的是插件扩展功能。Brackets本身并不支持AngularJS代码补全功能，但是可以通过安装插件的方式进行弥补。
    
#### 下载与安装AngularJS

AngularJS的安装过程非常简单，通过`<script>`标签把AngularJS库文件引入当前页面中即可。可以通过两种方式来完成：一种是 **通过Google的CDN（内容分发网络）引入** ，但基于众所周知的原因，可能会受到网络限制；另一种是 **下载AngularJS的Release版本，然后引入页面中** 。读者可以从AngularJS官方提供的下载地址中获取所有版本的库文件。

> AngularJS官方网站下载地址为https://code.angularjs.org/。

#### 第一个AngularJS应用

打开Brackets，新建一个HTML文件，接着我们编写第一个基于AngularJS框架的程序，内容如下：

```html
<!doctype html>
<html ng-app>
  <head>
    <meta charset="UTF-8">
    <title>ch01_03</title>
    <script type="text/javascript" src="../angular-1.5.5/angular.js">
    </script>
  </head>
  <body>
    <div>{{"Hello World!"}}</div>
    <div>{{"Hello AngularJS!"}}</div>
  </body>
</html>
```
#### AngularJS应用剖析

##### 第一个AngularJS应用解惑

在1.3节中，我们一起完成了第一个AngularJS应用，并看到了运行效果，从未接触过AngularJS的读者可能会产生以下几个疑问：

- `<html>`标签中的ng-app属性是什么？
- 嵌套的两个大括号又是什么？
- 为什么页面显示的内容变成了“Hello World!”和“Hello AngularJS!”？

ng-app是AngularJS的一个内置指令，可以出现在任意位置，并有两个作用，一个是 **启动AngularJS框架** ，另一个是 **告诉AngularJS框架从ng-app指令所在标签的开始标签到结束标签之间的所有DOM元素由AnguarJS框架进行管理** 。

嵌套的两个大括号是AngularJS的表达式形式，由两个嵌套的大括号构成，大括号中间为表达式内容，AngularJS会对表达式内容进行解析，然后将表达式结果输出到浏览器。在上面的例子中，表达式内容为字符串字面量，AngularJS会将该字面量输出到页面。

##### AngularJS应用构成元素

- 模型（Model）：AngularJS程序中用于展示到页面的数据，本质是一个JavaScript对象。
- 视图（VIew）：从用户的角度来看，视图就是用户所看到的网页内容；从AngularJS应用的角度来说，视图则是AngularJS指令与表达式经过解析后的DOM元素。
- 控制器（Controller）：AngularJS应用中用于处理业务逻辑的JavaScript方法。
- 作用域（Scope）：可以把作用域理解为一个容器，在控制器中可以访问这个容器，然后往容器中放入一些模型数据，在视图中就可以通过表达式将数据展现给用户。
- 指令（Directives）：扩展的HTML属性或标签，能够被AngularJS框架识别，根据不同的指令执行相应的动作。例如，前面提到的ng-app指令，作为html元素的扩展属性，能够被AngularJS框架识别，从而启动AngularJS框架。
- 表达式（Expressions）：用于向页面输出信息，在前面已经接触过，下节将会对表达式做更详细的介绍。
- 模板（Template）：AngularJS以HTML作为模板语言，AngularJS模板实际上就是HTML片段。

#### AngularJS表达式

- 表达式的定义方式

  ```
  {{expression}}
  ```
  AngularJS框架遇到嵌套的两层大括号时会把嵌套大括号中的内容作为表达式处理。

- 表达式中的四则运算

  AngularJS表达式支持加减乘除四则运算及字符串拼接运算。

- 表达式中的逻辑运算

	AngularJS表达式除了支持算术运算外，还支持逻辑运算。
  
> 我们又接触了一个新指令ng-init。该指令用于初始化作用域，在上面的代码中，我们通过ng-init指令向作用域中增加一个person对象和arr数组，然后通过表达式输出person对象的name属性和arr数组的第一个元素。

## 双向数据绑定

数据绑定是AngularJS框架最优秀的特性之一，能够帮助Web前端开发人员在很大程度上 **减少对DOM的操作**。

### AngularJS双向数据绑定

**数据绑定** 是 **AngularJS框架在视图（DOM元素）与作用域之间建立的数据同步机制** 。所谓“双向”，是指 **界面的操作能够实时同步到作用域中** ，**作用域中的数据修改也能够实时回显到界面中** 。

作用域可以被视为一个容器，里面有一些基于key-value的数据。当用户输入内容发生变化时，AngularJS框架就把表单内容同步到作用域中对应的变量中，而当我们改变作用域中的变量值时，AngularJS又会把修改后的变量值同步到表单中，这就是AngularJS的双向数据绑定。

![AngularJS双向数据绑定图解](https://graphbed.qiniu.songxingguo.com/AngularJS/%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A.jpg)

### ng-model指令

为了建立数据绑定，我们需要用到AngularJS另外一个内置指令ng-model，该指令 **只能用在表单元素上** ，使用方法如下：

```html
<input type="text" name="uname"  ng-model="uname" />
```
在input输入框上添加ng-model指令后，**AngularJS框架就会在对应的作用域中创建一个uname属性和该输入框进行绑定**。

如果使用原生的JavaScript，可以通过document对象的getElementById()方法获取输入框对象，响应输入框的keyup事件，在输入框的keyup事件响应方法中获取输入框内容，然后同样以操作DOM的方式把输入框内容显示到页面中，具体代码如下：

```js
var uname = document.getElementById（"uname"）;
var info = document.getElementById（"info"）;
uname.onkeyup = function(){
  info.innerHTML = uname.value;
}
```
我们再使用AngularJS数据绑定机制来完成这个案例，具体代码如下：

```html
<div>用户名：<input type="text" ng-model="uname" /></div>
<div>{{uname}}</div>
```
面案例中的ng-app指令用于启动AngularJS应用。需要注意的一点是，当AngularJS遇到ng-app指令时就会创建一个名为 **$rootSocpe的作用域** ，该作用域为AngularJS应用的根作用域。

作用域其实是一个 **简单的JavaScript对象** ，形式如下：

```
var $rootScope={uname:”江荣波"};
```
我们把ng-model指令作为属性添加到input标签中，此时AngularJS会在$rootSocpe作用域中添加一个uname属性和input输入框绑定，当我们在input表单中输入内容时，AngularJS会自动把输入的内容同步到作用域的uname属性中，因此我们不需要自己响应input标签的keyup事件去处理。

`\{\{uname\}\}`为AngularJS的表达式形式，可以访问AngularJS作用域里的属性，这里我们使用表达式把uname属性输出到页面中。同时AngularJS还能够监视作用域中数据的变化，当数据发生变化时，页面显示内容也能够得到实时更新。

> 在上面的例子中，我们向$rootScope作用域中添加属性进行了数据绑定，会造成全局作用域污染，实际项目中并不会这么做，而是把表单和控制器作用域中的属性进行数据绑定。控制器作用域是根作用域$rootScope的子作用域，后面会接触到。

### ng-bind指令

ng-bind指令是和数据绑定相关的另一个指令，其作用是实现作用域到视图的单向数据绑定，和表达式功能类似，可用于向界面中输出作用域中的数据。

```html
<div>{{uname}}</div>
可以替换为：
<div ng-bind="uname"></div>
或者
<div class="ng-bind:uname"></div>
```
不同的是，在使用AngularJS表达式{{name}}时，如果遇到网络问题，就会导致AngularJS加载缓慢，我们会看到浏览器直接把AngularJS表达式当作字符串渲染到页面中，这样用户体验将不是很好，所以推荐使用ng-bind指令。ng-bind指令在AngularJS没有加载完毕的时候是不会解析执行的，只有AngularJS加载完毕才会执行。

## AngularJS与MVC

### MVC模式简介

如果读者接触过Web服务端编程，就不会对MVC模式陌生。MVC是一种软件架构模式，独立于任何一门语言，于1970年起源于Smalltalk语言，随后随着桌面应用的普及广泛用于桌面应用开发中，发展到今天可谓无处不在。在Java EE领域，基于MVC的框架就有很多，例如较为优秀的Struts、WebWork、Spring MVC等。

MVC是Model（模型）、View（视图）、Controller（控制器）的首字母缩写，MVC的核心思想是把数据的管理、业务逻辑控制和数据的展示分离开，使程序的逻辑性和可维护性更强。它们之间的关系可以用图3.1表示。

![模型、视图、控制器关系图](https://graphbed.qiniu.songxingguo.com/AngularJS/MVC%E5%85%B3%E7%B3%BB%E5%9B%BE.jpg)

- View（视图）为用户可操作的软件界面，用户通过视图和程序进行交互，在视图中会触发不同的事件，例如单击按钮、输入文字等，不同的事件能够触发控制器执行相应的业务逻辑处理。

- Controller（控制器）主要用于响应用户请求，在控制器中可操作模型数据，进行业务逻辑处理，根据处理结果分发到不同的视图。

- Model（模型）为程序中的模型数据，是控制器与视图之间传递信息的载体。

### AngularJS中的MVC

MVC是一种软件设计思想，并不是一门具体的技术，AngularJS框架中也引入了这种思想，下面就来看一下AngularJS中的MVC分别指的是什么。
- Model（模型）：前面章节中提到过的 **作用域对象**（例如$rootScope对象）中的属性。

- View（视图）：大家所熟悉的 **DOM元素** ，从用户的角度来看就是 **HTML页面** ，在View中可以通过AngularJS表达式访问模型数据。

- Controller（控制器）：**用户自定义的构造方法** ，作用域中的模型数据可以通过依赖注入的方式注入控制器中。

> 目前普遍认为AngularJS是一款MVW（Model-View-Whatever）框架，这是因为早期的AngularJS框架比较接近于MVC，而随着新版本代码的重构与API的改进，$scope对象可以认为是由一个方法（Controller）包装后的ViewModel，看上去更接近于MVVM（Model-View-ViewModel）框架，那么到底是MVC还是MVVM呢？不管怎样都行（Whatever），这就是MVW框架的由来。

### AngularJS控制器的定义

AngularJS控制器是一个构造方法。有些JavaScript编程经验的朋友都知道，JavaScript方法可以作为对象模板实例化对象，当方法作为对象模板时，方法本身即为对象的构造方法。所以我们可以像定义一个普通方法一样定义一个控制器，例如：

```js
script type="text/javascript">
function LoginController （$scope,$log） {
$scope.name="admin";
$scope.pword="123456";
}
</script>
```
除此之外，我们还可以使用模块实例的controller()方法来声明一个控制器。该方法可接收两个参数，第一个参数为 **控制器名称** ，第二个参数为 **一个匿名方法** ， **即控制器的构造方法**v，具体使用方法如下：

```js
script type="text/javascript">
var app =  angular.module（"app",[]）;
app.controller（"LoginController",function（$scope,$log）{
$scope.name="admin";
$scope.pword="123456";
}）;
</script>
```
如上面的代码所示，AngularJS框架在window对象中增加了一个全局的angular对象，我们可以调用angular对象的module()方法返回一个模块实例，然后调用模块实例的controller()方法来声明一个控制器。

在上面的案例代码中，我们定义控制器时指定了两个参数，即$scope和$log：$scope是作用域对象，是控制器与视图之间传递信息的载体；$log为AngularJS框架内置的日志服务对象，用于向控制台中输入日志信息。当我们为控制器构造方法指定这两个参数后，表示控制器依赖于这两个对象，控制器实例化时会把这两个对象注入控制器中。

> AngularJS 1.3版本之后已不再支持全局控制器，第一种控制器定义方式只适用于AngularJS 1.3之前的版本。

### 控制器对象的实例化

控制器对象的实例化非常简单，需要用到AngularJS内置的ng-controller指令。ng-controller的使用方法和ng-app指令类似，也是作为标签的扩展属性使用，具体如下：

```html
<div ng-controller="LoginController">
</div>
```
AngularJS框架遇到ng-controller指令时会根据ng-controller指令指定的控制器名称 **查找控制器构造方法** ，然后 **使用对应的构造方法实例化控制器对象** ，并 **将控制器依赖的对象注入控制器对象中** 。

除此之外，ng-controller指令还支持使用as语法指定控制器对象名称。

```js
<body ng-controller="CalcController as calc">
</body>
```
这样做的好处是，可以通过控制器对象名称访问控制器对象的成员属性及成员方法。

### 控制器的作用域范围

前面我们了解到使用 **ng-controller指令实例化控制器时会产生一个新的作用域对象** ，而且 **同一个页面中可以使用多个ng-controller指令来实例化不同的控制器对象** 。但是需要注意的是，**每个控制器对应的作用域对象只能与ng-controller指令所在标签的开始标签与结束标签之间的DOM元素建立数据绑定** 。

### 控制器中处理DOM事件

AngularJS应用中的DOM事件处理可以在控制器中完成。AngularJS框架为我们提供了一系列的事件绑定指令，这些指令是在原生的JavaScript事件名称前增加“ng-”前缀，例如ng-click、ng-keyup等。

## 应用模块化

### 应用模块划分的重要性

在软件设计过程中，为了能够对系统开发流程进行管理、保证系统的稳定性以及后期的可维护性，可以按照一定的准则对软件开发进行模块的划分。根据模块来进行系统开发，可提高系统的开发进度、明确系统的需求、保证系统的稳定性。

在系统设计的过程中，每个系统实现的功能有所不同，所以每个系统的需求不同，系统的设计方案也不同。在系统的开发过程中，有些需求在属性上往往会有一定的关联性，而有些需求之间的联系却很少。如果在设计的时候不对需求进行归类划分，那么在后期往往会造成混乱。

在软件设计过程中对软件进行模块划分可以拥有以下好处：

- 使程序实现的逻辑更加清晰，可读性强。
- 使多人合作开发的分工更加明确，容易控制。
- 能充分利用可以重用的代码。
- 抽象出可公用的模块，可维护性强，以避免同一处修改在多个地方出现。
- 系统运行可方便地选择不同的流程。
- 可基于模块化设计优秀的遗留系统，方便组装开发新的相似系统，甚至一个全新的系统。

### AngularJS中的模块

在前面的几章中，由于代码比较少，为了方便说明问题直接把控制器代码写在了HTML页面中，这样会使代码的可维护性差，实际项目中并不会这么做。通常的做法是把处理业务逻辑的代码写在单独的JS文件中，然后在HTML页面中引入该文件。

然而这样会带来新问题：在AngularJS 1.3版本之前是支持全局控制器的，如果我们的控制器全都定义在全局命名空间中，假设有一个公共的JS文件在多个页面同时引用，并且同一个项目由多人协作完成，不同开发人员在同一个文件中新增控制器或者服务的定义就容易造成命名冲突。AngularJS应用的模块划分就能够很好地解决这个问题。

#### AngularJS模块的定义

前面章节中提到过，AngularJS框架在window对象下新增了一个全局的angular对象，我们可以调用angular对象的module()方法返回一个模块实例，具体使用方法如下：

```JS
//定义一个无依赖模块
angular.module（'appModule',[]）;
//定义一个依赖module1、module2的模块
angular.module（'appModule',['module1','module2']）;
```
angular.module()方法能够接收3个参数。第一个参数为模块的名称。第二个参数是一个数组，用于指定该模块依赖的模块名称。如果我们的模块不需要依赖其他模块，第二个参数传递一个空数组即可。第三个参数为可选参数，该参数接收一个方法，用于对模块进行配置，作用和模块实例的config()方法相同。

angular.module()方法返回一个模块实例对象，我们可以调用该对象的controller()、directive()、filter()等方法向模块中添加控制器、指令、过滤器等其他组件。

模块定义完毕后，如何在HTML页面中引用呢？其实很简单，在使用ng-app指令时指定模块名即可，例如：

```html
<html ng-app="appModule">
```

上面代码中的appModule为调用angular.module()方法时指定的模块名称。

#### 使用模块解决命名冲突问题

调用angular.module()方法创建了两个模块实例，名称分别为loginModule和registerModule，然后分别调用这两个模块实例的controller()方法创建两个名称均为UserController的控制器，这两个控制器属于不同的模块，在login.html页面中使用ng-app="loginModule"指定页面使用loginModule模块中的内容，而在register.html页面中使用ng-app="registerModule"指定使用registerModule模块中的内容。


### 模块化最佳实践

在实际项目中，随着项目的进展代码会越来越多，我们需要以一种更加合理的方式组织这些代码。假设我们的项目名称为app，下面有两个模块，分别为login和register，参考AngularJS官方的建议，可以按照如下目录结构组织项目，对4.2.2小节案例进行重构。

![目录结构](https://graphbed.qiniu.songxingguo.com/AngularJS/%E4%BB%A3%E7%A0%81%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.jpg)

## 作用域与事件

### AngularJS作用域详解

从本书的第一个案例开始，我们就已经接触到了AngularJS的作用域，并且前面的每一个案例都和作用域息息相关，本章就对AngularJS作用域进行更深入的介绍。

众所周知，JavaScript对象是基于key-value键值对的，我们可以把JavaScript对象作为一个map数据结构使用，而AngularJS作用域本质上就是一个普通的JavaScript对象，形式如下：

```js
var obj = {
  name:'Jane',
  age:30,
  walk:function(){
    console.info（"walk"）;
  }
}
```
AngularJS作用域对象和普通JavaScript对象一样，都可以在作用域对象中增加属性或者方法，不同的是我们不需要手动去构造作用域对象，当HTML页面中出现ng-app和ng-controller指令时，**AngularJS框架会自动创建作用域对象** ，我们只需将其注入即可。

AngularJS程序中作用域的主要功能是存放模型数据，在控制器中，我们可以修改作用域中的模型数据或在作用域中新增模型数据（例如：$scope.address = 'China'），然后在视图中通过AngularJS表达式展示模型数据（例如：{{address}}）。

每个AngularJS应用至少会有一个名称为$rootScope的作用域，它是AngularJS应用的根作用域。前面我们接触过ng-app指令，它可以出现在任何HTML标签中，用于启动AngularJS框架。当AngularJS启动时会自动创建一个根作用域对象$rootScope，接着当我们使用ng-controller指令实例化控制器对象时，AngularJS框架会为我们创建一个子作用域$scope，默认情况下，该子作用域会继承$rootScope作用域的所有属性。

控制器作用域对象确实继承了$rootScope作用域的属性。事实上$rootScope是所有作用域的父作用域（孤立作用域除外，后续会接触到）。

### AngularJS作用域继承

#### JavaScript对象继承机制

JavaScript语言遵循ECMAScript规范，在ECMAScript 6规范之前，JavaScript语言并没有类的概念，也不具备面向对象多态的特性，所以严格地讲 **JavaScript并不是一门面向对象语言，而是一门基于对象的语言** 。

JavaScript构造对象通常有两种方式。第一种方式是通过字面量创建，形式如下：

```js
var obj = {
  name:'jane',
  age:32
};
```
一种方式是通过对象的构造方法来创建，需要用到function关键字。JavaScript语言中的方法可以作为构造方法创建对象，比较容易让人产生疑惑。例如，我们使用下面这段代码定义一个构造方法：

```js
function Person（name,age） {
  this.name = name;
  this.age = age;
  this.eat = function() {
    console.log（'eat..'）;
  }
}
```
是我们把它作为普通方法调用就不会有问题。当使用构造方法创建对象时需要用到JavaScript的new关键字，使用方法如下：

```js
var person = new Person（'Jane',32）;
```
在实际项目中，当我们明确地使用function关键字定义一个构造方法时，**构造方法** 名称通常采用 **帕斯卡命名法** ，即 **每个单词首字母大写**（例如：LoginController）；而使用function定义一个 **普通方法** 时，方法名称可采用 **驼峰命名法** ，驼峰命名法跟帕斯卡命名法相似，**只是首字母为小写**（例如：checkUser），看上去像驼峰，因此而得名。

JavaScript语言为我们提供了几个内置的构造方法，例如Object、Array、String、Boolean等。我们可以直接使用这些构造方法创建对象。

了解了JavaScript对象的创建方法后，我们再来看看 **JavaScript的对象继承机制** 。JavaScript语言有以下3种方式实现对象继承。

1. ##### 构造方法原型链继承

   每个JavaScript构造方法都有一个名称为prototype的属性，可以指向另一个对象。当我们访问对象属性时（例如obj.name），JavaScript引擎会从对象的所有属性中查找该属性，如果找到就返回属性值，如果没有找到就继续从prototype属性指向的对象属性中查找，如果仍然没有找到，则会沿着prototype链一直查找下去，直到prototype链结束或找到对象为止。接下来我们看一个原型链继承的案例，代码如下：
   
       ```js
        function Animal(){
          this.eat = function(){
            console.log（'eat...'）;
          }
        }
        function Cat（age）{
          this.age = age;
        }
        Cat.prototype = new Animal();
        var cat = new Cat（10）;
       ```
    如上面的代码所示，首先定义两个构造方法Animal和Cat，把Cat的prototype属性指向一个Animal对象：

    ```js
    Cat.prototype = new Animal();
    ```
    接下来通过new关键字创建一个Cat对象：

    ```js
    var cat = new Cat（10）;
    ```
    Cat构造方法中并没有定义eat()方法，而我们通过Cat实例调用eat()方法输出了内容，说明Cat对象通过prototype属性继承了Animal对象的eat()方法。

2. ##### 使用apply、call方法实现继承

    由于JavaScript构造方法的apply()、call()方法可以 **改变对象构造中“this”的上下文环境** ，使特定的对象实例具有对象构造中所定义的属性、方法，因此我们可以使用apply()、call()方法实现JavaScript对象的继承，例如下面的案例：

    ```js
    function Person（name,age）{
      this.name = name;
      this.age = age;
    }
    function Student（name,age,love）{
      //Person.apply（this,[name,age]）;
      Person.call（this,name,age）;
      this.love = love;
    }
    var student = new Student（'jane',23,'pingpong'）;
    ```
    如上面的代码所示，在本例中我们首先定义了Person构造方法，接着在Student构造方法中调用Person.call()方法实现了继承，然后通过new关键字创建一个Student对象，在控制台中输出Student对象的属性值。

    说明Student对象继承了Person对象的name和age属性。apply()方法和call()方法的不同之处在于apply()方法只接收两个参数，第二个参数是一个数组，而call()方法可以接收多个参数。

3. ##### 对象实例间继承

    在JavaScript语言中，对象可以继承另外一个对象的属性，例如下面的案例：

    ```js
    function Person（name,age）{
      this.name = name;
      this.age = age;
    }
    var person = new Person（'jane',28）;
    var student = Object.create（person）;
    student.love = "pingpong";
    console.log（Object.getPrototypeOf（student））;
    ```
    如上面的代码所示，在本例中我们用到了 **Object.create()方法** ，它的作用是 **以一个对象为原型创建另外一个对象，创建的对象和原对象具有相同的属性** ，我们可以通过Object. getPrototypeOf()方法获取新对象的原型。

#### AngularJS作用域对象原型继承

上一小节介绍了JavaScript语言中对象继承的3种方式，其中AngularJS作用域对象继承采用第一种方式，即构造方法原型链继承。AngularJS作用域构造方法中提供了一个$new()成员方法，用于创建子作用域。AngularJS框架创建子作用域的过程大致如下：

```js
var parent = $rootScope;
var child = parent.$new();
```
我们不妨了解一下$new()方法的定义，内容如下，有兴趣的读者可以参考AngularJS源码。

上面我们了解了AngularJS作用域继承机制，所有作用域对象都是$rootScope作用域的子作用域。还有一种情况需要我们考虑，即在AngularJS控制器中可以嵌套另外一个控制器，例如：

```html
<div ng-app>
  <div ng-controller="OuterController">
  <div ng-controller="InnerController">
</div>
</div>
```
在上面的代码片段中，ng-controller指令范围内嵌套了另外一个ng-controller指令，这种情况下AngularJS是如何处理作用域对象继承的呢？过程如下：

AngularJS框架遍历DOM元素，查找到ng-app指令时启动应用，创建$rootScope作用域。

然后AngularJS框架查找到第一个ng-controller指令，指向名称为OuterController的控制器，并调用$rootScope.$new()方法，以原型继承的方式创建$rootScope作用域的子作用域对象（记为$scope1）。当OuterController构造方法接收一个名称为$scope的参数时，AngularJS实例化控制器对象时会把$scope1对象注入控制器对象中。

接下来AngularJS继续遍历DOM元素，遇到第二个嵌套的ng-controller指令时调用$scope1. new()方法，以$scope1为原型创建子作用域（记为$scope2，$scope2作用域对象能够访问$scope1作用域对象的所有属性）。

了ng-app、ng-controller指令会创建作用域对象外，AngularJS指令也可能会产生子作用域，在后面的章节中我们会接触到。

### 作用域高级特性

前面介绍了AngularJS作用域的基本概念以及作用域原型继承机制，本节将介绍AngularJS作用域的一些高级特性，包括作用域属性监视、digest循环等。

#### $watch方法监视作用域

在使用AngulaJS框架编写应用时，我们经常需要做的一件事情就是对作用域中的属性进行监视，并对其发生的变化做出相应的处理。AngularJS为我们提供了一个非常方便的$watch()方法，可以帮助我们监视作用域中属性的变化。

在AngularJS内部，每当我们对ng-model绑定的name属性进行一次修改时，AngularJS内部的$digest循环就会运行一次，并在运行结束之后检查我们使用$watch()方法来监视的内容，如果和上一次进行$digest之前相比有了变化，就执行我们使用$watch()方法绑定的处理函数。

然而，我们在实际运用中常常不只是对一个原始类型的属性进行监视，如果读者还记得JavaScript语言中的6种数据类型，就一定会记得基本类型（数字、字符串）和引用类型的区别。对于基本类型，如果我们使用了一个赋值操作，这个基本类型变量就会“真正”被复制一次，然而对于引用类型，在进行赋值时，仅仅是将赋值的变量指向了这个引用类型。在AngularJS作用域对象的$watch()方法中，对基本类型和引用类型的操作有所不同。


$watch()方法在对待基本类型和引用类型时会有不同的处理方式，这需要首先介绍一下$watch()方法的第三个参数。在前面的例子中，我们知道，$watch()方法可以接收两个参数，第一个参数是需要监视的属性，第二个参数是在监视属性发生变化时需要回调的方法，实际上$watch()方法还能接收第三个参数，默认情况下参数值为false。

在默认情况下，即不显式指明第三个参数或者将其指明为false时，我们进行的监视叫作“**引用监视** ”（reference watch），也就是说只要监视的对象引用没有发生变化，就不算它发生变化。

具体来说，在上面的例子中，只要是items数组的引用没有发生变化，就算items数组中的一些元素属性发生了变化，$watch()方法也不会执行回调方法。那么在什么时候算是引用发生了变化呢？比如说将一个新的数组newItems赋值给items，此时$watch才会认为监视的属性发生了变化，进而调用回调方法。

相反，将$watch()方法的第三个变量设置为true，此时进行的监视就叫作“ **全等监视** ”（equality watch）。此时只要监视的属性发生变化，$watch就会执行相应的回调方法。

既然全等监视这么好，那么我们为什么不直接用全等监视呢？当然，任何事情都有好坏两个方面，全等监视固然好，但是在运行时需要先遍历整个监视对象，然后在每次$digest之前使用angular.copy()将整个对象深复制一遍，然后在运行之后用angular.equal()将前后的对象进行对比。上面例子中的items比较简单，因此可能在性能上不会有什么差别，但是到了实际生产环境时，我们要面对的数据千千万万，可能因为全等监视这一个设置就消耗大量的资源，让应用停滞不前。因此需要在使用时进行权衡，再决定究竟使用哪一种监视方式。

除了上面提到的两种方式之外，在angular 1.1.4版本之后，新增加了一个**$watchCollection()方法** 来针对数组（也就是集合）进行监视，它的性能介于全等监视和引用监视之间，即它并不会对数组中每一项的属性进行监视，但是可以对数组的项目增减做出反应。

把$watch()方法修改为$watchCollection()方法，如果改变了items[0]的value属性值，$watch()方法并不会做出反应，但是如果我们在items上push或者pop一个元素，$watch()方法就会执行回调方法了。

#### 作用域监视解除

这需要我们关注一下$watch()方法的返回值，该方法调用完毕后返回另一个方法，我们只需调用返回的方法即可解除作用域监视。

#### $apply方法与$digest循环

$apply与$digest是AngularJS中两个核心的概念，如果读者想深入研究AngularJS双向数据绑定实现原理，就必须先了解这两个概念，为了方便说明问题，我们先看下面的代码片段：

```html
<input  id="input" type="text" ng-model="name"/>
<div id="output">{{name}}</div>
```
当我们写下AngularJS表达式时，AngularJS框架会在幕后为我们在$scope中设置一个watcher，用来在数据发生变化的时候更新View。

换句话说，AngularJS是如何知晓name属性发生了变化才调用了对应的回调方法呢？它会周期性地运行一个函数来检查scope模型中的数据是否发生了变化，这就是所谓的$digest循环。

在$digest循环中，watchers会被触发。当一个watcher被触发时，AngularJS会检测scope模型，如果它发生了变化，那么关联到该watcher的回调方法就会被调用。

在调用了$scope.$digest()后，$digest循环就开始了。假设你在一个ng-click指令对应的事件处理方法中更改了scope中的一条数据，此时AngularJS会自动地通过调用$digest()来触发一轮$digest循环。当$digest循环开始后，它会触发每个watcher。这些watcher会检查scope中的当前属性值是否和上一次$digest循环时的属性值相同。如果不同，对应的回调方法就会被执行。调用该方法的结果就是View中的表达式内容被更新。除了ng-click指令，还有一些其他的AngularJS内置指令和服务来让我们能够更改模型数据（比如ng-model指令、$timeout服务等）和自动触发一次$digest循环。

#### $apply与$digest应用实战

当AngularJS作用域中的模型数据发生变化时，AngularJS会自动触发$digest循环，从而达到自动更新视图的目的。但是在有些情况下，模型数据修改后需要我们手动调用$apply()方法来触发$digest循环，例如使用JavaScript的setTimeout()方法来更新一个模型数据，AngularJS框架就没有办法知道我们修改了什么，也就无法触发$digest循环了。

#### $timeout与$interval服务介绍

在5.3.4小节中我们使用JavaScript的setTimeout()方法达到延迟执行某个方法的效果。JavaScript中还有一个与setTimeout()类似的方法setInterval()，作用是每隔一段时间调用一次特定的JavaScript方法。这两个方法都需要我们手动调用$apply()方法来触发$digest循环。

使用AngularJS内置的指令或服务通常不需要我们手动调用$apply()方法触发$digest循环，AngularJS为我们提供了两个实用的服务$timeout和$interval，功能和setTimeout()、setInterval()方法相同，使用这两个服务修改作用域属性时会自动触发$digest循环。

### 作用域事件路由与广播

AngularJS作用域的另外一个优秀特性是它的事件传播机制，在一些情况下，我们需要在控制器中对一些事件做出处理。例如Ajax请求完成事件，我们需要通知控制器处理服务端返回的数据，作用域事件传播机制可以帮我们完成这些操作。除此之外，当我们需要在控制器直接传递数据时，也可以使用作用域事件机制来完成。

AngularJS作用域支持下面两种事件传播方式：

- 事件从子作用域路由到父作用域中。
- 事件从父作用域广播到所有子作用域中。

![两种事件传播方式](https://graphbed.qiniu.songxingguo.com/AngularJS/%E4%BA%8B%E4%BB%B6%E4%BC%A0%E6%92%AD%E6%96%B9%E5%BC%8F.jpg)

与AngularJS作用域事件相关的方法有$on()、$emit()、$broadcast()，接下来以实际案例对作用域两种事件传播机制进行介绍。

#### $emit方法实现事件路由

AngularJS作用域对象提供了一个$emit()方法，用于实现事件从子作用域路由到父作用域中，$emit()方法第一个参数为事件名称，后面可以传入一个或多个参数，这些参数能够被传递到父作用域注册的事件监听器中，$emit()方法使用如下：

```js
$scope.$emit（"infoEvent",{name:"Jane",age:23}）;
```
消息发送出去以后，我们可以在父作用域中调用AngularJS作用域对象的$on()方法，注册一个事件监听器监听子作用域路由的事件。

#### $broadcast方法实现事件广播

$broadcast()方法的使用和$emit()方法相同，不同的是，它用于向子作用域广播事件，所有的子作用域只要注册了事件监听器就能收到父作用域的广播事件。

#### 作用域对象$on方法详解

前两个小节的案例中，我们调用作用域对象的$emit()和$broadcast()方法把事件传播出去后，使用到作用域对象的$on方法。

$on()方法用于注册一个事件监听器，该方法接收两个参数，第一个参数是要监听事件的名称，第二个参数是事件处理方法，具体使用如下：

```
$scope.$on（"infoEvent",function（event,data）{
}）;
```
事件处理方法的第一个参数event为事件对象，第二个参数data为调用$emit()或$broadcast()方法传递的数据。需要注意的是，event事件对象具有一些实用的
属性和方法，我们能够通过它获取更多关于事件的信息，具体如下：

- event.name：事件的名称。
- event.targetScope：事件源作用域对象。
- event.currentScope：当前作用域对象。
- event.stopPropagation()：这个方法用于停止事件的进一步传播。需要注意的是，该方法只对向父作用域路由事件起作用，当在某个事件监听处理方法中调用事件对象的stopPropagation()方法后，事件将不会再向上级父作用域路由。它对调用$broadcast()方法广播的事件不起作用。
- event.preventDefault()：这个方法实际上不会做什么操作，但是会设置defaultPrevented属性为true，直到事件监听器的实现者采取行动之前才会检查defaultPrevented的值。
- event.defaultPrevented：如果调用了event.preventDefault()方法，那么该属性将被设置为true。

## 路由与多视图

在AngularJS应用中，我们可以把一个完整的HTML页面拆分成多个视图，每个视图实际上就是一段HTML片段，路由机制就是在每个视图和URL之间建立映射关系，当通过AngularJS路由API访问URL时，页面中能够加载对应的视图内容。

图6.1能够帮助我们直观地理解AngularJS路由机制。在图6.1中，我们有两个URL，分别为ShowOrders和AddNewOrder，使用路由机制分别将这两个URL映射到订单列表视图和新增订单视图，这两个视图分别由两个不同的控制器进行管理，当访问ShowOrders时，界面中会加载订单列表视图，而访问AddNewOrder时则会显示新增订单视图的内容。

![路由图解](https://graphbed.qiniu.songxingguo.com/AngularJS/AngularJS%E8%B7%AF%E7%94%B1%E6%9C%BA%E5%88%B6%E5%9B%BE%E8%A7%A3.jpg)

### 创建多视图应用

#### 使用$routeProvider创建映射

为了建立URL到AngularJS视图的映射，我们需要用到AngularJS的一个内置Provider对象$routeProvider，该对象用于创建路由映射，提供了一个when(path, route)方法和otherwise(params)方法，能够帮助我们把控制器、视图模板、URL关联起来，下面是AngularJS官方文档中对这两个方法的介绍。

- when(path, route)方法接收两个参数，具体如下：

  1. path：string类型，路由路径（和$location.path相对应），如果$location.path路径后有多余的“/”或者缺少“/”，路由依然能够匹配，并且$location.path会依据路由定义删除多余的“/”或者增加“/”。除此之外，在path中还可以使用占位符，需要使用“:”隔开，例如/ ShowOrders:Num。
  2. route：Object类型，用于配置映射信息。该对象具有以下属性：

      - controller ：{string|function}类型，用于指定控制器名称或控制器构造方法。
      - controllerAs ：string类型，通过控制器标识符名称引用控制器。
      - template ： {string|function}类型，该属性可为字符串类型，用于指定视图模板，也可以是一个方法，该方法必须返回HTML模板内容。
      - templateUrl : string类型，作用和template属性相同，不同的是，它用于指定视图模板文件路径。
      - resolve ：Object类型，用于指定注入控制器中的内容。

- otherwise(params)，该方法接收一个string类型的参数，用于匹配路由中未定义的URL。

```js
var routeModule = angular.module（'routeModule', ['ngRoute']）;
routeModule.config（['$routeProvider',
  function（$routeProvider） {
    $routeProvider.
    when（'/addOrder', {
      templateUrl: 'templates/add-order.html',
      controller: 'AddOrderController'
    }）.
    when（'/showOrders', {
      templateUrl: 'templates/show-orders.html',
      controller: 'ShowOrdersController'
    }）.
    otherwise（{
      redirectTo: '/addOrder'
    }）;
}]）;
```
需要注意的是，AngularJS的路由模块作为一个单独的模块，模块名称为ngRoute，我们如果在自定义的模块中使用它，需要添加ngRoute模块依赖，代码如下：

```js
var routeModule = angular.module（'routeModule', ['ngRoute']）;
```

#### 创建多视图

前面我们使用$routeProvider对象把/addOrder和/showOrders映射到两个视图templates/add-order.html和templates/show-orders.html。

#### 通过路由切换视图

前面我们通过$routeProvider对象配置了路由，并创建了视图页面，接下来我们就在主页面中实现多视图的切换。

#### 通过URL向控制器传递参数

前面我们已经学习了如何定义路由，并通过路由机制实现了一个简单的多视图应用。

在路由定义时，我们可以在URL中增加一个参数orderId，用冒号隔开，如下面的代码所示。

```js
var routeModule = angular.module（'routeModule', ['ngRoute']）;
routeModule.config（['$routeProvider',
  function（$routeProvider） {
    $routeProvider.
    when（'/showOrder/:orderId', {
      templateUrl: 'templates/order-details.html',
      controller: 'ShowOrderController'
    }）
}]）;
outeModule.controller（"ShowOrderController”,function（$scope, $routeParams）{
  $scope.order_id = $routeParams.orderId;
}）;
```
为了获取URL中传递的参数，我们在控制器中注入$routeParams服务，通过$routeParams. orderId属性访问URL中传递的参数，这里把参数保存在$scope对象的order_id属性中：

```js
$scope.order_id = $routeParams.orderId;
```
### ng-template指令的使用

在本章前面的两个案例中，我们把每个视图都写在一个单独的HTML文件中，有时候视图内容比较少，我们可能会希望把不同的视图集成到同一个页面中，或者要开发一个单页面应用（SPA），就可以使用ng-template指令来实现。


### $location服务

$location服务是AngularJS中和浏览器地址栏URL相关的一个内置服务，始终和浏览器地址栏URL保持同步状态，浏览器地址栏发生改变时，$location服务会实时更新。使用$location服务可以获取地址栏URL，或者对地址栏URL进行修改，以达到访问不同路由的效果。

$location服务提供了一些setter和getter方法，用于获取或修改URL的不同部分。一些URL部分是无法修改的，例如host、protocol、port等，我们只能使用$location服务获取它们。另一部分是可以修改的，例如path、search、hash等。

下图来自AngularJS官方网站，详细地描述了URL的各个部分。

![URL的各个部分](https://graphbed.qiniu.songxingguo.com/AngularJS/URL%E7%9A%84%E4%B8%8D%E5%90%8C%E7%BB%84%E6%88%90%E9%83%A8%E5%88%86.jpg)

以地址http://foo.com:8080/bar?bar=23#baz为例，该URL主要由以下几部分构成：
- Protocol：协议名，通过$location.protocol()方法获取，该方法会返回字符串http。
- Host：主机名，通过$location.host()方法获取，返回foo.com。
- Port：端口号，通过$location.port()方法获取，本例中为8080。
- Path：AngularJS的$location服务支持两种URL模式，即HTML5模式和Hashbang模式。对于HTML5模式URL，Path为端口之后、请求参数之前的部分。例如，http://127.0.0.1:3324/ch06/ch06_04.html ，Path就为/ch06/ch06_04.html。Hashbang模式URL则不同，Path为第一个#后的内容。例如，http://127.0.0.1:3324/ch06/ch06_04.html#/abc ，Path就为/abc。

AngularJS的$location服务默认采用Hashbang模式访问与修改URL，但是我们可以通过$locationProvider对象进行模式设置。

```js
routeModule.config（['$locationProvider',
function（$locationProvider） {
  $locationProvider.html5Mode（true）;
}]）;
```
Search：Search可以认为是URL中的请求参数。前面我们在URL中通过$routeParams向控制器中传递参数，其实也可以使用$location.search()方法实现。
获取URL请求参数只需调用无参数的search方法：

```js
$location.search();
```
我们可以向search()方法中传递一个JavaScript对象来设置URL请求参数，例如：

```js
$location.search（{key1:value1,key2:value2}）;
```
Hash：Hash也是URL的组成部分，用于把当前浏览器窗口定位于HTML的某一部位。$location对象提供了一个hash()方法，用于获取或设置URL的hash部分。

获取URL的hash部分：

```js
$location.hash();
```
设置URL的hash部分：

```js
$location.hash（"div1"）;
```
这些方法的返回值仍然是一个$location对象，所以我们可以使用链式调用方式。例如，下面的调用方式也是允许的：

```js
$location.path（'/ch06_04.html'）.search（{key1:value1}）.hash（'div1'）;
```

### $location实现多视图切换

在前面的案例中，我们都是通过超链接的形式改变浏览器地址栏URL，从而达到视图切换的效果。

```js
var routeModule = angular.module（'routeModule',['ngRoute']）
routeModule.config（['$routeProvider',
  function（$routeProvider） {
    $routeProvider.
    when（'/view1', {
    	templateUrl: '/view1.html'
    }）.
    when（'/view2', {
    	templateUrl: '/view2.html'
    }）
}]）;
routeModule.controller（"MultiViewController”,
  function（$scope,$location）{
    $scope.goView1 = function() {
      $location.path（"/view1”）;
    };
    $scope.goView2 = function() {
      $location.path（"/view2”）;
    };
}）;
```
### 路由事件

使用AngularJS路由机制实现多视图应用时，有以下几个相关的事件需要我们关注。

1. $location相关事件

  - $locationChangeStart：该事件会在我们调用$location服务的一些方法（例如path()方法）改变浏览器地址栏之前触发，并会被传递到$rootScope作用域中。
  - $locationChangeSuccess：该事件在调用$location服务改变浏览器地址栏URL后触发，但是当我们监听$locationChangeStart事件并调用preventDefault()方法阻止了事件的传播后，该事件将不会被触发。

2. $route相关事件

  - $routeChangeStart：该事件会在AngularJS开始改变路由时触发，也会被传递到$rootScope作用域中。
  - $routeChangeSuccess：该事件会在路由改变成功时触发，AngularJS的ng-view指令会监听该事件，接收到该事件时会实例化相应的控制器并渲染视图。
  - $routeChangeError：路由切换异常时触发，该事件同样会传递到$rootScope作用域中。

## ng-include指令

在某些情况下我们希望把一些通用的HTML代码片段写在一个单独的文件中，然后在其他页面中把该片段包含进来，例如一个网站的导航栏、友情链接等区域，所有页面都是相同的，我们可以把这些代码剥离出来，放在一个单独的文件中，然后使用ng-include指令引入其他页面中。

### 6.8　UI Router框架使用

前面我们已经学习了可以使用AngularJS的ngRouter模块实现多视图切换效果，但是ngRouter模块有一些局限性，**多个视图之间不能进行嵌套** 。当我们的应用程序越来越复杂时，可能希望在一个视图中嵌套另一个视图。例如，我们通过ng-view指令加载了一个控制面板页面后，如果想在控制面板中添加一些子视图，ngRouter模块就不支持，此时我们可以使用UI Router框架来实现。UI Router是基于AngularJS的，用于编写单页面应用（Single Page Application）的路由框架，支持多视图嵌套和多个命名视图，接下来我们一起学习UI Router框架的使用。

#### UI Router下载与安装

UI Router和AngularJS框架一样，也是开源的，目前源码托管在github上，源码地址为：

https://github.com/angular-ui/ui-router/

有兴趣的读者可以把源码复制到本地进行学习与研究。本书的目的是介绍UI Router的使用方法，我们只需下载UI Router的Release版本即可，官方下载地址为：

http://angular-ui.github.io/ui-router/release/angular-ui-router.js

对应的压缩版本下载地址为：

http://angular-ui.github.io/ui-router/release/angular-ui-router.min.js

#### UI Router使用案例

获取到UI Router库文件后，我们就开始使用UI Router实现一个多视图切换案例，首先需要定义几个路由状态，代码如下：

```js
var routeModule = angular.module（'routeModule', ['ui.router']）;
routeModule.config（function（$stateProvider, $urlRouterProvider） {
  $urlRouterProvider.otherwise（"/"）;
  $stateProvider.state（"home",{
      url:"/home",
      templateUrl:"template/home.html",
      controller:function（$scope） {
        $scope.books = ["Think In Java","Learning Bootstrap","NG Book2"];
      }
  }）;
  $stateProvider.state（"about",{
    url:"/about",
    templateUrl:"template/about.html",
    controller:function（$scope） {
      $scope.name = "江荣波";
    }
  }）;
  $stateProvider.state（"contact",{
    url:"/contact",
    templateUrl:"template/contact.html"
  }）;
}）;
```
在主页面中使用UI Route内置的ui-sref指令实现多个视图之间的切换，代码如下：

```html
<ul class="breadcrumb">
  <li><a ui-sref="home">首页</a></li>
  <li><a ui-sref="about">关于</a></li>
  <li><a ui-sref="contact">联系我</a></li>
</ul>
```
## AngularJS表单校验

### Web前端表单校验的必要性

**Web前端表单校验对于提高Web安全性意义并不是很大** ，因为服务器接收到的请求不一定来自前端页面，还有可能是跨站伪造请求（CSRF），甚至攻击者可以根据HTTP协议规范手动构造一段HTTP请求报文，然后与Web应用服务器建立Socket连接，把手动构造的请求数据发往Web服务器，这样就绕过了前端页面，所以服务器端的数据校验永远都是必不可少的。但是 **Web前端数据校验的意义在于改善用户体验，用户不用等到将数据提交到服务器就知道哪些数据是不合法的，这样也能够减轻应用服务器的压力** 。

### AngularJS表单校验模式

本节将要介绍AngularJS表单验证的相关知识，不过在此之前我们有必要先来了解一下AngularJS表单验证的原理。

对于这些CSS样式，AngularJS在作用域中会维护一个状态属性与之对应。表7.1为CSS样式与状态属性对应关系。

![CSS样式与状态属性对应关系](https://graphbed.qiniu.songxingguo.com/AngularJS/CSS%E6%A0%B7%E5%BC%8F%E4%B8%8E%E7%8A%B6%E6%80%81%E5%B1%9E%E6%80%A7%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB.jpg)

这些状态属性的值是AngularJS框架维护的，我们只需关注这些状态的值即可知道表单输入数据是否合法。

接下来我们需要关注一些常用的AngularJS表单校验相关的属性和指令：

- 必填项

  验证某个表单是否已填写，只要在输入字段元素上添加HTML5标记required即可：

  ```html
  <input type="text" required />
  ```
- 最小长度

  验证表单输入的文本长度是否大于某个最小值，在输入字段上使用AngularJS指令ng-minleng="{number}"：

  ```html
  <input type="text" ng-minlength="5" />
  ```
- 最大长度

  验证表单输入的文本长度是否小于或等于某个最大值，在输入字段上使用AngularJS指令ng-maxlength="{number}"：

  ```html
  <input type="text" ng-maxlength="20" />
  ```
- 模式匹配

  使用ng-pattern="/PATTERN/"来确保输入能够匹配指定的正则表达式：

  ```html
  <input type="text" ng-pattern="[a-zA-Z]" />
  ```
- 电子邮件

  验证输入内容是否是电子邮件，只要像下面这样将input的类型设置为email即可：

  ```html
  <input type="email" name="email" ng-model="user.email" />
  ```
- 数字

  验证输入内容是否是数字，将input的类型设置为number即可：

  ```html
  <input type="number" name="age" ng-model="user.age" />
  ```
- URL

  验证输入内容是否满足URL格式要求，将input的类型设置为url即可：

  ```html
  <input type="url" name="homepage" ng-model="user.url" />
  ```
### ngMessages模块

ngMessages模块是AngularJS 1.3版本之后新增的表单验证相关的模块，前面我们通过用ng-show指令判断$error对象属性值的方法来向用户展示表单数据输入不合法的提示信息，例如：

```html
<span ng-show="user.email.$error.required">内容不能为空</span>
<span ng-show="user.email.$error.minlength">长度不能小于10</span>
<span ng-show="user.email.$error.maxlength">长度不能大于20</span>
<span ng-show="user.email.$error.email">不符合Email格式</span>
```
ngMessages模块的主要作用是对这些提示信息展示功能进行增强，这里以一个具体案例介绍ngMessages模块的使用。

ngMessages模块的全部内容定义在一个单独的angular-messages.js文件中，我们下载的AngularJS Release包中已经包含了该模块的内容，在使用之前只需要通过`<script>`标签引入页面中，代码如下：

  ```js
  <script src="/angular-1.5.5/angular.js"></script>
  <script src="/angular-1.5.5/angular-messages.js"></script>
  ```
然后我们需要在自定义模块中添加ngMessages模块的依赖，例如：

```js
angular.module（'formModule', ['ngMessages']）;
```
此之外，ngMessages模块支持错误信息复用，我们可以把公共的提示信息写在一个单独的errors.html文件中，例如：

```html
<div ng-message="required">用户留言不能为空！</div>
<div ng-message="minlength">留言信息长度不能少于10个字符！</div>
<div ng-message="maxlength">留言信息长度不能多于100个字符！</div>
```
然后使用ng-messages-include指令引入，代码如下：

```html
<div ng-messages="exampleForm.userMessage.$error"
ng-messages-include="errors/errors.html">
</div>
```
## AngularJS指令

一般情况下，AngularJS的内置指令并不是总能够满足我们的需求，必要时需要我们自定义指令。
指令是AngularJS应用最重要的组成部分之一，是对HTML属性或元素的扩展，通常情况下Web浏览器并不能识别这些属性或元素，但是AngularJS框架会把它们作为指令，然后执行相应的逻辑处理，最终将这些指令解析为Web浏览器能够识别的元素。指令的出现形式有以下4种：

1. 作为HTML元素出现，例如：

  ```html
  <my-directive></my-directive>
  ```
2. 作为HTML元素属性出现，例如：

  ```html
  <input type="text" my-directive/>
  ```
3. 作为CSS类样式出现，例如：

  ```html
  <input type="text" class="my-directive"/>
  ```
4. 作为HTML注释内容出现，例如：

  ```html
  <!--my-directive-->
  ```
通常情况下，我们会尽量将指令作为HTML元素或属性使用，这样代码可读性较强，比较容易理解，当然特殊情况除外。

### 置指令详解

本节我们一起回顾一下前面用到过的AngularJS内置指令，并对常用的指令使用做一个汇总，所有AngularJS内置指令都是以ng开头，具体如下：

- ng-app

  ng-app是我们接触到的第一个指令。每个AngularJS应用都离不开ng-app指令，主要用于启动AngularJS框架，通常声明在<html>标签中，让AngularJS框架管理整个DOM元素。
  
- ng-model

  表单元素和对应作用域中的属性建立数据绑定时，该指令用于表单元素中，例如：

  ```html
  <input type="text" ng-model="name"/>
  ```
- ng-init

  该指令用于初始化AngularJS作用域，可以出现在任何HTML元素中，例如：

  ```html
  <body ng-init="price=10;num=1">
  ```
- ng-controller

  该指令用于实例化指定的控制器对象，前面我们已经多次使用过，这里就不再赘述了。

- ng-disabled

  该指令用于控制表单元素是否被禁用，当属性值为true时表单被禁用，属性值为false时解除禁用，使用如下：

  ```html
  <input type="text" ng-disabled="true" ng-model="name"/>
  ```
  除此之外，ng-disabled指令还支持将AngularJS表达式返回结果作为判断条件，例如：

  ```html
  <input type="text" ng-disabled="{{result}}" ng-model="name"/>
  ```
- ng-checked

  根据该指令值true/false指定表单元素是否被选中，适用于checkbox和radio类型表单，同样可接收AngularJS表达式结果作为判断条件。使用方法如下：

  ```html
  <input type="checkbox" ng-checked="true" ng-model="sex"/>
  ```
  或者

  ```html
  <input type="checkbox" ng-checked="{{isMale}}" ng-model="sex"/>
  ```
- ng-change

  该指令用于将表单内容变化事件和控制器中定义的事件处理方法进行绑定。

  ```html
  <input type="checkbox" ng-change="change()" ng-model="name"/>
  ```
  在控制器中定义对应事件处理方法：

  ```html
  angular.module（"app",[]）.controller（"MainController",
    function（$scope）{
      $scope.change = function(){
        alert（"change function!"）;
      };
    }
  ）;
  ```
  与ng-change指令类似的还有ng-click、ng-blur等指令。

- ng-class

  指定HTML元素中使用的CSS样式，可接收AngularJS表达式返回结果，例如：

  ```html
  <div ng-class="{{style}}"></div>
  ```
- ng-show/ng-hide

  通过属性值true/false控制HTML元素的显示与隐藏，需要注意的是它并不是从DOM中移除元素。

- ng-bind

  该指令和AngularJS表达式{{expression}}效果类似，将作用域中的数据输出到界面，例如：

  ```html
  <span ng-bind="name"></span>
  ```
- ng-if

  可以在ng-if指令后指定一个表达式，表达式内容为false时，该指令所在的元素开始标签到结束标签之间的部分会从DOM中移除，例如：

  ```html
  <div ng-if="false">内容</div>
  ```
  ng-if指令值为false时整个`<div>`标签的内容不会出现在DOM中。

- ng-switch/ng-switch-default/ng-switch-when

  这一组指令类似于JavaScript语言中的switch…case语句，使用方式如下：

  ```html
  <div ng-switch on="1+1">
  <p ng-switch-default>0</p>
  <p ng-switch-when="1">1</p>
  <p ng-switch-when="2">2</p>
   <p ng-switch-when="3">3</p>
  </div>
  ```
  根据on属性指定的表达式结果决定哪一个分支的内容会显示在HTML页面中。

- ng-repeat

  该指令使用的频率比较高，用于遍历作用域中的集合数据。假设作用域中通过数组保存多个用户信息，格式如下：

  ```js
  $scope.persons=[{"name":"Smith","age":20,"address":"China"},
  {"name":"Jane","age":30,"address":"USA"}];
  ```
  在视图中可以通过ng-repeat指令对数组进行遍历，并将数据内容输出到界面，例如：

  ```js
  <div ng-repeat=”person in persons”>
  姓名：{{person.name}}<br/>
  年龄：{{person.age}}<br/>
  地址：{{person.address}}<br/>
  </div>
  ```
- ng-href/ng-src

  它们是对一些HTML标签的href和src属性的增强，这两个指令可以接收AngularJS表达式作为属性值，可以在作用域中通过改变模型数据来改变ng-href/ng-src的属性值。

- ng-include

  该指令用于将一个单独的HTML文件内容包含到指令出现的位置，使用方法如下：

  ```html
  <div ng-include="'templates/navbar.html'"></div>
  ```
  除此之外，还有一些事件相关的指令，例如ng-click、ng-mousedown等，使用方法和上面提到的ng-change指令相同，这里就不做过多介绍了。

### AngularJS自定义指令

一般情况下，AngularJS内置指令就能够满足我们的需求，但是为了简化开发过程，我们可以将通用的功能封装成指令，形成自定义的指令库。AngularJS模块对象提供了一个directive()方法来帮助我们实现自定义指令，使用方法如下：

```js
var app = angular.module（"directiveModule",[]）;
app.directive（"helloWorld",function(){
return {
restrict:'AEC',
replace:true,
template:'<h1>My First Custom Directive!</h1>'
}
}）;
```
irective()方法接收两个参数：第一个参数为指令名称，采用驼峰式命名法；第二个参数为指令定义方法，需要返回一个对象（称为指令定义对象DDO），用于描述指令的特征及指令对应的处理逻辑。我们可以向指令定义方法中注入一些依赖，例如$http、$rootScope等。

当我们的指令名称采用驼峰式命名法后，在指令使用时可以使用“-”或者“：”将多个单词隔开，例如上面我们自定义的helloWorld指令使用方法可以为：

```html
<hello-world></hello-world>
<hello:world></hello:world>
<span hello-world></span>
<span hello:world></span>
```
为了符合HTML5命名规范，我们还可以在指令名称前加上“x-”或“data-”，例如：

```html
<x-hello-world></x-hello-world>
<data-hello-world></data-hello-world>
```
当AngularJS框架匹配到自定义指令时，会自动将指令前缀“x-”或“data-”去除，并将指令名称中的“-”或者“：”分割符去掉，转换为驼峰式命名。

接下来我们了解一下自定义的helloWorld指令，指令定义方法返回对象（指令定义对象）的各个属性含义如下：

- restrict属性，前面已经提到过，AngularJS指令使用形式有4种，该属性用于约束我们自定义的指令可以以什么形式出现，如下表所示，取值有E（元素）、A（属性）、C（类）、M（注释），其中默认值为A。

  ![restrict属性值对应含义](https://graphbed.qiniu.songxingguo.com/AngularJS/restrict%E5%B1%9E%E6%80%A7%E5%80%BC%E5%AF%B9%E5%BA%94%E5%90%AB%E4%B9%89.jpg)

  需要注意的是，这些标志可以组合使用，例如restrict:'AEC'表示该指令可以同时作为HTML元素、属性、CSS样式使用，当我们不想让该指令以CSS样式出现时，去掉字母'C'即可。

- replace属性，该属性用于指定是否使用template属性定义的HTML模板内容替换指令所在的HTML元素。例如，helloWorld指令作为HTML属性使用时为 `<div hello-world></div>`，当replace属性为false时，打开浏览器开发人员工具，指令编译后生成的HTML内容如下：

  ```html
  <div hello-world="">
  <h1>My First Custom Directive!</h1>
  </div>
  ```
  当replace属性为true时，div元素则就被直接替换为指令模板的内容：

  ```html
  <h1 hello-world="">My First Custom Directive!</h1>
  ```
- template属性，该属性用于指定AngularJS指令被替换成的HTML模板，例如上面的helloWorld指令，我们指定template属性内容为一段字符串"`<h1>My First Custom Directive!</h1>`"，指令最终就会被解析成template属性指定的HTML模板内容输出到指令使用处。

  需要注意的是，template属性并非简单地指定一段HTML内容这么简单，我们可以在HTML内容中使用其他指令和AngularJS表达式，AngularJS会先解析指令模板中的其他指令和表达式，然后把最终的结果输出到指令使用处。在大多数情况下，我们会把指令模板内容写在一个单独的HTML文件中，或者使用ng-template指令定义一个模板，这时可以使用templateUrl指令替换template。例如，我们的指令模板写在一个单独的helloworld.html文件中，helloWorld指令就可以定义为：

  ```js
  app.directive（"helloWorld",function(){
    return {
      restrict:'AEC',
      replace:true,
      templateUrl:'helloworld.html'
    }
  }）;
  ```
  使用templateUrl指定一个HTML文件作为指令模板，效果和template属性一致。


### 指令定义对象详解

在8.2节我们已经接触了自定义指令，directive()方法第二个参数为指令定义方法，该方法返回一个对象，即指令定义对象（Directive Definition Object, DDO）。

#### link方法

在前面自定义的helloWorld指令的例子中已经提到过，指令定义对象的template属性用于指定AngularJS指令被替换成的HTML模板。在HTML模板中我们可以使用AngugarJS表达式{{property}}形式。

使用了指令定义对象的link()方法，接收如下3个参数：

- scope参数：表示指令作用域，默认情况下和父级作用域相同。例如，我们使用ng-controller指令声明实例化一个控制器对象，此时会创建一个作用域对象。当指令在ng-controller所在的元素开始标签和结束标签之间使用时，该指令会使用控制器对象的作用域，因此可以在link()方法中增加模型数据。在上面的代码中，我们在作用域对象中增加了一个name属性。

- elem参数：表示应用当前指令的HTML元素。例如 `<div say-hello></div>`，sayHello指令应用于div元素，elem为经过JQLite包装后的div元素。前面已经提到过，JQLite是jQuery的一个子集，用于方便基本的DOM操作。如果我们想使用jQuery，只需通过`<script>`标签将jQuery库文件引入当前页面中，elem将会成为jQuery包装后的元素。

- attrs参数：表示一个对象，包含指令的所有属性及属性值。例如，在上面的案例中，我们在sayHello指令使用时指定了一个name属性<say-hello name="Smith"></say-hello>，就可以通过attrs.name获取属性值。

另外，link()方法的作用并不仅仅局限于此，对link()方法的使用场合如下：
- 需要获取或修改自定义指令属性时。
- 当指令需要监视AngularJS作用域模型数据变化时。
- 当指令需要响应HTML模板中元素单击、鼠标移动等事件时。

在link()方法中，我们还可以响应HTML元素的事件和监视作用域模型数据变化。

#### compile方法

在介绍指令定义对象的compile()方法之前，我们有必要先来了解一下AngularJS处理指令的过程。浏览器开始渲染HTML页面时，首先加载HTML元素、创建文档对象模型（DOM），加载完成后会触发onload事件。我们通过`<script>`标签将AngularJS库引入HTML页面中，AngularJS就会监听onload事件，然后从DOM元素中查找ng-app指令，如果找到就启动AngularJS框架，接着从ng-app指令所在的HTML标签开始使用$compile服务遍历DOM元素。我们使用directive()方法注册的指令都会保存在一个容器中，AngularJS会从这些DOM元素中识别哪些属于指令元素，AngularJS框架会根据指令定义对象决定如何处理这些指令。一旦所有的指令都被识别，就会执行指令定义对象的compile()方法。

所有指令的compile()方法只会在AngularJS框架启动时调用一次，该方法的定义如下：

```js
compile: function（tElem, tAttrs）{
  return {
  link:function（scope, iElem, iAttrs）{ }
  }
}
```
compile()方法可接收两个参数，含义如下：

- tElem：指令所在的元素。
- tAttrs：指令元素的所有属性列表。

需要注意的是，compile()方法和link()方法有冲突，如果指定了指定定义对象的compile()方法就不能再为指定定义对象增加link()方法，但是link()方法可以由compile()方法作为返回值返回（如果没有指定compile()方法，就可以正常使用link()方法）。

compile()方法只在AngularJS应用启动时执行一次，但是link()方法会针对每个被复制的实例执行，如果分开处理，就会在性能上有一定提高。

compile()方法的主要作用是在link()方法执行之前对DOM元素进行修改。

#### scope属性与指令作用域

下面我们开始学习AngularJS指令作用域与父作用域的关系。默认情况下，指令直接使用父作用域，例如：

```html
<div ng-controller="MainController">
<hello-world></hello-world>
</div>
```
我们使用ng-controller指令实例化控制器对象时，AngularJS会创建一个新的作用域对象$scope。我们在`<div>`标签中使用了hello-world指令，如果不做处理，那么hello-world指令的作用域就和MainController控制器实例化时创建的作用域对象是同一个对象。有些时候这并不是我们想要的结果，因为某个指令对作用域模型数据的修改会对其他指令有影响。我们希望指令能够彻底摆脱父控制器的作用域，这样我们就可以对作用域中的数据进行任意修改。而且在一些情况下我们需要在作用域中添加一些模型数据仅供指令内部使用，例如，需要有一个属性控制指令元素的显示与隐藏，如果直接使用父作用域对象，这个属性在其他指令或控制器中也就8.3.3　scope属性与指令作用域
下面我们开始学习AngularJS指令作用域与父作用域的关系。默认情况下，指令直接使用父作用域，例如：
<div ng-controller="MainController">
<hello-world></hello-world>
</div>
我们使用ng-controller指令实例化控制器对象时，AngularJS会创建一个新的作用域对象$scope。我们在<div>标签中使用了hello-world指令，如果不做处理，那么hello-world指令的作用域就和MainController控制器实例化时创建的作用域对象是同一个对象。有些时候这并不是我们想要的结果，因为某个指令对作用域模型数据的修改会对其他指令有影响。我们希望指令能够彻底摆脱父控制器的作用域，这样我们就可以对作用域中的数据进行任意修改。而且在一些情况下我们需要在作用域中添加一些模型数据仅供指令内部使用，例如，需要有一个属性控制指令元素的显示与隐藏，如果直接使用父作用域对象，这个属性在其他指令或控制器中也就能够被访问与修改，这显然是不合理的，我们用以下两种方案摆脱父作用域：

- 使用子作用域从父作用域原型继承。
- 使用孤立作用域，即创建一个新的作用域对象，和父作用域没有任何关系。

指令作用域可以通过指令定义对象的scope属性进行配置，例如：

```js
var app = angular.module（'app',[]）;
app.directive（'helloWorld', function() {
  return {
    scope: true,
    restrict: 'AE',
    replace: true,
    template: '<h3>Hello, World!</h3>'
  };
}）;
```
在上面的代码片段中，我们将指令定义对象的scope属性设置为true，表示为指令创建一个新的作用域对象，该作用域对象会从父作用域原型继承。如果我们希望在指令作用域中拥有父作用域中的所有模型数据，就可以使用这种方式。

有时候我们不需要父作用域的模型数据，此时只需要为指令创建一个孤立作用域即可，例如：

```js
var app = angular.module（'app',[]）;
app.directive（'helloWorld', function() {
  return {
    scope: {},
    restrict: 'AE',
    replace: true,
    template: '<h3>Hello, World!</h3>'
  };
}）;
```
创建孤立作用域只需要将指令定义对象的scope属性设置为{}即可。图8.3能够直观地反映指令原型继承作用域与孤立作用域的区别。

![指令原型继承作用域与孤立作用域区别](https://graphbed.qiniu.songxingguo.com/AngularJS/%E6%8C%87%E4%BB%A4%E5%8E%9F%E5%9E%8B%E7%BB%A7%E6%89%BF%E4%BD%9C%E7%94%A8%E5%9F%9F%E4%B8%8E%E5%AD%A4%E7%AB%8B%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%8C%BA%E5%88%AB.jpg)

如上图所示，假如父作用域中有两个模型数据name、age，如果将指令定义对象的scope属性设置为true，指令作用域就会从父作用域原型继承，用于name和age属性，而如果将scope指令定义对象的属性值设置为{}，则指令作用域是一个全新的对象，不会继承父作用域的任何模型数据。

#### 孤立作用域与父作用域模型数据绑定

前面我们已经提到过，孤立作用域无法继承父作用域的模型数据。这点在我们需要为指令创建大量供指令内部使用的模型数据时是非常有用的，但是有时候出于某种目的，我们不得不在孤立作用域中访问父作用域中的模型数据。幸运的是，AngularJS提供了比较灵活的方式供我们在孤立作用域与父作用域之间建立模型数据绑定。

 下面我们就来看一下如何在孤立作用域与父作用域之间建立模型数据绑定以解决这个问题。AngularJS支持以下3种绑定方式。

 1. 使用@符号建立基于属性的绑定

    在上面的案例中，控制器作用域中有一个name属性，如果需要访问父作用域中的name属性，可以使用@符号建立基于属性的绑定。图8.5能够详细地说明这一过程。

    ![使用@符号建立基于属性的绑定过程](https://graphbed.qiniu.songxingguo.com/AngularJS/%E4%BD%BF%E7%94%A8@%E7%AC%A6%E5%8F%B7%E5%BB%BA%E7%AB%8B%E5%9F%BA%E4%BA%8E%E5%B1%9E%E6%80%A7%E7%9A%84%E7%BB%91%E5%AE%9A%E8%BF%87%E7%A8%8B.jpg)

 2. 使用=符号建立双向绑定

  使用@符号在指令的孤立作用域与父作用域之间建立基于属性的绑定是单向的，而且无法对对象、数组等复杂数据模型进行绑定，接下来我们学习如何使用‘=’符号建立双向绑定，方法和@符号建立单向绑定基本相同，差异如下。

  这里我们不再需要使用AngularJS表达式{{name}}输出模型数据内容，而是直接指定属性名称，接下来将@符号换成=符号即可

  ```
  scope:{
    name:'=nameAttr'
  }
  ```
  既然是双向数据绑定，那么当我们在指令定义对象的link()方法中修改指令作用域中的name属性值时，父作用域中的name属性也会跟着被修改。

3. 使用&符号调用父作用域中的方法

   在指令孤立作用域中，我们可以使用&符号和父作用域中的方法进行绑定，这样就能够达到调用父作用域方法的目的。

#### Transclusion

Transclusion是AngularJS自定义指令中一个比较重要的概念，如果读者去查询英文词典，就会发现词典中并没有对Transclusion一词的准确解释。它的网络释义为“嵌入”，基本也能够表达出一些层面的含义，实际上是计算机领域的一个专业词汇。

在计算机科学中，Transclusion是指将整个或部分文档通过超文本引用包含到其他单个或多个文档中。

![Transclusion含义图解](https://graphbed.qiniu.songxingguo.com/AngularJS/Transclusion%E5%90%AB%E4%B9%89%E5%9B%BE%E8%A7%A3.jpg)

1. 指令定义对象transclude属性介绍

    AngularJS指令中也提供了Transclusion特性。

    能够进行“嵌入”的前提是将指令定义对象的transclude属性设置为true，该属性默认情况下为false。除此之外，transclude属性值还可以为‘element’，接下来我们看一下属性值‘element’和true之间有什么区别。

    当我们把transclude属性值设置为‘element’时，表示把指令所作用的元素一同嵌入到ng-transclude指令所在的位置，例如把sayHello指令作`<p>`标签的属性使用：

    将transclude属性设置为‘element’，同时把restrict属性值设置为‘AE’。

2. 使用Transclusion实现DOM元素复制

    AngularJS指令的Transclusion特性的另外一个作用是实现DOM元素的复制，前面介绍了指令定义对象的link()方法，如果读者还有印象，肯定知道它可以接收3个参数，实际上还有两个可选的参数，其中最后一个参数为transclusion方法，我们可以使用该方法完成“嵌入”内容的复制。
    
#### controller方法与require属性

指令定义对象的controller方法为指令控制器的构造方法，主要用于和其他指令进行通信，我们可以在指令构造方法中定义一些属性或方法供其他指令调用。

### 自定义表单验证模式

自定义了一个密码强度校验的指令实现自定义表单校验模式。


## Service、Factory与Provider

前面章节中我们已经多次提及AngularJS的Service对象，例如我们使用过的$location、$timeout、$interval等，它们都是AngularJS的内置Service，在实际项目开发中，内置Service并不总是能够满足我们的需求，因此有时候需要我们创建自定义的Service，与Service类似的组件还有Factory和Provider，虽然AngularJS的这3种组件都是用来完成同样的任务，但是它们之间还是有一些细微差别的，本章我们会对它们进行深入学习。

Service、Factory与Provider都是AngularJS中的可注入类型，这意味着我们可以通过依赖注入机制将它们注入控制器或其他方法中（例如控制器构造方法）。除此之外，还有另外两种可注入类型，即Value和Constant。

### Service

AngularJS的Service是封装了一些特定业务逻辑的单例对象，既然是单例对象，就意味着它在每个应用中只会被实例化一次（由$injector实例化），并且是延迟加载的（需要时才会被创建），它对外提供一些方法供其他组件调用。AngularJS一共提供了大约30个内置Service对象，这些内置Service对象的使用方法非常简单，只需要通过依赖注入机制把它注入需要使用这些Service的组件中即可。

比如$location，下面是它的使用方法：

```js
var app = angular.module（'myApp', []）;
app.controller（'MainController', function（$scope, $location） {
  $scope.myUrl = $location.absUrl();
}）;
```
```
var app = angular.module（'app',[]）;
app.service（'myService',function(){
  this.sayHello=function(){
    alert（'This is my service!'）;
  }
}）;
```
AngularJS的模块实例提供了一个service()方法，用于帮助我们注册一个自定义的Service。该方法接收两个参数，第一个参数为Service的名称，第二个参数是Service对象的构造方法，在构造方法中我们可以定义一些成员属性及成员方法来封装一些处理逻辑。

### Factory

Factory是AngularJS中另一种可注入类型。我们可以使用AngularJS模块实例的factory()方法创建自定义的Factory，本质上和Service并没有太大的区别，为了证明这个结论我们不妨查看AngularJS源码中service()方法和factory()方法的定义：

```js
//AngularJS源码factory方法定义
function factory（name, factoryFn, enforce） {
  return provider（name, {
    $get: enforce !== false ? enforceReturnValue（name, factoryFn） :
    factoryFn
  }）;
}
//AngularJS源码service方法定义
function service（name, constructor） {
  return factory（name, ['$injector', function（$injector） {
  		return $injector.instantiate（constructor）;
  }]）;
}
```
从上面的AngularJS源码中可以得知，service()方法中调用了factory()方法，在factory()方法中通过$injector的instantiate()方法实例化Service对象。接下来我们看一下如何使用factory()方法创建自定义的Factory，代码如下：

```js
var app = angular.module（'app',[]）;
  app.factory（"myFactory",function(){
    return{
      sayHello:function(){
      alert（"welcome to you!"）;
    }
  }
}）;
```
在上面的代码片段中，我们使用了模块实例的factory()方法创建自定义的Factory对象，该方法的第一个参数为Factory对象名称，和service方法不同的是，它的第二个参数并不是Factory对象的构造方法，而是一个普通方法，该方法必须返回一个对象实例。
接下来我们就可以把自定义的Factory对象注入其他组件中了，例如注入控制器中：

```js
app.controller（"MainController",function（myFactory）{
  myFactory.sayHello();
}）;
```
### Provider

Provider是AngularJS中的服务提供者，是一种比较灵活的可注入类型，在使用之前我们可以调用模块实例的config()方法对Provider进行配置，例如我们前面使用过的$route服务，可以通过$routeProvider对URL模式（html5Mode和hashbang）进行配置。AngularJS为我们提供了一些内置的Provider对象，除此之外，还可以通过模块实例的provider()方法创建自定义的Provider。provider()方法的使用如下：

```js
var app = angular.module（'app',[]）;
app.provider（"custom",function(){
    this.name = "Smith";
    this.getName = function(){
    return name;
  }
  this.$get = function(){
    var name = this.name;
      return  function(){
        alert（"Hello " + name）;
      }
    }
  }
}）;
```

### Value&Constant

Value和Constant是AngularJS中另外两种相对简单的可注入类型，用于保存应用程序的相关信息，从某种程度上讲，它们有点类似于面向过程语言中的全局变量的概念，AngularJS模块实例分别提供了value()和constant()方法，用于创建Value和Constant对象。

我们通过constant方法和value方法创建了Constant对象（名称为version）和Value对象（名称为CONFIG），第一个参数为名称，第二个参数为对象的值，接着将它们注入控制器中，在控制台中输出它们的值。

## AngularJS过滤器

过滤器（Filter）的作用是接收一组输入数据，在数据输出到视图前，按照一定的规则对数据进行处理，然后返回处理后的结果。有些时候作用域中的模型数据并不满足我们的格式要求，例如我们的金额为阿拉伯数字，但是需要把它转换为人民币大写输出到界面中，这之间就涉及数据格式的转换，而AngularJS过滤器的作用正是用于对数据进行处理，类似这种需要对数据进行格式化的，都可以使用AngularJS过滤器来完成。

### 过滤器使用方法

AngularJS过滤器有3种使用方法，第一种是在表达式双大括号中使用，第二种是在指令中使用，另外一种是在Controller或Service、Factory、Provider等服务中使用。下面分别对这3种使用方法做详细介绍。

1. 表达式中使用过滤器

   表达式双大括号中使用过滤器也是最常见的方式，使用方法非常简单，在|符号后加上过滤器名称即可，语法如下：

    ```
    {{ expression | filter }}
    ```
   AngularJS运行时可以同时使用多个过滤器，上一个过滤器的输出将会作为下一个过滤器的输入，多个过滤器使用|符号分割（类似于Linux系统中的管道命令），语法如下：

    ```
    {{ expression | filter1 | filter2 | ... }}
    ```
   我们可以向过滤器传递一些参数，这些参数使用：符号隔开，例如：
    ```
    {{ expression | filter1:argument1:argument2 | filter2:argument1 | ... }}
    ```
2. 在指令中使用过滤器

   除了可以在表达式中使用过滤器，还可以在一些指令中使用，例如在ng-repeat指令中先对数组array进行过滤处理再循环输出：

    ```html
    <span ng-repeat="element in array | filter ">
    ```
3. 在Controller或AngularJS服务中使用过滤器

   在Controller、Service、Factory、Provider等服务中也可以使用过滤器，方式是我们所熟悉的依赖注入，例如我们需要在Controller中使用AngularJS内置的currency过滤器，只需要将过滤器注入到控制器中即可。

### AngularJS内置过滤器

1. filter

   filter过滤器用来处理一个数组，过滤出含有某个子串的元素，将过滤后的元素作为一个子数组来返回，既可以是字符串数组，也可以是对象数组。如果是对象数组，就可以匹配属性的值。另外，它可以接收一个参数，用来定义子元素的匹配规则。

    ```
    <li ng-repeat="person in persons|filter:personInfo">
    ```
2. currency

   currency过滤器可以将数字格式转化为货币，默认是美元符号。我们可以通过参数传入其他符号，除此之外，还可以对其精度进行控制。

   使用表达式输出amount属性值时使用了currency过滤器进行处理，默认情况下，AngularJS会把数字转换为美元格式金额，增加一个$符号作为前缀，我们可以使用第一个参数为指定金额前缀，例如使用人民币符号￥作为前缀，currency过滤器的第二个参数用于控制金额保留的精度，默认保留两位小数，我们可以指定只保留1位小数。

3. number

   number过滤器用于将数字格式化为文本，同时可以对数字的精度进行控制，默认情况下保留的小数位数小于或等于3，我们可以通过参数对保留的位数进行控制，当保留位数大于小数点位数时后面补零，小于小数位数时进行四舍五入。

4. lowercase&uppercase

   lowercase和uppercase是两个相对的过滤器，lowercase过滤器用于将字符串中的大写字母全部转换为小写，uppercase过滤器和它恰恰相反，用于把字符串中的小写字母全部转化为大写。

5. date

   原生的JavaScript对日期的处理能力是比较弱的，当我们需要将一个日期格式化输出时编写较多的JavaScript代码，AngularJS为我们提供了一个date过滤器，用于简化日期格式化操作。该过滤器可以接收一个参数，为日期字符串格式。

   它可以接收一个参数为日期格式化模板，其中yyyy、MM、dd为日期格式占位符，分别表示年、月、日，hh、mm、ss分别表示时、分、秒。
   
6. json

   json过滤器用于将JavaScript对象转换为json字符串，该过滤器不接收任何参数，使用较为简单，可用作调试用途。

7. limitTo

   limitTo过滤器用来截取数组或字符串，接收一个参数用来指定截取的长度，如果参数是负值，就从尾部开始截取。

8. orderBy

   orderBy过滤器可以将一个数组中的元素进行排序，接收一个参数来指定排序规则。这个参数既可以是一个字符串，表示以该属性名进行排序；也可以是一个方法，定义排序属性；还可以是一个数组，表示依次按数组中的属性值进行排序。
   
#### 自定义过滤器

AngularJS内置过滤器的功能相当有限，并不一定能够满足我们的需求，AngularJS允许我们自定义过滤器，过程相当简单，只需要调用模块实例的filter()方法即可。filter()方法使用如下：

```js
var app = angular.module（"app", []）;
app.filter（"filterName",function(){
  return function（input）{
  // 逻辑处理
  . . .
  return result;
  }
}）;
```
如上面的代码所示，filter()方法接收两个参数，第一个参数为过滤器名称，第二个参数为过滤器定义方法，该方法必须返回一个用于处理过滤器逻辑的方法，返回的方法可以接收一个参数，即过滤器的输出数据。

### 第三方过滤器库的使用

自定义过滤器是很有必要的，因为AngularJS内置的过滤器功能有限，并不能满足所有需求，但是为了提高开发效率、节省项目成本，我们应该充分利用开源社区，对于一些人已经实现的功能，我们完全没有必要去重新发明。

#### angular-filter

angular-filter过滤器库提供了包括集合元素操作、字符串操作、数学运算、布尔运算相关的几十个过滤器，基本能够满足我们大部分需求，该项目源码托管在Github上，源码地址为：
https://github.com/a8m/angular-filter

如上面的代码所示，要使用angular-filter过滤器库，我们首先需要将依赖的库文件引入当前页面中，例如：

```html
<script src="/angular-extends/angular-filter.js"></script>
```
接着我们需要在自定义模块中添加angular.filter依赖，代码如下：

```js
var app = angular.module（"app", ['angular.filter']）;
```
#### angular-emoji-filter

angular-emoji-filter也是Github上热度相对较高的过滤器库，相信读者对Emoji表情并不陌生，而angular-emoji-filter过滤器库正是用于在网页中显示Emoji表情的。
angular-emoji-filter过滤器库的源码地址为：
https://github.com/globaldev/angular-emoji-filter，

## AngularJS中的依赖注入

依赖注入（Dependency Injection）是AngularJS中最优秀的特性之一，与之对应的另一个概念是控制反转（Inversion of Control, IoC），如果读者不是一个纯粹的Web前端开发人员，对Java EE或者.Net开发有一定了解，那么对这两个概念一定不会陌生，例如在Java EE应用中，通过Spring框架对Web层、业务逻辑层、DAO层对象进行统一管理，而这些对象之间复杂的依赖关系都是通过Spring的IoC机制建立的。

有些读者可能会把控制反转（IoC）和依赖注入（DI）混为一谈，认为两者是相同的概念，事实上这两者还是有一点区别的，控制反转是一种软件设计思想，而依赖注入是实现控制反转最直接也最简单的方式，当然控制反转也并不一定非要通过依赖注入这种方式实现不可。

tack Overflow上面有一个非常有趣的问题，那就是“如何向一个5岁小孩解释什么是依赖注入”，其中得分最高的一个答案如下：

当你需要从冰箱里为自己拿一些东西的时候，你可能会遇到一些麻烦。你可能忘了关冰箱门，或者拿了一些你父母不希望你拿的东西，甚至你可能在寻找一些我们根本就没有或者已经过期的东西。所以你需要做的只是静静地坐在那里，告诉我们你想喝什么或者吃什么，我们会把你需要的所有东西送到你面前。

这段话描述的是我们生活中最普通的事情，却道出了依赖注入的精髓所在。所谓依赖注入，就是当你在一个组件中需要依赖其他组件时，不需要你自己创建这些组件，而是通过注入的方式直接把这些组件提供给你。

一般情况下，获取依赖可以通过3种方式完成，分别为创建依赖、全局查找依赖、依赖注入，具体如下：

1. 使用new操作符创建依赖。

2. 通过全局变量查找依赖。

3. 依赖需要时被注入。

前两种获取依赖的方式都不是很好，因为它们需要对依赖进行硬编码，使得修改依赖的时候变得困难，特别是在测试的时候，因为对某个部分进行孤立的测试常常需要模拟它的依赖。第三种方式是最好的，它不必在组件中去主动查找和获取依赖，而是由外界将依赖注入。

### JavaScript依赖注入实现

为了加深大家对AngularJS依赖注入的理解，我们有必要先来了解一下JavaScript语言中如何实现依赖注入，当然这需要读者有一定的JavaScript编程功底。

[JavaScript依赖注入实现](https://yuedu.baidu.com/ebook/a219bad05ebfc77da26925c52cc58bd63086935e?pn=1&click_type=10010002)

### AngularJS中的依赖注入

前面介绍了依赖注入的相关概念，并介绍了如何在JavaScript语言中实现依赖注入，最后实现了一个IoC容器，实际上这也是AngularJS依赖注入的雏形，只不过AngularJS框架中实现的功能比较完善而已。在前面的章节中我们接触到几种AngularJS中的可注入类型，例如Service、Factory、Provider等，以Service为例，我们可以使用模块实例的service()方法来创建一个Service对象，实际上AngularJS和我们自己实现的IoC容器一样，它也维护一个容器，当我们调用service()方法后，AngularJS也会以key-value的形式将Service名称及构造方法添加的容器中。需要注意的是，AngularJS出于效率考虑，实现了延迟初始化，也就是说，当这个对象没有被依赖时，它是不会被创建的，但是当实例化某个对象时发现依赖该对象，此时AngularJS才会去实例化该对象。

AngularJS为我们提供了3种依赖注入方式，具体如下：

- 通过数组标注在方法的参数中声明依赖。
- 使用控制器构造方法的$inject属性注入依赖。
- 通过方法参数名隐式推断依赖。

1. 数组标注方式

    这种方式是AngularJS官方比较推荐的一种，具体注入代码如下：

    ```js
    var appMod = angular.module（"appMod",[]）;
    appMod.controller（"MainController",['$scope','$http',function
    （$scope,$http）{
    }]）;
    ```
    在上面的代码中，通过第二个数组类型的参数声明了$scope和$http这两个依赖，数组的最后一个参数为控制器的构造方法，需要注意的是，构造方法的参数列表与前面的数组元素是一一对应的。

2. $inject属性注入依赖

除了数组标注方式注入依赖，我们还可以通过为控制器的构造方法添加$inject属性，并在该属性中添加依赖的方式定义依赖，代码如下：

```js
var appMod = angular.module（"appMod",[]）;
var MainController = function（$scope,$http）{
$scope.name = "Smith";
}
MainController.$inject = ['$scope','$http'];
appMod.controller（"MainController",MainController）;
```
需要注意的是，$inject中依赖的顺序与构造方法中的参数顺序需保持一致。

3. 隐式声明依赖

我们前面的代码都使用这种方式，使用这种方式的好处是可以保持代码清爽简洁，让构造方法的参数与依赖的名字一样即可，例如：

```js
var appMod = angular.module（"appMod",[]）;
appMod.controller（"MainController",function（$scope,$http）{
$scope.name = "Smith";
}）;
```
注入器可以从方法的参数名中推断所依赖的服务，上面的方法中声明了以$scope和$http服务作为依赖。需要注意一点，由于是根据参数名进行依赖推断，因此这种方式不能和Javascript的代码混淆器一起使用。

AngularJS默认情况下支持上述3种依赖注入模式，但是我们可以通过ng-strict-di指令切换到严格的依赖注入模式。在严格依赖注入模式下隐式声明依赖将会抛出异常。

### $provide服务介绍

在第9章中我们接触到了AngularJS几种可注入类型，例如Service、Factory、Provider等，相信读者应该还记得这几种类型的创建过程，我们可以使用模块实例的service()、factory()、 provider()方法来创建它们。

AngularJS为我们提供了一个内置的$provider服务，我们可以使用它来创建这几种可注入类型。以Provider类型为例，$provider服务提供了一个provider()方法供我们注册一个新的Provider对象，具体使用方法如下：

```js
var appMod = angular.module（"appMod",[]）;
appMod.config（function（$provide）{
  $provide.provider（"sayHello",function(){
    this.$get = function(){
      return function(){
        alert（"Hello,Welcome to you!"）;
      }
    }
  }）;
}）;
appMod.controller（"MainController",function（sayHello）{
  sayHello();
}）;
```
$provider除了提供provider()方法创建Provider对象外，还提供了const()、value()、service()、factory()等方法用于创建其他可注入类型。

### $injector服务介绍

$injector服务实际上就是一个IoC容器，当我们创建一个新的可注入类型，例如Service、Factory等，这些可注入类型会注册到IoC容器中，AngularJS通过$injector服务对这些可注入类型进行集中管理，这也就意味着我们可以通过$injector服务获取所有的可注入类型。

$injector服务提供了一个get()方法，用于从IoC容器中获取可注入对象，使用方法$injector.get（"serviceName"），参数为可注入对象名称。

除了可以使用依赖注入将$injector服务注入控制器中外，AngularJS还提供了一个全局的injector()方法用来获取$injector服务。

如果想知道某个方法依赖于哪些服务，就可以调用$injector服务的annotate()方法获取。

## AngularJS与动画

动画是Web应用的重要组成部分，对于提升Web应用用户体验起到至关重要的作用。AngularJS 1.2之后版本引入了ngAnimate模块，使得Web动画创建更加简单。

### Web动画实现原理

动画是基于视觉残留的，其基本原理是，如果我们的大脑看到很多类似的静态画面可以足够快地切换，就会表现得像是只有一个画面在动态变化。

在CSS3诞生之前，实现Web动画基本都是 **通过JavaScript操作CSS来实现的** 。假设页面中有一个元素，当我们使用JavaScript不停地修改该元素的位置属性时，就会呈现出移动动画效果。

另外，读者应该都玩过网页游戏，一些网页游戏就是通过JavaScript实现的，游戏中基本都是通过在短时间内切换静态图片完成角色的动画效果。

### 使用CSS3实现动画

在CSS3诞生之前，除了通过一些浏览器插件（例如Flash、Applet等）实现动画外，Web动画基本都是通过JavaScript操作CSS来实现的。CSS3的诞生为Web动画的创建带来了一种新的方式。AngularJS动画也是基于CSS3动画和JavaScript动画的，本节我们将学习如何使用CSS3实现动画效果。

CSS3动画的属性主要分为3类：transform、transition以及animation。transform属性主要用于控制网页元素的旋转、平移、缩放等操作。transition属性则用于控制元素的渐变效果。animation属性就相对复杂些了，可以通过不同的关键帧组合成一幅完整的动画，通常用于实现非常复杂的动画效果。下面分别对这些属性做详细的介绍。

#### CSS3中的Transform属性

Transform属性用于完成HTML元素的一些基础变换，包含一些变化方法，常用的有rotate（旋转）、skew（斜切）、scale（缩放）、translate（平移）等。

```
transform:rotate（20deg）; //表示旋转20度
transform:skew（20deg）; //斜切拉升20度
transform:scale（1.5）; //缩放1.5倍
transform:translate（100px,0）; //沿着x坐标平移100像素
```
浏览器之争带来的兼容性问题一直是令前端开发人员比较头疼的问题，为了兼容不同的浏览器，这些属性提供了不同浏览器的兼容版本。

```
transform:rotate（20deg）;
-ms-transform:rotate（20deg）; // IE
-webkit-transform:rotate（20deg）; // Safari 和 Chrome
-moz-transform:rotate（20deg）; // Firefox
-o-transform:rotate（20deg）; // Opera
```
些代码看似比较冗余，但是很有必要，通常为了让不同的浏览器访问时达到相同的效果，都需要使用该属性对应浏览器的版本。

了上面案例中用到的Transform属性的一些变换方法外，还有一些不常用的。表12.1中列举了Transform属性所有的变换方法，读者可以自行尝试它们的使用效果。

![CSS3 Transform属性的所有变换方法](https://graphbed.qiniu.songxingguo.com/AngularJS/CSS3%20Transform%E5%B1%9E%E6%80%A7%E7%9A%84%E6%89%80%E6%9C%89%E5%8F%98%E6%8D%A2%E6%96%B9%E6%B3%95.jpg)

需要注意的是，这些变化方法可以同时作用于同一个HTML元素，最终效果是几种变化叠加后的效果。

#### CSS3中的Transition属性

12.2.1小节我们学习了CSS3的Transform属性，用于对HTML元素进行变换，但是这种变换是静态的，我们看不到任何动画效果，本小节我们开始学习Transition属性，该属性用于定义变换的过渡效果，实现网页中常见的动画。

在CSS3中有4个与Transition属性相关的属性，分别为transition-property、transition-duration、transition-timing-function和transition-delay，使用语法如下：

```
transition：[ transition-property ] || [ transition-duration ]
[ transition-timing-function ] || [ transition-delay ]
```
1. transition-property表示过渡特效将会作用于HTML元素的哪个属性，如果不指定，默认为过渡特效将作用于所有属性。例如，当我们需要做一个背景渐变特效时，可以指定transition-property的值为background。
2. transition-duration用于指定渐变所用的时间，单位为秒。
3. transition-timing-function用于指定渐变的方式，例如平滑过渡、淡入淡出等，有几个固定的效果可供我们选择，如下表。

   ![transition-timing-function属性值](https://graphbed.qiniu.songxingguo.com/AngularJS/transition-timing-function%E5%B1%9E%E6%80%A7%E5%80%BC.jpg)

   当然我们可以使用cubic-bezier（n,n,n,n）方法自定义过渡效果，其中每个参数取值范围为0～1。

4. transition-delay表示过渡效果开始前等待的时间，单位是秒（s）。

在CSS3中一般通过鼠标事件或者鼠标状态定义动画，通常我们可以使用CSS中的伪类或者用JavaScript修改元素的样式属性，或者追加/删除CSS样式来触发定义的CSS3动画。在CSS中执行动画的伪类如下表所示。

![CSS中执行动画的伪类](https://graphbed.qiniu.songxingguo.com/AngularJS/CSS%E4%B8%AD%E6%89%A7%E8%A1%8C%E5%8A%A8%E7%94%BB%E7%9A%84%E4%BC%AA%E7%B1%BB.jpg)

#### CSS3中的Animation属性

前面我们学习的Transform属性和Transition属性是实现动画的基础，本小节我们要学习的Animation属性才是实现CSS3动画最重要的属性，主要用于通过若干个关键帧实现较为复杂的过渡动画效果，具体包含下面8个属性。

- animation-name，用于定义关键帧（keyframes）的名称。
- animation-duration，规定完成动画所花费的时间，默认值是0，意味着没有动画效果。
- animation-delay，可选，定义动画开始前等待的时间，以秒或毫秒计，默认值是0。- - animation-iteration-count，定义动画播放次数，如果值为infinite，就表示无限次循环播放。
- animation-direction，取值为[normal|alternate]，normal表示正常播放，alternate表示轮流反向播放。
- animation-play-state，取值为[paused|running]，paused表示动画暂停，running表示正在播放。
- animation-fill-mode，各属性值含义如下表所示。

  ![animation-fill-mode属性值](https://graphbed.qiniu.songxingguo.com/AngularJS/animation-fill-mode%E5%B1%9E%E6%80%A7%E5%80%BC.jpg)

- animation-timing-function，指定动画的过渡效果，和12.2.2小节中的transition-timing-function属性类似，取值如下表所示。

  ![transition-timing-function取值](https://graphbed.qiniu.songxingguo.com/AngularJS/transition-timing-function%E5%8F%96%E5%80%BC.jpg)

了解这些属性的含义后，我们还需要了解一下CSS3中的关键帧定义。@keyframes支持两种设置方式，分别用于简单动画和复杂动画，简单动画设置方式如下：

```css
@keyframes transparency {
  from{opacity: 1; }
  to{opacity: 0; }
}
```
如上面的代码所示，我们通过@keyframes定义关键帧名称为transparency，在简单模式下，只需要使用from和to关键字指定开始和结束状态即可。例如，上面的代码中指定了透明度从完全不透明过渡到完全透明。
对于比较复杂的动画，可以使用百分比指定动画完成过程中的状态，例如下面的代码指定了元素先向右平移再向下平移：

```css
@keyframes translate_animation {
  0% {transform: translate（0）; }
  20% {transform: translate（120px）; }
  40% {transform: translate（240px）; }
  60% {transform: translate（240px, 40px）; }
  80% {transform: translate（240px, 80px）; }
  100% {transform: translate（240px, 120px）; }
}
```
#### 常用的CSS3动画库

前面几小节我们一起学习了CSS3动画相关的属性，使用这些属性创建动画效果并不是很复杂，但是为了避免一些重复的工作，我们可以使用一些常用的CSS3预设动画库，使用这些动画库能完成我们日常所需90%的动画效果。下面介绍几款比较常用的动画库。

1. Animate.css

    Animate.css是使用CSS3编写的跨浏览器的预设动画库，提供了几十种常用的内置动画，我们可以从官方网站获取库文件，地址为http://daneden.github.io/animate.css/。它的使用方式很简单，我们只需要把库文件下载到本地，然后引入到页面中即可：

    ```html
    <head>
    <link rel="stylesheet" href="animate.min.css">
    </head>
    ```
    或者通过内容分发网络（CDN）引入到页面中，例如：

    ```html
    <head>
    <link rel="stylesheet" href="http://s.mlcdn.co/animate.css">
    </head>
    ```
    接着将预定义的CSS样式应用到HTML元素中，例如：

    ```html
    <div id="box" class="animated infinite fadeOutRight"></div>
    ```
2. Hover.css

    Hover.css与上面提到的Animate.css有些不同，主要用于实现鼠标悬停时的动画效果，其中包括2D过渡、背景过渡、图标变换、边框过渡、阴影过渡等效果，该动画库的官方网站地址为http://ianlunn.github.io/Hover/，使用方式和Animate.css类似，读者可以从官方网站下载Hover.css库，然后在页面中引入：

    ```html
    <head>
    <link rel="stylesheet" href="hover.css">
    </head>
    ```
    接着就可以把动画库内置的CSS样式应用到HTML元素中了，例如：

    ```html
    <a href="#" class="hvr-grow">Add to Basket</a>
    ```
除了上面两款常用的CSS3动画库外，比较优秀的还有Magic CSS3 Animation（官方网站地址为http://www.minimamente.com/example/magic_animations/ ）、Typebase.css（官方网站地址为http://devinhunt.github.io/typebase.css/ ）、Hint.css（官方网站地址为http://kushagragour.in/ lab/hint/ ）等。这些动画库的使用方式大同小异，有兴趣的读者可以自行研究。

#### AngularJS动画

前面两节我们学习了如何使用JavaScript操作CSS实现动画，以及如何使用CSS3内置的动画属性实现动画，这些都是AngularJS动画的基础，本节我们就来学习一下如何在AngularJS应用中实现动画效果。
动画并不是AngularJS框架的核心部分，它作为一个单独的模块封装在angular-animate.js文件中，我们要使用AngularJS动画首先要把模块文件引入到自己的页面中，

```html
<script src="/angular-1.5.5/angular.js"></script>
<script src="/angular-1.5.5/angular-animate.js"></script>
```
接着要在自定义的模块中添加ngAnimate模块依赖，代码如下：

```js
var myApp = angular.module（'myApp',['ngAnimate']）;
```
接下来就可以使用ngAnimate模块了。可以在AngularJS应用中通过两种方式实现动画效果，即前面两节学习的JavaScript动画和CSS3动画。

#### 基于事件驱动的CSS3动画

在使用ngAnimate模块之前，我们首先需要知道它的作用，**ngAnimate模块本身并不能使HTML元素产生动画，但是它可以动态地添加或移除CSS样式** ，ngAnimate模块还会监听事件，例如HTML元素隐藏与显示事件，当事件发生时ngAnimate就会使用预定义的CSS样式来设置HTML元素的动画。

AngularJS中能够用于添加或移除CSS样式的指令如表12.6所示。

![用于添加或移除CSS样式的指令](https://graphbed.qiniu.songxingguo.com/AngularJs/%E7%94%A8%E4%BA%8E%E6%B7%BB%E5%8A%A0%E6%88%96%E7%A7%BB%E9%99%A4CSS%E6%A0%B7%E5%BC%8F%E7%9A%84%E6%8C%87%E4%BB%A4.jpg)

以ng-if指令为例，当HTML元素显示时会触发enter事件，AngularJS会为HTML元素的class属性添加上ng-enter样式，当HTML元素显示完毕后AngularJS会为该元素的class标记ng-enter-active属性，其他指令的情况也与此类似，因此我们要做的就是自定义ng-enter和ng-enter-active样式的内容。

#### AngularJS中的JavaScript动画

使用JavaScript能够实现比CSS3更复杂、更绚丽的动画效果，例如我们要实现一个复杂的网页游戏特效，通过监听键盘事件触发，可以考虑使用JavaScript实现。AngularJS框架也提供动画的JavaScript实现方式。

当我们把ngAnimate模块添加依赖到模块中时，在模块实例中增加了一个animation()方法，这意味着我们可以通过该方法来注册一个JavaScript动画。该方法可以接收两个参数，分别为className和animationFunction，第一个参数className为string类型，用于指定应用动画的CSS样式名称，第二个参数animationFunction是一个方法，可以是一个匿名方法，用于执行动画效果，需要接收两个参数element和done。element参数为一个jqLite或jQuery对象，done参数是一个方法，我们需要在动画代码执行后显式地去调一次done()方法。animation()方法的使用方法如下：

```js
var app = angular.module（'myApp', ['ngAnimate']）;
app.animation（'.fade', function() {
  return {
    enter: function（element, done） {
      $（element）.fadeIn（1000, function() {
        done();
      }）;
    },
    leave: function（element, done） {
      $（element）.fadeOut（1000, function() {
        done();
      }）;
    },
    move: function（element, done） {
      $（element）.slideDown（500, function() {
        done();
      }）;
    }
  }
}）
```
如上面的代码所示，AngularJS中的JavaScript动画就是通过模块实例的animation()方法来实现的。

#### ngView视图切换动画案例

在本书的第6章中我们已经学习了AngularJS多视图与路由机制，通过路由我们可以实现多个视图之间的切换，但是我们的视图切换比较单调，没有任何过渡效果。本章的前面几节我们学过了AngularJS的动画机制。

#### ngAnimate与CSS3动画库整合

AngularJS框架的ngAnimate模块简化了Web动画开发的过程，但是我们依然需要自己使用JavaScript或CSS3实现过渡效果，前面我们已经学习了一些CSS3预设动画库，例如Animate. css就是目前应用非常广泛的一款。非常庆幸的是，我们可以将ngAnimate动画模块和一些常用的CSS3动画库整合起来，这样就能够使用一些预设的CSS3动画样式来简化AngularJS应用动画的开发过程。接下来将会以一个备忘录程序的案例说明ngAnimate模块与Animate.css动画库的整合方法。

#### ngFx动画扩展库

ngFx是基于Animate.css实现的一款开源AngularJS动画模块，其源码托管在Github上，源码地址为https://github.com/AngularClass/ng-fx，使用它可以更加方便地创建Web动画效果。我们可以使用NPM工具获取ngFx动画模块的Release版本。关于NPM，我们目前只需要知道它是一款JavaScript包管理工具，一些常用的前端库Release版本都可以通过NPM工具从互联网中获取（例如jQuery、React等）。NPM包管理工具的安装与使用在后面章节中将会有所介绍，它运行在Linux/UNIX终端或Windows控制台中，我们可以使用如下命令获取ngFx动画模块相关的库文件：

```
npm install --save ng-fx
```
## Cookie读写

无论是Web前端还是服务端开发人员，对Cookie应该都不会陌生，本章我们开始学习是如何在AngularJS应用中操作Cookie，在此之前我们需要先来了解一下什么是Cookie，以及如何通过原生的JavaScript操作Cookie。

### Cookie简介

Cookie的中文释义为“小型文本文件”或“小甜饼”，指某些网站为了辨别用户身份而储存在用户本地的数据。这些数据通常是经过加密的，总是保存在客户端。按照在客户端中的存储位置，Cookie可分为内存Cookie和硬盘Cookie。内存Cookie由浏览器维护，保存在内存中，浏览器关闭后消失，存活时间是比较短暂的。硬盘Cookie保存在硬盘里，有一个过期时间，除非用户手工清理或到了过期时间，否则硬盘Cookie不会被删除，其存在时间是长期的。所以，按存在时间，Cookie可分为非持久Cookie和持久Cookie。

Cookie技术的出现，在某种意义上将是为了弥补HTTP协议的不足，因为HTTP协议是无状态的，即服务器不知道用户上一次做了什么，这严重阻碍了交互式Web应用程序的实现。举一个典型的例子，大家应该都有过网上购物的经历，我们浏览多个页面后选中几款商品，到最后一起结账时，由于HTTP的无状态性不通过额外的手段，服务器并不知道我们前面选择了什么商品。目前的各电商网站的做法是模拟现实世界中的购物车，我们可以把要购买的商品添加到购物车中，将购物车中的商品信息提交到服务器端处理。这种虚拟的购物车就是通过Cookie来实现的。

虽然Cookie技术被浏览器广泛使用，但是也存在一定的缺陷，具体表现为以下几点：

- Cookie会被附加在每个HTTP请求中，所以无形中增加服务端请求的数据量。
- 由于在HTTP请求中的Cookie是明文传递的，因此安全性存在一定的问题。
- Cookie的大小一般限制在4KB左右，对于复杂的存储需求来说总是不够用的。

需要注意的是，如果在一台计算机中安装多个浏览器，那么每个浏览器都会以独立的空间存放Cookie。因为Cookie中不但可以确认用户信息，还能包含计算机和浏览器的信息，所以一个用户使用不同的浏览器或者用不同的计算机，都会得到不同的Cookie信息，另一方面，对于在同一台计算机上使用同一浏览器的多用户群，Cookie不会区分他们的身份，除非他们使用不同的用户名登录。

### 在JavaScript中操作Cookie

前面我们了解了什么是Cookie以及Cookie的作用，本节我们就来学习如何使用原生的JavaScript操作Cookie，主要包括如何创建Cookie、读取Cookie信息以及设置Cookie有效期。
我们首先学习如何通过JavaScript创建Cookie，过程很简单，需要使用Document对象的cookie属性，使用方式如下：

```js
document.cookie = 'username=Smith';
```
需要注意的是，Cookie是以key-value形式进行存储的。上面代码中的‘username’表示Cookie名称，‘Smith’表示这个名称对应的值，如果Cookie名称并不存在，就创建一个新的Cookie，如果存在就修改这个Cookie名称对应的值。如果需要创建多个Cookie，就将这些属性用分号隔开即可，例如：

```js
document.cookie = 'username=Smith;password=123456a';
```
接着我们需要了解如何读取Cookie信息，这个过程稍微复杂些，因为我们通过document. cookie属性获得了所有的Cookie信息，内容如下：

```js
username=Smith;password=123456a
```
这些属性通过分号（;）进行分割，如果想要获取具体的Key对应的Value值，就需要手动对上面的字符串进行拆分。

#### 在AngularJS中操作Cookie

前面我们学习了使用JavaScript操作Cookie，相对来讲还是比较复杂的，有些操作需要封装一个方法来简化操作。为了简化Cookie操作，AngularJS封装了一个单独的ngCookies模块，该模块代码放在单独的angular-cookies.js文件中，因此我们在使用ngCookies模块之前要先将它引入到我们的页面中，例如：

```html
<script src="/angular-1.5.5/angular-cookies.js"></script>
```
接着需要在自定义模块中添加ngCookies模块依赖，代码如下：

```js
var cookieMod = angular.module（"cookieMod",['ngCookies']）;
```
AngularJS提供的与Cookie操作相关的API有$cookiesProvider、$cookies、$cookieStore。其中，$cookiesProvider用于对$cookies服务的默认行为进行配置，主要有以下几个属性：

- path：字符串类型，Cookie只在这个路径及其子路径可用。默认情况下，这个值将会是出现在`<base>`标签上的网址路径。
- domain：字符串类型，Cookie只在这个域及其子域可用。为了安全问题，如果当前域不是需求域或者其子域，那么用户代理不会接受Cookie。
- expires：字符串类型，过期日期。“Wdy, DD Mon YYYY HH:MM:SS GMT”格式的字符串或者一个日期对象表示Cookie将在这个确切日期/时间过期。
- secure：布尔类型，该Cookie将只在安全连接中被提供。

我们可以调用AngularJS模块实例的config()方法对$cookies进行配置，例如：

```js
angular.module（'myApp', []）.config（["$cookiesProvider",cookiesFn]）;
function cookiesFn（$cookiesProvider） {
  $cookiesProvider.defaults = {
    path: 	yourPath,
    domain: yourDomain,
    expires: expireDate,
    secure: true/false
  };
}
```

接下来我们再来了解一下$cookies服务，它提供了浏览器Cookie读写的一些方法，具体如下：
- get(key)，根据Cookie名称返回对应的Cookie值。
- getObject(key)，根据key获取对应Cookie的反序列化值。
- getAll()，返回所有的Cookie键值对格式对象。
- put(key,value,[options])，根据给定的Cookie名称创建Cookie。value为Cookie的值，options为可选参数，用于设置Cookie的一些额外信息，例如Cookie的有效日期。
- putObject(key,value,[options])，根据Cookie名称创建一个Cookie。
- remove(key,[options])，根据Cookie名称删除Cookie信息。

最后我们了解一下使用较为广泛的$cookieStore服务。我们可以使用它将JavaScript对象保存到Cookie中，或从Cookie中读取JavaScript对象。被存入和取出的对象将自动调用angular. toJson()/angular.fromJson()方法进行序列化/反序列化。由于它是对JavaScript对象直接进行操作，因此使用起来非常方便，应用也比较广泛。

$cookieStore服务提供了3个方法对Cookie进行操作，分别为get、put、remove方法。

- get(key)，根据Cookie名称获取Cookie值。
- put(key,value)，根据指定的Cookie名称和值创建Cookie。
- remove(key)，删除指定的Cookie信息。

## Promise

Promise代表未来某个将要发生的事件（通常是一个异步操作）。它的好处在于，有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象还提供了一整套完整的接口，使得可以更加容易地控制异步操作。Promise的概念最初被提出是在E语言中，如今已经成为ES6规范之一，取得JavaScript原生支持。Promise作为一个异步操作的结果，主要通过then()方法与其交互，then()方法接收一个回调方法作为参数，这个回调方法接收执行成功的返回值或执行失败的错误原因，错误原因一般是Error对象。需要注意的是，then()方法执行的返回值是一个Promise对象，而then()方法接收回调方法的返回值则可以是任意JavaScript对象（包括Promise对象），基于这种机制，Promise对象就实现了链式调用效果。

ES6标准中的Promise对象是一个构造方法，用来生成Promise实例，例如：

```js
var promise = new Promise（function（resolve, reject） {
  if （/* 异步操作成功 */）{
    resolve（value）;
  } else {
    reject（error）;
  }
}）;
promise.then（function（value） {
  // success
}, function（value） {
  // failure
}）;
```
如上面的代码所示，Promise构造方法接收一个方法作为参数，该方法具有两个参数，分别为resolve()方法和reject()方法，如果异步操作成功就调用resolve()方法，否则调用reject()方法。Promise实例生成以后，可以调用Promise实例的then()方法分别指定resolve()方法和reject()方法的回调方法。

### AngularJS中的Promise机制

AngularJS也提供了对Promise机制的支持，主要通过内置的$q服务来实现。为了说明如何使用$q服务实现Promise链式调用。

如果需要让add()方法支持Promise链式调用，需要借助AngularJS的内置服务$q，我们调用$q.defer()方法获取了一个Deferred对象。Deferred对象的用途是与Promise对象相关联，提供一些API处理成功或失败的任务状态，具有如下方法和属性：

- resolve(value)：用于标识当前任务状态成功，可以继续链式调用then方法。
- reject(reason)：用于标识当前任务状态失败，会跳转到catch方法，类似于编程语言中的try…catch语法。
- notify(value)：用于更新Promise的执行状态，在Promise被处理或被拒绝之前可能会被调用多次。
- promise - {Promise}：和Deferred对象相关联的Promise对象。

Promise对象具有then、catch、finally三个方法，每个方法的调用都返回一个Promise对象，这为链式调用提供了支撑，finally方法和编程语言中的try…catch…finally语句类似，一定会被执行到。

AngularJS的Promise机制主要用在和Web应用服务器通信中，当我们向服务器发送一个请求时，因为网络原因可能并不会立刻获得服务器的响应，此时可以使用Promise机制绑定成功和失败时的回调方法，当服务器有响应时，对应的回调方法会自动被调用。下节我们开始学习AngularJS与Web服务器交互相关的API。

#### AngularJS请求Web服务

Web应用的页面数据通常都不会是一成不变的，这些数据一般保存在服务器端，我们需要向Web服务器发送HTTP请求，服务器接收到请求后将数据以特定的格式返回给客户端应用，返回数据格式通常为XML或JSON，然后我们就可以对返回的数据进行处理，将它们动态显示到页面中，当服务器端数据发生变化时，页面显示的内容也就跟着发生变化。

在AngularJS应用中，主要通过XHR和JSONP两种方式向服务器端发送HTTP请求。读者应该对XHR比较熟悉，它是利用浏览器内置的XMLHttpRequest对象向服务器发送Ajax请求。JSONP是一种可以绕过浏览器的安全限制、从不同域请求数据的方法，原理是利用`<script>`标签的src属性发起一个GET请求，例如：

```js
<script src="GetUserInfo.do?UserId=41102"></script>
```
该标签会向服务端发起HTTP请求，请求路径为GetUserInfo.do，请求参数为UserId=41102，JSONP方式会生成一个`<script>`标签插入到DOM中，通过src属性指定请求的地址。

#### $http服务

AngularJS与Web应用服务器交互的操作都封装在$http服务中，使用$http服务发送请求，我们需要一个配置对象，使用方式如下：

```js
$http（{
  method:'post',
  url:'loginAction.do'
}）
```
请求配置对象用于描述请求方式以及请求如何被处理，下面对它的属性进行详细说明：

- method - {string}：用于描述HTTP请求方式，例如'GET'、'POST'等。
- url - {string}：请求资源的路径。
- params - {Object.<string|Object>}：请求参数。
- data - {string|Object}：请求数据。
- headers - {Object}：HTTP请求头字符串或返回HTTP请求头的方法，如果方法的返回值为null，请求头将不会被发送。
- eventHandlers - {Object}：用于指定一个XMLHttpRequest对象的事件监听方法。
- uploadEventHandlers - {Object}：用于绑定文件上传事件监听方法。
- xsrfHeaderName - {string}：用于设置一个防止XSRF（跨站伪造请求）的Token。
- xsrfCookieName - {string}：包含防XSRF（跨站伪造请求）Token的Cookie名称。
- transformRequest - {function（data, headersGetter）|Array.<function（data, headersGetter）>}：指定转换方法或转换方法数组，转换方法接收HTTP请求头和请求正文返回它们的转换（通常是反序列化）版本。
- paramSerializer - {string|function（Object<string,string>）:string}：指定一个方法名称或方法，该方法用于将请求对象转换为一个字符串形式的请求参数，如果指定为方法名称，就必须在$injector容器中注册。
- cache - {boolean|Object}：布尔值或通过$cacheFactory创建的对象，用于指定是否禁用HTTP响应缓存。
- timeout - {number|Promise}：用于指定超时的毫秒数。
- withCredentials - {boolean}：是否为XHR对象设置withCredentials标志。
- responseType - {string}：参考XMLHttpRequest.responseType。

14.1节我们学习了AngularJS的Promise机制，$http服务就是使用这种机制实现的，$http服务的调用会返回一个Promise对象，我们可以调用Promise对象的success方法处理成功的请求，调用error方法处理失败的请求，例如：

```js
$http（{method:'post',url:'loginAction.do'
}）.success（function（data,status,headers,config）{
  //正常响应
}）.error（function（data,status,headers,config）{
  //错误响应
}）;
```
常状态码在200～299之间时响应是成功的，Promise对象的success()和error()方法接收一个方法作为参数，接收的方法具有如下几个参数：
- data：服务器端返回的响应数据，通常为json或xml格式。
- status：服务器响应状态。
- headers：服务器端的响应头。
- config：发起HTTP请求的配置对象信息。
- statusText：HTTP响应状态码对应的文本描述。

#### 使用Express构建RESTful服务

在上一个案例中，我们通过一个JSON文件模拟服务端的响应数据，本小节就来介绍如何搭建真实的Web服务，需要使用到的工具是Express。它是一款基于Node.js的Web服务端应用开发框架，提供了一系列强大的特性（帮助我们创建各种Web应用）和丰富的HTTP工具。Express框架核心特性如下：

- 可以设置中间件来响应HTTP请求。
- 定义了路由表用于执行不同的HTTP请求动作。
- 可以通过向模板传递参数来动态渲染HTML页面。

接下来我们就开始学习如何使用Express构建一个Web服务。Express应用的运行需要有Node.js的支持，本书假设读者对Node.js有一定了解并且机器上已经安装了Node.js及包管理工具NPM。通过NPM安装Express工具包的安装方式有两种，即全局安装和局部安装。这个过程比较简单，打开Windows控制台（或者Linux终端），在全局安装方式下执行下面两行命令：

```
npm install -gd express
npm install -gd express-generator
```
以局部安装方式安装后只能在某个项目中使用Express框架。首先需要进入项目的根目录，然后使用"npm install -gd express"命令进行安装，安装完毕后项目根目录下会生成一个node_modules目录，该目录下为Express库文件及其所依赖的库文件。

#### $resource服务

前面我们学习了$http服务，本小节我们再来了解一个用于和RESTful API交互的服务$resource。它在$http服务的基础上做了一层封装，为RESTful API的CRUD（增删改查）操作提供了方便。$resource服务是在ngResource模块中定义的。ngResource模块的代码封装在一个单独的angular-resource.js文件中，为了使用$resource服务，首先需要通过`<script>`标签将angular-resource.js文件引入到页面中，例如：

```html
<script src="/angular-1.5.5/angular-resource.js"></script>
```
接着需要在自定义的模块中添加ngResource模块依赖：

```js
var app = angular.module（"app", ['ngResource']）;
```
然后我们就可以将$resource服务注入控制器中使用了。$resource服务虽然可以方便AngularJS应用对RESTful API进行CRUD操作，但是它对RESTful API的设计也有一定的要求，请求的URL必须满足如表14.1所示的格式要求。

![URL格式要求](https://graphbed.qiniu.songxingguo.com/AngularJs/URL%E6%A0%BC%E5%BC%8F%E8%A6%81%E6%B1%82.jpg)

$resource服务返回一个对象，该对象是与RESTful的后端服务进行交互的接口，具有如下几个方法：

- get()方法用于通过GET方式获取单个资源，可以以对象的形参传入多个请求参数。
- query()方法用于向指定URL发送一个GET请求，并期望返回一个JSON格式的资源对象集合。
- save()方法向指定URL发送一个POST请求，并用数据体来生成请求体，用于在服务器上生成一个新资源。
- delete()方法会向指定URL发送一个DELETE请求，并用数据体来生成请求体，被用来在服务器上删除一个实例。
- remove()方法的作用和delete()方法完全相同。由于delete是JavaScript中的一个关键字，在IE浏览器下使用delete()方法，可能会带来一些意想不到的问题，因此通常使用remove()方法取代delete()方法。

#### AngularJS文件上传

文件上传功能在Web应用中相当常见，本节介绍如何通过AngularJS实现文件上传功能。传统浏览器并不支持Ajax方式文件上传，文件上传通常都是通过表单提交的方式，所以文件上传通常会伴随着页面的跳转。随着HTML5的诞生，一些支持HTML5的浏览器（例如Chrome、Firefox等）提供了内置的FormData对象，我们可以使用FormData对象以Ajax方式提交文件。AngularJS应用的文件上传功能正是基于这种方式实现的。

打开Windows控制台（或者Linux终端），进入项目根目录，也就是restfulApi目录，输入如下命令：

```
npm install multiparty
```
安装完毕后，会在node_modules目录下生成一个multiparty目录，存放multiparty的所有库文件及其依赖的库文件。
我们将客户端上传的文件保存在restfulApi\files目录下。关于multiparty的使用方法，这里不做过多介绍，读者可以通过下面的地址了解到更多multiparty库的API及使用方法：

https://www.npmjs.com/package/multiparty
https://github.com/andrewrk/node-multiparty

#### Angular File Upload模块介绍

Angular File Upload是一款轻量级的AngularJS第三方文件上传模块，支持拖曳式文件上传、上传进度处理、校验过滤器及文件上传队列，支持原生的HTML5方式上传，但是对旧版本的浏览器采用iframe方式上传，下面是该模块的所有特性：

- 支持上传进度控制，支持上传进度，在上传的时候可以取消或者中止，支持文件拖曳（HTML5）、目录拖曳（Weikit）、CORS、PUT（HTML5）/POST方法。
- 支持使用Flash polyfill FileAPI跨浏览器上传（HTML5和non-HTML5），允许客户端在上传之前验证或者修改文件。
- 轻量级，使用常规的$http来上传（支持非HTML5浏览器），所以提供所有AngularJS $http功能。
- 将非HTML5方式上传代码封装在一个单独的文件中，这意味着当使用HTML5方式上传时，页面中无须引入额外的代码。

接下来我们就开始学习Angular File Upload模块的使用方法。在此之前首先需要获取Angular File Upload模块的库文件，可以使用Node包管理工具（NPM）获取，打开控制台（或Linux终端）输入如下命令：

```
npm install angular-file-upload
```
除此之外，FileUploader对象还提供了很多文件操作的方法以及监控文件上传状态的方法，读者可以参考Angular File Upload官方案例及案例源码：

官方案例：http://nervgh.github.io/pages/angular-file-upload/examples/simple/
案例源码：https://github.com/nervgh/angular-file-upload

从代码上来看，Angular File Upload模块大大简化了AngularJS应用文件上传过程，所以在实际项目中，为了降低开发难度、提高开发效率，尽量使用第三方文件上传模块。

与Angular File Upload模块类似的文件上传模块还有以下几款：

- ngFile Upload模块

  官方案例：https://angular-file-upload.appspot.com/
  源码地址：http://github.com/danialfarid/ng-file-upload
  
- Flow.js模块

  官方案例：http://flowjs.github.io/ng-flow/
  源码地址：https://github.com/flowjs/ng-flow

- ngUpload模块

  官方网站：http://twilson63.github.io/ngUpload/
  源码地址：https://github.com/twilson63/ngUpload

## AngularUI

AngularUI是一个专门提供AngularJS框架相关的一些优秀扩展模块、IDE插件的团队，例如我们前面学习AngularJS路由与多视图章节使用的ui-router库就是由AngularUI团队提供，AngularUI官方地址为https://angular-ui.github.io/ 。

为了使一些常用的开发工具（例如TextMate、Atom、Brackets等）支持AngularJS关键字补全功能，我们可以在开发工具中安装AngularUI提供的对应编辑器的插件，例如本书使用的Brackets工具，可以安装对应AngularJS-brackets插件，Github地址为：

https://github.com/angular-ui/AngularJS-brackets

我们可以选择在线安装或者从上面的地址下载插件离线安装包进行安装。
AngularUI还提供一些界面相关的模块及指令，例如日历、表格、编辑器等，本章会介绍其中比较常用的一些模块。

### UI Bootstrap

相信大家对Bootstrap已经不陌生了，Bootstrap是Twitter公司推出的很受欢迎的Web前端开发的框架，主要目的是简化Web前端布局及界面美化，即使不懂美工知识的后端程序员也能够使用Bootstrap框架开发出美观的界面，所以深受后端开发人员的喜爱。

实际上Bootstrap框架可以分为两个部分：其中一个部分是我们前面一直在使用的CSS样式库，例如希望DIV的外观看起来像按钮一样，将Bootstrap内置的按钮样式应用到DIV上即可；另一部分是和用户交互的JavaScript库，例如单击下拉菜单后展开子菜单，这部分是由JavaScript控制的。本节所要介绍的UI Bootstrap模块就是由AngularUI团队把Bootstrap与用户交互的这部分封装成模块，使它能够和AngularJS应用更方便地集成。
接下来我们就开始学习UI Bootstrap模块的使用，和其他第三方模块一样，我们首先需要获取UI Bootstrap模块的库文件，读者可以从官方网站中下载：

https://angular-ui.github.io/bootstrap/

进入主页后，单击Download按钮即可下载UI Bootstrap模块的最新版本。另一种方式是通过NPM（Node.js包管理工具）获取，打开Windows控制台（或Linux终端），输入如下命令：

```
npm install angular-ui-bootstrap
​````
使用UI Bootstrap模块我们首先需要使用`<script>`标签将库文件引入到页面中：
  
​```html
<script src="/angular-extends/ui-bootstrap/ui-bootstrap.js"></script>
<script src="/angular-extends/ui-bootstrap/ui-bootstrap-tpls.js">
</script>
```
需要注意的是，ui-bootstrap-tpls.js文件中为UI Bootstrap模块中定义的指令模板，我们必须将这两个文件同时引入到页面中，既然是使用Bootstrap，那么Bootstrap的CSS样式文件当然不能少，同样需要引入到页面中，代码如下：

```html
<link rel="stylesheet" href="/bootstrap/css/bootstrap.css"/>
<link rel="stylesheet" href="/bootstrap/css/bootstrap-theme.css"/>
```
到目前为止，我们的准备工作已经完成，接下来分几小节介绍Bootstrap中常用的一些组件并配合UI Bootstrap模块实现和用户交互。

#### 警告框案例

Bootstrap警告框主要用于向用户提示一些信息，可以根据信息的重要程度设置不同的颜色，单击右侧的按钮可以关闭警告框。

原生的HTML中提供了复选框控件，只需要将<input>标签的type属性设置为checkbox即可。但是HTML中的复选框样式过于单一，本小节我们就利用UI Bootstrap模块，使用按钮来模拟HTML中的复选框，使它和HTML中的复选框具有相同的行为。

#### 日历控件案例

日历控件在Web应用中非常常见，当我们需要用户填写日期时，为了简化操作同时对用户输入的日期格式进行控制，可以提供一个日历控件供用户选择日期，这样可以在很大程度上提升用户体验。

#### 模态对话框案例

在日常浏览网页时，可能会遇到登录或注册的情况，一些网站的做法是弹出一个对话框供用户做登录或注册操作，操作完毕后关闭对话框，这样用户可以依然停留在之前的页面，UI Bootstrap模块也提供了这样的模块对话框组件。

为了创建对话框实例，我们需要用到UI Bootstrap模块内置的$uibModal，通过$uibModal.open()方法创建一个对话框实例对象，并显示该对话框，$uibModal.open()方法接收一个对象，该对象用于对对话框的行为进行描述，本案例中的几个参数含义如下：

- animation：布尔类型，表示打开或关闭对话框时是否伴随动画效果。
- templateUrl：字符串，对话框模板ID，在本案例中通过ng-template指令定义的对话框模板ID为myModalContent.html。
- controller：字符串，对话框控制器名称。
- size：字符串，用于设置对话框的尺寸，lg表示大尺寸，sm表示小尺寸。
- resolve：对象，该属性和Router中的resolve属性作用一样，用于向对话框作用域传递数据。

$uibModal.open()方法返回一个对话框实例，我们可以使用它来处理对话框关闭或取消事件。

#### 下拉菜单案例

下拉菜单在Web应用中也非常常见，本小节我们再来学习一下如何使用UI Bootstrap模块实现下拉菜单效果并响应菜单的单击事。

### UI Ace

Ace是一个开源、独立的、基于浏览器的代码编辑器，可以嵌入到任何Web页面或JavaScript应用程序中。Ace支持60多种语言语法高亮，并能够处理多达400万行代码的大型文档。Ace开发团队称，Ace在性能和功能上可以媲美本地代码编辑器（如Sublime Text、TextMate和Vim等）。

>Ace的官方网站地址为https://ace.c9.io/。
Ace源码托管地址为https://github.com/ajaxorg/ace。

UI Ace是AngularUI团队在Ace的基础上开发的AngularJS模块，宗旨是能够在AngularJS中通过指令的形式便捷地将Ace代码编辑器集成到AngularJS应用中。在一些博客应用或者论坛系统中代码编辑器应用比较广泛，本节我们就来学习如何通过UI Ace模块来实现Ace代码编辑器。

UI Ace官方网站地址为http://angular-ui.github.io/ui-ace/
UI Ace源码托管地址为https://github.com/angular-ui/ui-ace。

正如其他AngularJS第三方模块一样，使用UI Ace模块，我们首先需要获取UI Ace模块库文件，需要用到另外一款基于Node.js的Web前端包管理工具bower。关于bower工具，后面会有详细介绍，这里只简单介绍一下安装与使用。首先打开Windows控制台（或者Linux/UNIX等系统终端），输入如下命令安装：

```
npm install -g bower
```
上面的命令会全局安装bower工具，安装完毕后就可以在控制台中运行bower命令了，接下来我们就来使用bower工具获取UI Ace模块库文件，输入如下命令：

```
bower install angular-ui-ace
```
### UI Grid

UI Grid是AngularUI团队基于原生的AngularJS实现的一款强大的表格相关模块，并不依赖于其他第三方库，能够高效处理超过1万行的数据，而且内置排序功能，能够简化应用中表格的实现过程。本节我们就来学习UI Grid模块的使用。

>UI Grid模块官方网站地址为http://ui-grid.info/。
UI Grid模块源码托管地址为https://github.com/angular-ui/ui-grid。

我们首先需要获取UI Grid模块的库文件，可以使用NPM进行安装，在控制台（或终端）中输入如下命令：

```
npm install angular-ui-grid
```
首先需要在页面中引入UI Grid模块的库文件和CSS样式文件，例如：

```html
<link rel="stylesheet" href="/angular-extends/ui-grid/ui-grid.css" />
<script src="/angular-extends/ui-grid/ui-grid.js"></script>
```
然后需要在自定义模块中添加UI Gird模块依赖，例如：

```js
var myApp = angular.module（'myApp',['ui.grid']）;
```
### UI Date

在前面介绍UI Bootstrap模块使用时，我们学习了UI Bootstrap模块中日历控件的使用，本节将要介绍如何使用UI Date模块创建日历控件，它是jQuery UI Datepicker的AngularJS版本。接下来就开始学习UI Date模块的使用。

> UI Date官方演示案例地址为https://angular-ui.github.io/ui-date/。
UI Date源码地址为https://github.com/angular-ui/ui-date。

首先需要获取UI Date模块库文件及它所依赖的jQuery UI库文件，我们可以使用bower包管理工具获取。打开Windows控制台（或Linux/UNIX终端），输入如下命令：

```
bower install angular-ui-date
```
####　UI Select

UI Select模块是Select2/Selectize的AngularJS实现，Select2是一款功能强大的可定制的下拉框样式库，支持搜索、标签、远程数据集、无限滚动等特性，而Selectize是用于处理下拉框事件的JavaScript库，这两者通常会结合起来使用，本节将会介绍UI Select模块的使用。

> Select2官方地址为https://select2.github.io/。
Selectize的官方地址为http://selectize.github.io/selectize.js/。
UI Select的官方地址为http://angular-ui.github.io/ui-select/。

首先需要获取UI Select模块库文件，可以通过bower包管理工具获取，输入如下命令：

```
bower install angular-ui-select
```
## AngularJS精华扩展

上一章中我们学习了AngularUI中一些比较常用的扩展模块，这些模块功能比较强大，能够大大简化AngularJS应用的开发过程，所以我们尽量合理利用这些第三方扩展模块，避免重新发明轮子。本章再向读者介绍几款功能强大的AngularJS扩展。

### 利用Angular Chart生成图表

在应用中为了直观地对数据进行统计分析，通常会将数据以图表的形式展现给用户。Angular Chart是一款基于Chart.js功能强大的图表生成模块，使用Angular Chart我们可以将一组数据快速地生成曲线图、柱状图、饼状图等几何图形，本节我们就来学习Angular Chart模块的使用。

> Char.js官方网站为http://www.chartjs.org/。
Angular Chart官方网站为https://jtblin.github.io/angular-chart.js/。
Angular Chart源码地址为https://github.com/jtblin/angular-chart.js。

首先需要获取Angular Chart模块库文件，我们可以通过NPM（Node.js包管理工具）获取。打开Windows控制台（或Linux/UNIX终端），输入下面的命令获取：

```
npm install --save angular-chart.js
```
### 利用Videogular实现播放器

Videogular是基于HTML5实现的AngularJS播放器模块，我们可以使用它实现Web视频播放器，其美观程度与功能不亚于国内知名的视频网站。在写作本书时，国内大部分视频网站中的播放器基本都还是使用Flash技术实现，众所周知，一些知名的浏览器（例如Safari），在默认情况下是禁用Flash的，而新版的Chrome浏览器，对Flash插件的支持也逐渐在削弱，就Flash本身存在的缺点与安全问题来看，我们可以大胆地预言，Flash技术终究会被HTML5取代。关于HTML5的优势这里就不过多介绍了，接下来就来了解一下Videogular模块的使用。

> Videogular官方网站地址为http://www.videogular.com/。
Videogular源码地址为https://github.com/videogular/videogular。

我们首先需要获取Videogular模块库文件，读者可以使用NPM包管理工具获取，命令如下：

```
npm install videogular
```
除此之外，我们还可以获取Videogular模块的主题及插件，命令如下：

```
npm install videogular-themes-default
npm install videogular-controls
npm install videogular-buffering
npm install videogular-overlay-play
npm install videogular-poster
npm install videogular-ima-ads
npm install videogular-angulartics
npm install videogular-dash
```
### 利用Angular Masonry实现照片墙

Angular Masonry模块是Masonry库的AngularJS版本，Masonry是一款功能强大的JavaScript网格布局库，可以根据垂直方向上可用的空间将元素摆放在合适的位置。这种特效在互联网上随处可见，例如一些电商网站中的商品列表或者一些相册应用的照片墙。本节我们就开始学习Angular Masonry模块的使用。

> Masonry官方网站为http://masonry.desandro.com/。
Angular Masonry官方网站为http://passy.github.io/angular-masonry/。

第一步需要先获取Angular Masonry模块及其他所依赖的库文件，可以通过bower包管理工具获取，命令如下：

```
bower install angular-masonry
```
### 利用ngDialog实现对话框

上一节中我们学习了如何使用UI Bootstrap模块实现模态对话框，本节将向读者介绍另外一款对话框相关扩展ngDialog。该模块基于原生的AngularJS构建，并不依赖于第三方库，功能强大，使用也较为广泛。

>ngDialog官方网站地址为http://likeastore.github.io/ngDialog/。
ngDialog源码地址为https://github.com/likeastore/ngDialog。

接下来我们就开始学习ngDialog模块的使用，首先需要获取ngDialog模块库文件，可以通过bower包管理工具获取，命令如下：

```
bower install ng-dialog
```
首先需要在页面中引入ngDialog模块的库文件及样式文件，代码如下：

```html
<script src="/angular-extends/ng-dialog/ngDialog.js"></script>
<link rel="stylesheet" href="/angular-extends/ng-dialog/theme/ngDialog.
css"/>
<link rel="stylesheet"
href="/angular-extends/ng-dialog/theme/ngDialog-theme-default.min.css"/>
<link rel="stylesheet"
href="/angular-extends/ng-dialog/theme/ngDialog-theme-plain.css">
<link rel="stylesheet"
href="/angular-extends/ng-dialog/theme/ngDialog-custom-width.css">
```
然后需要在自定义模块中添加ngDialog模块依赖，代码如下：

```js
var myApp = angular.module（"myApp", ['ngDialog']）;
```
## 常用Web前端工具集

在前面章节中我们接触到了一些Web前端工具，例如使用基于Node.js的Express框架发布RESTful风格的Web服务，使用NPM或者Bower工具获取AngularJS第三方扩展模块等。本章我们就开始学习一些常用的Web前端工具的安装与使用。

对于一名前端开发人员来说，使用JavaScript语言最多的场景是编写和用户交互部分的代码。实际上有些编译原理基础的读者都知道，一门计算机语言可以认为是由两部分组成的，一部分是语言的语法规范，另一部分为语言的编译器或解析器。JavaScript是一门弱类型的编程语言，由JavaScript解析引擎解释执行。目前开源的JavaScript解析引擎很多，例如SpiderMonkey、Chrome V8等。

需要注意的是，JavaScript语言的应用场景并不仅仅是作为浏览器的脚本语言，在一些商业的游戏引擎中（例如Unity3D），也可以使用JavaScript语言控制游戏角色的行为，只要实现特定的运行环境，甚至可以使用JavaScript来编写桌面应用程序。

前面介绍了这么多，就是希望读者不要把JavaScript语言的应用场景局限于Web浏览器中。接下来就开始介绍本章的主角Node.js，它是基于Chrome V8引擎构建的JavaScript运行环境，如果读者是一位全栈开发工程师，对Web服务端编程语言Java或C#较为熟悉，那么理解JavaScript和Node.js的关系就比较容易，二者的关系类似于Java和JVM（或者C#和.net framework），对于纯粹的前端开发人员，只需要知道Node.js的作用是在操作系统环境下运行JavaScript代码即可。

Node.js是运行NPM、Bower等其他Web前端工具的基础，接下来我们就开始学习Node.js的安装与使用，对于Windows系统，读者可以从Node.js官方网站下载安装包，在创作本书时Node. js最新稳定版本为v6.3.1，但是官方网站中推荐的版本为v4.4.7，所以本书采用v4.4.7版本。

>Node.js官方网站地址为https://nodejs.org/。


### Npm包管理工具

Npm（Node Package Manager，Node包管理工具）的宗旨是在互联网中建立一个代码仓库，帮助成千上万的开发者查找、共享和复用JavaScript库，从某种程度上讲，它有点类似于Java世界中的Maven，接下来我们就开始学习Npm的安装与使用。

> Npm的官方网站地址为https://www.npmjs.com/。

### Bower管理工具

当一个新的Web项目开始时，我们总是很自然地去下载一些需要的JavaScript库文件，例如jQuery、Chart.js等，而一些库文件会依赖于其他库文件，当依赖的文件与项目中使用的库文件发生冲突时，又不得不通过更换库文件来解决冲突，这个过程是相当复杂的，我们必须要知道谁依赖谁，还要明确依赖库文件的版本，对开发人员来说过于烦琐。

我们在前面章节中也已经接触到Bower管理工具，它是Twitter推出的一款开源的包管理工具，基于Node.js的模块化思想把功能分散到各个模块中，让模块和模块之间存在联系，通过Bower来管理模块间的这种联系。当项目中需要某些库文件时，可以通过Bower工具来获取，它能够自动解析库文件之间的依赖关系，将依赖的其他库文件一并获取。这就解决了前面提到了库版本冲突问题。本节将详细介绍Bower包管理工具的安装与使用。

> Bower官方网站地址为https://bower.io/。
Bower源码地址为https://github.com/bower/bower。

### Gulp项目管理工具

Gulp是一个基于Node.js的流式构建工具。开发者可以使用它进行项目管理，方便地执行一些常见任务，例如CSS压缩、LESS编译、资源合并等。
Gulp本身虽然不能完成很多任务，但却拥有大量可用的插件（可以通过访问插件页面或者使用Npm搜索gulpplugin看到）。这些插件可以用来执行JSHint、编译CoffeeScript、执行Mocha测试等。Gulp特点如下：

- 使用方便，通过代码优于配置的策略，可以很方便地管理复杂的任务。
- 构建快速，通过流式操作，减少频繁的IO操作，更快地构建项目。
- 插件高质，严格的插件指导策略，可以确保插件能简单高效地工作。
- 易于学习，少量的API，掌握Gulp可以毫不费力，构建就像流管道一样，轻松、愉快。

#### 利用JSHint验证JavaScript代码

JSHint是JavaScript代码验证工具，可以检查JavaScript代码中存在的问题并提供相关的代码改进意见。对于我们的JavaScript代码，可以使用多种方式进行验证。进入JSHint首页，在首页编辑框粘贴我们的JavaScript代码，JSHint官方网站会自动检测代码中存在的问题并给出修改意见。这种方式是非常烦琐的。使用Gulp的JSHint插件的方式相对简便。接下来我们开始学习JSHint插件的安装与使用。

>JSHint官方网站地址为http://jshint.com/。

JSHint插件的安装比较简单，需要借助Npm包管理工具。进入项目根目录（本案例根目录为下载资源中的ch17\ch17_04目录），在控制台中输入如下安装命令：

```
npm install -d jshint gulp-jshint --save-dev
```
#### 压缩JavaScript代码

在前面的章节中，我们学习了不少Angular第三方模块的使用，读者可能都会注意到，每个库文件通常都会有一个以min.js结尾的库文件与之对应，例如angular-chart.min.js，这些以min.js结尾的库文件就是库文件对应的压缩版本。

对JavaScript代码进行压缩能够有效减小JavaScript文件的体积，从而降低服务器传输的流量及带宽占用。经过压缩后的JavaScript代码可读性比较差，在一定程度上能够起到保护源代码的作用，所以生产环境使用的JavaScript代码通常都是经过压缩的。
在了解完JavaScript代码压缩的必要性后，本小节我们就开始学习如何使用Gulp对JavaScript代码进行压缩。

目前能够对JavaScript代码进行压缩的工具比较多，下面向读者介绍一款基于Node.js的JavaScript代码压缩工具Uglify.js。我们先来了解一些Uglify.js的安装与使用，安装过程比较简单，需要借助Npm包管理工具，安装命令如下：

```
npm install uglify-js -gd
```
#### 使用Gulp Changed插件更新文件

在使用Gulp进行项目管理时，为了提高Gulp编译或者打包的执行速度，可以只对修改过的文件进行编译或打包，此时可以使用Gulp的gulp-changed插件，该插件会检查流中的文件是否被修改过，只有修改过的文件才会执行后续操作，这样就在很大程度上提升了任务的执行速度，接下来我们开始学习gulp-changed插件的使用。

本小节我们依然使用ch17_04作为演示项目，首先需要安装gulp-changed插件，和其他Gulp插件的安装过程类似，需要借助Npm包管理工具，安装命令如下：

```
npm install --save-dev gulp-changed
```
#### 使用Gulp Plumber插件处理异常

在17.4.4小节中我们实现了一个watch任务，该任务用于监视文件的变化，当我们对一个JavaScript文件修改保存后，Gulp就会自动调用compress任务对修改后的文件进行压缩。细心的读者可能会发现这样一个问题，当我们的JavaScript文件中存在语法错误时，Uglify.js插件会抛出一个异常，该异常会直接导致watch任务中断，迫使我们不得不重新在控制台中运行该任务。为了解决这个问题，我们可以使用Gulp Plumber插件来处理这种异常。

接下来就开始学习Gulp Plumber插件的使用。首先需要安装插件，打开控制台，进入ch17\ch17_04目录，执行如下命令安装：

```
npm install --save-dev gulp-plumber
```
#### 使用Gulp压缩图片

图片是Web应用中非常重要的资源，如果图片的质量过高，图片资源占用的数据量会比较大，就会导致网络传输速度慢，带来的效果就是页面加载速度慢，直接影响用户体验，但是如果图片质量过低又会导致图片严重失真，用户体验同样会很差，所以合理控制图片质量对Web应用来说非常重要。本小节我们就来学习如何通过Gulp对图片进行压缩。

我们需要用到另外一款名称为gulp-imagemin的插件，该插件可用于对PNG、JPEG、GIF及SVG等格式的图片资源进行压缩，下面是该插件的安装命令：

```
npm install --save-dev gulp-imagemin
```
#### 使用Gulp编译Less

Less是一门CSS预处理语言，扩充了CSS语言，增加了诸如变量、混合（mixin）、函数等功能，让CSS更易维护、方便制作主题、容易扩充。Less可以运行在Node.js或浏览器端，它的运行需要依赖Less语言编译器或解析器。
Less有两种使用方式，一种是首先将Less语法编译成常规的CSS语法，然后在浏览器中引入编译过后的CSS文件，另外一种方式是直接在浏览器中引入Less文件，然后引入Less的解析器。

> Less解析器源码地址为https://github.com/less/less.js。
Less官方网站地址为http://lesscss.org/。

我们先来了解第一种方式。在使用Less语言之前，我们首先需要安装Less编译器，需要借助Npm包管理工具，安装命令如下：

```
npm install less -gd
```
### Jasmine&Karma单元测试工具

在软件工程领域，行为驱动开发（Behavior Driven Development, BDD）是从测试驱动开发（Test Driven Development, TDD）中产生的一种软件开发过程。行为驱动开发结合了测试驱动开发在领域驱动设计和面向对象的分析与设计中的一般技术和原则，提供软件开发和管理团队共享工具来协作软件开发过程。
本节我们要学习的Jasmine就是一款针对JavaScript语言的行为驱动开发测试框架，它不依赖于浏览器、DOM以及其他JavaScript框架。因此它适用于网页、Node.js项目以及其他JavaScript运行环境。

> Jasmine官方网站地址为http://jasmine.github.io/。
Jasmine源码地址为https://github.com/jasmine/jasmine。

我们要学习的另一款单元测试工具Karma。Karma是一款基于Node.js的JavaScript测试执行过程管理工具（Test Runner），允许我们在多个真实的浏览器中执行JavaScript代码，它的宗旨是让测试驱动开发变得简单、快捷和有趣。

> Karma官方网站地址为https://karma-runner.github.io/。
Karma源码地址为https://github.com/karma-runner/karma。

Jasmine和Karma二者结合起来可以用于AngularJS代码单元测试工具，接下来我们开始学习Jasmine和Karma的安装与使用。

### AngularJS单元测试

前面我们学习了目前主流的JavaScript代码测试框架Jasmine及测试管理工具Karma的使用方法，本小节开始学习如何使用Jasmine和Karma对AngularJS代码进行单元测试。需要注意的是，由于AngularJS框架自身的原因，对象是以依赖注入的方式加载和实例化的。为了便于开发人员使用第三方测试框架（例如Jasmine）对代码进行单元测试，AngularJS官方提供了一个ngMock模块。该模块是一个单独的项目，我们可以使用ngMock模块简化AngularJS单元测试过程。
