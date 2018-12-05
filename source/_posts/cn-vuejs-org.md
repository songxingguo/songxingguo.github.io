title: Vue.js文档学习
author: songxingguo
tags:
  - Vue
categories:
  - 读书笔记
date: 2018-12-05 14:27:00
---
## 写在前面

[Vue官方文档地址](https://cn.vuejs.org/v2/guide/)


## Vue.js 是什么

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。

- Vue 被设计为可以自底向上逐层应用。

- Vue 的核心库只关注视图层。

- Vue 能够为复杂的单页应用提供驱动。

[介绍视频](https://cn.vuejs.org/v2/guide)

<!-- more -->

## 起步

[在线尝试地址](https://jsfiddle.net/chrisvfritz/50wL7mdz/)

### 引入 Vue

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
或者：

```html
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
[安装教程](https://cn.vuejs.org/v2/guide/installation.html)

[Scrimba 上的系列教程](https://scrimba.com/playlist/pXKqta)

### 声明式渲染

Vue.js 的核心是 **一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统** ：

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
数据和 DOM 已经被建立了关联，所有东西都是响应式的。

除了文本插值，我们还可以像这样来 **绑定元素特性** ：

```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```
v-bind 特性被称为指令。指令带有前缀 v-，以表示它们是 **Vue 提供的特殊特性** 。可能你已经猜到了，**它们会在渲染的 DOM 上应用特殊的响应式行为** 。在这里，该指令的意思是：“**将这个元素节点的 title 特性和 Vue 实例的 message 属性保持一致** ”。

### 条件与循环

控制切换一个元素是否显示也相当简单：

```html
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
```

```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

不仅可以 **把数据绑定到 DOM 文本或特性**  ，还可以 **绑定到 DOM 结构** 。此外，Vue 也提供一个强大的过渡效果系统，可以 **在 Vue 插入/更新/移除元素时自动应用过渡效果** 。

v-for 指令可以 **绑定数组的数据来渲染一个项目列表** ：


```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

### 处理用户输入

可以用 v-on 指令添加一个事件监听器，通过它调用在 Vue 实例中定义的方法：

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
> 注意在 reverseMessage 方法中，我们更新了应用的状态，但没有触碰 DOM—— **所有的 DOM 操作都由 Vue 来处理** ，你编写的代码 **只需要关注逻辑层面即可** 。

Vue 还提供了 v-model 指令，它能 **轻松实现表单输入和应用状态之间的双向绑定** 。

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
### 组件化应用构建

组件系统是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

![组件树](https://graphbed.qiniu.songxingguo.com/cn-vuejs-org/%E7%BB%84%E4%BB%B6%E6%A0%91.png)

在 Vue 里，一个组件本质上是 **一个拥有预定义选项的一个 Vue 实例** 。在 Vue 中 **注册组件** 很简单：

```js
// 定义名为 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是个待办项</li>'
})
```
现在你可以用它构建另一个组件模板：

```html
<ol>
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ol>
```
**从父作用域将数据传到子组件** 才对。让我们来修改一下组件的定义，使之能够接受一个 prop：

```js
Vue.component('todo-item', {
  // todo-item 组件现在接受一个
  // "prop"，类似于一个自定义特性。
  // 这个 prop 名为 todo。
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```
现在，我们可以使用 v-bind 指令 **将待办项传到循环输出的每个组件中** ：

```html
<div id="app-7">
  <ol>
    <!--
      现在我们为每个 todo-item 提供 todo 对象
      todo 对象是变量，即其内容可以是动态的。
      我们也需要为每个组件提供一个“key”，稍后再
      作详细解释。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```

```js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其它什么人吃的东西' }
    ]
  }
})
```

在一个大型应用中，有必要将整个应用程序划分为组件，以使开发更易管理。

在[后续教程](https://cn.vuejs.org/v2/guide/components.html)中我们将详述组件，不过这里有一个 (假想的) 例子，以展示使用了组件的应用模板是什么样的：

```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```
#### 与自定义元素的关系

 Vue 组件非常类似于自定义元素——它是 [Web 组件规范](https://www.w3.org/wiki/WebComponents/)的一部分，这是因为 Vue 的组件语法部分参考了该规范。例如 Vue 组件实现了 [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) 与 is 特性。但是，还是有几个关键差别：

1. Web Components 规范已经完成并通过，但未被所有浏览器原生实现。目前 Safari 10.1+、Chrome 54+ 和 Firefox 63+ 原生支持 Web Components。相比之下，**Vue 组件不需要任何 polyfill** ，并且在所有支持的浏览器 (IE9 及更高版本) 之下表现一致。必要时，Vue 组件也可以包装于原生自定义元素之内。

2. Vue 组件提供了纯自定义元素所不具备的一些重要功能，最突出的是 **跨组件数据流** 、**自定义事件通信** 以及 **构建工具集成** 。

## 实例

### 创建一个 Vue 实例

每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的：

```js
var vm = new Vue({
  // 选项
})
```
虽然没有完全遵循 MVVM 模型，但是 Vue 的设计也受到了它的启发。

当创建一个 Vue 实例时，你可以传入一个选项对象。可以在 [选项 / 数据](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE) 中浏览完整的选项列表。

一个 Vue 应用由一个 **通过 new Vue 创建的根 Vue 实例** ，以及 **可选的嵌套的、可复用的组件树** 组成。举个例子，一个 todo 应用的组件树可以是这样的：

```
根实例
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。

