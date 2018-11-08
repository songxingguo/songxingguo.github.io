title: 《深入浅出React和Redux》-读书笔记
author: songxingguo
tags:
  - React
  - Redux
categories:
  - 读书笔记
date: 2018-10-04 16:01:00
---
## 前言

自从jQuery问世以来，它就在网页开发领域占据统治地位，同时，还有许多MVC框架如雨后春笋般出现。但是业界也普遍发现，jQuery和各种MVC框架在开发大型复杂应用时，依然面临很多难以克服的困难。

当2014年Facebook推出React时，给整个业界带来全新的看待网页应用开发的方式，和React一同问世的Flux，也克服传统MVC框架的很多弊病。技术在不断发展，在2015年，Flux的一个变体Redux出现，进一步优化了Flux的功能。

<!-- more -->

React和Redux的结合，让网页开发的方式耳目一新。

开发者应该接触不同的开发模式，才能融会贯通，对技术有一个全面的认识，若要掌握某种技术，就要深入学习。对React和Redux的了解不要只是停留在能用的表面功夫，重要的是理解内在的原理。

读者可以在Github（网址 https://github.com/mocheng/react-and-redux ）上找到所有代码，代码按照所属章节内容组织。

## React新的前端思维方式

我们先来直观认识React，对任何一种工具，只有使用才能够熟练掌握，React也不例外。通过对React快速入手，我们会解析React的工作原理，并通过与功能相同的jQuery程序对比，从而看出React的特点。

### 初始化一个React项目

React是一个JavaScript语言的工具库，在这个JavaScript工具铺天盖地的时代，没有意外，你需要安装Node.js，React本身并不依赖于Node.js，但是我们开发中用到的诸多工具需要Node.js的支持。

在Node.js的官网（ https://nodejs.org/ ）可以找到合适的安装方式，安装Node.js的同时也就安装了npm，npm是Node.js的安装包管理工具，因为我们不可能自己开发所有功能，会大量使用现有的安装包，就需要npm的帮助。

#### create-react-app工具

React技术依赖于一个很庞大的技术栈，比如，转译JavaScript代码需要使用Babel，模块打包工具又要使用Webpack，定制build过程需要grunt或者gulp……

create-react-app是一个通过npm发布的安装包，在确认Node.js和npm安装好之后，命令行中执行下面的命令安装create-react-app：

```
npm install –-global create-react-app
```
安装过程结束之后，你的电脑中就会有create-react-app这样一个可以执行的命令，这个命令会在当前目录下创建指定参数名的应用目录。

我们在命令行中执行下面的命令：

```
create-react-app first_react_app
```
这个命令会在当前目录下创建一个名为first_react_app的目录，在这个目录中会自动添加一个应用的框架，随后我们只需要在这个框架的基础上修改文件就可以开发React应用，避免了大量的手工配置工作：

在create-react-app命令一大段文字输出之后，根据提示，输入下面的命令：

```
cd first_react_app
npm start
```
这个命令会启动一个开发模式的服务器，同时也会让你的浏览器自动打开了一个网页，指向本机地址http://localhost：3000/ ，显示界面如下图所示。

