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

## 基础

### Vue.js 是什么

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。

- Vue 被设计为可以自底向上逐层应用。

- Vue 的核心库只关注视图层。

- Vue 能够为复杂的单页应用提供驱动。

[介绍视频](https://cn.vuejs.org/v2/guide)

<!-- more -->

### 起步

[在线尝试地址](https://jsfiddle.net/chrisvfritz/50wL7mdz/)

#### 引入 Vue

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

#### 声明式渲染

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

#### 条件与循环

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

#### 处理用户输入

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
#### 组件化应用构建

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
##### 与自定义元素的关系

 Vue 组件非常类似于自定义元素——它是 [Web 组件规范](https://www.w3.org/wiki/WebComponents/)的一部分，这是因为 Vue 的组件语法部分参考了该规范。例如 Vue 组件实现了 [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) 与 is 特性。但是，还是有几个关键差别：

1. Web Components 规范已经完成并通过，但未被所有浏览器原生实现。目前 Safari 10.1+、Chrome 54+ 和 Firefox 63+ 原生支持 Web Components。相比之下，**Vue 组件不需要任何 polyfill** ，并且在所有支持的浏览器 (IE9 及更高版本) 之下表现一致。必要时，Vue 组件也可以包装于原生自定义元素之内。

2. Vue 组件提供了纯自定义元素所不具备的一些重要功能，最突出的是 **跨组件数据流** 、**自定义事件通信** 以及 **构建工具集成** 。

### 实例

#### 创建一个 Vue 实例

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

#### 数据与方法

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

#### 实例生命周期钩子

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

#### 生命周期图示

下图展示了实例的生命周期。

![生命周期图示](https://graphbed.qiniu.songxingguo.com/cn-vuejs-org/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA.png)

### 模板语法

Vue.js 使用了 **基于 HTML 的模板语法** ，**允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据** 。所有 **Vue.js 的模板都是合法的 HTML** ，所以 **能被遵循规范的浏览器和 HTML 解析器解析** 。

在底层的实现上，**Vue 将模板编译成虚拟 DOM 渲染函数** 。结合响应系统，**Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少** 。

如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，**直接写渲染 (render) 函数** ，使用可选的 **JSX 语法** 。

#### 插值

##### 文本

数据绑定最常见的形式就是使用“Mustache”语法 (**双大括号**) 的文本插值：

```html
<span>Message: {{ msg }}</span>
```
**Mustache 标签将会被替代为对应数据对象上 msg 属性的值。** 无论何时，**绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新** 。

通过使用 **v-once** 指令，你也能执行 **一次性地插值** ，**当数据改变时，插值处的内容不会更新** 。但请留心 **这会影响到该节点上的其它数据绑定** ：

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```
##### 原始 HTML

**双大括号会将数据解释为普通文本** ，而非 HTML 代码。为了 **输出真正的 HTML** ，你需要使用 **v-html** 指令：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
这个 span 的内容将会被替换成为属性值 rawHtml，直接作为 HTML—— **会忽略解析属性值中的数据绑定** 。注意，你 **不能使用 v-html 来复合局部模板** ，因为 **Vue 不是基于字符串的模板引擎** 。反之，**对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位** 。

> 你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请 **只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值** 。

##### 特性

**Mustache 语法不能作用在 HTML 特性上** ，遇到这种情况应该使用 **v-bind** 指令：

```html
<div v-bind:id="dynamicId"></div>
```
在布尔特性的情况下，它们的存在即暗示为 true，v-bind 工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```
如果 **isButtonDisabled 的值是 null、undefined 或 false** ，则 **disabled 特性甚至不会被包含在渲染出来的 `<button>` 元素中** 。

##### 用 JavaScript 表达式

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

#### 指令

**指令 (Directives) 是带有 v- 前缀的特殊特性** 。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：

```html
<p v-if="seen">现在你看到我了</p>
```
这里，v-if 指令将根据表达式 seen 的值的真假来插入/移除 `<p>` 元素。

##### 参数

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

##### 修饰符

**修饰符** (Modifiers) 是以 **半角句号 . 指明的特殊后缀** ，用于 **指出一个指令应该以特殊方式绑定。** 例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 **event.preventDefault()** ：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```
#### 缩写

Vue.js 为 v-bind 和 v-on 这两个最常用的指令，提供了特定简写：

##### v-bind 缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```
##### v-on 缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```
它们看起来可能与普通的 HTML 略有不同，但 ** : 与 @ 对于特性名来说都是合法字符，在所有支持 Vue.js 的浏览器都能被正确地解析** 。而且，它们 **不会出现在最终渲染的标记中** 。

### 计算属性和侦听器

#### 计算属性

板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
所以，对于任何复杂逻辑，你都应当使用计算属性。

##### 基础例子

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```
结果：

```
Original message: "Hello"

Computed reversed message: "olleH"
```
这里我们声明了一个 **计算属性 reversedMessage** 。我们提供的函数将用作属性 **vm.reversedMessage 的 getter 函数** ：

```html
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```
你可以打开浏览器的控制台，自行修改例子中的 vm。vm.reversedMessage 的值始终取决于 vm.message 的值。

你可以像绑定普通属性一样在模板中绑定计算属性。Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，所有依赖 vm.reversedMessage 的绑定也会更新。而且最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 getter 函数是没有副作用 (side effect) 的，这使它更易于测试和理解。

##### 计算属性缓存 vs 方法

你可能已经注意到我们可以通过在表达式中调用方法来达到同样的效果：

```html
<p>Reversed message: "{{ reversedMessage() }}"</p>
```
```js
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，**不同的是计算属性是基于它们的依赖进行缓存的** 。**只在相关依赖发生改变时它们才会重新求值** 。这就意味着 **只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数** 。

这也同样意味着 **下面的计算属性将不再更新** ，因为 Date.now() 不是响应式依赖：

```js
computed: {
  now: function () {
    return Date.now()
  }
}
```
相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。

##### 计算属性 vs 侦听属性

Vue 提供了  **一种更通用的方式来观察和响应 Vue 实例上的数据变动** ：**侦听属性** 。当你有一些数据需要随着其它数据变动而变动时，你很 **容易滥用 watch** ——特别是如果你之前使用过 **AngularJS** 。然而，**通常更好的做法是使用计算属性而不是命令式的 watch 回调** 。细想一下这个例子：

```html
<div id="demo">{{ fullName }}</div>
```
```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
上面代码是命令式且重复的。将它与计算属性的版本进行比较：

```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

##### 计算属性的 setter

**计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter** ：

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```
现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。

#### 侦听器

虽然计算属性在大多数情况下更合适，但有时也需要一个 **自定义的侦听器** 。这就是为什么 Vue 通过 **watch 选项提供了一个更通用的方法**，来响应数据的变化。

例如：

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```
```html
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```
在这个示例中，使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

除了 watch 选项之外，您还可以使用命令式的 [vm.$watch API](https://cn.vuejs.org/v2/api/#vm-watch)。

### Class 与 Style 绑定

在将 v-bind 用于 class 和 style 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

#### 绑定 HTML Class

##### 对象语法

我们可以传给 v-bind:class 一个对象，以 **动态地切换 class** ：

```html
<div v-bind:class="{ active: isActive }"></div>
```
上面的语法表示 active 这个 class 存在与否将取决于数据属性 isActive 的 truthiness。

你可以在对象中传入更多属性来动态切换多个 class。此外，**v-bind:class 指令也可以与普通的 class 属性共存** 。当有如下模板:

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```
和如下 data：

```js
data: {
  isActive: true,
  hasError: false
}
```
结果渲染为：

```html
<div class="static active"></div>
```
当 isActive 或者 hasError 变化时，class 列表将相应地更新。例如，如果 hasError 的值为 true，class 列表将变为 "static active text-danger"。

**绑定的数据对象不必内联定义在模板里** ：

```html
<div v-bind:class="classObject"></div>
```
```js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
渲染的结果和上面一样。我们也 **可以在这里绑定一个返回对象的计算属性** 。这是一个 **常用且强大的模式** ：

```html
<div v-bind:class="classObject"></div>
```
```js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
##### 数组语法

我们可以把一个数组传给 v-bind:class，以应用一个 **class 列表** ：

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```
```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染为：

```html
<div class="active text-danger"></div>
```
如果你也想 **根据条件切换列表中的 class** ，可以用三元表达式：

```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
这样写将始终添加 errorClass，但是 **只有在 isActive 是 truthy[1] 时才添加 activeClass** 。

不过，当有多个条件 class 时这样写有些繁琐。所以 **在数组语法中也可以使用对象语法**：

```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```
##### 用在组件上

当在一个自定义组件上使用 class 属性时，**这些类将被添加到该组件的根元素上面** 。**这个元素上已经存在的类不会被覆盖** 。

例如，如果你声明了这个组件：

```js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
然后在使用它的时候添加一些 class：

```html
<my-component class="baz boo"></my-component>
```
HTML 将被渲染为:

```html
<p class="foo bar baz boo">Hi</p>
```
对于带数据绑定 class 也同样适用：

```html
<my-component v-bind:class="{ active: isActive }"></my-component>
```
当 isActive 为 truthy[1] 时，HTML 将被渲染成为：

```html
<p class="foo bar active">Hi</p>
```
#### 绑定内联样式

##### 对象语法

v-bind:style 的对象语法十分直观——看着非常像 CSS，但 **其实是一个 JavaScript 对象** 。CSS 属性名可以用 **驼峰式** (camelCase) 或 **短横线分隔** (kebab-case，记得用单引号括起来) 来命名：

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
```js
data: {
  activeColor: 'red',
  fontSize: 30
}
```
**直接绑定到一个样式对象通常更好** ，这会让模板更清晰：

```html
<div v-bind:style="styleObject"></div>
```
```js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
同样的，对象语法常常结合返回对象的计算属性使用。

##### 数组语法

**v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上** ：

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
##### 自动添加前缀

当 v-bind:style 使用需要 **添加浏览器引擎前缀的 CSS 属性** 时，如 transform， **Vue.js 会自动侦测并添加相应的前缀** 。

#####  多重值

>2.3.0+

从 2.3.0 起你可以为 style 绑定中的属性提供 **一个包含多个值的数组** ，常用于提供多个带前缀的值，例如：

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
**这样写只会渲染数组中最后一个被浏览器支持的值** 。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。

#### 条件渲染

##### v-if

在字符串模板中，比如 Handlebars，我们得像这样写一个条件块：

```html
<!-- Handlebars 模板 -->
{{#if ok}}
  <h1>Yes</h1>
{{/if}}
```
在 Vue 中，我们使用 v-if 指令实现同样的功能：

```html
<h1 v-if="ok">Yes</h1>
```
也可以用 v-else 添加一个“else 块”：

```html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```
##### 在 `<template>` 元素上使用 v-if 条件渲染分组
  
因为 v-if 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把**一个 `<template>` 元素当做不可见的包裹元素** ，并在上面使用 v-if。最终的渲染结果将不包含 `<template>` 元素。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```
##### v-else

你可以使用 v-else 指令来表示 v-if 的“else 块”：

```html
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```
v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。

##### v-else-if

>2.1.0 新增

v-else-if，顾名思义，充当 v-if 的“else-if 块”，可以连续使用：

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
类似于 v-else，v-else-if 也必须紧跟在带 v-if 或者 v-else-if 的元素之后。

##### 用 key 管理可复用的元素

Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。例如，如果你 **允许用户在不同的登录方式之间切换** ：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```
那么在上面的代码中切换 loginType 将 **不会清除用户已经输入的内容** 。因为 **两个模板使用了相同的元素，`<input>` 不会被替换掉**——仅仅是替换了它的 placeholder。

自己动手试一试，在输入框中输入一些文本，然后按下切换按钮：

这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“**这两个元素是完全独立的，不要复用它们** ”。**只需添加一个具有唯一值的 key 属性即可** ：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
现在，每次切换时，输入框都将被重新渲染。

> 注意，`<label>` 元素仍然会被高效地复用，因为它们没有添加 key 属性。

#### v-show

另一个用于根据条件展示元素的选项是 v-show 指令。用法大致一样：

```html
<h1 v-show="ok">Hello!</h1>
```
不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。 **v-show 只是简单地切换元素的 CSS 属性 display。**

>注意，**v-show 不支持 `<template>` 元素，也不支持 v-else**。

#### v-if vs v-show

v-if 是“真正”的条件渲染，因为它会 **确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建** 。

**v-if 也是惰性的** ：如果 **在初始渲染时条件为假** ，则什么也不做——**直到条件第一次变为真时，才会开始渲染条件块。**

相比之下，v-show 就简单得多——**不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换** 。

一般来说，**v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。** 因此，**如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。**

#### v-if 与 v-for 一起使用

**不推荐同时使用 v-if 和 v-for。** 请查阅 [风格指南](https://cn.vuejs.org/v2/style-guide/#%E9%81%BF%E5%85%8D-v-if-%E5%92%8C-v-for-%E7%94%A8%E5%9C%A8%E4%B8%80%E8%B5%B7-%E5%BF%85%E8%A6%81) 以获取更多信息。

**当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。** 请查阅 [列表渲染指南](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if) 以获取详细信息。

### 列表渲染

#### 用 v-for 把一个数组对应为一组元素

我们用 v-for 指令 **根据一组数组的选项列表进行渲染** 。**v-for 指令需要使用 item in items 形式的特殊语法** ，**items 是源数据数组** 并且 **item 是数组元素迭代的别名** 。

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```
```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
在 v-for 块中，我们 **拥有对父作用域属性的完全访问权限** 。**v-for 还支持一个可选的第二个参数为当前项的索引** 。

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```
```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
你也 **可以用 of 替代 in 作为分隔符** ，因为它是 **最接近 JavaScript 迭代器的语法** ：

```html
<div v-for="item of items"></div>
```
#### 一个对象的 v-for

你也可以用 **v-for 通过一个对象的属性来迭代** 。

```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```
```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```
你也可以提供 **第二个的参数为键名** ：

```html
<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>
```
**第三个参数为索引** ：

```html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```
>在遍历对象时，是 **按 Object.keys() 的结果遍历** ，但是 **不能保证它的结果在不同的 JavaScript 引擎下是一致的** 。

#### key

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果 **数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序** ， 而是 **简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素** 。这个类似 Vue 1.x 的 track-by="$index" 。

这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你 **需要为每项提供一个唯一 key 属性** 。**理想的 key 值是每项都有的唯一 id** 。这个特殊的属性相当于 Vue 1.x 的 track-by ，但它的工作方式类似于一个属性，所以你 **需要用 v-bind 来绑定动态值** (在这里使用简写)：

```html
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```
**建议尽可能在使用 v-for 时提供 key，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。**

因为它是 Vue 识别节点的一个通用机制，key 并不与 v-for 特别关联，key 还具有其他用途，我们将在后面的指南中看到其他用途。

#### 数组更新检测

##### 变异方法

Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

你打开控制台，然后用前面例子的 items 数组调用变异方法：example1.items.push({ message: 'Baz' }) 。

##### 替换数组

**变异方法** (mutation method)，顾名思义，**会改变被这些方法调用的原始数组** 。相比之下，也有 **非变异 (non-mutating method) 方法** ，例如：**filter(), concat() 和 slice() ** 。这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组：

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
你可能认为这将导致 Vue 丢弃现有 DOM 并重新渲染整个列表。幸运的是，事实并非如此。Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的、启发式的方法，所以 **用一个含有相同元素的数组去替换原来的数组是非常高效的操作** 。

##### 注意事项

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength
举个例子：

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```
为了解决第一类问题，以下两种方式都可以实现和 vm.items[indexOfItem] = newValue 相同的效果，同时也将触发状态更新：

```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```
你也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名：

```js
vm.$set(vm.items, indexOfItem, newValue)
```
为了解决第二类问题，你可以使用 splice：

```js
vm.items.splice(newLength)
```
##### 对象更改检测注意事项

还是由于 JavaScript 的限制，**Vue 不能检测对象属性的添加或删除** ：

```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
```
对于已经创建的实例，**Vue 不能动态添加根级别的响应式属性** 。但是，**可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性** 。例如，对于：

```js
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
```
你 **可以添加一个新的 age 属性到嵌套的 userProfile 对象** ：

```js
Vue.set(vm.userProfile, 'age', 27)
```
你还可以使用 **vm.$set** 实例方法，它只是全局 Vue.set 的别名：

```js
vm.$set(vm.userProfile, 'age', 27)
```
有时你可能需要 **为已有对象赋予多个新属性**       ，比如使用 Object.assign() 或 _.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：

```js
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```
你应该这样做：

```js
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```
##### 显示过滤/排序结果

有时，我们想要 **显示一个数组的过滤或排序副本** ，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。

例如：

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```
```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
**在计算属性不适用的情况下** (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：

```html
<li v-for="n in even(numbers)">{{ n }}</li>
```
```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
##### 一段取值范围的 v-for

v-for 也可以取整数。在这种情况下，它将重复多次模板。

```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
##### v-for on a `<template>`
  
类似于 v-if，你也可以利用带有 v-for 的 `<template>` 渲染多个元素。比如：

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```
##### v-for with v-if

当它们处于同一节点，**v-for 的优先级比 v-if 更高** ，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当你想为仅有的一些项渲染节点时，这种优先级的机制会十分有用，如下：

```html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```
上面的代码只传递了未完成的 todos。

而如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 (或 `<template>`)上。如：

```html
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```
##### 一个组件的 v-for

在自定义组件里，你可以像任何普通元素一样用 v-for 。

```html
<my-component v-for="item in items" :key="item.id"></my-component>
```
2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。

然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要用 props ：

```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```
不自动将 item 注入到组件里的原因是，这会使得组件与 v-for 的运作紧密耦合。明确组件数据的来源能够使组件在其他场合重复使用。

下面是一个简单的 todo list 的完整例子：

```html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```
> 注意这里的 is="todo-item" 属性。这种做法在使用 DOM 模板时是十分必要的，因为在 `<ul>` 元素内只有 `<li>` 元素会被看作有效内容。这样做实现的效果与 `<todo-item>` 相同，但是可以避开一些潜在的浏览器解析错误。查看 [DOM 模板解析说明](https://cn.vuejs.org/v2/guide/components.html#%E8%A7%A3%E6%9E%90-DOM-%E6%A8%A1%E6%9D%BF%E6%97%B6%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9) 来了解更多信息。

```js
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">Remove</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
```
### 事件处理

#### 监听事件

可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 **JavaScript 代码** 。

示例：

```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```
```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```
#### 事件处理方法

然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 v-on 指令中是不可行的。因此 v-on 还可以接收一个需要调用的 **方法名称** 。

示例：

```html
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>
```
```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```
#### 内联处理器中的方法

除了直接绑定到一个方法，也可以 **在内联 JavaScript 语句中调用方法** ：

```html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```
```js
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```
有时也需要 **在内联语句处理器中访问原始的 DOM 事件** 。可以用 **特殊变量 $event** 把它传入方法：

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```
```js
// ...
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
  }
}
```
#### 事件修饰符

在事件处理程序中调用 **event.preventDefault()** 或 **event.stopPropagation()** 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：**方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节** 。

为了解决这个问题，Vue.js 为 v-on 提供了 **事件修饰符** 。之前提过，**修饰符是由点开头的指令后缀来表示的** 。

- .stop
- .prevent
- .capture
- .self
- .once
- .passive

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```
使用修饰符时，**顺序很重要** ；相应的代码会以同样的顺序产生。因此，**用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击**。

2.1.4 新增

```html
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```
不像其它只能对原生的 DOM 事件起作用的修饰符，.once 修饰符还能被用到自定义的组件事件上。

2.3.0 新增

Vue 还对应 **addEventListener 中的 passive 选项** 提供了 .passive 修饰符。

```html
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```
这个 .passive 修饰符尤其能够提升移动端的性能。

**不要把 .passive 和 .prevent 一起使用** ，因为 **.prevent 将会被忽略，同时浏览器可能会向你展示一个警告** 。请记住，.passive 会告诉浏览器你不想阻止事件的默认行为。

#### 按键修饰符

在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 v-on 在监听键盘事件时添加 **按键修饰符** ：

```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
```
记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

```html
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```
全部的 **按键别名** ：

- .enter
-  .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
-  .up
- .down
- .left
- .right

可以通过 **全局 config.keyCodes 对象自定义按键修饰符别名** ：

```js
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

##### 自动匹配按键修饰符

2.5.0 新增

你也可直接 **将 KeyboardEvent.key 暴露的任意有效按键名转换为 kebab-case 来作为修饰符** ：

```html
<input @keyup.page-down="onPageDown">
```
在上面的例子中，处理函数仅在 $event.key === 'PageDown' 时被调用。

>有一些按键 (.esc 以及所有的方向键) 在 IE9 中有不同的 key 值, 如果你想支持 IE9，它们的内置别名应该是首选。

#### 系统修饰键

2.1.0 新增

可以用如下修饰符来实现仅 **在按下相应按键时才触发鼠标或键盘事件的监听器** 。

- .ctrl
- .alt
- .shift
- .meta

>注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

例如：

```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```
> 请注意修饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 ctrl 的情况下释放其它按键，才能触发 keyup.ctrl。而单单释放 ctrl 也不会触发事件。如果你想要这样的行为，请为 ctrl 换用 keyCode：keyup.17。

##### .exact 修饰符

2.5.0 新增

.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```
##### 鼠标按钮修饰符

2.2.0 新增

- .left
- .right
- .middle

这些修饰符会限制处理函数仅响应特定的鼠标按钮。

#### 为什么在 HTML 中监听事件?

实际上，使用 v-on 有几个好处：

- 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。

- 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，**和 DOM 完全解耦** ，更易于测试。

- **当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除** 。你无须担心如何清理它们。

### 表单输入绑定

#### 基础用法

你可以用 **v-model** 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上**创建双向数据绑定** 。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

对于需要使用输入法 (如中文、日文、韩文等) 的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 input 事件。

##### 文本

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

#### 多行文本

```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
在文本区域插值 (`<textarea></textarea>`) 并不会生效，应用 v-model 来代替。

##### 复选框

单个复选框，绑定到布尔值：

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
多个复选框，绑定到同一个数组：

```
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
```
```js
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
 Jack  John  Mike 
Checked names: []
单选按钮
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```


##### 选择框

单选时：

```html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```
```js
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```
> 如果 v-model 表达式的初始值未能匹配任何选项，`<select>` 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，**更推荐像上面这样提供一个值为空的禁用选项** 。

多选时 (**绑定到一个数组**)：

```html
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```
```js
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```
**用 v-for 渲染的动态选项** ：

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```
```js
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```
#### 值绑定

对于单选按钮，复选框及选择框的选项，**v-model 绑定的值通常是静态字符串** (对于复选框也可以是布尔值)：

```html
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```
但是有时我们可能想把值绑定到 Vue 实例的一个 **动态属性** 上，这时可以用 **v-bind** 实现，并且这个属性的值可以不是字符串。

##### 复选框

```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```
```js
// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```
> 这里的 true-value 和 false-value 特性并不会影响输入控件的 value 特性，因为浏览器在提交表单时并不会包含未被选中的复选框。如果要确保表单中这两个值中的一个能够被提交，(比如“yes”或“no”)，请换用单选按钮。

##### 单选按钮

```html
<input type="radio" v-model="pick" v-bind:value="a">
```
```js
// 当选中时
vm.pick === vm.a
```
##### 选择框的选项

```html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

```js
// 当选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```
##### 修饰符

- .lazy

在默认情况下，v-model 在 **每次 input 事件触发后将输入框的值与数据进行同步** (除了上述输入法组合文字时)。你可以添加 **lazy 修饰符** ，从而 **转变为使用 change 事件进行同步** ：

```HTML
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
````
- .number

如果想 **自动将用户的输入值转为数值类型** ，可以给 v-model 添加 number 修饰符：

```html
<input v-model.number="age" type="number">
```
这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。**如果这个值无法被 parseFloat() 解析，则会返回原始的值** 。

- .trim

如果要 **自动过滤用户输入的首尾空白字符** ，可以给 v-model 添加 trim 修饰符：

```html
<input v-model.trim="msg">
```

#### 在组件上使用 v-model

HTML 原生的输入元素类型并不总能满足需求。幸好，Vue 的组件系统允许你创建具有完全自定义行为且可复用的输入组件。这些输入组件甚至可以和 v-model 一起使用！要了解更多，请参阅组件指南中的 [自定义输入组件](https://cn.vuejs.org/v2/guide/components.html#%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-v-model)。

### 组件基础

基本示例
这里有一个 Vue 组件的示例：

```html
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```
组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 <button-counter>。我们可以在一个通过 new Vue 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```
```js
new Vue({ el: '#components-demo' })
```
因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 这样根实例特有的选项。

#### 组件的复用

你可以将组件进行任意次数的复用：

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```
注意当点击按钮时，**每个组件都会各自独立维护它的 count**。因为你每用一次组件，就会有一个它的新实例被创建。

##### data 必须是一个函数

当我们定义这个 `<button-counter>` 组件时，你可能会发现它的 data 并不是像这样直接提供一个对象：

```js
data: {
  count: 0
}
```
取而代之的是，**一个组件的 data 选项必须是一个函数** ，因此 **每个实例可以维护一份被返回对象的独立的拷贝** ：

```js
data: function () {
  return {
    count: 0
  }
}
```
##### 组件的组织

通常一个应用会以一棵嵌套的组件树的形式来组织：

![组件树](https://graphbed.qiniu.songxingguo.com/cn-vuejs-org/%E7%BB%84%E4%BB%B6%E6%A0%91.png)

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：全局注册和局部注册。至此，我们的组件都只是通过 **Vue.component 全局注册** 的：

```html
Vue.component('my-component-name', {
  // ... options ...
})
```
全局注册的组件可以用在其被注册之后的任何 (通过 new Vue) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

#### 通过 Prop 向子组件传递数据

早些时候，我们提到了创建一个博文组件的事情。问题是如果你不能向这个组件传递某一篇博文的标题或内容之类的我们想展示的数据的话，它是没有办法使用的。这也正是 prop 的由来。

Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 prop 特性的时候，它就变成了那个组件实例的一个属性。为了给博文组件传递一个标题，我们可以用一个 props 选项将其包含在该组件可接受的 prop 列表中：

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```
一个组件默认可以拥有 **任意数量的 prop** ，**任何值都可以传递给任何 prop** 。在上述模板中，你会发现我们能够在组件实例中访问这个值，就像访问 data 中的值一样。

一个 prop 被注册之后，你就可以像这样把数据作为一个自定义特性传递进来：

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```
然而在一个典型的应用中，你可能在 data 里有一个博文的数组：

```js
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})
```
并想要为每篇博文渲染一个组件：

```js
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```
如上所示，你会发现我们可以 **使用 v-bind 来动态传递 prop** 。这在你一开始不清楚要渲染的具体内容，比如从一个 API 获取博文列表的时候，是非常有用的。

#### 单个根元素

当构建一个 <blog-post> 组件时，你的模板最终会包含的东西远不止一个标题：

```html
<h3>{{ title }}</h3>
```
最最起码，你会包含这篇博文的正文：

```html
<h3>{{ title }}</h3>
<div v-html="content"></div>
```
然而如果你在模板中尝试这样写，Vue 会显示一个错误，并解释道 every component must have a single root element (每个组件必须只有一个根元素)。你可以将模板的内容包裹在一个父元素内，来修复这个问题，例如：

```html
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```
看起来当组件变得越来越复杂的时候，我们的博文不只需要标题和内容，还需要发布日期、评论等等。为每个相关的信息定义一个 prop 会变得很麻烦：

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
  v-bind:content="post.content"
  v-bind:publishedAt="post.publishedAt"
  v-bind:comments="post.comments"
></blog-post>
```
所以是时候重构一下这个 `<blog-post>` 组件了，让它变成接受一个单独的 post prop：

```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>
```
```js
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})
```
>上述的这个和一些接下来的示例使用了 JavaScript 的模板字符串来让多行的模板更易读。它们在 IE 下并没有被支持，所以如果你需要在不 (经过 Babel 或 TypeScript 之类的工具) 编译的情况下支持 IE，请使用折行转义字符取而代之。

现在，不论何时为 post 对象添加一个新的属性，它都会自动地在 `<blog-post>` 内可用。

#### 通过事件向父级组件发送消息

在我们开发 `<blog-post>` 组件时，它的一些功能可能要求我们和父级组件进行沟通。例如我们可能会引入一个可访问性的功能来放大博文的字号，同时让页面的其它部分保持默认的字号。

在其父组件中，我们可以通过添加一个 **postFontSize** 数据属性来支持这个功能：

```js
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
```
它可以在模板中用来控制所有博文的字号：

```html
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```
现在我们在每篇博文正文之前添加一个按钮来放大字号：

```js
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button>
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})
```
问题是这个按钮不会做任何事：

```html
<button>
  Enlarge text
</button>
```
当点击这个按钮时，我们需要告诉父级组件放大所有博文的文本。幸好 Vue 实例提供了一个自定义事件的系统来解决这个问题。我们可以调用内建的 **$emit** 方法并传入事件的名字，来向父级组件触发一个事件：

```html
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```
然后我们可以用 v-on 在博文组件上监听这个事件，就像监听一个原生 DOM 事件一样：

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```
##### 使用事件抛出一个值

有的时候用一个事件来抛出一个特定的值是非常有用的。例如我们可能想让 `<blog-post>` 组件决定它的文本要放大多少。这时可以使用 $emit 的第二个参数来提供这个值：

```html
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```
然后当在父级组件监听这个事件的时候，我们可以通过 **$event** 访问到被抛出的这个值：

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```
或者，如果这个事件处理函数是一个方法：

```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```
那么这个值将会作为第一个参数传入这个方法：

```js
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```
##### 在组件上使用 v-model

自定义事件也可以用于 **创建支持 v-model 的自定义输入组件** 。记住：

```html
<input v-model="searchText">
```
等价于：

```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```
当用在组件上时，v-model 则会这样：

```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```
为了让它正常工作，这个组件内的 `<input>` 必须：

- 将其 value 特性绑定到一个名叫 value 的 prop 上
- 在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出

写成代码之后是这样的：

```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```
现在 v-model 就应该可以在这个组件上完美地工作起来了：

```html
<custom-input v-model="searchText"></custom-input>
```
#### 通过插槽分发内容

和 HTML 元素一样，我们经常需要 **向一个组件传递内容** ，像这样：

```html
<alert-box>
  Something bad happened.
</alert-box>
```
可能会渲染出这样的东西：

幸好，Vue 自定义的 `<slot>` 元素让这变得非常简单：

```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```
#### 动态组件

有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：

上述内容可以通过 Vue 的 `<component>` 元素加一个特殊的 is 特性来实现：

```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```
在上述示例中，currentTabComponent 可以包括

- 已注册组件的名字，或

- 一个组件的选项对象

#### 解析 DOM 模板时的注意事项

有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。

这会导致我们使用这些有约束条件的元素时遇到一些问题。例如：

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```
这个 **自定义组件 `<blog-post-row>` 会被作为无效的内容提升到外部** ，并导致最终渲染结果出错。幸好这个特殊的 **is 特性给了我们一个变通的办法** ：

```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```
需要注意的是如果我们从以下来源使用模板的话，这条限制是不存在的：

- 字符串 (例如：template: '...')
- 单文件组件 (.vue)
- `<script type="text/x-template">`

## 深入了解组件

### 组件注册

#### 组件名

在注册一个组件的时候，我们始终需要给它一个名字。比如在全局注册的时候我们已经看到了：

```js
Vue.component('my-component-name', { /* ... */ })
```
该组件名就是 Vue.component 的第一个参数。

你给予组件的名字可能依赖于你打算拿它来做什么。当直接在 DOM 中使用一个组件 (而不是在字符串模板或单文件组件) 的时候，我们强烈推荐遵循 W3C 规范中的自定义组件名 (字母全小写且必须包含一个连字符)。这会帮助你避免和当前以及未来的 HTML 元素相冲突。

你可以在风格指南中查阅到关于组件名的其它建议。

#### 组件名大小写

定义组件名的方式有两种：

使用 kebab-case

```js
Vue.component('my-component-name', { /* ... */ })
```
当使用 kebab-case (**短横线分隔命名**) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case，例如 `<my-component-name>`。

使用 PascalCase

```js
Vue.component('MyComponentName', { /* ... */ })
```
当使用 PascalCase (**驼峰式命名**) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。

### 全局注册

到目前为止，我们只用过 **Vue.component** 来创建组件：

```js
Vue.component('my-component-name', {
  // ... 选项 ...
})
```
这些组件是全局注册的。也就是说它们在注册之后可以用在任何新创建的 Vue 根实例 (new Vue) 的模板中。比如：

```js
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```
```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>
```
在所有子组件中也是如此，也就是说这三个组件在各自内部也都可以相互使用。

#### 局部注册

全局注册往往是不够理想的。比如，如果你使用一个像 webpack 这样的构建系统，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。

在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```
然后在 components 选项中定义你想要使用的组件：

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```
对于 components 对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象。

注意 **局部注册的组件在其子组件中不可用** 。例如，如果你希望 ComponentA 在 ComponentB 中可用，则你需要这样写：

```js
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```
或者如果你通过 Babel 和 webpack 使用 ES2015 模块，那么代码看起来更像：

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```
注意在 ES2015+ 中，在对象中放一个类似 ComponentA 的变量名其实是 ComponentA: ComponentA 的缩写，即这个变量名同时是：

- 用在模板中的自定义元素的名称
- 包含了这个组件选项的变量名

### 模块系统

如果你没有通过 import/require 使用一个模块系统，也许可以暂且跳过这个章节。如果你使用了，那么我们会为你提供一些特殊的使用说明和注意事项。

#### 在模块系统中局部注册

如果你还在阅读，说明你使用了诸如 Babel 和 webpack 的模块系统。在这些情况下，我们推荐创建一个 components 目录，并将每个组件放置在其各自的文件中。

然后你需要在局部注册之前导入每个你想使用的组件。例如，在一个假设的 ComponentB.js 或 ComponentB.vue 文件中：

```js
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
```
现在 ComponentA 和 ComponentC 都可以在 ComponentB 的模板中使用了。

#### 基础组件的自动化全局注册

可能你的许多组件只是包裹了一个输入框或按钮之类的元素，是相对通用的。我们有时候会把它们称为基础组件，它们会在各个组件中被频繁的用到。

所以会导致很多组件里都会有一个包含基础组件的长列表：

```js
import BaseButton from './BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import BaseInput from './BaseInput.vue'

export default {
  components: {
    BaseButton,
    BaseIcon,
    BaseInput
  }
}
```
而只是用于模板中的一小部分：

```js
<BaseInput
  v-model="searchText"
  @keydown.enter="search"
/>
<BaseButton @click="search">
  <BaseIcon name="search"/>
</BaseButton>
```
幸好如果你使用了 webpack (或在内部使用了 webpack 的 Vue CLI 3+)，那么就可以 **使用 require.context 只全局注册这些非常通用的基础组件** 。这里有一份可以让你在应用入口文件 (比如 src/main.js) 中全局导入基础组件的示例代码：

```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 剥去文件名开头的 `./` 和结尾的扩展名
      fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```
记住全局注册的行为必须在根 Vue 实例 (通过 new Vue) 创建之前发生。

### Prop

####  Prop 的大小写 (camelCase vs kebab-case)

HTML 中的特性名是大小写不敏感的，所以 **浏览器会把所有大写字符解释为小写字符** 。这意味着当你使用 DOM 中的模板时，**camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名** ：

```js
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```
```html
<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```
重申一次，如果你使用字符串模板，那么这个限制就不存在了。

#### Prop 类型

到这里，我们只看到了以字符串数组形式列出的 prop：

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```
但是，通常你希望每个 prop 都有指定的值类型。这时，你可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型：

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```
这不仅为你的组件提供了文档，还会在它们遇到错误的类型时从浏览器的 JavaScript 控制台提示用户。你会在这个页面接下来的部分看到类型检查和其它 prop 验证。

##### 传递静态或动态 Prop

像这样，你已经知道了可以像这样给 prop 传入一个 **静态的值** ：

```js
<blog-post title="My journey with Vue"></blog-post>
```
你也知道 prop 可以 **通过 v-bind 动态赋值** ，例如：

```js
<!-- 动态赋予一个变量的值 -->
<blog-post v-bind:title="post.title"></blog-post>

<!-- 动态赋予一个复杂表达式的值 -->
<blog-post v-bind:title="post.title + ' by ' + post.author.name"></blog-post>
```
在上述两个示例中，我们传入的值都是字符串类型的，但实际上 **任何类型的值都可以传给一个 prop** 。

##### 传入一个数字

```html
<!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:likes="42"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:likes="post.likes"></blog-post>
```
##### 传入一个布尔值

```html
<!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>

<!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:is-published="false"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
```
##### 传入一个数组

```html
<!-- 即便数组是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
```
##### 传入一个对象

```html
<!-- 即便对象是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:author="{ name: 'Veronica', company: 'Veridian Dynamics' }"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:author="post.author"></blog-post>
```
##### 传入一个对象的所有属性

如果你想要将一个对象的所有属性都作为 prop 传入，你可以使用不带参数的 v-bind (取代 v-bind:prop-name)。例如，对于一个给定的对象 post：

```js
post: {
  id: 1,
  title: 'My Journey with Vue'
}
```
下面的模板：

```html
<blog-post v-bind="post"></blog-post>
```
等价于：

```html
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```
#### 单向数据流

所有的 prop 都使得其父子 prop 之间形成了一个 **单向下行绑定** ：**父级 prop 的更新会向下流动到子组件中** ，但是反过来则不行。这样会 **防止从子组件意外改变父级组件的状态** ，从而导致你的应用的数据流向难以理解。

额外的，**每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值** 。这意味着你不应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

这里有两种常见的试图改变一个 prop 的情形：

1. 这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值：

  ```html
  props: ['initialCounter'],
  data: function () {
    return {
      counter: this.initialCounter
    }
  }
  ```
2. 这个 prop 以一种原始的值传入且需要进行转换。在这种情况下，最好使用这个 prop 的值来定义一个计算属性：

  ```html
  props: ['size'],
  computed: {
    normalizedSize: function () {
      return this.size.trim().toLowerCase()
    }
  }
  ```
> 注意在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身将会影响到父组件的状态。

#### Prop 验证

为了定制 prop 的验证方式，你可以为 props 中的值提供 **一个带有验证需求的对象** ，而不是一个字符串数组。例如：

```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```
当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

>注意那些 prop 会在一个组件实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 default 或 validator 函数中是不可用的。

##### 类型检查

type 可以是下列原生构造函数中的一个：

- String
- Number
- Boolean
-  Array
- Object
- Date
- Function
- Symbol

额外的，**type 还可以是一个自定义的构造函数** ，并且**通过 instanceof 来进行检查确认** 。例如，给定下列现成的构造函数：

```js
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```
你可以使用：

```js
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```
来验证 author prop 的值是否是通过 new Person 创建的。

#### 非 Prop 的特性

一个非 prop 特性是指 **传向一个组件，但是该组件并没有相应 prop 定义的特性** 。

因为显式定义的 prop 适用于向一个子组件传入信息，然而组件库的作者并不总能预见组件会被用于怎样的场景。这也是为什么组件可以接受任意的特性，而这些特性会被添加到这个组件的根元素上。

例如，想象一下你通过一个 Bootstrap 插件使用了一个第三方的 <bootstrap-date-input> 组件，这个插件需要在其 `<input>` 上用到一个 data-date-picker 特性。我们可以将这个特性添加到你的组件实例上：

```html
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
```
然后这个 data-date-picker="activated" 特性就会自动添加到 `<bootstrap-date-input>` 的根元素上。

##### 替换/合并已有的特性

想象一下 `<bootstrap-date-input>` 的模板是这样的：

```html
<input type="date" class="form-control">
```
为了给我们的日期选择器插件定制一个主题，我们可能需要像这样添加一个特别的类名：

```html
<bootstrap-date-input
  data-date-picker="activated"
  class="date-picker-theme-dark"
></bootstrap-date-input>
```
在这种情况下，我们定义了两个不同的 class 的值：

- form-control，这是在组件的模板内设置好的
- date-picker-theme-dark，这是从组件的父级传入的

对于绝大多数特性来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 type="text" 就会替换掉 type="date" 并把它破坏！庆幸的是，**class 和 style 特性会稍微智能一些，即两边的值会被合并起来，从而得到最终的值：form-control date-picker-theme-dark。**

##### 禁用特性继承

如果你不希望组件的根元素继承特性，你可以在组件的选项中设置 inheritAttrs: false。例如：

```js
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```
这尤其适合配合实例的 $attrs 属性使用，该属性包含了传递给一个组件的特性名和特性值，例如：

```js
{
  class: 'username-input',
  placeholder: 'Enter your username'
}
```
有了 inheritAttrs: false 和 $attrs，你就可以手动决定这些特性会被赋予哪个元素。在撰写基础组件的时候是常会用到的：

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    </label>
  `
})
```
这个模式允许你在使用基础组件的时候更像是使用原始的 HTML 元素，而不会担心哪个元素是真正的根元素：

```js
<base-input
  v-model="username"
  class="username-input"
  placeholder="Enter your username"
></base-input>
```
### 自定义事件

#### 事件名

不同于组件和 prop，事件名不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。举个例子，如果触发一个 camelCase 名字的事件：

```js
this.$emit('myEvent')
```
则监听这个名字的 kebab-case 版本是不会有任何效果的：

```html
<my-component v-on:my-event="doSomething"></my-component>
```
不同于组件和 prop，事件名不会被用作一个 JavaScript 变量名或属性名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 v-on:myEvent 将会变成 v-on:myevent——导致 myEvent 不可能被监听到。

因此，我们推荐你始终使用 kebab-case 的事件名。

#### 自定义组件的 v-model

2.2.0+ 新增

一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value 特性用于不同的目的。model 选项可以用来避免这样的冲突：

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```
现在在这个组件上使用 v-model 的时候：

```html
<base-checkbox v-model="lovingVue"></base-checkbox>
```
这里的 lovingVue 的值将会传入这个名为 checked 的 prop。同时当 `<base-checkbox>` 触发一个 change 事件并附带一个新的值的时候，这个 lovingVue 的属性将会被更新。

> 注意你仍然需要在组件的 props 选项里声明 checked 这个 prop。

### 将原生事件绑定到组件

你可能有很多次想要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 v-on 的 .native 修饰符：


```html
<base-input v-on:focus.native="onFocus"></base-input>
```
在有的时候这是很有用的，不过在你尝试监听一个类似 `<input>` 的非常特定的元素时，这并不是个好主意。比如上述 <base-input> 组件可能做了如下重构，所以根元素实际上是一个 `<label>` 元素：

```html
<label>
  {{ label }}
  <input
    v-bind="$attrs"
    v-bind:value="value"
    v-on:input="$emit('input', $event.target.value)"
  >
</label>
```
这时，父级的 .native 监听器将静默失败。它不会产生任何报错，但是 onFocus 处理函数不会如你预期地被调用。

为了解决这个问题，Vue 提供了一个 $listeners 属性，它是一个对象，里面包含了作用在这个组件上的所有监听器。例如：

```html
{
  focus: function (event) { /* ... */ }
  input: function (value) { /* ... */ },
}
```
有了这个 $listeners 属性，你就可以配合 v-on="$listeners" 将所有的事件监听器指向这个组件的某个特定的子元素。对于类似 <input> 的你希望它也可以配合 v-model 工作的组件来说，为这些监听器创建一个类似下述 inputListeners 的计算属性通常是非常有用的：

```html
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  computed: {
    inputListeners: function () {
      var vm = this
      // `Object.assign` 将所有的对象合并为一个新对象
      return Object.assign({},
        // 我们从父级添加所有的监听器
        this.$listeners,
        // 然后我们添加自定义监听器，
        // 或覆写一些监听器的行为
        {
          // 这里确保组件配合 `v-model` 的工作
          input: function (event) {
            vm.$emit('input', event.target.value)
          }
        }
      )
    }
  },
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on="inputListeners"
      >
    </label>
  `
})
```
现在 `<base-input>` 组件是一个完全透明的包裹器了，也就是说它可以完全像一个普通的 <input> 元素一样使用了：所有跟它相同的特性和监听器的都可以工作。

#### .sync 修饰符

2.3.0+ 新增

在有些情况下，我们可能 **需要对一个 prop 进行“双向绑定”** 。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。

这也是为什么我们推荐以 update:myPropName 的模式触发事件取而代之。举个例子，在一个包含 title prop 的假设的组件中，我们可以用以下方法表达对其赋新值的意图：

```html
this.$emit('update:title', newTitle)
```
然后父组件可以监听那个事件并根据需要更新一个本地的数据属性。例如：

```html
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```
为了方便起见，我们为这种模式提供一个缩写，即 .sync 修饰符：

```html
<text-document v-bind:title.sync="doc.title"></text-document>
```
>注意带有 .sync 修饰符的 v-bind 不能和表达式一起使用 (例如 v-bind:title.sync=”doc.title + ‘!’” 是无效的)。取而代之的是，你只能提供你想要绑定的属性名，类似 v-model。

当我们用一个对象同时设置多个 prop 的时候，也可以将这个 .sync 修饰符和 v-bind 配合使用：

```html
<text-document v-bind.sync="doc"></text-document>
```
这样会把 doc 对象中的每一个属性 (如 title) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器。

>将 v-bind.sync 用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”，是无法正常工作的，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。

## 对比其他框架