### 数据与方法

当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```js
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的属性
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

当这些 **数据改变时，视图会进行重渲染** 。值得注意的是 **只有当实例被创建时 data 中存在的属性才是响应式的** 。也就是说如果你 **添加一个新的属性** ，比如：

```js
vm.b = 'hi'
```
那么 **对 b 的改动将不会触发任何视图的更新** 。如果你知道你会在晚些时候需要一个属性，但是 **一开始它为空或不存在，那么你仅需要设置一些初始值** 。比如：

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```
这里唯一的例外是使用 **Object.freeze()**，这会**阻止修改现有的属性** ，也意味着 **响应系统无法再追踪变化** 。

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```
```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- 这里的 `foo` 不会更新！ -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```
除了数据属性，**Vue 实例还暴露了一些有用的实例属性与方法** 。它们都有前缀 **$** ，以便与用户定义的属性区分开来。例如：

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```
[实例属性](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)

### 实例生命周期钩子

**每个 Vue 实例在被创建时都要经过一系列的初始化过程** ——例如，需要 **设置数据监听** 、**编译模板** 、**将实例挂载到 DOM** 并 **在数据变化时更新 DOM** 等。同时在这个过程中也会运行一些叫做 **生命周期钩子的函数** ，**这给了用户在不同阶段添加自己的代码的机会** 。

比如 created 钩子可以 **用来在一个实例被创建之后执行代码** ：

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```
也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 mounted、updated 和 destroyed。**生命周期钩子的 this 上下文指向调用它的 Vue 实例。**

> **不要在选项属性或回调上使用箭头函数** ，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())。因为 **箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例** ，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误。

### 生命周期图示

下图展示了实例的生命周期。

![生命周期图示](https://graphbed.qiniu.songxingguo.com/cn-vuejs-org/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA.png)

## 模板语法

Vue.js 使用了 **基于 HTML 的模板语法** ，**允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据** 。所有 **Vue.js 的模板都是合法的 HTML** ，所以 **能被遵循规范的浏览器和 HTML 解析器解析** 。

在底层的实现上，**Vue 将模板编译成虚拟 DOM 渲染函数** 。结合响应系统，**Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少** 。

如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，**直接写渲染 (render) 函数** ，使用可选的 **JSX 语法** 。

### 插值

#### 文本

数据绑定最常见的形式就是使用“Mustache”语法 (**双大括号**) 的文本插值：

```html
<span>Message: {{ msg }}</span>
```
**Mustache 标签将会被替代为对应数据对象上 msg 属性的值。** 无论何时，**绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新** 。

通过使用 **v-once** 指令，你也能执行 **一次性地插值** ，**当数据改变时，插值处的内容不会更新** 。但请留心 **这会影响到该节点上的其它数据绑定** ：

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```
#### 原始 HTML

**双大括号会将数据解释为普通文本** ，而非 HTML 代码。为了 **输出真正的 HTML** ，你需要使用 **v-html** 指令：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
这个 span 的内容将会被替换成为属性值 rawHtml，直接作为 HTML—— **会忽略解析属性值中的数据绑定** 。注意，你 **不能使用 v-html 来复合局部模板** ，因为 **Vue 不是基于字符串的模板引擎** 。反之，**对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位** 。

> 你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请 **只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值** 。

#### 特性

**Mustache 语法不能作用在 HTML 特性上** ，遇到这种情况应该使用 **v-bind** 指令：

```html
<div v-bind:id="dynamicId"></div>
```
在布尔特性的情况下，它们的存在即暗示为 true，v-bind 工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```
如果 **isButtonDisabled 的值是 null、undefined 或 false** ，则 **disabled 特性甚至不会被包含在渲染出来的 `<button>` 元素中** 。

#### 用 JavaScript 表达式

对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```
这些 **表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析** 。有个限制就是，**每个绑定都只能包含单个表达式** ，所以下面的例子都不会生效。

```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```
> **模板表达式都被放在沙盒中，只能访问全局变量的一个白名单** ，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

### 指令

**指令 (Directives) 是带有 v- 前缀的特殊特性** 。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：

```html
<p v-if="seen">现在你看到我了</p>
```
这里，v-if 指令将根据表达式 seen 的值的真假来插入/移除 `<p>` 元素。

#### 参数

**一些指令能够接收一个“参数”，在指令名称之后以冒号表示。** 例如，v-bind 指令可以用于响应式地更新 HTML 特性：

```html
<a v-bind:href="url">...</a>
```
在这里 **href 是参数** ，**告知 v-bind 指令将该元素的 href 特性与表达式 url 的值绑定** 。

另一个例子是 v-on 指令，它用于 **监听 DOM 事件** ：

```html
<a v-on:click="doSomething">...</a>
```
在这里参数是监听的事件名。我们也会更详细地讨论事件处理。

#### 修饰符

**修饰符** (Modifiers) 是以 **半角句号 . 指明的特殊后缀** ，用于 **指出一个指令应该以特殊方式绑定。** 例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 **event.preventDefault()** ：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```
### 缩写

Vue.js 为 v-bind 和 v-on 这两个最常用的指令，提供了特定简写：

#### v-bind 缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```
#### v-on 缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```
它们看起来可能与普通的 HTML 略有不同，但 ** : 与 @ 对于特性名来说都是合法字符，在所有支持 Vue.js 的浏览器都能被正确地解析** 。而且，它们 **不会出现在最终渲染的标记中** 。

## 对比其他框架