![由create-react-app创造的React应用界面](https://graphbed.qiniu.songxingguo.com/React/%E7%AC%AC%E4%B8%80%E4%B8%AAReact%E5%BA%94%E7%94%A8.jpg)

[第一个React应用](https://songxingguo.github.io/React/)

<iframe src="https://songxingguo.github.io/React/" width="100%" height="300"></iframe>

### 增加一个新的React组件

React的首要思想是通过组件（Component）来开发应用。所谓组件，简单说，指的是能完成某个特定功能的独立的、可重用的代码。

基于组件的应用开发是广泛使用的软件开发模式，用分而治之的方法，把一个大的应用分解成若干小的组件，每个组件只关注于某个小范围的特定功能，但是把组件组合起来，就能够构成一个功能庞大的应用。如果分解功能的过程足够巧妙，那么每个组件可以在不同场景下重用，那样不光可以构建庞大的应用，还可以构建出灵活的应用。打个比方，每个组件是一块砖，而一个应用是一座楼，想要一次锻造就创建一座楼是不现实的。实际上，总是先锻造很多砖，通过排列组合这些砖，才能构建伟大的建筑。

我们先看一看create-react-app给我们自动产生的代码，在first-react-app目录下包含如下文件和目录：

```
src/
public/
README.md
package.json
node_modules/
```
我们要定义一个新的能够计算点击数组件，名叫ClickCounter。

关键代码：

```js
import React, { Component } from 'react';

class ClickCounter extends Component {

    constructor(props) {
        super(props);
        this.onClickButton = this.onClickButton.bind(this);
        this.state = {
            count: 0
        }
    }

    onClickButton() {
        this.setState({count: this.state.count + 1});
    }

    render() {
        const counterStyle = {
            margin: '16px'
        }
        return (
            <div style={counterStyle}>
                <button onClick={this.onClickButton}>Click Me</button>
                <div>
                    Click Count: <span id="clickCount">{this.state.count}</span>
                </div>
            </div>
        );
    }
}

export default ClickCounter;
```
效果：

![ClickCounter](https://graphbed.qiniu.songxingguo.com/React/ClickCounter.jpg)

import是ES6（EcmaScript6）语法中导入文件模块的方式，ES6语法是一个大集合，大部分功能都被最新浏览器支持。不过这个import方法却不在广泛支持之列，这没有关系，ES6语法的JavaScript代码会被webpack和babel转译成所有浏览器都支持的ES5语法，而这一切都无需开发人员做配置，create-react-app已经替我们完成了这些工作。

Component作为所有组件的基类，提供了很多组件共有的功能。

> 在React出现之初，使用的是React.createClass方式来创造组件类，这种方法已经被废弃了，但是在互联网上依然存在大量的文章基于React.createClass来讲解React，这些文章中依然有很多真知灼见的部分，但是读者要意识到，使用React.createClass是一种过时的方法。

在使用JSX的代码文件中，即使代码中并没有直接使用React，也一定要导入这个React，这是因为JSX最终会被转译成依赖于React的表达式。

#### JSX

所谓JSX，是 **JavaScript的语法扩展**（eXtension），让我们 **在JavaScript中可以编写像HTML一样的代码** 。在ClickCounter.js的render函数中，就出现了类似这样的HTML代码，在index.js中，ReactDOM.render的第一个参数`<App/>`也是一段JSX代码。

JSX中的这几段代码看起来和HTML几乎一模一样，都可以使用`<div>` `<button>`之类的元素，所以只要熟悉HTML，学习JSX完全不成问题，但是，我们一定要明白两者的不同之处。

首先，在JSX中使用的“元素”不局限于HTML中的元素，可以是任何一个React组件，在App.js中可以看到，我们创建的ClickCounter组件被直接应用在JSX中，使用方法和其他元素一样，这一点是传统的HTML做不到的。

React判断一个元素是HTML元素还是React组件的原则就是看第一个字母是否大写，如果在JSX中我们不用ClickCounter而是用clickCounter，那就得不到我们想要的结果。

其次，在JSX中可以通过onClick这样的方式给一个元素添加一个事件处理函数，当然，在HTML中也可以用onclick（【注意】和onClick拼写有区别），但在HTML中直接书写onclick一直就是为人诟病的写法，网页应用开发界一直倡导的是用jQuery的方法添加事件处理函数，直接写onclick会带来代码混乱的问题。

#### JSX是进步还是倒退

以前用HTML来代表内容，用CSS代表样式，用JavaScript来定义交互行为，这三种语言分在三种不同的文件里面，实际上是把不同技术分开管理了，而不是逻辑上的“分而治之”。

根据做同一件事的代码应该有高耦合性的设计原则。

我们在JSX中看到一个组件使用了onClick，但并没有产生直接使用onclick（【注意】是onclick不是onClick）的HTML，而是使用了事件委托（event delegation）的方式处理点击事件，无论有多少个onClick出现，其实最后都只在DOM树上添加了一个事件处理函数，挂在最顶层的DOM节点上。所有的点击事件都被这个事件处理函数捕获，然后根据具体组件分配给特定函数，使用事件委托的性能当然要比为每个onClick都挂载一个事件处理函数要高。

除了在组件中定义交互行为，我们还可以在React组件中定义样式，我们可以修改ClickCounter.js中的render函数，代码如下：

```js
render() {
  const counterStyle = {
    margin: '16px'
  }
  return (
    <div style={counterStyle}>
      <button onClick={this.onClickButton}>Click Me</button>
      <div>
        Click Count: <span id=”clickCount”>{this.state.count}</span>
      </div>
    </div>
  );
}
```
我们在JavaScript代码中定义一个counterStyle对象，然后在JSX中赋值给顶层div的style属性，可以看到网页中这个部分的margin真的变大了。

你看，React的组件可以把JavaScript、HTML和CSS的功能在一个文件中，实现真正的组件封装。

#### 分解React应用

我们启动React应用的命令是npm start，看一看package.json中对start脚本的定义，如下所示：

```
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test --env=jsdom",
  "eject": "react-scripts eject"
}
```
你可以发现package.json文件中和start并列还有其他几个命令，其中build可以创建生产环境优化代码，test用于单元测试，还有一个eject命令很有意思。

这个eject（弹射）命令做的事情，就是把潜藏在react-scripts中的一系列技术栈配置都“弹射”到应用的顶层，然后我们就可以研究这些配置细节了，而且可以更灵活地定制应用的配置。

> eject命令是不可逆的，就好像战斗机飞行员选择“弹射”出驾驶舱，等于是放弃了这架战斗机，是不可能再飞回驾驶舱的。所以，当你执行eject之前，最好做一下备份。

我们在命令行下执行下面的命令，完成“弹射”操作：

```
npm run eject
```
当前目录下会增加两个目录，一个是scripts，另一个是config，同时，package.json文件中的scripts部分也发生了变化：

```
"scripts": {
  "start": "node scripts/start.js",
  "build": "node scripts/build.js",
  "test": "node scripts/test.js --env=jsdom"
},
```
从此之后，start脚本将使用scripts目录下的start.js，而不是node_modules目录下的react-scripts，弹射成功，再也回不去了。

在config目录下的webpack.config.dev.js文件，定制的就是npm start所做的构造过程，其中有一段关于babel的定义：

```js
{
  test: /\.(js|jsx)$/,
  include: paths.appSrc,
  loader: 'babel',
  query: {
    // This is a feature of `babel-loader` for webpack (not Babel itself).
    // It enables caching results in ./node_modules/.cache/babel-loader/
    // directory for faster rebuilds.
    cacheDirectory: true
  }
},
```
代码中paths.appSrc的值就是src，所以这段配置的含义指的是所有以js或者jsx为扩展名的文件，都会由babel所处理。

并不是所有的浏览器都支持所有ES6语法，但是有了babel，我们就可以不用顾忌太多，因为babel会把ES6语法的JavaScript代码转译（transpile）成浏览器普遍支持的JavaScript代码，实际上，在React的社区中，不使用ES6语法写代码才显得奇怪。

### React的工作方式

#### jQuery如何工作

假设我们用jQuery来实现ClickCounter的功能，该怎么做呢？首先，我们要产生一个网页的HTML，写一个index.html文件如下所示：

```html
<!doctype html>
<html>
  <body>
    <div>
      <button id="clickMe">Click Me</button>
      <div>
        Click Count: <span id="clickCount">0</span>
      </div>
    </div>
    <script src="http://lib.sinaapp.com/js/jquery/1.9.1/jquery-1.9.1.min.js"></script>
    <script src="./clickCounter.js"></script>
  </body>
</html>
```
实际产品中，产生这样的HTML可以用PHP、Java、Ruby on Rails或者任何一种服务器端语言和框架来做，也可以在浏览器中用Mustache、Hogan这样的模板产生，这里我们只是把问题简化，直接书写HTML。

上面的HTML只是展示样式，并没有任何交互功能，现在我们用jQuery来实现交互功能，和jQuery的传统一样，我们把JavaScript代码写在一个独立的文件clickCounter.js里面，如下所示：

```js
$(function() {
  $('#clickMe').click(function() {
    var clickCounter = $('#clickCount');
    var count = parseInt(clickCounter.text(), 10);
    clickCounter.text(count+1);
  })
})
```
在jQuery的解决方案中，首先根据CSS规则找到id为clickCount的按钮，挂上一个匿名事件处理函数，在事件处理函数中，选中那个需要被修改的DOM元素，读取其中的文本值，加以修改，然后修改这个DOM元素。

**选中一些DOM元素，然后对这些元素做一些操作，这是一种最容易理解的开发模式** 。jQuery的发明人John Resig就是发现了网页应用开发者的这个编程模式，才创造出了jQuery，其一问世就得到普遍认可，因为这种模式直观易懂。但是，对于庞大的项目，这种模式会造成代码结构复杂，难以维护，每个jQuery的使用者都会有这种体会。

#### React的理念

打一个比方，React是一个聪明的建筑工人，而jQuery是一个比较傻的建筑工人，开发者你就是一个建筑的设计师，**如果是jQuery这个建筑工人为你工作，你不得不事无巨细地告诉jQuery“如何去做”**，要告诉他这面墙要拆掉重建，那面墙上要新开一个窗户，反之，**如果是React这个建筑工人为你工作，你所要做的就是告诉这个工人“我想要什么样子”，只要把图纸递给React这个工人** ，他就会替你搞定一切，当然他不会把整个建筑拆掉重建，而是很聪明地把这次的图纸和上次的图纸做一个对比，发现不同之处，然后只去做适当的修改就完成任务了。

显而易见，React的工作方式把开发者从繁琐的操作中解放出来，开发者只需要着重“ **我想要显示什么** ”，而 **不用操心** “ **怎样去做** ”。

这种新的思维方式，对于一个 **简单的例子** 也要编写不少代码，**感觉像是用高射炮打蚊子** ，但是对于一个 **大型的项目** ，这种方式编写的 **代码会更容易管理** ，因为整个React应用要做的就是渲染，开发者关注的是渲染成成什么样子，而不用关心如何实现增量渲染。

React的理念，归结为一个公式，就像下面这样：

```
UI＝render（data）
```
让我们来看看这个公式表达的含义，用户看到的界面（UI），应该是一个函数（在这里叫render）的执行结果，只接受数据（data）作为参数。这个函数是一个 **纯函数** ，所谓纯函数，指的是 **没有任何副作用，输出完全依赖于输入的函数，两次函数调用如果输入相同，得到的结果也绝对相同** 。如此一来，最终的用户界面，**在render函数确定的情况下完全取决于输入数据** 。

对于开发者来说，重要的是区分开哪些属于data，哪些属于render，想要更新用户界面，要做的就是更新data，用户界面自然会做出响应，所以React实践的也是“ **响应式编程** ”（Reactive Programming）的思想，这也就是React为什么叫做React的原因。

#### Virtual DOM

既然React应用就是通过重复渲染来实现用户交互，你可能会有一个疑虑：这样的重复渲染会不会效率太低了呢？毕竟，在jQuery的实现方式中，我们可以清楚地看到每次只有需要变化的那一个DOM元素被修改了；可是，在React的实现方式中，看起来每次render函数被调用，都要把整个组件重新绘制一次，这样看起来有点浪费。

事实并不是这样，**React利用Virtual DOM，让每次渲染都只重新渲染最少的DOM元素** 。

要了解Virtual DOM，就要先了解DOM，**DOM是结构化文本的抽象表达形式** ，特定于Web环境中，这个结构化文本就是HTML文本，HTML中的每个元素都对应DOM中某个节点，这样，因为HTML元素的逐级包含关系，DOM节点自然就构成了一个树形结构，称为DOM树。

浏览器为了渲染HTML格式的网页，会先 **将HTML文本解析以构建DOM树** ，然后 **根据DOM树渲染出用户看到的界面** ，当要 **改变界面内容** 的时候，就去 **改变DOM树上的节点** 。

Web前端开发关于性能优化有一个原则：尽量 **减少DOM操作** 。虽然DOM操作也只是一些简单的JavaScript语句，但是 **DOM操作会引起浏览器对网页进行重新布局，重新绘制** ，这就是一个比JavaScript语句执行慢很多的过程。

如果使用mustache或者hogan这样的模板工具，那就是生成HTML字符串塞到网页中，浏览器又要做一次解析产生新的DOM节点，然后替换DOM树上对应的子树部分，这个过程肯定效率不高。虽然 **JSX看起来很像是一个模板** ，但 **是最终会被Babel解析为一条条创建React组件或者HTML元素的语句** ，神奇之处在于，**React并不是通过这些语句直接构建DOM树** ，而是 **首先构建Virtual DOM** 。

既然 **DOM树是对HTML的抽象** ，那 **Virtual DOM就是对DOM树的抽象** 。Virutal DOM不会触及浏览器的部分，只是存在于JavaScript空间的树形结构，**每次自上而下渲染React组件时，会对比这一次产生的Virtual DOM和上一次渲染的Virtual DOM，对比就会发现差别，然后修改真正的DOM树时就只需要触及差别中的部分就行** 。

以ClickCounter为例，一开始点击计数为0，用户点击按钮让点击计数变成1，这一次重新渲染，React通过Virtual DOM的对比发现其实只是id为clickCount的span元素中内容从0变成了1而已：

```html
<span id=”clickCount”>{this.state.count}</span>
```
React发现这次渲染要做的事情只是更换这个span元素的内容而已，其他DOM元素都不需要触及，于是执行类似下面的语句，就完成了任务：

```js
document.getElementById("clickCount").innerHTML = "1";
```
React对比Virtual DOM寻找差异的过程比较复杂，在第5章，我们会详细介绍对比的过程。

#### React工作方式的优点

毫无疑问，jQuery的方式直观易懂，对于初学者十分适用，但是当项目逐渐变得庞大时，用jQuery写出的代码往往互相纠缠，形成类似图1-4的状况，难以维护。

![jQuery方式造成的纠缠代码结构](https://graphbed.qiniu.songxingguo.com/React/jQuery%E6%96%B9%E5%BC%8F%E9%80%A0%E6%88%90%E7%9A%84%E7%BA%A0%E7%BC%A0%E4%BB%A3%E7%A0%81%E7%BB%93%E6%9E%84.jpg)

使用React的方式，就可以避免构建这样复杂的程序结构，无论何种事件，引发的都是React组件的重新渲染，至于如何只修改必要的DOM部分，则完全交给React去操作，开发者并不需要关心，程序的流程简化为图1-5的样式。

React利用函数式编程的思维来解决用户界面渲染的问题，最大的优势是开发者的效率会大大提高，开发出来的代码可维护性和可阅读性也大大增强。

![React的程序流程](https://graphbed.qiniu.songxingguo.com/React/React%E7%9A%84%E7%A8%8B%E5%BA%8F%E6%B5%81%E7%A8%8B.jpg)

React等于强制所有组件都按照这种由数据驱动渲染的模式来工作，无论应用的规模多大，都能让程序处于可控范围内。

### 本章小结

在这一章里，我们用create-react-app创造了一个简单的React应用，在一开始，我们就按照组件的思想来开发应用，React的主要理念之一就是基于组件来开发应用。

通过和同样功能的jQuery实现方式对比，我们了解了React的工作方式，**React利用声明式的语法** ，让开发者专注于描述用户界面“ **显示成什么样子** ”，而 **不是** 重复思考“ **如何去显示** ”，这样可以大大提高开发效率，也让代码更加容易管理。

虽然 **React是通过重复渲染来实现动态更新效果** ，但是 **借助Virtual DOM技术** ，实际上 **这个过程并不牵涉太多的DOM操作** ，所以 **渲染效率很高** 。

理解React的工作方式，是踏入React世界的关键一步，接下来我们详细介绍如何构建高质量的React组件。

## 设计高质量的React组件

为一个合格的开发者，不要只满足于编写出了可以运行的代码，而要了解代码背后的工作原理；不要只满足于自己编写的程序能够运行，还要让自己的代码可读而且易于维护。这样才能开发出高质量的软件。

### 易于维护组件的设计要素

任何一个复杂的应用，都是由一个简单的应用发展而来的，当应用还很简单的时候，因为功能很少，可能只有一个组件就足够了，但是，随着功能的增加，把越来越多的功能放在一个组件中就会显得臃肿和难以管理。

就和 **一个人最好一次只专注做一件事** 一样，也应该尽量保持 **一个组件只做一件事** 。当开发者发现一个组件功能太多代码量太大的时候，就要考虑拆分这个组件，用多个小的组件来代替。**每个小的组件只关注实现单个功能，但是这些功能组合起来，也能满足复杂的实际需求** 。

这就是“**分而治之**”的策略，把问题分解为多个小问题，这样既容易解决也方便维护，虽然“分而治之”是一个好策略，但是不要滥用，**只有必要的时候才去拆分组件，不然可能得不偿失** 。

拆分组件最关键的就是 **确定组件的边界** ，每个组件都应该是可以独立存在的，**如果两个组件逻辑太紧密** ，**无法清晰定义各自的责任** ，那也许 **这两个组件本身就不该被拆开，作为同一个组件也许更合理** 。

虽然组件是应该独立存在的，但是并不是说组件就是孤岛一样的存在，**不同组件之间总是会有通信交流** ，这样才可能组合起来完成更大的功能。

作为软件设计的通则，组件的划分要满足 **高内聚（High Cohesion）和低耦合（Low Coupling）的原则** 。

**高内聚** 指的是 **把逻辑紧密相关的内容放在一个组件中** 。用户界面无外乎内容、交互行为和样式。传统上，内容由HTML表示，交互行放在JavaScript代码文件中，样式放在CSS文件中定义。这虽然满足一个功能模块的需要，却要放在三个不同的文件中，这其实不满足高内聚的原则。React却不是这样，**展示内容的JSX** 、**定义行为的JavaScript代码** ，甚至 **定义样式的CSS** ，都可以 **放在一个JavaScript文件中** ，因为它们本来就是为了实现一个目的而存在的，所以说 **React天生具有高内聚的特点** 。

**低耦合** 指的是 **不同组件之间的依赖关系要尽量弱化** ，也就是 **每个组件要尽量独立** 。保持整个系统的低耦合度，需要对系统中的功能有充分的认识，然后 **根据功能点划分模块** ，**让不同的组件去实现不同的功能** ，这个功夫还在开发者身上，不过，React组件的对外接口非常规范，方便开发者设计低耦合的系统。

### React组件的数据

>“差劲的程序员操心代码，优秀的程序员操心数据结构和它们之间的关系。”
—Linus Torvalds，Linux创始人

毫无疑问，如何组织数据是程序的最重要问题。

React组件的数据分为两种，**prop** 和 **state** ，**无论prop或者state的改变，都可能引发组件的重新渲染** ，那么，设计一个组件的时候，什么时候选择用prop什么时候选择用state呢？其实原则很简单，**prop是组件的对外接口** ，**state是组件的内部状态** ，**对外用prop，内部用state** 。

为了演示属性的使用，我们构造一个应用包含两个组件，Counter组件和ControlPanel组件，其中ControlPanel组件是父组件，包含若干个Counter组件，相关代码在Github上对应本书的代码库 https://github.com/mocheng/react-and-redux/tree/master/chapter-02/cont-rolpanel 中。

#### React的prop

在React中，prop（property的简写）是从外部传递给组件的数据，一个React组件通过定义自己能够接受的prop就定义了自己的对外公共接口。

每个React组件都是独立存在的模块，组件之外的一切都是外部世界，外部世界就是通过prop来和组件对话的。

1. 给prop赋值

    我们先从外部世界来看，prop是如何使用的，在下面的JSX代码片段中，就使用了prop：

    ```js
    <SampleButton
    id="sample" borderWidth={2} onClick={onButtonClick}
    style={{color: "red"}}
    />
    ```
    在上面的例子中，创建了名为SampleButton的组件实例，使用了名字分别为id、border Width、onClick和style的prop，看起来，React组件的prop很像是HTML元素的属性，不过，HTML组件属性的值都是字符串类型，即使是内嵌JavaScript，也依然是字符串形式表示代码。React组件的prop所能支持的类型则丰富得多，除了字符串，可以是任何一种JavaScript语言支持的数据类型。

    比如在上面的SampleButton中，borderWidth就是数字类型，onClick是函数类型，style的值是一个包含color字段的对象，当prop的类型不是字符串类型时，在JSX中必须用花括号{}把prop值包住，所以style的值有两层花括号，外层花括号代表是JSX的语法，内层的花括号代表这是一个对象常量。

    当外部世界要传递一些数据给React组件，一个最直接的方式就是通过prop；同样，React组件要反馈数据给外部世界，也可以用prop，因为prop的类型不限于纯数据，也可以是函数，函数类型的prop等于让父组件交给了子组件一个回调函数，子组件在恰当的实际调用函数类型的prop，可以带上必要的参数，这样就可以反过来把信息传递给外部世界。

    对于Counter组件，父组件ControlPanel就是外部世界，我们看ControlPanel是如何用prop传递信息给Counter的，代码如下：

    ```js
    class ControlPanel extends Component {
      render() {
        return (
          <div>
            <Counter caption="First" initValue={0} />
            <Counter caption="Second" initValue={10} />
            <Counter caption="Third" initValue={20} />
          </div>
        );
      }
    }
    ```
    ControlPanel组件包含三个Counter组件实例，在ControlPanel的render函数中将这三个子组件实例用div包起来，因为React要求render函数只能返回一个元素。

    > 虽然React声称将来可以支持render返回数组以支持多个组件，但是到本书写作时最新版v15.4为止，这个功能依然没有实现。

    在每个Counter组件实例中，都使用了caption和initValue两个prop。通过名为caption的prop，ControlPanel传递给Counter组件实例说明文字。通过名为initValue的prop传递给Counter组件一个初始的计数值。

2. 读取prop值

    我们再来看Counter组件内部是如何接收传入的prop的，首先是构造函数，代码如下：

    ```js
    class Counter extends Component {
      constructor(props) {
        super(props);
        this.onClickIncrementButton = this.onClickIncrementButton.bind(this);
        this.onClickDecrementButton = this.onClickDecrementButton.bind(this);
        this.state = {
          count: props.initValue || 0
        }
      }
    }
    ```
    如果一个组件需要定义自己的构造函数，一定要记得在构造函数的第一行通过super调用父类也就是React.Component的构造函数。如果在构造函数中没有调用super（props），那么组件实例被构造之后，类实例的所有成员函数就无法通过this.props访问到父组件传递过来的props值。很明显，给this.props赋值是React.Component构造函数的工作之一。

    在Counter的构造函数中还给两个成员函数绑定了当前this的执行环境，因为ES6方法创造的React组件类并不自动给我们绑定this到当前实例对象。

    在构造函数的最后，我们可以看到读取传入prop的方法，在构造函数中可以通过参数props获得传入prop值，在其他函数中则可以通过this.props访问传入prop的值，比如在Counter组件的render函数中，我们就是通过this.props获得传入的caption，render函数代码如下：

    ```js
    render() {
      const {caption} = this.props;
      return (
        <div>
          <button style={buttonStyle} onClick={this.onClickIncrementButton}>+</button>
          <button style={buttonStyle} onClick={this.onClickDecrementButton}>-</button>
          <span>{caption} count: {this.state.count}</span>
        </div>
      );
    }
    ```
    在上面的代码中，我们使用了ES6的解构赋值（destructuring assignment）语法从this.props中获得了名为caption的prop值。

3. propTypes检查

既然prop是组件的对外接口，那么就应该有某种方式让组件声明自己的接口规范。简单说，一个组件应该可以规范以下这些方面：

- 这个组件支持哪些prop；
- 每个prop应该是什么样的格式。

React通过propTypes来支持这些功能。

在ES6方法定义的组件类中，可以通过增加类的propTypes属性来定义prop规格，这不只是声明，而且是一种限制，在运行时和静态代码检查时，都可以根据propTypes判断外部世界是否正确地使用了组件的属性。

比如，对于Counter组件的propTypes定义代码如下：

```js
Counter.propTypes = {
  caption: PropTypes.string.isRequired,
  initValue: PropTypes.number
};
```

其中要求caption必须是string类型，initValue必须是number类型。可以看到，两者除了类型不同之外，还有一个区别：caption带上了isRequried，这表示使用Counter组件必须要指定caption；而initValue因为没有isRequired，则表示如果没有也没关系。
为了验证propTypes的作用，可以尝试故意违反propTypes的规定使用Counter实例，比如在ControlPanel的render函数中增加下列的代码：

```html
<Counter caption={123} initValue={20} />
```
我们在Chrome浏览器开发工具的Console界面，可以看到一个红色的警告提示，如下图所示。

![错误prop类型的错误提示](https://graphbed.qiniu.songxingguo.com/React/%E9%94%99%E8%AF%AFprop%E7%B1%BB%E5%9E%8B%E7%9A%84%E9%94%99%E8%AF%AF%E6%8F%90%E7%A4%BA.jpg)

这段出错的含义是，caption属性预期是字符串类型，得到的却是一个数字类型。
我们尝试删掉这个Counter实例的caption属性，代码如下：

```html
<Counter initValue={20} />
```
这时可以看到Console选项卡中依然有红色警告信息，如下图所示。

![缺失必须存在prop的错误提示](https://graphbed.qiniu.songxingguo.com/React/%E7%BC%BA%E5%A4%B1%E5%BF%85%E9%A1%BB%E5%AD%98%E5%9C%A8prop%E7%9A%84%E9%94%99%E8%AF%AF%E6%8F%90%E7%A4%BA.jpg)

提示的含义是，caption是Counter必需的属性，但是却没有赋值。

很明显，有了propTypes的检查，可以很容易发现对prop的不正确使用方法，可尽早发现代码中的错误。

从上面可以看得出来propTypes检查可以防止不正确的prop使用方法，那么如果组件根本就没有定义propTypes会怎么样呢？

可以尝试在src/Counter.js文件中删除掉那一段给Counter.propTypes赋值的语句，在浏览器Console里可以看到红色警告不再出现。可见，没有propTypes定义，组件依然能够正常工作，而且，即使在上面propTypes检查出错的情况下，组件依旧能工作。也就是说propTypes检查只是一个辅助开发的功能，并不会改变组件的行为。

**propTypes虽然能够在开发阶段发现代码中的问题，但是放在产品环境中就不大合适了。**

首先，定义类的propTypes属性，无疑是要占用一些代码空间，而且propTypes检查也是要消耗CPU计算资源的。其次，在产品环境下做propTypes检查没有什么帮助，毕竟，propTypes产生的这些错误信息只有开发者才能看得懂，放在产品环境下，在最终用户的浏览器Console中输出这些错误信息没什么意义。

所以，最好的方式是，开发者在代码中定义propTypes，在开发过程中避免犯错，但是在发布产品代码时，用一种自动的方式将propTypes去掉，这样最终部署到产品环境的代码就会更优。现有的 **babel-react-optimize** 就具有这个功能，可以通过npm安装，但是应该确保只在发布产品代码时使用它。

#### React的state

驱动组件渲染过程的除了prop，还有state，state代表组件的内部状态。由于React组件不能修改传入的prop，所以需要记录自身数据变化，就要使用state。

在Counter组件中，最初显示初始计数，可以通过initValue这个prop来定制，在Counter已经被显示之后，用户会点击“+”和“-”按钮改变这个计数，这个变化的数据就要Counter组件自己通过state来存储了。

1. 初始化state

通常在组件类的构造函数结尾处初始化state，在Counter构造函数中，通过对this.state的赋值完成了对组件state的初始化，代码如下：

```js
constructor(props) {
  ...
  this.state = {
    count: props.initValue || 0
  }
}
```

因为initValue是一个可选的props，要考虑到父组件没有指定这个props值的情况，我们优先使用传入属性的initValue，如果没有，就使用默认值0。

**组件的state必须是一个JavaScript对象** ，**不能是string或者number这样的简单数据类型** ，即使我们需要存储的只是一个数字类型的数据，也只能把它存作state某个字段对应的值，Counter组件里，我们的唯一数据就存在count字段里。

> 在React创建之初，使用的是React.createClass方法创建组件类，这种方式下，通过定义组件类的一个getInitialState方法来获取初始state值，但是这种做法已经被废弃了，我们现在都用ES6的语法定义组件类，所以不再考虑定义getInitialState方法。

由于在PropType声明中没有用isRequired要求必须有值的prop，例如上面的initValue，我们需要在代码中判断所给prop值是否存在，如果不存在，就给一个默认的初始值。不过，让这样的判断逻辑充斥在我们组件的构造函数之中并不是一件美观的事情，而且容易有遗漏。我们可以用React的defaultProps功能，让代码更加容易读懂。

给Counter组件添加defaultProps的代码如下：

```js
Counter.defaultProps = {
  initValue: 0
};
```
有了这样的设定，Counter构造函数中的this.state初始化中可以省去判断条件，可以认为代码执行到这里，必有initValue属性值，代码可以简化为这样：

```js
this.state = {
  count: props.initValue
}
```
以后，即使Counter的使用者没有指定initValue，在组件中就会收到一个默认的属性值0。

2. 读取和更新state

通过给button的onClick属性挂载点击事件处理函数，我们可以改变组件的state，以点击“+”按钮的响应函数为例，代码如下：

```js
onClickIncrementButton() {
  this.setState({count: this.state.count + 1});
}
```
在代码中，通过this.state可以读取到组件的当前state。值得【注意】的是，我们改变组件state必须要使用this.setState函数，而不能直接去修改this.state。如果不相信，你可以尝试把onClickIncrementButton函数修改成下面的样子：

```js
onClickIncrementButton() {
  this.state.count = this.state.count + 1;
}
```
既然this.state就是一个JavaScript对象，上面的代码逻辑看起来也没有什么问题，我们在界面上点击几个按钮看一下实际效果就会发现问题。

首先，在浏览器的Console中会有如下的提示：

```
warning Do not mutate state directly. Use setState() react/no-direct-mutation-state
```
这是在警告：“不要直接修改state，应该使用setState（）。”

当你点击“+”按钮，也不会看到后面的计数值有任何变化，但是当你点击“-”按钮，就会立即看到计数值发生变化，而且计数值会发生“跳跃”，比如在初始计数值为0的情况下，连续点击“+”按钮三次，计数值没有任何变化依然为0，点击了一下“-”按钮一次，就会看到计数值一下子变成了2，这是怎么回事？

**直接修改this.state的值** ，虽然事实上 **改变了组件的内部状态** ，但只是野蛮地修改了state，却 **没有驱动组件进行重新渲染** ，既然组件没有重新渲染，当然不会反应this.state值的变化；而 **this.setState（）函数所做的事情** ，首先是 **改变this.state的值** ，然后 **驱动组件经历更新过程** ，这样才有机会让this.state里新的值出现在界面上。

在上面描述的操作中，连续点击三次“+”按钮，this.state中的count字段值已经被增加到了3，但是没有重新渲染，这时候点击一次“-”按钮，触发的onClick-DecrementButton函数依然是用this.setState改变组件状态，这个函数调用首先把this.state中的count值从3减少为2，然后触发重新渲染，于是我们看到界面上的数字一下子从0跳跃为2。

#### prop和state的对比

总结一下prop和state的区别：

- prop用于定义外部接口，state用于记录内部状态；
- prop的赋值在外部世界使用组件时，state的赋值在组件内部；
- 组件不应该改变prop的值，而state存在的目的就是让组件来改变的。

组件的state，就相当于组件的记忆，其存在意义就是被修改，每一次通过this.setState函数修改state就改变了组件的状态，然后通过渲染过程把这种变化体现出来。

但是，**组件是绝不应该去修改传入的props值的** ，我们设想一下，假如父组件包含多个子组件，然后把一个JavaScript对象作为props值传给这几个子组件，而某个子组件居然改变了这个对象的内部值，那么，接下来其他子组件读取这个对象会得到什么值呢？当时读取了修改过的值，但是其他子组件是每次渲染都读取这个props的值呢？还是只读一次以后就用那个最初值呢？一切皆有可能，完全不可预料。也就是说，一个子组件去修改props中的值，可能让程序陷入一团混乱之中，这就完全违背了React设计的初衷。
还记得第1章中我们看到的那个公式吗？

```
UI＝render（data）
```
React组件扮演的是render函数的角色，应该是一个没有副作用的纯函数。修改props的值，是一个副作用，组件应该避免。

严格来说，React并没有办法阻止你去修改传入的props对象。所以，每个开发者就把这当做一个规矩，在编码中一定不要踩这儿红线，不然最后可能遇到不可预料的bug。

### 组件的生命周期

为了理解React的工作过程，我们就必须要了解React组件的生命周期，如同人有生老病死，自然界有日月更替，每个组件在网页中也会被创建、更新和删除，如同有生命的机体一样。

React严格定义了组件的生命周期，生命周期可能会经历如下三个过程：

- 装载过程（Mount），也就是把组件第一次在DOM树中渲染的过程；
- 更新过程（Update），当组件被重新渲染的过程；
- 卸载过程（Unmount），组件从DOM中删除的过程。

三种不同的过程，React库会依次调用组件的一些成员函数，这些函数称为生命周期函数。所以，要 **定制一个React组件，实际上就是定制这些生命周期函数** 。

#### 装载过程

我们先来看装载过程，当组件第一次被渲染的时候，依次调用的函数是如下这些：

- constructor
- getInitialState
- getDefaultProps
- componentWillMount
- render
- componentDidMount

我们来逐个地详细解释这些函数的功能。

1. constructor

    我们先来看第一个constructor，也就是ES6中每个类的构造函数，要创造一个组件类的实例，当然会调用对应的构造函数。

    要【注意】，并不是每个组件都需要定义自己的构造函数。在后面的章节我们可以看到，无状态的React组件往往就不需要定义构造函数，一个React组件需要构造函数，往往是为了下面的目的：

    - 初始化state，因为组件生命周期中任何函数都可能要访问state，那么整个生命周期中第一个被调用的构造函数自然是初始化state最理想的地方；
    - 绑定成员函数的this环境。

    在 **ES6语法** 下，**类的每个成员函数在执行时的this并不是和类实例自动绑定的** 。而在构造函数中，this就是当前组件实例，所以，为了方便将来的调用，往往在构造函数中将这个实例的特定函数绑定this为当前实例。

    以Counter组件为例，我们的构造函数有这样如下的代码：

    ```js
    this.onClickIncrementButton = this.onClickIncrementButton.bind(this);
    this.onClickDecrementButton = this.onClickDecrementButton.bind(this);
    ```
    这两条语句的作用，就是通过bind方法让当前实例中onClickIncrementButton和onClickDecrementButton函数被调用时，this始终是指向当前组件实例。

    > 在某些教程中，大家还会看到另一种bind函数的方式，类似下面的语句：
    ```
    this.foo = ::this.foo；
    ```
    > 等同于下面的语句：
    ```
    this.foo = this.foo.bind(this);
    ```
    > 这里所使用的两个冒号的：：操作符叫做bind操作符，虽然有babel插件支持这种写法，但是bind操作符可能不会成为ES标准语法的一部分，所以，虽然这种写法看起来很简洁，我们在本书中并不使用它。

2. getInitialState和getDefaultProps

getInitialState这个函数的返回值会用来初始化组件的this.state，但是，这个方法只有用React.createClass方法创造的组件类才会发生作用，本书中我们一直使用的ES6语法，所以这个函数根本不会产生作用。

getDefaultProps函数的返回值可以作为props的初始值，和getInitialState一样，这个函数只在React.createClass方法创造的组件类才会用到。总之，**实际上getInitialState和getDefaultProps两个方法在ES6的方法定义的React组件中根本不会用到** 。

假如我们用React.createClass方法定义一个组件Sample，设定内部状态foo的初始值为字符串bar，同时设定一个叫sampleProp的prop初始值为数字值0，代码如下：

```js
const Sample = React.createClass({
  getInitialState: function() {
    return {foo: 'bar'};
  },
  getDefaultProps: function() {
    return {sampleProp: 0}
  })
});
```
用ES6的话，在构造函数中通过给this.state赋值完成状态的初始化，通过给类属性（【注意】是类属性，而不是类的实例对象属性）defaultProps赋值指定props初始值，达到的效果是完全一样的，代码如下：

```js
class Sample extends React.Component {
  constructor(props) {
  super(props);
    this.state = {foo: 'bar'};
  }
});

Sample.defaultProps = {
  return {sampleProp: 0}
};
```
React.createClass已经被Facebook官方逐渐废弃，但是在互联网上还能搜索到很多使用React.createClass的教材，虽然 **强烈建议不再要使用React.createClass** ，但是如果读者你真的要用的话，需要【注意】关于getInitialState只出现在装载过程中，也就是说在一个组件的整个生命周期过程中，**这个函数只被调用一次，不要在里面放置预期会被多次执行的代码**。

3. render

render函数无疑是React组件中最重要的函数，**一个React组件可以忽略其他所有函数都不实现，但是一定要实现render函数** ，因为 **所有React组件的父类React.Component类对除render之外的生命周期函数都有默认实现** 。

通常一个组件要发挥作用，总是要渲染一些东西，render函数并不做实际的渲染动作，它只是返回一个JSX描述的结构，最终由React来操作渲染过程。

当然，某些特殊组件的作用不是渲染界面，或者，组件在某些情况下选择没有东西可画，那就让render函数返回一个null或者false，等于告诉React，这个组件这次不需要渲染任何DOM元素。

需要【注意】，**render函数应该是一个纯函数** ，完全 **根据this.state和this.props来决定返回的结果，而且不要产生任何副作用** 。**在render函数中去调用this.setState毫无疑问是错误的，因为一个纯函数不应该引起状态的改变** 。我们在后面的章节会对render函数做详细的介绍。

4. componentWillMount和componentDidMount

在装载过程中，componentWillMount会在调用render函数之前被调用，component-DidMount会在调用render函数之后被调用，这两个函数就像是render函数的前哨和后卫，一前一后，把render函数夹住，正好分别做render前后必要的工作。

不过，我们通常不用定义componentWillMount函数，顾名思义，componentWillMount发生在“将要装载”的时候，这个时候没有任何渲染出来的结果，即使调用this.setState修改状态也不会引发重新绘制，一切都迟了。换句话说，**所有可以在这个component-WillMount中做的事情，都可以提前到constructor中间去做** ，可以认为这个函数存在的主要目的就是 **为了和componentDidMount对称** 。

而componentWillMount的这个兄弟componentDidMount作用就大了。

需要【注意】的是，render函数被调用完之后，componentDidMount函数并不是会被立刻调用，**componentDidMount被调用的时候，render函数返回的东西已经引发了渲染，组件已经被“装载”到了DOM树上** 。

我们还是以ControlPanel为例，在ControlPanel中有三个Counter组件，我们稍微修改Counter的代码，让装载过程中所有生命周期函数都用console.log输出函数名和caption的值，比如，componentWillMount函数的内容如下：

```
componentWillMount() {
  console.log('enter componentWillMount ' + this.props.caption);
}
```
上面修改并没有添加任何功能，只是通过console.log输出一些内容，然后我们刷新网页，在浏览器的console里我们能够看见：

```
enter constructor: First
enter componentWillMount First
enter render First
enter constructor: Second
enter componentWillMount Second
enter render Second
enter constructor: Third
enter componentWillMount Third
enter render Third
enter componentDidMount First
enter componentDidMount Second
enter componentDidMount Third
```
可以清楚地看到，虽然componentWillMount都是紧贴着自己组件的render函数之前被调用，**componentDidMount可不是紧跟着render函数被调用** ，当所有三个组件的render函数都被调用之后，三个组件的componentDidMount才连在一起被调用。

之所以会有上面的现象，是因为 **render函数本身并不往DOM树上渲染或者装载内容** ，它 **只是返回一个JSX表示的对象** ，然后 **由React库来根据返回对象决定如何渲染** 。而React库肯定是要把所有组件返回的结果综合起来，才能知道该如何产生对应的DOM修改。所以，**只有React库调用三个Counter组件的render函数之后，才有可能完成装载** ，**这时候才会依次调用各个组件的componentDidMount函数作为装载过程的收尾** 。

componentWillMount和componentDidMount这对兄弟函数还有一个区别，就是 **component-WillMount可以在服务器端被调用** ，也可以 **在浏览器端被调用** ；而 **component-DidMount只能在浏览器端被调用** ，**在服务器端使用React的时候不会被调用** 。

到目前为止，我们构造的React应用例子都只在浏览器端使用React，所以看不出区别，但后面第12章关于“同构”应用的介绍时，我们会探讨在服务器端使用React的情况。

至于为什么只有componentDidMount仅在浏览器端执行，这是一个实现上的决定，而不是设计时刻意为之。不过，如果非要有个解释的话，可以这么说，既然“装载”是一个创建组件并放到DOM树上的过程，那么，真正的“装载”是不可能在服务器端完成的，因为服务器端渲染并不会产生DOM树，通过React组件产生的只是一个纯粹的字符串而已。

不管怎样，componentDidMount只在浏览器端执行，倒是给了我们开发者一个很好的位置去做只有浏览器端才做的逻辑，比如通过AJAX获取数据来填充组件的内容。

**在componentDidMount被调用的时候，组件已经被装载到DOM树上了，可以放心获取渲染出来的任何DOM** 。

在实际开发过程中，可能会需要让React和其他UI库配合使用，比如，因为项目前期已经用jQuery开发了很多功能，需要继续使用这些基于jQuery的代码，有时候其他的UI库做某些功能比React更合适，比如d3.js已经支持了丰富的绘制图表的功能，在这些情况下，我们不得不考虑如何让React和其他UI库和平共处。

以和jQuery配合为例，我们知道，React是用来取代jQuery的，但如果真的要让React和jQuery配合，就需要是 **利用componentDidMount函数** ，当 **componentDidMount被执行时，React组件对应的DOM已经存在** ，**所有的事件处理函数也已经设置好，这时候就可以调用jQuery的代码，让jQuery代码在已经绘制的DOM基础上增强新的功能** 。

在componentDidMount中调用jQuery代码只处理了装载过程，要和jQuery完全结合，又要考虑React的更新过程，就需要使用下面要讲的componentDidUpdate函数。


#### 更新过程

当组件被装载到DOM树上之后，用户在网页上可以看到组件的第一印象，但是要提供更好的交互体验，就要让该组件可以随着用户操作改变展现的内容， **当props或者state被修改的时候，就会引发组件的更新过程** 。

更新过程会依次调用下面的生命周期函数，其中render函数和装载过程一样，没有差别。

- componentWillReceiveProps
- shouldComponentUpdate
- componentWillUpdate
- render
- componentDidUpdate

有意思的是，并不是所有的更新过程都会执行全部函数，下面会介绍到各种特例。

1. componentWillReceiveProps（nextProps）

关于这个componentWillReceiveProps存在一些误解。在互联网上有些教材声称这个函数只有当组件的props发生改变的时候才会被调用，其实是不正确的。实际上，只要是父组件的render函数被调用，在render函数里面被渲染的子组件就会经历更新过程，不管父组件传给子组件的props有没有改变，都会触发子组件的componentWill-ReceiveProps函数。

【注意】，通过this.setState方法触发的更新过程不会调用这个函数，这是因为这个函数适合根据新的props值（也就是参数nextProps）来计算出是不是要更新内部状态state。更新组件内部状态的方法就是this.setState，如果this.setState的调用导致component-WillReceiveProps再一次被调用，那就是一个死循环了。

让我们对ControlPanel做一些小的改进，来体会一下上面提到的规则。

我们首先在Counter组件类里增加函数定义，让这个函数componentWillReceive-Props在console上输出一些文字，代码如下：

```js
componentWillReceiveProps(nextProps) {
  console.log('enter componentWillReceiveProps ' + this.props.caption)
}
```
在ControlPanel组件的render函数中，我们也做如下修改：

```js
render() {
  console.log('enter ControlPanel render');
  return (
    <div style={style}>
      . . .
      <button onClick={ () => this.forceUpdate() }>
      Click me to repaint!
      </button>
    </div>
  );
}
```
除了在ControlPanel的render函数入口处增加console输出，我们还增加了一个按钮，这个按钮的onClick事件引发一个匿名函数，当这个按钮被点击的时候，调用this.forceUpdate，每个React组件都可以通过forceUpdate函数强行引发一次重新绘制。

> 似上面的代码，在JSX用直接把匿名函数赋值给onClick的方法，看起来非常简洁而且方便，其实并不是值得提倡的方法。因为每次渲染都会创造一个新的匿名方法对象，而且有可能引发子组件不必要的重新渲染，原因在后面的章节会有详细介绍。

在网页中，我们去点击那个新增加的按钮，可以看到浏览器的console中有如下的输出：

```
enter ControlPanel render
enter componentWillReceiveProps First
enter render First
enter componentWillReceiveProps Second
enter render Second
enter componentWillReceiveProps Third
enter render Third
```
可以看到，引发forceUpdate之后，首先是ControlPanel的render函数被调用，随后第一个Counter组件的componentWillReceiveProps函数被调用，然后Counter组件的render函数被调用，随后第二第三个函数的这两个函数也依次被调用。

然而，ControlPanel在渲染三个子组件的时候，提供的props值一直就没有变化，可见componentWillReceiveProps并不是当props值变化的时候才被调用，所以，这个函数有必要把传入参数nextProps和this.props作必要对比。nextProps代表的是这一次渲染传入的props值，this.props代表的上一次渲染时的props值，只有两者有变化的时候才有必要调用this.setState更新内部状态。

在网页中，我们再尝试点击第一个Counter组件的“+”按钮，可以看到浏览器的console输出如下：

```
enter render First
```
明显，只有第一个组件Counter的render函数被调用，函数componentWillReceive-Props没有被调用。因为点击“+”按钮引发的是第一个Counter组件的this.setState函数的调用，就像上面说过的一样，**this.setState不会引发这个函数componentWillReceive-Props被调用** 。

从这个例子中我们也会发现，在React的组件组合中，**完全可以只渲染一个子组件，而其他组件完全不需要渲染** ，这是提高React性能的重要方式。

2. shouldComponentUpdate（nextProps，nextState）

除了render函数，shouldComponentUpdate可能是React组件生命周期中最重要的一个函数了。

说render函数重要，是因为render函数决定了该渲染什么，而说shouldComponent-Update函数重要，是因为它决定了一个组件什么时候不需要渲染。

render和shouldComponentUpdate函数，也是React生命周期函数中唯二两个要求有返回结果的函数。render函数的返回结果将用于构造DOM对象，而shouldComponent-Update函数返回一个布尔值，告诉React库这个组件在这次更新过程中是否要继续。

在更新过程中，React库首先调用shouldComponentUpdate函数，如果这个函数返回true，那就会继续更新过程，接下来调用render函数；反之，如果得到一个false，那就立刻停止更新过程，也就不会引发后续的渲染了。

说shouldComponentUpdate重要，就是因为只要使用恰当，他就能够大大提高React组件的性能，虽然React的渲染性能已经很不错了，但是，不管渲染有多快，如果发现没必要重新渲染，那就干脆不用渲染好了，速度会更快。

我们知道render函数应该是一个纯函数，这个纯函数的逻辑输入就是组件的props和state。所以，shouldComponentUpdate的参数就是接下来的props和state值。如果我们要定义shouldComponentUpdate，那就根据这两个参数，外加this.props和this.state来判断出是返回true还是返回false。

如果我们给组件添加shouldCompomentUpdate函数，那就沿用所有React组件父类React.Component中的默认实现方式，默认实现方式就是简单地返回true，也就是每次更新过程都要重新渲染。当然，这是最稳妥的方式，大不了浪费一点，但是绝对不会出错。不过若我们要追求更高的性能，就不能满足于默认实现，需要定制这个函数shouldComponentUpdate。

让我们尝试来给Counter组件增加一个shouldComponentUpdate函数。先来看看props，Counter组件支持两个props，一个叫caption，一个叫initValue。很明显，只有caption这个prop改变的时候，才有必要重新渲染。对于initValue，只是创建Counter组件实例时用于初始化计数值，在组件实例创建之后，无论怎么改，都不应该让Counter组件重新渲染。

再来看看state，Counter组件的state只有一个值count，如果count发生了变化，那肯定应该重新渲染，如果count没变化，那就没必要了。

现在，让我们给Counter组件类增加shouldComponentUpdate函数的定义，代码如下：

```js
shouldComponentUpdate(nextProps, nextState) {
  return (nextProps.caption !== this.props.caption)
    (nextState.count !== this.state.count);
}
```
现在，只有当caption改变，或者state中的count值改变，shouldComponent才会返回true。

值得一提的是，通过this.setState函数引发更新过程，并不是立刻更新组件的state值，在执行到到函数shouldComponentUpdate的时候，this.state依然是this.setState函数执行之前的值，所以我们要做的实际上就是在nextProps、nextState、this.props和this.state中互相比对。

我们在网页中引发一次ControlPanel的重新绘制，可以看到浏览器的console中的输出是这样：

```
enter ControlPanel render
enter componentWillReceiveProps First
enter componentWillReceiveProps Second
enter componentWillReceiveProps Third
```
可以看到，三个Counter组件的render函数都没有被调用，因为这个刷新没有改变caption的值，更没有引发组件内状态改变，所以完全没有必要重新绘制Counter。

对于Counter这个简单的组件，我们无法感觉到性能的提高，但是，实际开发中会遇到更复杂更庞大的组件，这种情况下避免没必要的重新渲染，就会大大提高性能。

3.componentWillUpdate和componentDidUpdate

如果组件的shouldComponentUpdate函数返回true，React接下来就会依次调用对应组件的componentWillUpdate、render和componentDidUpdate函数。

componentWillMount和componentDidMount，componentWillUpdate和componentDid-Update，这两对函数一前一后地把render函数夹在中间。

和装载过程不同的是，当在服务器端使用React渲染时，这一对函数中的Did函数，也就是componentDidUpdate函数，并不是只在浏览器端才执行的，无论更新过程发生在服务器端还是浏览器端，该函数都会被调用。

在介绍componentDidMount函数时，我们说到可以利用componentDidMount函数执行其他UI库的代码，比如jQuery代码。当React组件被更新时，原有的内容被重新绘制，这时候就需要在componentDidUpdate函数再次调用jQuery代码。

读者可能会问，componentDidUpdate函数不是可能会在服务器端也被执行吗？在服务器端怎么能够使用jQuery呢？实际上，使用React做服务器端渲染时，基本不会经历更新过程，因为服务器端只需要产出HTML字符串，一个装载过程就足够产出HTML了，所以正常情况下服务器端不会调用componentDidUpdate函数，如果调用了，说明我们的程序有错误，需要改进。

#### 卸载过程

React组件的卸载过程只涉及一个函数componentWillUnmount，当React组件要从DOM树上删除掉之前，对应的componentWillUnmount函数会被调用，所以这个函数适合做一些清理性的工作。

和装载过程与更新过程不一样，这个函数没有配对的Did函数，就一个函数，因为卸载完就完了，没有“卸载完再做的事情”。

不过，componentWillUnmount中的工作往往和componentDidMount有关，比如，在componentDidMount中用非React的方法创造了一些DOM元素，如果撒手不管可能会造成内存泄露，那就需要在componentWillUnmount中把这些创造的DOM元素清理掉。

### 组件向外传递数据

通过构造ControlPanel和Counter，现在我们已经知道如何通过prop从父组件传递数据给子组件，但是，组件之间的交流是相互的，子组件某些情况下也需要把数据传递给父组件，我们接下来看看在React中如何实现这个功能。

在ControlPanel中，包含三个Control子组件实例，每个Counter都有一个可以动态改变的计数值，我们希望ControlPanel能够即时显示出这三个子组件当前计数值之和。
这个功能看起来很简单，但是要解决一个问题，就是要让ControlPanel“知道”三个子组件当前的计数值，而且是每次变更都要立刻知道，而Control组件的当前值组件的内部状态，如何让外部世界知道这个值呢？

解决这个问题的方法，依然是利用prop。组件的prop可以是任何JavaScript对象，而在JavaScript中，函数是一等公民，函数本身就可以被看做一种对象，既可以像其他对象一样作为prop的值从父组件传递给子组件，又可以被子组件作为函数调用，这样事情就好办了。

#### 应用实例

展示这个功能的代码存在于https://github.com/mocheng/react-and-redux/代码库的chapter-02/controlpanel_with_summary目录下，在界面上，可以看到效果如下图所示：

![包含总数的ControlPanel应用效果图](https://graphbed.qiniu.songxingguo.com/React/%E5%8C%85%E5%90%AB%E6%80%BB%E6%95%B0%E7%9A%84ControlPanel%E5%BA%94%E7%94%A8%E6%95%88%E6%9E%9C%E5%9B%BE.jpg)

点击任何一个Counter的“+”按钮或者“-”按钮，可以看见除了所属Counter的计数变化，底部的总计数也会随之变化，这是因为Counter能够把自己状态改变的信息传递给外层的组件。

接下来看实现这个功能的关键代码。

在Counter组件中，对于点击“+”和“-”按钮的事件处理方法做了改动，代码如下：

```
onClickIncrementButton() {
	this.updateCount(true);
}
onClickDecrementButton() {
	this.updateCount(false);
}
updateCount(isIncrement) {
  const previousValue = this.state.count;
  const newValue = isIncrement ? previousValue + 1 : previousValue - 1;
  this.setState({count: newValue})
  this.props.onUpdate(newValue, previousValue)
}
```
现在，onClickIncrementButton函数和onClickDecrementButton的任务除了调用this.setState改变内部状态，还要调用this.props.onUpdate这个函数，为了避免重复代码，我们对原有代码做了一下重构，提取了共同部分到updateCount函数里。
对应的，Counter组件的propTypes和defaultProps就要增加onUpdate的定义，代码如下：

```js
Counter.propTypes = {
  caption: PropTypes.string.isRequired,
  initValue: PropTypes.number,
  onUpdate: PropTypes.func
};
Counter.defaultProps = {
  initValue: 0,
  onUpdate: f => f //默认是一个什么都不做的函数
};
```
新增加的prop叫做onUpdate，类型是一个函数，当Counter的状态改变的时候，就会调用这个给定的函数，从而达到通知父组件的作用。

这样，Counter的onUpdate就成了作为子组件的Counter向父组件ControlPanel传递数据的渠道，我们先约定这个函数的第一个参数是Counter更新之后的新值，第二个参数是更新之前的值，至于如何使用这两个参数的值，是父组件ControlPanel的逻辑，Counter不用操心，而且根据两个参数的值足够可以推导出数值是增加还是减少。

从使用Counter组件的角度，在ControlPanel组件中也要做一些修改，现在Control-Panel需要包含自己的state，首先是构造函数部分，代码如下：

```js
constructor(props) {
  super(props);
  this.onCounterUpdate = this.onCounterUpdate.bind(this);
  this.initValues = [ 0, 10, 20];
  const initSum = this.initValues.reduce((a, b) => a+b, 0);
  this.state = {
    sum: initSum
  };
}
```
在ControlPanel组件被第一次渲染的时候，就需要显示三个计数器数值的总和，所以我们在构造函数中用initValues数组记录所有的Counter的初始值，在初始化this.state之前，将initValues数组中所有值加在一起，作为this.state中sum字段的初始值。

ControlPanel传递给Counter组件onUpdate这个prop的值是onCounterUpdate函数，代码如下：

```js
onCounterUpdate(newValue, previousValue) {
  const valueChange = newValue - previousValue;
  this.setState({ sum: this.state.sum + valueChange});
}
```
onCounterUpdate函数的参数和Counter中调用onUpdate prop的参数规格一致，第一个参数为新值，第二个参数为之前的值，两者之差就是改变值，将这个改变作用到this.state.sum上就是新的sum状态。

遗憾的是，React虽然有PropType能够检查prop的类型，却没有任何机制来限制prop的参数规格，参数的一致性只能靠开发者来保证。

ControlPanel组件的render函数中需要增加对this.state.sum和onCounterUpdate的使用，代码如下：

```js
render() {
  return (
    <div style={style}>
      <Counter onUpdate={this.onCounterUpdate} caption="First" />
      <Counter onUpdate={this.onCounterUpdate} caption="Second" initValue={this.initValues[1]} />
      <Counter onUpdate={this.onCounterUpdate} caption="Third" initValue={this.initValues[2]} />
      <hr/>
      <div>Total Count: {this.state.sum}</div>
    </div>
  );
}
```
### React组件state和prop的局限

是时候重新思考一下多个组件之间的数据管理问题了。在上面修改的代码中，不难发现其实实现得并不精妙，每个Counter组件有自己的状态记录当前计数，而父组件ControlPanel也有一个状态来存储所有Counter计数总和，也就是说，数据发生了重复。
数据如果出现重复，带来的一个问题就是如何保证重复的数据一致，如果数据存多份而且不一致，那就很难决定到底使用哪个数据作为正确结果了。

在上面的例子中，ControlPanel通过onUpdate回调函数传递的新值和旧值来计算新的计数总和，设想一下，由于某种bug的原因，某个按钮的点击更新没有通知到Control-Panel，结果就会让ControlPanel中的sum状态和所有子组件Counter的count状态之和不一致，这时候，是应该相信ControlPanel还是Counter呢？

如图2-5所示，逻辑上应该相同的状态，分别存放在不同组件中，就会导致这种困局。

对于上面所说的问题，一个直观的解决方法是以某一个组件的状态为准，这个组件是状态的“领头羊”，其余组件都保持和“领头羊”的状态同步，但是在实际情况下这种方法可能难以实施。比如上面的例子中，每个Counter记录自己的计数值是很自然的，但是有三个Counter组件，也就有三只“领头羊”，让ControlPanel跟着三只“领头羊”走，似乎不是一个好主意。

另一种思路，就是干脆不要让任何一个React组件扮演“领头羊”的角色，把数据源放在React组件之外形成全局状态，如图2-6所示，让各个组件保持和全局状态的一致，这样更容易控制。

![组件状态不一致的困惑](https://graphbed.qiniu.songxingguo.com/React/%E7%BB%84%E4%BB%B6%E7%8A%B6%E6%80%81%E4%B8%8D%E4%B8%80%E8%87%B4%E7%9A%84%E5%9B%B0%E6%83%91.jpg)

![React中提取出来](https://graphbed.qiniu.songxingguo.com/React/React%E4%B8%AD%E6%8F%90%E5%8F%96%E5%87%BA%E6%9D%A5.png)

图2-6中所示，全局状态就是唯一可靠的数据源，在第3章中我们会介绍，这就是Flux和Redux中Store的概念。

除了state，利用prop在组件之间传递信息也会遇到问题。设想一下，在一个应用中，包含三级或者三级以上的组件结构，顶层的祖父级组件想要传递一个数据给最低层的子组件，用prop的方式，就只能通过父组件中转，如图2-6所示。

也许中间那一层父组件根本用不上这个prop，但是依然需要支持这个prop，扮演好搬运工的角色，只因为子组件用得上，这明显违反了低耦合的设计要求。第3章中我们会探讨如何解决这样的困局。

### 本章小结

在这一章中，我们学习了构建高质量组件的原则，应用React一样要以构建高内聚低耦合的组件为目标，而保证组件高质量的一个重要工作就是保持组件对外接口清晰简洁。

React利用prop来定义组件的对外接口，用state来代表内部的状态，某个数据选择用prop还是用state表示，取决于这个数据是对外还是对内。

我们还介绍了React的生命周期，了解了装载过程、更新过程和卸载过程涉及的所有生命周期函数。

在本章中我们利用CountrolPanel和Counter两个组件演示了组件之间的通信方式，包括子组件向父组件传递信息的方式，同时也看出了使用React的state来存储状态的一个缺点，那就是数据的冗余和重复，这就是我们接下来要解决的问题。

![跨级传递prop的困局](https://graphbed.qiniu.songxingguo.com/React/%E8%B7%A8%E7%BA%A7%E4%BC%A0%E9%80%92prop%E7%9A%84%E5%9B%B0%E5%B1%80.jpg)

