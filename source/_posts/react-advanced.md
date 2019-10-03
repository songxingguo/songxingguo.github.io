title: React进阶
author: songxingguo
tags:
  - React
categories:
  - 开发者手册
date: 2018-03-26 16:44:00
---
## React进阶

### 前端应用状态管理 —— 状态提升

我们将这种组件之间共享的状态交给组件最近的公共父节点保管，然后通过 props 把状态传递给子组件，这样就可以在组件之间共享数据了。

总结一下：当某个状态被多个组件依赖或者影响的时候，就把该状态提升到这些组件的最近公共父组件中去管理，用 props 传递数据或者函数来管理这种依赖或着影响的行为。

你可以看到 React.js 并没有提供好的解决方案来管理这种组件之间的共享状态。在实际项目当中状态提升并不是一个好的解决方案，所以我们后续会引入 Redux 这样的状态管理工具来帮助我们来管理这种共享状态，但是在讲解到 Redux 之前，我们暂时采取状态提升的方式来进行管理。

对于不会被多个组件依赖和影响的状态（例如某种下拉菜单的展开和收起状态），一般来说只需要保存在组件内部即可，不需要做提升或者特殊的管理。

<!-- more -->

### 挂载阶段的组件生命周期（一）

React.js 将组件渲染，并且构造 DOM 元素然后塞入页面的过程称为组件的挂载。这一节我们学习了 React.js 控制组件在页面上挂载和删除过程里面几个方法：

componentWillMount：组件挂载开始之前，也就是在组件调用 render 方法之前调用。
componentDidMount：组件挂载完成以后，也就是 DOM 元素已经插入页面后调用。
componentWillUnmount：组件对应的 DOM 元素从页面中删除之前调用。
但这一节并没有讲这几个方法到底在实际项目当中有什么作用，下一节我们通过例子来讲解一下这几个方法的用途。

### 挂载阶段的组件生命周期（二）
一般来说，所有关于组件自身的状态的初始化工作都会放在 constructor 里面去做。你会发现本书所有组件的 state 的初始化工作都是放在 constructor 里面的。

一些组件启动的动作，包括像 Ajax 数据的拉取操作、一些定时器的启动等，就可以放在 componentWillMount 里面进行，例如 Ajax：
```bash
...
  componentWillMount () {
    ajax.get('http://json-api.com/user', (userData) => {
      this.setState({ userData })
    })
  }
...
```
多次的隐藏和显示会让 React.js 重新构造和销毁 Clock 组件，每次构造都会重新构建一个定时器。而销毁组件的时候没有清除定时器，所以你看到报错会越来越多。而且因为 JavaScript 的闭包特性，这样会导致严重的内存泄漏。

这时候componentWillUnmount 就可以派上用场了，它的作用就是在组件销毁的时候，做这种清场的工作。例如清除该组件的定时器和其他的数据清理工作。我们给 Clock 添加 componentWillUnmount，在组件销毁的时候清除该组件的定时器：

```bash
...
  componentWillUnmount () {
    clearInterval(this.timer)
  }
...
```
我们一般会把组件的 state 的初始化工作放在 constructor 里面去做；在 componentWillMount 进行组件的启动工作，例如 Ajax 数据拉取、定时器的启动；组件从页面上销毁的时候，有时候需要一些数据的清理，例如定时器的清理，就会放在 componentWillUnmount 里面去做。

说一下本节没有提到的 componentDidMount 。一般来说，有些组件的启动工作是依赖 DOM 的，例如动画的启动，而 componentWillMount 的时候组件还没挂载完成，所以没法进行这些启动工作，这时候就可以把这些操作放在 componentDidMount 当中。componentDidMount 的具体使用我们会在接下来的章节当中结合 DOM 来讲。

### 更新阶段的组件生命周期

从之前的章节我们了解到，组件的挂载指的是将组件渲染并且构造 DOM 元素然后插入页面的过程。这是一个从无到有的过程，React.js 提供一些生命周期函数可以给我们在这个过程中做一些操作。

除了挂载阶段，还有一种“更新阶段”。说白了就是 setState 导致 React.js 重新渲染组件并且把组件的变化应用到 DOM 元素上的过程，这是一个组件的变化过程。而 React.js 也提供了一系列的生命周期函数可以让我们在这个组件更新的过程执行一些操作。

关于更新阶段的组件生命周期：

shouldComponentUpdate(nextProps, nextState)：你可以通过这个方法控制组件是否重新渲染。如果返回 false 组件就不会重新渲染。这个生命周期在 React.js 性能优化上非常有用。
componentWillReceiveProps(nextProps)：组件从父组件接收到新的 props 之前调用。
componentWillUpdate()：组件开始重新渲染之前调用。
componentDidUpdate()：组件重新渲染并且把更改变更到真实的 DOM 以后调用。

