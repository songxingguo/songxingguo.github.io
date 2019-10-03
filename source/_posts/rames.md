title: React、Vue、AngularJS 、Angular 前端框架的异同
author: songxingguo
tags: []
categories:
  - 开发者手册
date: 2019-02-14 10:40:00
---
## React 

### 基本特点

- 使用 Virtual DOM 。
- 提供了响应式 (Reactive) 和组件化 (Composable) 的视图组件。
- 将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库。

> React 比 Vue 更好的地方，比如更丰富的生态系统。

<!-- more -->

### 运行时性能

在 React 应用中，当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树。

### HTML & CSS

在 React 中，一切都是 JavaScript。不仅仅是 HTML 可以用 JSX 来表达，现在的潮流也越来越多地将 CSS 也纳入到 JavaScript 中来处理。

**JSX vs Templates**

> 在 React 中，所有的组件的渲染功能都依靠 JSX。JSX 是使用 XML 语法编写 JavaScript 的一种语法糖。

使用 JSX 的渲染函数优势：

- 可以使用完整的编程语言 JavaScript 功能来构建你的视图页面。

- 开发工具对 JSX 的支持相比于现有可用的其他 Vue 模板还是比较先进的 (比如，linting、类型检查、编辑器的自动完成)。

Vue 模板的优势：

- 对于很多习惯了 HTML 的开发者来说，模板比起 JSX 读写起来更自然。

- 基于 HTML 的模板使得将已有的应用逐步迁移到 Vue 更为容易。

- 这也使得设计师和新人开发者更容易理解和参与到项目中。

- 你甚至可以使用其他模板预处理器，比如 Pug 来书写 Vue 的模板。 

> 把组件区分为两类：一类是偏视图表现的 (presentational)，一类则是偏逻辑的 (logical)。

**组件作用域内的 CSS**

CSS 作用域在 React 中是通过 CSS-in-JS 的方案实现的 (比如 styled-components、glamorous 和 emotion)。

### 规模

**向上扩展**

React 使用 Flux、Redux 进行状态管理。

**向下扩展**

React 学习曲线陡峭，在你开始学 React 前，你需要知道 JSX 和 ES2015，因为许多示例用的是这些语法。

### 原生渲染

React Native 能使你用相同的组件模型编写有本地渲染能力的 APP (iOS 和 Android)。能同时跨多平台开发，对开发者是非常棒的。

### Preact 和其它类 React 库

类 React 的库们往往尽可能地与 React 共享 API 和生态。

## AngularJS

### 数据绑定

AngularJS 使用双向绑定，Vue 在不同组件间强制使用单向数据流。

### 指令与组件

在 Vue 中指令和组件分得更清晰。指令只封装 DOM 操作，而组件代表一个自给自足的独立单元——有自己的视图和数据逻辑。

在 AngularJS 中，每件事都由指令来做，而组件只是一种特殊的指令。

### 运行时性能

Vue 有更好的性能，并且非常非常容易优化，因为它不使用脏检查。

在 AngularJS 中，当 watcher 越来越多时会变得越来越慢，因为作用域内的每一次变化，所有 watcher 都要重新计算。并且，如果一些 watcher 触发另一个更新，脏检查循环 (digest cycle) 可能要运行多次。

Vue 则根本没有这个问题，因为它使用基于依赖追踪的观察系统并且异步队列更新，所有的数据变化都是独立触发，除非它们之间有明确的依赖关系。

## Angular

### TypeScript

Angular 事实上必须用 TypeScript 来开发，因为它的文档和学习资源几乎全部是面向 TS 的。TS 有很多好处——静态类型检查在大规模的应用中非常有用，同时对于 Java 和 C# 背景的开发者也是非常提升开发效率的。

### 运行时性能

这两个框架都很快，有非常类似的 benchmark 数据。

### 体积

在体积方面，最近的 Angular 版本中在使用了 AOT 和 tree-shaking 技术后使得最终的代码体积减小了许多。但即使如此，一个包含了 Vuex + Vue Router 的 Vue 项目 (gzip 之后 30kB) 相比使用了这些优化的 angular-cli 生成的默认项目尺寸 (~65KB) 还是要小得多。

### 学习曲线

Angular 本身的复杂度是因为它的设计目标就是只针对大型的复杂应用；但不可否认的是，这也使得它对于经验不甚丰富的开发者相当的不友好。

### 其他框架

Ember 、Knockout、 Polymer、Riot

## Vue
