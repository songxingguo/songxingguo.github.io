---
title: "Vue指令速查\U0001F680"
urlname: nholynuhnqrhg0tg
date: '2023-06-17 08:40:16 +0000'
tags: []
categories: []
---

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fg2BDn7uyE_NJeeZ1oP_r6bF8g5R.jpeg)

<!--more-->

# 静态 API

## 全局配置

用法：`Vue.config.xxx`

| **选项**                                                                           | **描述**                                                                                                                                                                                                                       |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [**silent**](https://v2.cn.vuejs.org/v2/api/#silent)                               | 配置是否取消 Vue 所有的日志与警告。                                                                                                                                                                                            |
| [**optionMergeStrategies**](https://v2.cn.vuejs.org/v2/api/#optionMergeStrategies) | [自定义选项的混入策略](https://v2.cn.vuejs.org/v2/guide/mixins.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%80%89%E9%A1%B9%E6%B7%B7%E5%85%A5%E7%AD%96%E7%95%A5)。                                                                       |
| [**devtools**](https://v2.cn.vuejs.org/v2/api/#devtools)                           | 配置是否允许 [vue-devtools](https://github.com/vuejs/vue-devtools) 检查代码。开发版本默认为 true，生产版本默认为 false。生产版本设为 true 可以启用检查。                                                                       |
| [**errorHandler**](https://v2.cn.vuejs.org/v2/api/#errorHandler)                   | 指定组件的渲染和观察期间未捕获错误的处理函数。这个处理函数被调用时，可获取错误信息和 Vue 实例。                                                                                                                                |
| [**warnHandler**](https://v2.cn.vuejs.org/v2/api/#warnHandler)                     | Vue 的运行时警告赋予一个自定义处理函数。注意这只会在开发者环境下生效，在生产环境下它会被忽略。                                                                                                                                 |
| [**ignoredElements**](https://v2.cn.vuejs.org/v2/api/#ignoredElements)             | 须使 Vue 忽略在 Vue 之外的自定义元素\*\* \*\*(e.g. 使用了 Web Components APIs)。否则，它会假设你忘记注册全局组件或者拼错了组件名称，从而抛出一个关于 Unknown custom element 的警告。                                           |
| [**keyCodes**](https://v2.cn.vuejs.org/v2/api/#keyCodes)                           | 给 v-on 自定义键位别名。                                                                                                                                                                                                       |
| [**performance**](https://v2.cn.vuejs.org/v2/api/#performance)                     | 设置为 true 以在浏览器开发工具的性能/时间线面板中启用对组件初始化、编译、渲染和打补丁的性能追踪。只适用于开发模式和支持 [performance.mark](https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark) API 的浏览器上。 |
| [**productionTip**](https://v2.cn.vuejs.org/v2/api/#productionTip)                 | 设置为 false 以阻止 vue 在启动时生成生产提示。                                                                                                                                                                                 |

## 全局 API

用法：`Vue.xxx()`

| **选项**                                                                                                                                                                                                                                                                                        | **描述**                                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [**Vue.extend()**](https://v2.cn.vuejs.org/v2/api/#Vue-extend)                                                                                                                                                                                                                                  | 使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。data 选项是特例，需要注意 - 在 Vue.extend() 中它必须是函数。                                                                                                                                                                              |
| [**Vue.nextTick()**](https://v2.cn.vuejs.org/v2/api/#Vue-nextTick)                                                                                                                                                                                                                              | 在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。[异步更新队列](https://v2.cn.vuejs.org/v2/guide/reactivity.html#%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%E9%98%9F%E5%88%97)                                                                                           |
| [**Vue.set()**](https://v2.cn.vuejs.org/v2/api/#Vue-set)                                                                                                                                                                                                                                        | 向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 this.myObject.newProperty = 'hi')注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。                                             |
| [**Vue.delete()**](https://v2.cn.vuejs.org/v2/api/#Vue-delete)                                                                                                                                                                                                                                  | 删除对象的 property。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到 property 被删除的限制。                                                                                                                                                                                 |
| 注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。                                                                                                                                                                                                                                            |
| [**Vue.directive()**](https://v2.cn.vuejs.org/v2/api/#Vue-directive)                                                                                                                                                                                                                            | 注册或获取全局指令。[自定义指令](https://v2.cn.vuejs.org/v2/guide/custom-directive.html)                                                                                                                                                                                                                     |
| [**Vue.filter()**](https://v2.cn.vuejs.org/v2/api/#Vue-filter)                                                                                                                                                                                                                                  | 注册或获取全局过滤器。[过滤器](https://v2.cn.vuejs.org/v2/guide/filters.html)                                                                                                                                                                                                                                |
| [**Vue.component()**](https://v2.cn.vuejs.org/v2/api/#Vue-component)                                                                                                                                                                                                                            | 注册或获取全局组件。注册还会自动使用给定的 id 设置组件的名称。[组件](https://v2.cn.vuejs.org/v2/guide/components.html)                                                                                                                                                                                       |
| [**Vue.use()**](https://v2.cn.vuejs.org/v2/api/#Vue-use)                                                                                                                                                                                                                                        | 安装 Vue.js 插件。如果插件是一个对象，必须提供 install 方法。如果插件是一个函数，它会被作为 install 方法。install 方法调用时，会将 Vue 作为参数传入。该方法需要在调用 new Vue() 之前被调用。当 install 方法被同一个插件多次调用，插件将只会被安装一次。[插件](https://v2.cn.vuejs.org/v2/guide/plugins.html) |
| [**Vue.mixin()**](https://v2.cn.vuejs.org/v2/api/#Vue-mixin)                                                                                                                                                                                                                                    | 全局注册一个混入，影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混入，向组件注入自定义的行为。不推荐在应用代码中使用。[全局混入](https://v2.cn.vuejs.org/v2/guide/mixins.html#%E5%85%A8%E5%B1%80%E6%B7%B7%E5%85%A5)                                                                                   |
| [**Vue.compile()**](https://v2.cn.vuejs.org/v2/api/#Vue-compile)                                                                                                                                                                                                                                | 将一个模板字符串编译成 render 函数。只在完整版时可用。[渲染函数](https://v2.cn.vuejs.org/v2/guide/render-function.html)                                                                                                                                                                                      |
| [**Vue.observable()**](https://v2.cn.vuejs.org/v2/api/#Vue-observable)                                                                                                                                                                                                                          | 让一个对象可响应。Vue 内部会用它来处理 data 函数返回的对象。                                                                                                                                                                                                                                                 |
| 返回的对象可以直接用于[渲染函数](https://v2.cn.vuejs.org/v2/guide/render-function.html)和[计算属性](https://v2.cn.vuejs.org/v2/guide/computed.html)内，并且会在发生变更时触发相应的更新。也可以作为最小化的跨组件状态存储器。[深入响应式原理](https://v2.cn.vuejs.org/v2/guide/reactivity.html) |
| [**Vue.version()**](https://v2.cn.vuejs.org/v2/api/#Vue-version)                                                                                                                                                                                                                                | 提供字符串形式的 Vue 安装版本号。这对社区的插件和组件来说非常有用，你可以根据不同的版本号采取不同的策略。                                                                                                                                                                                                    |

# 实例 API

## Vue 选项

```vue
new Vue({ xxx: xxx })
```

### 选项-数据

| **选项**                                         | **描述**                                                                                                             |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| [**data**](https://v2.cn.vuejs.org/v2/api/#data) | Vue 实例的数据对象。Vue 会递归地把 data 的 property 转换为 getter/setter，从而让 data 的 property 能够响应数据变化。 |

- 在创建实例之前，就声明所有的根级响应式 property。
- 实例创建之后，可以通过 `vm.$data` 访问原始数据对象。Vue 实例也代理了 data 对象上所有的 property，因此访问 vm.a 等价于访问 vm.$data.a。
- 以 \_ 或 $ 开头的 property 不会被 Vue 实例代理，因为它们可能和 Vue 内置的 property、API 方法冲突。你可以使用例如 vm.$data.\_property 的方式访问这些 property。
- 当一个组件被定义，data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！通过提供 data 函数，每次创建一个新实例后，我们能够调用 data 函数，从而返回初始数据的一个全新副本数据对象。
- 如果需要，可以通过将 vm.$data 传入 JSON.parse(JSON.stringify(...)) 得到深拷贝的原始数据对象。
  |
  | [**props**](https://v2.cn.vuejs.org/v2/api/#props) | props 可以是数组或对象，用于接收来自父组件的数据。props 可以是简单的数组，或者使用对象作为替代，对象允许配置高级选项，如类型检测、自定义验证和设置默认值。
- type：可以是下列原生构造函数中的一种：String、Number、Boolean、Array、Object、Date、Function、Symbol、任何自定义构造函数、或上述内容组成的数组。会检查一个 prop 是否是给定的类型，否则抛出警告。Prop 类型的[更多信息在此](https://v2.cn.vuejs.org/v2/guide/components-props.html#Prop-%E7%B1%BB%E5%9E%8B)。
- default：any
  为该 prop 指定一个默认值。如果该 prop 没有被传入，则换做用这个值。对象或数组的默认值必须从一个工厂函数返回。
- required：Boolean
  定义该 prop 是否是必填项。在非生产环境中，如果这个值为 truthy 且该 prop 没有被传入的，则一个控制台警告将会被抛出。
- validator：Function
  自定义验证函数会将该 prop 的值作为唯一的参数代入。在非生产环境下，如果该函数返回一个 falsy 的值 (也就是验证失败)，一个控制台警告将会被抛出。你可以在[这里](https://v2.cn.vuejs.org/v2/guide/components-props.html#Prop-%E9%AA%8C%E8%AF%81)查阅更多 prop 验证的相关信息。
  |
  | [**propsData**](https://v2.cn.vuejs.org/v2/api/#propsData) | 只用于 `new` 创建的实例中。创建实例时传递 props。**主要作用是方便测试。** |
  | [**computed**](https://v2.cn.vuejs.org/v2/api/#computed) | 计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。注意如果你为一个计算属性使用了箭头函数，则 `this` 不会指向这个组件的实例，不过你仍然可以将其实例作为函数的第一个参数来访问。[计算属性](https://v2.cn.vuejs.org/v2/guide/computed.html)
- 计算属性的结果会被缓存，除非依赖的响应式 property 变化才会重新计算。注意，如果某个依赖 (比如非响应式 property) 在该实例范畴之外，则计算属性是不会被更新的。
  |
  | [**methods**](https://v2.cn.vuejs.org/v2/api/#methods) | methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 `this` 自动绑定为 Vue 实例。
  注意，不应该使用箭头函数来定义 method 函数 (例如 plus: () => this.a++)。理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.a 将是 undefined。[事件处理器](https://v2.cn.vuejs.org/v2/guide/events.html) |
  | [**watch**](https://v2.cn.vuejs.org/v2/api/#watch) | 一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个 property。
  注意，不应该使用箭头函数来定义 watcher 函数，原因同上。 |

### 选项-DOM

| **选项**                                     | **描述**                                                                                                     |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| [**el**](https://v2.cn.vuejs.org/v2/api/#el) | 提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。 |

在实例挂载之后，元素可以用 vm.$el 访问。
如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 `vm.$mount()` 手动开启编译。 |
| [**template**](https://v2.cn.vuejs.org/v2/api/#template) | 一个字符串模板作为 Vue 实例的标识使用。模板将会替换挂载的元素。挂载元素的内容都将被忽略，除非模板的内容有分发插槽。
如果值以 # 开始，则它将被用作选择符，并使用匹配元素的 innerHTML 作为模板。常用的技巧是用 <script type="x-template"> 包含模板。 |
| [**render()**](https://v2.cn.vuejs.org/v2/api/#render) | 字符串模板的代替方案，允许你发挥 JavaScript 最大的编程能力。该渲染函数接收一个 createElement 方法作为第一个参数用来创建 VNode。
如果组件是一个函数组件，渲染函数还会接收一个额外的 context 参数，为没有实例的函数组件提供上下文信息。[渲染函数](https://v2.cn.vuejs.org/v2/guide/render-function.html) |
| [**renderError()**](https://v2.cn.vuejs.org/v2/api/#renderError) | 只在开发者环境下工作。当 render 函数遭遇错误时，提供另外一种渲染输出。其错误将会作为第二个参数传递到 renderError。这个功能配合 hot-reload 非常实用。 |

### 选项-生命周期

> 所有生命周期钩子的 this 上下文将自动绑定至实例中，因此你可以访问 data、computed 和 methods。这意味着你**不应该使用箭头函数来定义一个生命周期方法 **(例如 created: () => this.fetchTodos())。因为箭头函数绑定了父级上下文，所以 this 不会指向预期的组件实例，并且 this.fetchTodos 将会是 undefined。

生命周期![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fhstsdj9wpF7QyqEQiTl7aJPUahU.png)

| **选项**                                                                                                                                                                               | **描述**                                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**beforeCreate**](https://v2.cn.vuejs.org/v2/api/#beforeCreate)                                                                                                                       | 在实例初始化之后,进行数据侦听和事件/侦听器的配置之前同步调用。                                                                                                                                          |
| [**created**](https://v2.cn.vuejs.org/v2/api/#created)                                                                                                                                 | 在实例创建完成后被立即同步调用。在这一步中，实例已完成对选项的处理，意味着以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。然而，挂载阶段还没开始，且 $el property 目前尚不可用。 |
| [**beforeMount**](https://v2.cn.vuejs.org/v2/api/#beforeMount)                                                                                                                         | 在挂载开始之前被调用：相关的 render 函数首次被调用。该钩子在服务器端渲染期间不被调用。                                                                                                                  |
| [**mounted**](https://v2.cn.vuejs.org/v2/api/#mounted)                                                                                                                                 | 实例被挂载后调用，这时 el 被新创建的 vm.$el 替换了。如果根实例挂载到了一个文档内的元素上，当 mounted 被调用时 vm.$el 也在文档内。                                                                       |
| 注意 mounted 不会保证所有的子组件也都被挂载完成。如果你希望等到整个视图都渲染完毕再执行某些操作，可以在 mounted 内部使用 [vm.$nextTick](https://v2.cn.vuejs.org/v2/api/#vm-nextTick)。 |
| [**beforeUpdate**](https://v2.cn.vuejs.org/v2/api/#beforeUpdate)                                                                                                                       | 在数据发生改变后，DOM 被更新之前被调用。这里适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。                                                                                         |
| [**updated**](https://v2.cn.vuejs.org/v2/api/#updated)                                                                                                                                 | 在数据更改导致的虚拟 DOM 重新渲染和更新完毕之后被调用。                                                                                                                                                 |

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用[计算属性](https://v2.cn.vuejs.org/v2/api/#computed)或 [watcher](https://v2.cn.vuejs.org/v2/api/#watch) 取而代之。
注意，updated 不会保证所有的子组件也都被重新渲染完毕。如果你希望等到整个视图都渲染完毕，可以在 updated 里使用 [vm.$nextTick](https://v2.cn.vuejs.org/v2/api/#vm-nextTick)。 |
| [**activated**](https://v2.cn.vuejs.org/v2/api/#activated) | 被 keep-alive 缓存的组件激活时调用。 |
| [**deactivated**](https://v2.cn.vuejs.org/v2/api/#deactivated) | 被 keep-alive 缓存的组件失活时调用。 |
| [**beforeDestroy**](https://v2.cn.vuejs.org/v2/api/#beforeDestroy) | 实例销毁之前调用。在这一步，实例仍然完全可用。 |
| [**destroyed**](https://v2.cn.vuejs.org/v2/api/#destroyed) | 实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。 |
| [**errorCaptured**](https://v2.cn.vuejs.org/v2/api/#errorCaptured) | 在捕获一个来自后代组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。
错误传播规则

- 默认情况下，如果全局的 config.errorHandler 被定义，所有的错误仍会发送它，因此这些错误仍然会向单一的分析服务的地方进行汇报。
- 如果一个组件的 inheritance chain (继承链)或 parent chain (父链)中存在多个 errorCaptured 钩子，则它们将会被相同的错误逐个唤起。
- 如果此 errorCaptured 钩子自身抛出了一个错误，则这个新错误和原本被捕获的错误都会发送给全局的 config.errorHandler。
- 一个 errorCaptured 钩子能够返回 false 以阻止错误继续向上传播。本质上是说“这个错误已经被搞定了且应该被忽略”。它会阻止其它任何会被这个错误唤起的 errorCaptured 钩子和全局的 config.errorHandler。
  |

### 选项-资源

| **选项**                                                     | **描述**                                                                                            |
| ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| [**directives**](https://v2.cn.vuejs.org/v2/api/#directives) | 包含 Vue 实例可用指令的哈希表。[自定义指令](https://v2.cn.vuejs.org/v2/guide/custom-directive.html) |
| [**filters**](https://v2.cn.vuejs.org/v2/api/#filters)       | 包含 Vue 实例可用过滤器的哈希表。                                                                   |
| [**components**](https://v2.cn.vuejs.org/v2/api/#components) | 包含 Vue 实例可用组件的哈希表。                                                                     |

### 选项-组合

| **选项**                                                                                                              | **描述**                                                                                                                                                                                                                                                                                                                             |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [**parent**](https://v2.cn.vuejs.org/v2/api/#parent)                                                                  | 指定已创建的实例之父实例，在两者之间建立父子关系。子实例可以用 `this.$parent `访问父实例，子实例被推入父实例的 `$children `数组中。                                                                                                                                                                                                  |
| 节制地使用 $parent 和 $children - 它们的主要目的是作为访问组件的应急方法。更推荐用 props 和 events 实现父子组件通信。 |
| [**mixins**](https://v2.cn.vuejs.org/v2/api/#mixins)                                                                  | `mixins` 选项接收一个混入对象的数组。这些混入对象可以像正常的实例对象一样包含实例选项，这些选项将会被合并到最终的选项中，使用的是和 Vue.extend() 一样的选项合并逻辑。也就是说，如果你的混入包含一个 created 钩子，而创建组件本身也有一个，那么两个函数都会被调用。Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用。 |
| [**extends**](https://v2.cn.vuejs.org/v2/api/#extends)                                                                | 允许声明扩展另一个组件 (可以是一个简单的选项对象或构造函数)，而无需使用 Vue.extend。这主要是为了便于扩展单文件组件。这和 `mixins` 类似。                                                                                                                                                                                             |
| [**provide / inject**](https://v2.cn.vuejs.org/v2/api/#provide-inject)                                                | 这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在其上下游关系成立的时间里始终生效。如果你熟悉 React，这与 React 的上下文特性很相似。                                                                                                                                                    |

### 选项-其他

| **选项**                                                         | **描述**                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**name**](https://v2.cn.vuejs.org/v2/api/#name)                 | 只有作为组件选项时起作用。允许组件模板递归地调用自身。注意，组件在全局用 Vue.component() 注册时，全局 ID 自动作为组件的 name。指定 name 选项的另一个好处是便于调试。有名字的组件有更友好的警告信息。另外，当在有 [vue-devtools](https://github.com/vuejs/vue-devtools)，未命名组件将显示成 <AnonymousComponent>，这很没有语义。通过提供 name 选项，可以获得更有语义信息的组件树。             |
| [**delimiters**](https://v2.cn.vuejs.org/v2/api/#delimiters)     | 这个选项只在完整构建版本中的浏览器内编译时可用。改变纯文本插入分隔符。                                                                                                                                                                                                                                                                                                                        |
| [**functional**](https://v2.cn.vuejs.org/v2/api/#functional)     | 使组件无状态 (没有 data) 和无实例 (没有 this 上下文)。他们用一个简单的 render 函数返回虚拟节点使它们渲染的代价更小。[函数式组件](https://v2.cn.vuejs.org/v2/guide/render-function.html#%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BB%84%E4%BB%B6)                                                                                                                                                         |
| [**model**](https://v2.cn.vuejs.org/v2/api/#model)               | 允许一个自定义组件在使用 v-model 时定制 prop 和 event。默认情况下，一个组件上的 v-model 会把 value 用作 prop 且把 input 用作 event，但是一些输入类型比如单选框和复选框按钮可能想使用 value prop 来达到不同的目的。使用 model 选项可以回避这些情况产生的冲突。                                                                                                                                 |
| [**inheritAttrs**](https://v2.cn.vuejs.org/v2/api/#inheritAttrs) | 默认情况下父作用域的不被认作 props 的 attribute 绑定 (attribute bindings) 将会“回退”且作为普通的 HTML attribute 应用在子组件的根元素上。当撰写包裹一个目标元素或另一个组件的组件时，这可能不会总是符合预期行为。通过设置 inheritAttrs 到 false，这些默认行为将会被去掉。而通过 (同样是 2.4 新增的) 实例 property $attrs 可以让这些 attribute 生效，且可以通过 v-bind 显性的绑定到非根元素上。 |
| [**comments**](https://v2.cn.vuejs.org/v2/api/#comments)         | 这个选项只在完整构建版本中的浏览器内编译时可用。当设为 true 时，将会保留且渲染模板中的 HTML 注释。默认行为是舍弃它们。                                                                                                                                                                                                                                                                        |

## Vue 实例

```vue
new Vue({ created: function () { console.log(this.xx) } })
```

### 实例属性

| **选项**                                                        | **描述**                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**vm.$data**](https://v2.cn.vuejs.org/v2/api/#vm-data)         | Vue 实例观察的数据对象。Vue 实例代理了对其 data 对象 property 的访问。                                                                                                                                                                                                                                                                                                                                                              |
| [**vm.$props**](https://v2.cn.vuejs.org/v2/api/#vm-props)       | 当前组件接收到的 props 对象。Vue 实例代理了对其 props 对象 property 的访问。                                                                                                                                                                                                                                                                                                                                                        |
| [**vm.$el**](https://v2.cn.vuejs.org/v2/api/#vm-el)             | Vue 实例使用的根 DOM 元素。                                                                                                                                                                                                                                                                                                                                                                                                         |
| [**vm.$options**](https://v2.cn.vuejs.org/v2/api/#vm-options)   | 用于当前 Vue 实例的初始化选项。需要在选项中包含自定义 property 时会有用处。                                                                                                                                                                                                                                                                                                                                                         |
| [**vm.$parent**](https://v2.cn.vuejs.org/v2/api/#vm-parent)     | 父实例，如果当前实例有的话。                                                                                                                                                                                                                                                                                                                                                                                                        |
| [**vm.$root**](https://v2.cn.vuejs.org/v2/api/#vm-root)         | 当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己。                                                                                                                                                                                                                                                                                                                                                               |
| [**vm.$children**](https://v2.cn.vuejs.org/v2/api/#vm-children) | 当前实例的直接子组件。需要注意 $children 并不保证顺序，也不是响应式的。如果你发现自己正在尝试使用 $children 来进行数据绑定，考虑使用一个数组配合 v-for 来生成子组件，并且使用 Array 作为真正的来源。                                                                                                                                                                                                                                |
| [**vm.$slots**](https://v2.cn.vuejs.org/v2/api/#vm-slots)       | 用来访问被[插槽分发](https://v2.cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87%E6%8F%92%E6%A7%BD%E5%88%86%E5%8F%91%E5%86%85%E5%AE%B9)的内容。每个[具名插槽](https://v2.cn.vuejs.org/v2/guide/components-slots.html#%E5%85%B7%E5%90%8D%E6%8F%92%E6%A7%BD)有其相应的 property (例如：v-slot:foo 中的内容将会在 vm.$slots.foo 中被找到)。default property 包括了所有没有被包含在具名插槽中的节点，或 v-slot:default 的内容。 |

请注意插槽不是响应性的。如果你需要一个组件可以在被传入的数据发生变化时重渲染，我们建议改变策略，依赖诸如 props 或 data 等响应性实例选项。
注意：v-slot:foo 在 2.6 以上的版本才支持。对于之前的版本，你可以使用[废弃了的语法](https://v2.cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95)。
在使用[渲染函数](https://v2.cn.vuejs.org/v2/guide/render-function.html)书写一个组件时，访问 vm.$slots 最有帮助。 |
| [**vm.$scopedSlots\*\*](https://v2.cn.vuejs.org/v2/api/#vm-scopedSlots) | 用来访问[作用域插槽](https://v2.cn.vuejs.org/v2/guide/components-slots.html#%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%8F%92%E6%A7%BD)。对于包括 默认 slot 在内的每一个插槽，该对象都包含一个返回相应 VNode 的函数。
vm.$scopedSlots 在使用[渲染函数](https://v2.cn.vuejs.org/v2/guide/render-function.html)开发一个组件时特别有用。
注意：从 2.6.0 开始，这个 property 有两个变化：

1. 作用域插槽函数现在保证返回一个 VNode 数组，除非在返回值无效的情况下返回 undefined。
2. 所有的 $slots 现在都会作为函数暴露在 $scopedSlots 中。如果你在使用渲染函数，不论当前插槽是否带有作用域，我们都推荐始终通过 $scopedSlots 访问它们。这不仅仅使得在未来添加作用域变得简单，也可以让你最终轻松迁移到所有插槽都是函数的 Vue 3。
 |
| [**vm.$refs**](https://v2.cn.vuejs.org/v2/api/#vm-refs) | 一个对象，持有注册过 [refattribute](https://v2.cn.vuejs.org/v2/api/#ref) 的所有 DOM 元素和组件实例。 |
   | [**vm.$isServer**](https://v2.cn.vuejs.org/v2/api/#vm-isServer) | 当前 Vue 实例是否运行于服务器。[服务端渲染](https://v2.cn.vuejs.org/v2/guide/ssr.html) |
| [**vm.$attrs**](https://v2.cn.vuejs.org/v2/api/#vm-attrs) | 包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件——在创建高级别的组件时非常有用。 |
   | [**vm.$listeners**](https://v2.cn.vuejs.org/v2/api/#vm-listeners) | 包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件——在创建更高层次的组件时非常有用。 |

### 实例方法-数据

| **选项**                                                  | **描述**                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [**vm.$watch**](https://v2.cn.vuejs.org/v2/api/#vm-watch) | 观察 Vue 实例上的一个表达式或者一个函数计算结果的变化。回调函数得到的参数为新值和旧值。表达式只接受简单的键路径。对于更复杂的表达式，用一个函数取代。注意：在变更 (不是替换) 对象或数组时，旧值将与新值相同，因为它们的引用指向同一个对象/数组。Vue 不会保留变更之前值的副本。 |

- 选项：deep，为了发现对象内部值的变化，可以在选项参数中指定 deep: true。注意监听数组的变更不需要这么做。
- 选项：immediate，在选项参数中指定 immediate: true 将立即以表达式的当前值触发回调。
  |
  | [**vm.$set**](https://v2.cn.vuejs.org/v2/api/#vm-set) | 这是全局 `[Vue.set](https://v2.cn.vuejs.org/v2/api/#Vue-set)` 的别名。 |
  | [**vm.$delete**](https://v2.cn.vuejs.org/v2/api/#vm-delete) | 这是全局 `Vue.delete` 的别名。 |

### 实例方法-事件

| **选项**                                                | **描述**                                                                                             |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| [**vm.$on**](https://v2.cn.vuejs.org/v2/api/#vm-on)     | 监听当前实例上的自定义事件。事件可以由 vm.$emit 触发。回调函数会接收所有传入事件触发函数的额外参数。 |
| [**vm.$once**](https://v2.cn.vuejs.org/v2/api/#vm-once) | 监听一个自定义事件，但是只触发一次。一旦触发之后，监听器就会被移除。                                 |
| [**vm.$off**](https://v2.cn.vuejs.org/v2/api/#vm-off)   | 移除自定义事件监听器。                                                                               |

- 如果没有提供参数，则移除所有的事件监听器；
- 如果只提供了事件，则移除该事件所有的监听器；
- 如果同时提供了事件与回调，则只移除这个回调的监听器。
  |
  | [**vm.$emit**](https://v2.cn.vuejs.org/v2/api/#vm-emit) | 触发当前实例上的事件。附加参数都会传给监听器回调。 |

### 实例方法-生命周期

| **选项**                                                              | **描述**                                                                                                                                                                                                                                                                                                  |
| --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**vm.$mount**](https://v2.cn.vuejs.org/v2/api/#vm-mount)             | 如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 vm.$mount() 手动地挂载一个未挂载的实例。如果没有提供 elementOrSelector 参数，模板将被渲染为文档之外的的元素，并且你必须使用原生 DOM API 把它插入文档中。这个方法返回实例自身，因而可以链式调用其它实例方法。 |
| [**vm.$forceUpdate**](https://v2.cn.vuejs.org/v2/api/#vm-forceUpdate) | 迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。                                                                                                                                                                                                                   |
| [**vm.$nextTick**](https://v2.cn.vuejs.org/v2/api/#vm-nextTick)       | 将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。                                                                                                                                        |
| [**vm.$destroy**](https://v2.cn.vuejs.org/v2/api/#vm-destroy)         | 完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。                                                                                                                                                                                                                                  |
| 触发 beforeDestroy 和 destroyed 的钩子。                              |

# 内置内容

## 指令

用法：`<span v-xx="xx"></span>`

| **选项**                                                                                                                         | **描述**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**v-text**](https://v2.cn.vuejs.org/v2/api/#v-text)                                                                             | 更新元素的 `textContent`。如果要更新部分的 textContent，需要使用 {{ Mustache }} 插值。[数据绑定语法 - 插值](https://v2.cn.vuejs.org/v2/guide/syntax.html#%E6%8F%92%E5%80%BC)                                                                                                                                                                                                                                                                                                                                                              |
| [**v-html**](https://v2.cn.vuejs.org/v2/api/#v-html)                                                                             | 更新元素的 innerHTML。注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译。如果试图使用 v-html 组合模板，可以重新考虑是否通过使用组件来替代。                                                                                                                                                                                                                                                                                                                                                                                          |
| [**v-show**](https://v2.cn.vuejs.org/v2/api/#v-show)                                                                             | 根据表达式之真假值，切换元素的 display CSS property。当条件变化时该指令触发过渡效果。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| [**v-if**](https://v2.cn.vuejs.org/v2/api/#v-if)                                                                                 | 根据表达式的值的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 来有条件地渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 <template>，将提出它的内容作为条件块。当条件变化时该指令触发过渡效果。                                                                                                                                                                                                                                                                                              |
| [**v-else**](https://v2.cn.vuejs.org/v2/api/#v-else)                                                                             | 为 v-if 或者 v-else-if 添加“else 块”。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| [**v-else-if**](https://v2.cn.vuejs.org/v2/api/#v-else-if)                                                                       | 表示 v-if 的“else if 块”。可以链式调用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| [**v-for**](https://v2.cn.vuejs.org/v2/api/#v-for)                                                                               | 基于源数据多次渲染元素或模板块。此指令之值，必须使用特定语法 alias in expression，为当前遍历的元素提供别名。v-for 的默认行为会尝试原地修改元素而不是移动它们。要强制其重新排序元素，你需要用特殊 attribute key 来提供一个排序提示。从 2.6 起，v-for 也可以在实现了[可迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%AF%E8%BF%AD%E4%BB%A3%E5%8D%8F%E8%AE%AE)的值上使用，包括原生的 Map 和 Set。不过应该注意的是 Vue 2.x 目前并不支持可响应的 Map 和 Set 值，所以无法自动探测变更。 |
| 当和 v-if 一起使用时，v-for 的优先级比 v-if 更高。详见[列表渲染教程](https://v2.cn.vuejs.org/v2/guide/list.html#v-for-with-v-if) |
| [**v-on**](https://v2.cn.vuejs.org/v2/api/#v-on)                                                                                 | 绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。                                                                                                                                                                                                                                                                                                                                                                                                                                  |

用在普通元素上时，只能监听[原生 DOM 事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)。用在自定义元素组件上时，也可以监听子组件触发的自定义事件。
在监听原生 DOM 事件时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 $event property：v-on:click="handle('ok', $event)"。
从 2.4.0 开始，v-on 同样支持不带参数绑定一个事件/监听器键值对的对象。注意当使用对象语法时，是不支持任何修饰器的。

- 修饰符：
  - .stop - 调用 event.stopPropagation()。
  - .prevent - 调用 event.preventDefault()。
  - .capture - 添加事件侦听器时使用 capture 模式。
  - .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  - .{keyCode | keyAlias} - 只当事件是从特定键触发时才触发回调。
  - .native - 监听组件根元素的原生事件。
  - .once - 只触发一次回调。
  - .left - (2.2.0) 只当点击鼠标左键时触发。
  - .right - (2.2.0) 只当点击鼠标右键时触发。
  - .middle - (2.2.0) 只当点击鼠标中键时触发。
  - .passive - (2.3.0) 以 { passive: true } 模式添加侦听器
    |
    | [**v-bind**](https://v2.cn.vuejs.org/v2/api/#v-bind) | 动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。
    在绑定 class 或 style attribute 时，支持其它类型的值，如数组或对象。可以通过下面的教程链接查看详情。
    在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。
    没有参数时，可以绑定到一个包含键值对的对象。注意此时 class 和 style 绑定不支持数组和对象。
- 修饰符：
  - .prop - 作为一个 DOM property 绑定而不是作为 attribute 绑定。([差别在哪里？](https://stackoverflow.com/questions/6003819/properties-and-attributes-in-html#answer-6004028))
  - .camel - (2.1.0+) 将 kebab-case attribute 名转换为 camelCase。(从 2.1.0 开始支持)，驼峰命名。
  - .sync (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 v-on 侦听器。
    |
    | [**v-model**](https://v2.cn.vuejs.org/v2/api/#v-model) | 在表单控件或者组件上创建双向绑定。
- 修饰符：
  - [.lazy](https://v2.cn.vuejs.org/v2/guide/forms.html#lazy) - 取代 input 监听 change 事件
  - [.number](https://v2.cn.vuejs.org/v2/guide/forms.html#number) - 输入字符串转为有效的数字
  - [.trim](https://v2.cn.vuejs.org/v2/guide/forms.html#trim) - 输入首尾空格过滤
    |
    | [**v-slot**](https://v2.cn.vuejs.org/v2/api/#v-slot) | 提供具名插槽或需要接收 prop 的插槽。 |
    | [**v-pre**](https://v2.cn.vuejs.org/v2/api/#v-pre) | 跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。 |
    | [**v-cloak**](https://v2.cn.vuejs.org/v2/api/#v-cloak) | 这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。 |
    | [**v-once**](https://v2.cn.vuejs.org/v2/api/#v-once) | 只渲染元素和组件一次。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。 |

## 特殊属性

用法：`<p xxx="xxx">hello</p>`

| **选项**                                       | **描述**                                                                                                                                                                                                                                                                |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**key**](https://v2.cn.vuejs.org/v2/api/#key) | key 的特殊 attribute 主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。 |

有相同父元素的子元素必须有独特的 key。重复的 key 会造成渲染错误。
它也可以用于强制替换元素/组件而不是重复使用它。当你遇到如下场景时它可能会很有用：

- 完整地触发组件的生命周期钩子
- 触发过渡

当 text 发生改变时，<span> 总是会被替换而不是被修改，因此会触发过渡。 |
| [**ref**](https://v2.cn.vuejs.org/v2/api/#ref) | ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例。

- 当 v-for 用于元素或组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。
- 关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！$refs 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。
  |
  | [**is**](https://v2.cn.vuejs.org/v2/api/#is) | 用于[动态组件](https://v2.cn.vuejs.org/v2/guide/components.html#%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6)且基于 [DOM 内模板的限制](https://v2.cn.vuejs.org/v2/guide/components.html#%E8%A7%A3%E6%9E%90-DOM-%E6%A8%A1%E6%9D%BF%E6%97%B6%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)来工作。 |

## 内置组件

用法：`<xxx></xxx>`

| **选项**                                                                 | **描述**                                                                                                                                                         |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**component**](https://v2.cn.vuejs.org/v2/api/#component)               | 渲染一个“元组件”为动态组件。依 is 的值，来决定哪个组件被渲染。[动态组件](https://v2.cn.vuejs.org/v2/guide/components.html#%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6)  |
| [**transition**](https://v2.cn.vuejs.org/v2/api/#transition)             | <transition> 元素作为单个元素/组件的过渡效果。<transition> 只会把过渡效果应用到其包裹的内容上，而不会额外渲染 DOM 元素，也不会出现在可被检查的组件层级中。       |
| [**transition-group**](https://v2.cn.vuejs.org/v2/api/#transition-group) | <transition-group> 元素作为多个元素/组件的过渡效果。<transition-group> 渲染一个真实的 DOM 元素。默认渲染 <span>，可以通过 tag attribute 配置哪个元素应该被渲染。 |

注意，每个 <transition-group> 的子节点必须有独立的 key，动画才能正常工作
<transition-group> 支持通过 CSS transform 过渡移动。当一个子节点被更新，从屏幕上的位置发生变化，它会被应用一个移动中的 CSS 类 (通过 name attribute 或配置 move-class attribute 自动生成)。如果 CSS transform property 是“可过渡”property，当应用移动类时，将会使用 [FLIP 技术](https://aerotwist.com/blog/flip-your-animations/)使元素流畅地到达动画终点。 |
| [**keep-alive**](https://v2.cn.vuejs.org/v2/api/#keep-alive) | <keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 <transition> 相似，<keep-alive> 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在组件的父组件链中。
当组件在 <keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。主要用于保留组件状态或避免重新渲染。
注意，<keep-alive> 是用在其一个直属的子组件被开关的情形。如果你在其中有 v-for 则不会工作。如果有上述的多个条件性的子元素，<keep-alive> 要求同时只有一个子元素被渲染。

- Props：
  - include - 字符串或正则表达式。只有名称匹配的组件会被缓存。
  - exclude - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
  - max - 数字。最多可以缓存多少组件实例。
    |
    | [**slot**](https://v2.cn.vuejs.org/v2/api/#slot) | <slot> 元素作为组件模板之中的内容分发插槽。<slot> 元素自身将被替换。
    详细用法，请参考下面教程的链接。[通过插槽分发内容](https://v2.cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87%E6%8F%92%E6%A7%BD%E5%88%86%E5%8F%91%E5%86%85%E5%AE%B9) |

# 参考链接

[Vuex 极速入门](https://www.yuque.com/kanding/ktech/otll9x?view=doc_embed)
[Vue2 快速上门(1)-基础知识](https://www.yuque.com/kanding/ktech/kx70h0?view=doc_embed)
[Vue2 快速上门(2)-模板语法](https://www.yuque.com/kanding/ktech/lrqg93?view=doc_embed)
[Vue2 快速上门(3)-组件与复用](https://www.yuque.com/kanding/ktech/cky107?view=doc_embed)
[API — Vue.js](https://v2.cn.vuejs.org/v2/api/)
[API 参考 | Vue.js](https://cn.vuejs.org/api/)
[简介 | Vue.js](https://cn.vuejs.org/guide/introduction.html)
[介绍 — Vue.js](https://v2.cn.vuejs.org/v2/guide/)

---

最后，重学前端，将思考固定下来。

- 笔记和记忆力是提高开发效率的最好方法。
- 如果没有对旧事物进行大量练习，你不太可能发现新事物。
- 努力学习最感兴趣的东西。
  > ©️ 版权申明：版权所有@宋玉，本文内容仅供学习，欢迎指正、交流，转载请注明出处！[原文地址-语雀](https://www.yuque.com/songxingguo/devhints/nholynuhnqrhg0tg)