但这里建议大家可以先简单了解 React.js 组件是有更新阶段的，并且有这么几个更新阶段的生命周期即可。然后在深入项目实战的时候逐渐地掌握理解他们，现在并不需要对他们放过多的精力。

irtual-DOM 策略:
https://github.com/livoras/blog/issues/13

### ref 和 React.js 中的 DOM 操作

在 React.js 当中你基本不需要和 DOM 直接打交道。React.js 提供了一系列的 on* 方法帮助我们进行事件监听，所以 React.js 当中不需要直接调用 addEventListener 的 DOM API；以前我们通过手动 DOM 操作进行页面更新（例如借助 jQuery），而在 React.js 当中可以直接通过 setState 的方式重新渲染组件，渲染的时候可以把新的 props 传递给子组件，从而达到页面更新的效果。

React.js 这种重新渲染的机制帮助我们免除了绝大部分的 DOM 更新操作，也让类似于 jQuery 这种以封装 DOM 操作为主的第三方的库从我们的开发工具链中删除。

但是 React.js 并不能完全满足所有 DOM 操作需求，有些时候我们还是需要和 DOM 打交道。比如说你想进入页面以后自动 focus 到某个输入框，你需要调用 input.focus() 的 DOM API，比如说你想动态获取某个 DOM 元素的尺寸来做后续的动画，等等。

React.js 当中提供了 ref 属性来帮助我们获取已经挂载的元素的 DOM 节点，你可以给某个 JSX 元素加上 ref属性：
```bash
class AutoFocusInput extends Component {
  componentDidMount () {
    this.input.focus()
  }

  render () {
    return (
      <input ref={(input) => this.input = input} />
    )
  }
}

ReactDOM.render(
  <AutoFocusInput />,
  document.getElementById('root')
)
```
可以看到我们给 input 元素加了一个 ref 属性，这个属性值是一个函数。当 input 元素在页面上挂载完成以后，React.js 就会调用这个函数，并且把这个挂载以后的 DOM 节点传给这个函数。在函数中我们把这个 DOM 元素设置为组件实例的一个属性，这样以后我们就可以通过 this.input 获取到这个 DOM 元素。

然后我们就可以在 componentDidMount 中使用这个 DOM 元素，并且调用 this.input.focus() 的 DOM API。整体就达到了页面加载完成就自动 focus 到输入框的功能（大家可以注意到我们用上了 componentDidMount 这个组件生命周期）。

我们可以给任意代表 HTML 元素标签加上 ref 从而获取到它 DOM 元素然后调用 DOM API。但是记住一个原则：能不用 ref 就不用。特别是要避免用 ref 来做 React.js 本来就可以帮助你做到的页面自动更新的操作和事件监听。多余的 DOM 操作其实是代码里面的“噪音”，不利于我们理解和维护。

顺带一提的是，其实可以给组件标签也加上 ref ，例如：
```bash
<Clock ref={(clock) => this.clock = clock} />
```
这样你获取到的是这个 Clock 组件在 React.js 内部初始化的实例。但这并不是什么常用的做法，而且也并不建议这么做，所以这里就简单提及，有兴趣的朋友可以自己学习探索。

### props.children 和容器类组件


### dangerouslySetHTML 和 style 属性

#### dangerouslySetHTML
于安全考虑的原因（XSS 攻击），在 React.js 当中所有的表达式插入的内容都会被自动转义，就相当于 jQuery 里面的 text(…) 函数一样，任何的 HTML 格式都会被转义掉：

```bash
class Editor extends Component {
  constructor() {
    super()
    this.state = {
      content: '<h1>React.js 小书</h1>'
    }
  }

  render () {
    return (
      <div className='editor-wrapper'>
        {this.state.content}
      </div>
    )
  }
}
```
表达式插入并不会把一个渲染到页面，而是把它的文本形式渲染了。那要怎么才能做到设置动态 HTML 结构的效果呢？React.js 提供了一个属性 dangerouslySetInnerHTML，可以让我们设置动态设置元素的 innerHTML：

```bash
...
  render () {
    return (
      <div
        className='editor-wrapper'
        dangerouslySetInnerHTML={{__html: this.state.content}} />
    )
  }
...
```
需要给 dangerouslySetInnerHTML 传入一个对象，这个对象的 __html 属性值就相当于元素的 innerHTML，这样我们就可以动态渲染元素的 innerHTML 结构了。

有写朋友会觉得很奇怪，为什么要把一件这么简单的事情搞得这么复杂，名字又长，还要传入一个奇怪的对象。那是因为设置 innerHTML 可能会导致跨站脚本攻击（XSS），所以 React.js 团队认为把事情搞复杂可以防止（警示）大家滥用这个属性。这个属性不必要的情况就不要使用。

#### style

在 React.js 中你需要把 CSS 属性变成一个对象再传给元素：
```bash
<h1 style={{fontSize: '12px', color: 'red'}}>React.js 小书</h1>
```
style 接受一个对象，这个对象里面是这个元素的 CSS 属性键值对，原来 CSS 属性中带 - 的元素都必须要去掉 - 换成驼峰命名，如 font-size 换成 fontSize，text-align 换成 textAlign。

用对象作为 style 方便我们动态设置元素的样式。我们可以用 props 或者 state 中的数据生成样式对象再传给元素，然后用 setState 就可以修改样式，非常灵活：

```bash
<h1 style={{fontSize: '12px', color: this.state.color}}>React.js 小书</h1>
```
只要简单地 setState({color: 'blue'}) 就可以修改元素的颜色成蓝色。

### PropTypes 和组件参数验证

都说 JavaScript 是一门灵活的语言 —— 这就是像是说“你是个好人”一样，凡事都有背后没有说出来的话。JavaScript 的灵活性体现在弱类型、高阶函数等语言特性上。而语言的弱类型一般来说确实让我们写代码很爽，但是也很容易出 bug。

变量没有固定类型可以随意赋值，在我们构建大型应用程序的时候并不是什么好的事情。你写下了 let a = {} ，如果这是个共享的状态并且在某个地方把 a = 3，那么 a.xxx 就会让程序崩溃了。而这种非常隐晦但是低级的错误在强类型的语言例如 C/C++、Java 中是不可能发生的，这些代码连编译都不可能通过，也别妄图运行。

大型应用程序的构建其实更适合用强类型的语言来构建，它有更多的规则，可以帮助我们在编写代码阶段、编译阶段规避掉很多问题，让我们的应用程序更加的安全。JavaScript 早就脱离了玩具语言的领域并且投入到大型的应用程序的生产活动中，因为它的弱类型，常常意味着不是很安全。所以近年来出现了类似 TypeScript 和 Flow 等技术，来弥补 JavaScript 这方面的缺陷。

于是 React.js 就提供了一种机制，让你可以给组件的配置参数加上类型验证，就用上述的评论组件例子，你可以配置 Comment 只能接受对象类型的 comment 参数，你传个数字进来组件就强制报错。我们这里先安装一个 React 提供的第三方库 prop-types：

它可以帮助我们验证 props 的参数类型，例如：
```bash
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class Comment extends Component {
  static propTypes = {
    comment: PropTypes.object
  }

  render () {
    const { comment } = this.props
    return (
      <div className='comment'>
        <div className='comment-user'>
          <span>{comment.username} </span>：
        </div>
        <p>{comment.content}</p>
      </div>
    )
  }
}
```
注意我们在文件头部引入了 PropTypes，并且给 Comment 组件类添加了类属性 propTypes，里面的内容的意思就是你传入的 comment 类型必须为 object（对象）。

虽然 propTypes 帮我们指定了参数类型，但是并没有说这个参数一定要传入，事实上，这些参数默认都是可选的。可选参数我们可以通过配置 defaultProps，让它在不传入的时候有默认值。但是我们这里并没有配置 defaultProps，所以如果直接用 <Comment /> 而不传入任何参数的话，comment 就会是 undefined，comment.username 会导致程序报错：

![程序报错](https://graphbed.qiniu.songxingguo.com/pasted-4.png)

这个出错信息并不够友好。我们可以通过 isRequired 关键字来强制组件某个参数必须传入：
```bash
...
static propTypes = {
  comment: PropTypes.object.isRequired
}
...
```
React.js 提供的 PropTypes 提供了一系列的数据类型可以用来配置组件的参数：
```bash
PropTypes.array
PropTypes.bool
PropTypes.func
PropTypes.number
PropTypes.object
PropTypes.string
PropTypes.node
PropTypes.element
...
```
更多类型及其用法可以参看官方文档： Typechecking With PropTypes - React。
https://facebook.github.io/react/docs/typechecking-with-proptypes.html

组件参数验证在构建大型的组件库的时候相当有用，可以帮助我们迅速定位这种类型错误，让我们组件开发更加规范。另外也起到了一个说明文档的作用，如果大家都约定都写 propTypes ，那你在使用别人写的组件的时候，只要看到组件的 propTypes 就清晰地知道这个组件到底能够接受什么参数，什么参数是可选的，什么参数是必选的。

#### 总结

通过 PropTypes 给组件的参数做类型限制，可以在帮助我们迅速定位错误，这在构建大型应用程序的时候特别有用；另外，给组件加上 propTypes，也让组件的开发、使用更加规范清晰。

这里建议大家写组件的时候尽量都写 propTypes，有时候有点麻烦，但是是值得的。
