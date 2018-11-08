title: 《深入浅出Node.js》-读书笔记-模块机制
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
  - ''
date: 2018-10-06 00:06:00
---
## 模块机制

在Web 1.0时代，这种脚本语言在网络中主要有两个作用广为流传，一个是表单校验，另一个是网页特效。

直到Web 2.0时代，前端工程师利用它大大提升了网页上的用户体验。在这个过程中，B/S应用展现出比C/S应用优越的地方。至此，JavaScript才被广泛重视起来。

<!-- more -->

在Web 2.0流行的过程中，各种前端库和框架被开发出来，它们最初用于兼容各个版本的浏览器，随后随着更多的用户需求在前端被实现，JavaScript也从表单校验跃迁到应用开发的级别上。在这个过程中，它大致经历了工具类库、组件库、前端框架、前端应用的变迁，如下图所示。

![JavaScript的变迁](https://graphbed.qiniu.songxingguo.com/Node-Module/JavaScript%E7%9A%84%E5%8F%98%E8%BF%81.jpg)

另一个角度而言，它也道出了JavaScript先天就缺乏的一项功能：模块。

在其他高级语言中，Java有类文件，Python有import机制，Ruby有require，PHP有include和require。而JavaScript通过`<script>`标签引入代码的方式显得杂乱无章，语言自身毫无组织和约束能力。人们不得不用命名空间等方式人为地约束代码，以求达到安全和易用的目的。

###  CommonJS规范

CommonJS规范为JavaScript制定了一个美好的愿景——希望JavaScript能够在任何地方运行。

#### CommonJS的出发点

由于官方规范（ECMAScript）规范化的时间较早，规范涵盖的范畴非常小。

这些规范中包含词法、类型、上下文、表达式、声明（statement）、方法、对象等语言的基本要素。在实际应用中，JavaScript的表现能力取决于宿主环境中的API支持程度。

在Web 1.0时代，只有对DOM、BOM等基本的支持。随着Web 2.0的推进，HTML5崭露头角，它将Web网页带进Web应用的时代，在浏览器中出现了更多、更强大的API供JavaScript调用，这得感谢W3C组织对HTML5规范的推进以及各大浏览器厂商对规范的大力支持。但是，Web在发展，浏览器中出现了更多的标准API，这些过程发生在前端，后端JavaScript的规范却远远落后。

对于JavaScript自身而言，它的规范依然是薄弱的，**还有以下缺陷**。

- 没有模块系统。

- 标准库较少。ECMAScript仅定义了部分核心库，对于文件系统，I/O流等常见需求却没有标准的API。就HTML5的发展状况而言，W3C标准化在一定意义上是在推进这个过程，但是它仅限于浏览器端。

- 没有标准接口。在JavaScript中，几乎没有定义过如Web服务器或者数据库之类的标准统一接口。

- 缺乏包管理系统。这导致JavaScript应用中基本没有自动加载和安装依赖的能力。

CommonJS规范的提出，主要是为了弥补当前JavaScript没有标准的缺陷，以达到像Python、Ruby和Java具备开发大型应用的基础能力，而不是停留在小脚本程序的阶段。他们期望那些用CommonJS API写出的应用可以具备跨宿主环境执行的能力。这样不仅可以利用JavaScript开发富客户端应用，而且还可以编写以下应用。

- 服务器端JavaScript应用程序。
- 命令行工具。
- 桌面图形界面应用程序。
- 混合应用（Titanium和Adobe AIR等形式的应用）。

下图是Node与浏览器以及W3C组织、CommonJS组织、ECMAScript之间的关系，共同构成了一个繁荣的生态系统。

![Node与浏览器以及W3C组织、CommmonJs组织、ECMAScript之间的关系]()

Node借鉴CommonJS的Modules规范实现了一套非常易用的模块系统，NPM对Packages规范的完好支持使得Node应用在开发过程中事半功倍。

####  CommonJS的模块规范

CommonJS对模块的定义十分简单，主要分为模块引用、模块定义和模块标识3个部分。

1. 模块引用

模块引用的示例代码如下：

```js
var math = require('math');
```
在CommonJS规范中，存在require()方法，这个方法接受模块标识，以此引入一个模块的API到当前上下文中。

2. 模块定义

在模块中，上下文提供require()方法来引入外部模块。对应引入的功能，上下文提供了exports对象用于导出当前模块的方法或者变量，并且它是唯一导出的出口。在模块中，还存在一个module对象，它代表模块自身，而exports是module的属性。在Node中，一个文件就是一个模块，将方法挂载在exports对象上作为属性即可定义导出的方式：

```js
// math.js
exports.add = function () {
  var sum = 0, 
    i = 0,
    args = arguments, 
    l = args.length;
  while (i < l) { 
    sum += args[i++];
  } 
  return sum;
};
```
在另一个文件中，我们通过require()方法引入模块后，就能调用定义的属性或方法了：

```js
// program.js
var math = require('math');
exports.increment = function (val) { 
  return math.add(val, 1);
};
```
3. 模块标识

模块标识其实就是传递给require()方法的参数，它必须是符合小驼峰命名的字符串，或者以.、..开头的相对路径，或者绝对路径。它可以没有文件名后缀.js。

模块的定义十分简单，接口也十分简洁。它的意义在于将类聚的方法和变量等限定在私有的作用域中，同时支持引入和导出功能以顺畅地连接上下游依赖。如下图所示，每个模块具有独立的空间，它们互不干扰，在引用时也显得干净利落。

![模块定义](https://graphbed.qiniu.songxingguo.com/Node-Module/%E6%A8%A1%E5%9D%97%E5%AE%9A%E4%B9%89.jpg)

**CommonJS构建的这套模块导出和引入机制** 使得用户完全不必考虑 **变量污染** ，**命名空间** 等方案与之相比相形见绌。

### Node的模块实现

在Node中引入模块，需要经历如下3个步骤。

（1）路径分析
（2）文件定位
（3）编译执行

在Node中，模块分为两类：一类是Node提供的模块，称为核心模块；另一类是用户编写的模块，称为文件模块。

- **核心模块** 部分 **在Node源代码的编译过程中，编译进了二进制执行文件** 。在Node进程启动时，部分核心模块就被直接加载进内存中，所以这部分核心模块引入时，文件定位和编译执行这两个步骤可以省略掉，并且在路径分析中优先判断，所以它的加载速度是最快的。

- **文件模块则是在运行时动态加载，需要完整的路径分析、文件定位、编译执行过程，速度比核心模块慢** 。

接下来，我们展开详细的模块加载过程。

#### 先从缓存加载

展开介绍路径分析和文件定位之前，我们需要知晓的一点是，与前端浏览器会缓存静态脚本文件以提高性能一样，Node对引入过的模块都会进行缓存，以减少二次引入时的开销。不同的地方在于，浏览器仅仅缓存文件，而Node缓存的是编译和执行之后的对象。

不论是核心模块还是文件模块，**require()方法对相同模块的二次加载都一律采用缓存优先的方式** ，这是第一优先级的。**不同之处在于核心模块的缓存检查先于文件模块的缓存检查** 。

#### 路径分析和文件定位

因为标识符有几种形式，对于不同的标识符，模块的查找和定位有不同程度上的差异。

1. 模块标识符分析

前面提到过，require()方法接受一个标识符作为参数。在Node实现中，正是基于这样一个标识符进行模块查找的。模块标识符在Node中主要分为以下几类。

- 核心模块，如http、fs、path等。
- .或..开始的相对路径文件模块。
- 以/开始的绝对路径文件模块。
- 非路径形式的文件模块，如自定义的connect模块。

核心模块

核心模块的优先级仅次于缓存加载，它在Node的源代码编译过程中已经编译为二进制代码，其加载过程最快。

**如果试图加载一个与核心模块标识符相同的自定义模块，那是不会成功的** 。如果自己编写了一个http用户模块，想要加载成功，必须选择一个不同的标识符或者换用路径的方式。

路径形式的文件模块

以.、..和/开始的标识符，这里都被当做文件模块来处理。在分析路径模块时，require()方法会将路径转为绝对路径，并以真实路径作为索引，将编译执行后的结果存放到缓存中，以使二次加载时更快。

由于文件模块给Node指明了确切的文件位置，所以 **在查找过程中可以节约大量时间，其加载速度慢于核心模块。**

自定义模块

自定义模块指的是非核心模块，也不是路径形式的标识符。它是一种特殊的文件模块，可能是一个文件或者包的形式。**这类模块的查找是最费时的，也是所有方式中最慢的一种。**

在介绍自定义模块的查找方式之前，需要先介绍一下模块路径这个概念。

模块路径是Node在定位文件模块的具体文件时制定的查找策略，具体表现为一个路径组成的数组。关于这个路径的生成规则，我们可以手动尝试一番。

（1）创建module_path.js文件，其内容为console.log(module.paths);。
（2）将其放到任意一个目录中然后执行node module_path.js。

在Linux下，你可能得到的是这样一个数组输出：

```
[ '/home/jackson/research/node_modules',
'/home/jackson/node_modules',
'/home/node_modules', 
'/node_modules' ]
```

而在Windows下，也许是这样：

```
[ 'c:\\nodejs\\node_modules', 
'c:\\node_modules' ]
```
可以看出，模块路径的生成规则如下所示。

- 当前文件目录下的node_modules目录。
- 父目录下的node_modules目录。
- 父目录的父目录下的node_modules目录。
- 沿路径向上逐级递归，直到根目录下的node_modules目录。

**它的生成方式与JavaScript的原型链或作用域链的查找方式十分类似。** 在加载的过程中，Node会逐个尝试模块路径中的路径，直到找到目标文件为止。**可以看出，当前文件的路径越深，模块查找耗时会越多，这是自定义模块的加载速度是最慢的原因** 。

2. 文件定位

从缓存加载的优化策略使得二次引入时不需要路径分析、文件定位和编译执行的过程，大大提高了再次加载模块时的效率。

但在文件的定位过程中，还有一些细节需要注意，这主要包括文件扩展名的分析、目录和包的处理。

文件扩展名分析

require()在分析标识符的过程中，会出现标识符中不包含文件扩展名的情况。CommonJS模块规范也允许在标识符中不包含文件扩展名，这种情况下，**Node会按.js、.node、.json的次序补足扩展名，依次尝试** 。

在尝试的过程中，需要调用fs模块同步阻塞式地判断文件是否存在。因为Node是单线程的，所以这里是一个会引起性能问题的地方。小诀窍是：**如果是.node和.json文件，在传递给require()的标识符中带上扩展名，会加快一点速度** 。另一个诀窍是：**同步配合缓存，可以大幅度缓解Node单线程中阻塞式调用的缺陷** 。

目录分析和包

在分析标识符的过程中，require()通过分析文件扩展名之后，可能没有查找到对应文件，但却得到一个目录，这在引入自定义模块和逐个模块路径进行查找时经常会出现，此时Node会将目录当做一个包来处理。

在这个过程中，Node对CommonJS包规范进行了一定程度的支持。首先，Node在当前目录下查找package.json（CommonJS包规范定义的包描述文件），通过JSON.parse()解析出包描述对象，从中取出main属性指定的文件名进行定位。如果文件名缺少扩展名，将会进入扩展名分析的步骤。

而如果 **main属性指定的文件名错误，或者压根没有package.json文件，Node会将index当做默认文件名，然后依次查找index.js、index.node、index.json。** 如果在目录分析的过程中没有定位成功任何文件，则自定义模块进入下一个模块路径进行查找。如果模块路径数组都被遍历完毕，依然没有查找到目标文件，则会抛出查找失败的异常。

#### 模块编译

**在Node中，每个文件模块都是一个对象** ，它的定义如下：

```js
function Module(id, parent) {
  this.id = id; 
  this.exports = {};
  this.parent = parent; 
  if (parent && parent.children) {
    parent.children.push(this); 
  }
  
  this.filename = null;
  this.loaded = false; 
  this.children = [];
}
```
编译和执行是引入文件模块的最后一个阶段。定位到具体的文件后，Node会新建一个模块对象，然后根据路径载入并编译。

对于不同的文件扩展名，其载入方法也有所不同，具体如下所示。

- js文件。通过fs模块同步读取文件后编译执行。
- node文件。这是用C/C++编写的扩展文件，通过dlopen()方法加载最后编译生成的文件。
- json文件。通过fs模块同步读取文件后，用JSON.parse()解析返回结果。
- 其余扩展名文件。它们都被当做.js文件载入。

每一个编译成功的模块都会将其文件路径作为索引缓存在Module._cache对象上，以提高二次引入的性能。

根据不同的文件扩展名，Node会调用不同的读取方式，如.json文件的调用如下：

```js
// Native extension for .json
Module._extensions['.json'] = function(module, filename) {
  var content = NativeModule.require('fs').readFileSync(filename, 'utf8'); 
  try {
    module.exports = JSON.parse(stripBOM(content)); 
  } catch (err) {
    err.message = filename + ': ' + err.message; throw err;
  }
};
```
其中，Module._extensions会被赋值给require()的extensions属性，所以通过在代码中访问require.extensions可以知道系统中已有的扩展加载方式。编写如下代码测试一下：

```js
console.log(require.extensions);
```
得到的执行结果如下：

```js
{ '.js': [Function], '.json': [Function], '.node': [Function] }
```
如果想对自定义的扩展名进行特殊的加载，可以通过类似require.extensions['.ext']的方式实现。早期的CoffeeScript文件就是通过添加require.extensions['.coffee']扩展的方式来实现加载的。但是从v0.10.6版本开始，官方不鼓励通过这种方式来进行自定义扩展名的加载，而是期望先将其他语言或文件编译成JavaScript文件后再加载，这样做的好处在于不将烦琐的编译加载等过程引入Node的执行过程中。

在确定文件的扩展名之后，Node将调用具体的编译方式来将文件执行后返回给调用者。

1. JavaScript模块的编译

回到CommonJS模块规范，我们知道每个模块文件中存在着require、exports、module这3个变量，但是它们在模块文件中并没有定义，那么从何而来呢？甚至在Node的API文档中，我们知道每个模块中还有__filename、__dirname这两个变量的存在，它们又是从何而来的呢？如果我们把直接定义模块的过程放在浏览器端，会存在污染全局变量的情况。

事实上，**在编译的过程中，Node对获取的JavaScript文件内容进行了头尾包装。** 在头部添加了(function (exports, require, module, __filename, __dirname) {\n，在尾部添加了\n});。

一个正常的JavaScript文件会被包装成如下的样子：

```js
(function (exports, require, module, __filename, __dirname) {
  var math = require('math'); 
  exports.area = function (radius) {
    return Math.PI * radius * radius; 
  };
});
```
这样每个模块文件之间都进行了作用域隔离。包装之后的代码会通过vm原生模块的runInThisContext()方法执行（类似eval，只是具有明确上下文，不污染全局），返回一个具体的function对象。最后，将当前模块对象的exports属性、require()方法、module（模块对象自身），以及在文件定位中得到的完整文件路径和文件目录作为参数传递给这个function()执行。

这就是这些变量并没有定义在每个模块文件中却存在的原因。在执行之后，模块的exports属性被返回给了调用方。exports属性上的任何方法和属性都可以被外部调用到，但是模块中的其余变量或属性则不可直接被调用。

至此，require、exports、module的流程已经完整，这就是Node对CommonJS模块规范的实现。

此外，许多初学者都曾经纠结过为何存在exports的情况下，还存在module.exports。理想情况下，只要赋值给exports即可：

```js
exports = function () {
  // My Class
};
```
但是通常都会得到一个失败的结果。其原因在于，exports对象是通过形参的方式传入的，直接赋值形参会改变形参的引用，但并不能改变作用域外的值。测试代码如下：

```js
var change = function (a) {
  a = 100; console.log(a); // => 100
};
var a = 10;change(a); 
console.log(a); // => 10
```
如果要达到require引入一个类的效果，请赋值给module.exports对象。这个迂回的方案不改变形参的引用。

2. C/C++模块的编译

Node调用process.dlopen()方法进行加载和执行。在Node的架构下，dlopen()方法在Windows和*nix平台下分别有不同的实现，通过libuv兼容层进行了封装。

实际上，.node的模块文件并不需要编译，因为它是编写C/C++模块之后编译生成的，所以这里只有加载和执行的过程。在执行的过程中，模块的exports对象与.node模块产生联系，然后返回给调用者。

C/C++模块给Node使用者带来的优势主要是执行效率方面的，劣势则是C/C++模块的编写门槛比JavaScript高。

3. JSON文件的编译

.json文件的编译是3种编译方式中最简单的。**Node利用fs模块同步读取JSON文件的内容之后，调用JSON.parse()方法得到对象，然后将它赋给模块对象的exports，以供外部调用** 。

JSON文件在用作项目的配置文件时比较有用。如果你定义了一个JSON文件作为配置，那就不必调用fs模块去异步读取和解析，直接调用require()引入即可。此外，你还可以享受到模块缓存的便利，并且二次引入时也没有性能影响。

### 核心模块

Node的核心模块在编译成可执行文件的过程中被编译进了二进制文件。**核心模块其实分为C/C++编写的和JavaScript编写的两部分，其中C/C++文件存放在Node项目的src目录下，JavaScript文件存放在lib目录下。**

#### JavaScript核心模块的编译过程

在编译所有C/C++文件之前，编译程序需要将所有的JavaScript模块文件编译为C/C++代码，此时是否直接将其编译为可执行代码了呢？其实不是。

1. 转存为C/C++代码

Node采用了V8附带的js2c.py工具，将所有内置的JavaScript代码（src/node.js和lib/*.js）转换成C++里的数组，生成node_natives.h头文件，相关代码如下：

```js
namespace node {
  const char node_native[] = { 47, 47, ..};
  const char dgram_native[] = { 47, 47, ..}; 
  const char console_native[] = { 47, 47, ..};
  const char buffer_native[] = { 47, 47, ..}; 
  const char querystring_native[] = { 47, 47, ..};
  const char punycode_native[] = { 47, 42, ..}; 
  ...
  struct _native { 
    const char* name;
    const char* source; size_t source_len;
  };
  static const struct _native natives[] = { 
    { "node", node_native, sizeof(node_native)-1 },
    { "dgram", dgram_native, sizeof(dgram_native)-1 }, 
    ...
  }; 
}
```
在这个过程中，JavaScript代码以字符串的形式存储在node命名空间中，是不可直接执行的。在启动Node进程时，JavaScript代码直接加载进内存中。在加载的过程中，JavaScript核心模块经历标识符分析后直接定位到内存中，比普通的文件模块从磁盘中一处一处查找要快很多。

2. 编译JavaScript核心模块

lib目录下的所有模块文件也没有定义require、module、exports这些变量。在引入JavaScript核心模块的过程中，也经历了头尾包装的过程，然后才执行和导出了exports对象。与文件模块有区别的地方在于：获取源代码的方式（核心模块是从内存中加载的）以及缓存执行结果的位置。

JavaScript核心模块的定义如下面的代码所示，源文件通过process.binding('natives')取出，编译成功的模块缓存到NativeModule._cache对象上，文件模块则缓存到Module._cache对象上：

```js
function NativeModule(id) { 
  this.filename = id + '.js';
  this.id = id; this.exports = {};
  this.loaded = false;
}
NativeModule._source = process.binding('natives');
NativeModule._cache = {};
```

#### C/C++核心模块的编译过程

在核心模块中，有些模块全部由C/C++编写，有些模块则由C/C++完成核心部分，其他部分则由JavaScript实现包装或向外导出，以满足性能需求。后面这种C++模块主内完成核心，JavaScript主外实现封装的模式是Node能够提高性能的常见方式。通常，脚本语言的开发速度优于静态语言，但是其性能则弱于静态语言。而Node的这种复合模式可以在开发速度和性能之间找到平衡点。

这里我们 **将那些由纯C/C++编写的部分统一称为内建模块** ，因为它们 **通常不被用户直接调用** 。Node的buffer、crypto、evals、fs、os等模块都是部分通过C/C++编写的。


1. 内建模块的组织形式在Node中，内建模块的内部结构定义如下：

```c
struct node_module_struct { 
  int version;
  void *dso_handle; const char *filename;
  void (*register_func) (v8::Handle<v8::Object> target); 
  const char *modname;
};
```
每一个内建模块在定义之后，都通过NODE_MODULE宏将模块定义到node命名空间中，模块的具体初始化方法挂载为结构的register_func成员：

```c
#define NODE_MODULE(modname, regfunc)                     \
extern "C" {                                      \
  NODE_MODULE_EXPORT node::node_module_struct modname ## _module = \ 
  {                                           \
    NODE_STANDARD_MODULE_STUFF,                        \ 
    regfunc,                                     \
    NODE_STRINGIFY(modname)                           \ 
   };                                          \
}
```
node_extensions.h文件将这些散列的内建模块统一放进了一个叫node_module_list的数组中，这些模块有：

```
node_buffer
node_crypto
node_evals
node_fs
node_http_parser
node_os
node_zlib
node_timer_wrap
node_tcp_wrap
node_udp_wrap
node_pipe_wrap
node_cares_wrap
node_tty_wrap
node_process_wrap
node_fs_event_wrap
node_signal_watcher
```
这些内建模块的取出也十分简单。Node提供了get_builtin_module()方法从node_module_list数组中取出这些模块。

内建模块的优势在于：首先，它们本身由C/C++编写，**性能上优于脚本语言** ；其次，**在进行文件编译时，它们被编译进二进制文件** 。**一旦Node开始执行，它们被直接加载进内存中，无须再次做标识符定位、文件定位、编译等过程，直接就可执行** 。

2. 内建模块的导出

在Node的所有模块类型中，存在着如下图所示的一种依赖层级关系，即文件模块可能会依赖核心模块，核心模块可能会依赖内建模块。

![依赖层级关系](https://graphbed.qiniu.songxingguo.com/Node-Module/%E4%BE%9D%E8%B5%96%E5%B1%82%E7%BA%A7%E5%85%B3%E7%B3%BB.jpg)

通常，不推荐文件模块直接调用内建模块。如需调用，直接调用核心模块即可，因为核心模块中基本都封装了内建模块。那么内建模块是如何将内部变量或方法导出，以供外部JavaScript核心模块调用的呢？

Node在启动时，会生成一个全局变量process，并提供Binding()方法来协助加载内建模块。Binding()的实现代码在src/node.cc中，具体如下所示：

```c
static Handle<Value> Binding(const Arguments& args) {

HandleScope scope;

Local<String> module = args[0]->ToString();

String::Utf8Value module_v(module); 
node_module_struct* modp

if (binding-cache.isEmpty()){
  binding_cache = Persistent<Object>::New(Object::New());
} 

Local<Object> exports;

if (binding_cache->Has(module)) { 
  exports = binding_cache->Get(module)->ToObject(); 
  return scope.Close(exports);
}

// Append a string to process.moduleLoadList char buf[l024]；
snprintf(buf, 1024, "Binding %s", *module_v); 
uint32_t l = module_load_list->Length(); 
module一load_list->Set(l, String::New(buf));

if ((modp = get_builtin_module(*module_v)) != NULL) { 
exports = Object::New(); 
modp->register一func(exports); 
binding一cache->Set(module, exports);

} else if (!strcmp(*module一v, "constants")) { exports = Object::New();

DefineConstants(exports);

binding一cache->Set(module, exports);

#ifdef _POSIX_
} else if (!strcmp(*module一v, "io一watcher")) { exports = Object::New();

IOWatcher::Initialize(exports); 
binding一cache->Set(module, exports);

#endif

} else if (!strcmp(*module一v, "natives")) { 
  exports = Object::New();
  DefineJavaScript(exports);
  binding一cache->Set(module, exports);
} else {

  return ThrowException(Exception::Error(String::New("No such module")));
}

  return scope.Close(exports);
}
```
在加载内建模块时，我们先创建一个exports空对象，然后调用get_builtin_module()方法取出内建模块对象，通过执行register_func()填充exports对象，最后将exports对象按模块名缓存，并返回给调用方完成导出。

这个方法不仅可以导出内建方法，还能导出一些别的内容。前面提到的JavaScript核心文件被转换为C/C++数组存储后，便是通过process.binding('natives')取出放置在NativeModule._source中的：

```c
NativeModule._source = process.binding('natives');
```
该方法将通过js2c.py工具转换出的字符串数组取出，然后重新转换为普通字符串，以对JavaScript核心模块进行编译和执行。

### 核心模块的引入流程

从下图所示的os原生模块的引入流程可以看到，为了符合CommonJS模块规范，从JavaScript到C/C++的过程是相当复杂的，它要经历C/C++层面的内建模块定义、（JavaScript）核心模块的定义和引入以及（JavaScript）文件模块层面的引入。但是对于用户而言，require()十分简洁、友好。

![os原生模块的引入流程](https://graphbed.qiniu.songxingguo.com/Node-Module/os%E5%8E%9F%E7%94%9F%E6%A8%A1%E5%9D%97%E7%9A%84%E5%BC%95%E5%85%A5%E6%B5%81%E7%A8%8B.jpg)

#### 编写核心模块

核心模块被编译进二进制文件需要遵循一定规则。

核心模块中的JavaScript部分几乎与文件模块的开发相同，遵循CommonJS模块规范，上下文中除了拥有require、module、exports外，还可以调用Node中的一些全局变量，这里不做描述。

下面我们以C/C++模块为例演示如何编写内建模块。为了便于理解，我们先编写一个极其简单的JavaScript版本的原型，这个方法返回一个Hello world!字符串：

```js
exports.sayHello = function () { 
  return 'Hello world!';
};
```
编写内建模块通常分两步完成：

编写头文件和编写C/C++文件。

（1）将以下代码保存为node_hello.h，存放到Node的src目录下：

```c
#ifndef NODE_HELLO_H_
#define NODE_HELLO_H_
#include <v8.h>

namespace node { 
  // 预定义方法
  v8::Handle<v8::Value> SayHello(const v8::Arguments& args);
} #endif
```
（2）编写node_hello.cc，并存储到src目录下：

```c
#include <node.h>
#include <node_hello.h>
#include <v8.h>

namespace node {

using namespace v8;
// 实现预定义的方法
Handle<Value> SayHello(const Arguments& args) {
  HandleScope scope;
  return scope.Close(String::New("Hello world!"));
}
// 给传入的目标对象添加sayHello方法
void Init_Hello(Handle<Object> target) {
  target->Set(String::NewSymbol("sayHello"), FunctionTemplate::New(SayHello)->GetFunction());}
}
// 调用NODE_MODULE()将注册方法定义到内存中
NODE_MODULE(node_hello, node::Init_Hello)
```
以上两步完成了内建模块的编写，但是真正要让Node认为它是内建模块，还需要更改src/node_extensions.h，在NODE_EXT_LIST_END前添加NODE_EXT_LIST_ITEM(node_hello)，以将node_hello模块添加进node_module_list数组中。

其次，还需要让编写的两份代码编译进执行文件，同时需要更改Node的项目生成文件node.gyp，并在'target_name': 'node'节点的sources中添加上新编写的两个文件。然后编译整个Node项目，具体的编译步骤请参见附录A。

编译和安装后，直接在命令行中运行以下代码，将会得到期望的效果：

```js
$ node
> var hello = process.binding('hello');undefined
> hello.sayHello();'Hello world!'
>
```
至此，原生编写过程中需要注意的细节都已表述过了。可以看出，简单的模块通过JavaScript来编写可以大大提高生产效率。这里我们写作本节的目的是希望有能力的读者可以深入Node的核心模块，去学习它或者改进它。


### C/C++扩展模块

对于前端工程师来说，C/C++扩展模块或许比较生疏和晦涩，但是如果你了解了它，在模块出现性能瓶颈时将会对你有极大的帮助。JavaScript的一个典型弱点就是位运算。

JavaScript的位运算参照Java的位运算实现，但是Java位运算是在int型数字的基础上进行的，而JavaScript中只有double型的数据类型，在进行位运算的过程中，需要将double型转换为int型，然后再进行。所以，**在JavaScript层面上做位运算的效率不高** 。

**在应用中，会频繁出现位运算的需求，包括转码、编码等过程，如果通过JavaScript来实现，CPU资源将会耗费很多，这时编写C/C++扩展模块来提升性能的机会来了。**

C/C++扩展模块属于文件模块中的一类。前面讲述文件模块的编译部分时提到，C/C++模块通过预先编译为.node文件，然后调用process.dlopen()方法加载执行。在这一节中，我们将分析整个C/C++扩展模块的编写、编译、加载、导出的过程。

在开始编写扩展模块之前，需要强调的一点是，Node的原生模块一定程度上是可以跨平台的，其前提条件是源代码可以支持在*nix和Windows上编译，其中*nix下通过g++/gcc等编译器编译为动态链接共享对象文件（.so），在Windows下则需要通过Visual C++的编译器编译为动态链接库文件（.dll），如图2-6所示。这里有一个让人迷惑的地方，那就是引用加载时却是.node文件。其实.node的扩展名只是为了看起来更自然一点，不会因为平台差异产生不同的感觉。实际上，在Windows下它是一个.dll文件，在*nix下则是一个.so文件。为了实现跨平台，dlopen()方法在内部实现时区分了平台，分别用的是加载.so和.dll的方式。下图为扩展模块在不同平台上编译和加载的详细过程。

![扩展模块在不同平台上编译和加载的详细过程](https://graphbed.qiniu.songxingguo.com/Node-Module/%E6%89%A9%E5%B1%95%E6%A8%A1%E5%9D%97%E5%9C%A8%E4%B8%8D%E5%90%8C%E5%B9%B3%E5%8F%B0%E4%B8%8A%E7%BC%96%E8%AF%91%E5%92%8C%E5%8A%A0%E8%BD%BD%E7%9A%84%E8%AF%A6%E7%BB%86%E8%BF%87%E7%A8%8B.jpg)

值得注意的是，一个平台下的.node文件在另一个平台下是无法加载执行的，必须重新用各自平台下的编译器编译为正确的.node文件。

#### 前提条件

如果想要编写高质量的C/C++扩展模块，还需要深厚的C/C++编程功底才行。除此之外，以下这些条目都是不能避开的，在了解它们之后，可以让你在编写过程中事半功倍。

- GYP项目生成工具。在Node 0.6中，第三方模块通过它自身提供的node_waf工具实现编译，但是它是*nix平台下的产物，无法实现跨平台编译。在Node 0.8中，Node决定摒弃掉node_waf而采用跨平台效果更好的项目生成器，它就是GYP工具，即“Generate Your Projects”短句的缩写。它的好处在于，可以帮助你生成各个平台下的项目文件，比如Windows下的Visual Studio解决方案文件（.sln）、Mac下的XCode项目配置文件以及Scons工具。在这个基础上，再动用各自平台下的编译器编译项目。这大大减少了跨平台模块在项目组织上的精力投入。

  Node源码中一度出现过各种项目文件，后来均统一为GYP工具。这除了可以减少编写跨平台项目文件的工作量外，另一个简单的原因就是Node自身的源码就是通过GYP编译的。为此，Nathan Rajlich基于GYP为Node提供了一个专有的扩展构建工具node-gyp，这个工具通过npm install -g node-gyp这个命令即可安装。
  
- V8引擎C++库。V8是Node自身的动力来源之一。它自身由C++写成，可以实现JavaScript与C++的互相调用。

- libuv库。它是Node自身的动力来源之二。Node能够实现跨平台的一个诀窍就是它的libuv库，这个库是跨平台的一层封装，通过它去调用一些底层操作，比自己在各个平台下编写实现要高效得多。libuv封装的功能包括事件循环、文件操作等。

- Node内部库。写C++模块时，免不了要做一些面向对象的编程工作，而Node自身提供了一些C++代码，比如node::ObjectWrap类可以用来包装你的自定义类，它可以帮助实现对象回收等工作。

- 其他库。其他存在deps目录下的库在编写扩展模块时也许可以帮助你，比如zlib、openssl、http_parser等。


#### C/C++扩展模块的编写

在介绍C/C++内建模块时，其实已经介绍了C/C++模块的编写方式。普通的扩展模块与内建模块的区别在于无须将源代码编译进Node，而是通过dlopen()方法动态加载。所以在编写普通的扩展模块时，无须将源代码写进node命名空间，也不需要提供头文件。下面我们将采用同一个例子来介绍C/C++扩展模块的编写。

它的JavaScript原型代码与前面的例子一样：

```js
exports.sayHello = function () {
  return 'Hello world!';
};
```
新建hello目录作为自己的项目位置，编写hello.cc并将其存储到src目录下，相关代码如下：

```c
#include <node.h>
#include <v8.h>

using namespace v8;
// 实现预定义的方法
Handle<Value> SayHello(const Arguments& args) { 
  HandleScope scope;
  return scope.Close(String::New("Hello world!"));
}
// 给传入的目标对象添加sayHello()方法
void Init_Hello(Handle<Object> target) { 
  target->Set(String::NewSymbol("sayHello"), FunctionTemplate::New(SayHello)->GetFunction());
}
// 调用NODE_MODULE()方法将注册方法定义到内存中
NODE_MODULE(hello, Init_Hello)
```
C/C++扩展模块与内建模块的套路一样，将方法挂载在target对象上，然后通过NODE_MODULE声明即可。

由于不像编写内建模块那样将对象声明到node_module_list链表中，所以无法被认作是一个原生模块，只能通过dlopen()来动态加载，然后导出给JavaScript调用。

#### C/C++扩展模块的编译

在GYP工具的帮助下，C/C++扩展模块的编译是一件省心的事情，无须为每个平台编写不同的项目编译文件。写好.gyp项目文件是除编码外的头等大事，然而你也无须担心此事太难，因为.gyp项目文件是足够简单的。node-gyp约定.gyp文件为binding.gyp，其内容如下所示：

```js
{ 
  'targets': [
    { 
      'target_name': 'hello',
      'sources': [ 
        'hello.cc'
      ], 
      'conditions': [
        ['OS == "win"', 
        {
          'libraries': ['-lnode.lib'] 
        }
        ] 
      ]
    } 
  ]
}
```
然后调用：

```
$ node-gyp configure
```
会得到如下的输出结果：

```
gyp info it worked if it ends with okgyp info using node-gyp@0.8.3
gyp info using node@0.8.14 | darwin | x64gyp info spawn python
gyp info spawn args [ '/usr/local/lib/node_modules/node-gyp/gyp/gyp',gyp info spawn args 'binding.gyp',
gyp info spawn args '-f',gyp info spawn args 'make',
gyp info spawn args '-I',gyp info spawn args '/Users/jacksontian/git/diveintonode/examples/02/addon/build/config.gypi',
gyp info spawn args '-I',gyp info spawn args '/usr/local/lib/node_modules/node-gyp/addon.gypi',
gyp info spawn args '-I',gyp info spawn args '/Users/jacksontian/.node-gyp/0.8.14/common.gypi',
gyp info spawn args '-Dlibrary=shared_library',gyp info spawn args '-Dvisibility=default',
gyp info spawn args '-Dnode_root_dir=/Users/jacksontian/.node-gyp/0.8.14',gyp info spawn args '-Dmodule_root_dir=/Users/jacksontian/git/diveintonode/examples/02/addon',
gyp info spawn args '--depth=.',gyp info spawn args '--generator-output',
gyp info spawn args 'build',gyp info spawn args '-Goutput_dir=.' ] gyp info ok
node-gyp configure
```
这个命令会在当前目录中创建build目录，并生成系统相关的项目文件。在*nix平台下，build目录中会出现Makefile等文件；在Windows下，则会生成vcxproj等文件。
继续执行如下代码：

```
$ node-gyp build
```
会得到如下的输出结果：

```
gyp info it worked if it ends with ok
gyp info using node-gyp@0.8.3gyp info using node@0.8.14 | darwin | x64
gyp info spawn makegyp info spawn args [ 'BUILDTYPE=Release', '-C', 'build' ]
CXX(target) Release/obj.target/hello/hello.o SOLINK_MODULE(target) Release/hello.node
SOLINK_MODULE(target) Release/hello.node: Finished gyp info ok
```
编译过程会根据平台不同，分别通过make或vcbuild进行编译。编译完成后，hello.node文件会生成在build/Release目录下。


### C/C++扩展模块的加载

得到hello.node结果文件后，如何调用扩展模块其实在前面已经提及。require()方法通过解析标识符、路径分析、文件定位，然后加载执行即可。下面的代码引入前面编译得到的.node文件，并调用执行其中的方法：

```
var hello = require('./build/Release/hello.node');console.log(hello.sayHello());
```
以上代码存为hello.js，调用node hello.js命令即可得到如下的输出结果：

```
Hello world!
```
对于以.node为扩展名的文件，Node将会调用process.dlopen()方法去加载文件：

```
//Native extension for .node
Module._extensions['.node'] = process.dlopen;
```
对于调用者而言，require()是轻松愉快的。对于扩展模块的编写者来说，process.dlopen()中隐含的过程值得了解一番。

如下图所示，require()在引入.node文件的过程中，实际上经历了4个层面上的调用。

![require()在引入.node文件的过程](https://graphbed.qiniu.songxingguo.com/Node-Module/require%28%29%E5%9C%A8%E5%BC%95%E5%85%A5.node%E6%96%87%E4%BB%B6%E7%9A%84%E8%BF%87%E7%A8%8B.jpg)

加载.node文件实际上经历了两个步骤，第一个步骤是调用uv_dlopen()方法去打开动态链接库，第二个步骤是调用uv_dlsym()方法找到动态链接库中通过NODE_MODULE宏定义的方法地址。这两个过程都是通过libuv库进行封装的：在*nix平台下实际上调用的是dlfcn.h头文件中定义的dlopen()和dlsym()两个方法；在Windows平台则是通过LoadLibraryExW()和GetProcAddress()这两个方法实现的，它们分别加载.so和.dll文件（实际为.node文件）。

这里对libuv函数的调用充分表现Node利用libuv实现跨平台的方式，这样的情景在很多地方还会出现。

由于编写模块时通过NODE_MODULE将模块定义为node_module_struct结构，所以在获取函数地址之后，将它映射为node_module_struct结构几乎是无缝对接的。接下来的过程就是将传入的exports对象作为实参运行，将C++中定义的方法挂载在exports对象上，然后调用者就可以轻松调用了。

C/C++扩展模块与JavaScript模块的区别在于加载之后不需要编译，直接执行之后就可以被外部调用了，其加载速度比JavaScript模块略快。

使用C/C++扩展模块的一个好处在于可以更灵活和动态地加载它们，保持Node模块自身简单性的同时，给予Node无限的可扩展性。

关于node-gyp工具的更多细节可以参见 https://github.com/TooTallNate/node-gyp （作者为Nathan Rajlich，Node源码的核心贡献者之一）。

### 模块调用栈

结束文件模块、核心模块、内建模块、C/C++扩展模块等的阐述之后，有必要明确一下各种模块之间的调用关系，如下图所示。

![模块之间的调用关系](https://graphbed.qiniu.songxingguo.com/Node-Module/%E6%A8%A1%E5%9D%97%E4%B9%8B%E9%97%B4%E7%9A%84%E8%B0%83%E7%94%A8%E5%85%B3%E7%B3%BB.jpg)

C/C++内建模块属于最底层的模块，它属于核心模块，主要提供API给JavaScript核心模块和第三方JavaScript文件模块调用。如果你不是非常了解要调用的C/C++内建模块，请尽量避免通过process.binding()方法直接调用，这是不推荐的。

JavaScript核心模块主要扮演的职责有两类：一类是作为C/C++内建模块的封装层和桥接层，供文件模块调用；一类是纯粹的功能模块，它不需要跟底层打交道，但是又十分重要。文件模块通常由第三方编写，包括普通JavaScript模块和C/C++扩展模块，主要调用方向为普通JavaScript模块调用扩展模块。

### 包与NPM

Node组织了自身的核心模块，也使得第三方文件模块可以有序地编写和使用。但是在第三方模块中，模块与模块之间仍然是散列在各地的，相互之间不能直接引用。而在模块之外，包和NPM则是将模块联系起来的一种机制。

在介绍NPM之前，不得不提起 CommonJS的包规范。JavaScript不似Java或者其他语言那样，具有模块和包结构。Node对模块规范的实现，一定程度上解决了变量依赖、依赖关系等代码组织性问题。包的出现，则是在模块的基础上进一步组织JavaScript代码。下图为包组织模块示意图。

![包组织模块示意图](https://graphbed.qiniu.songxingguo.com/Node-Module/%E5%8C%85%E7%BB%84%E7%BB%87%E6%A8%A1%E5%9D%97%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

CommonJS的包规范的定义其实也十分简单，它由包结构和包描述文件两个部分组成，前者用于组织包中的各种文件，后者则用于描述包的相关信息，以供外部读取分析。

#### 包结构

包实际上是一个存档文件，即一个目录直接打包为.zip或tar.gz格式的文件，安装后解压还原为目录。完全符合CommonJS规范的包目录应该包含如下这些文件。

- package.json：包描述文件。bin：用于存放可执行二进制文件的目录。
- lib：用于存放JavaScript代码的目录。doc：用于存放文档的目录。
- test：用于存放单元测试用例的代码。

可以看到，CommonJS包规范从文档、测试等方面都做过考虑。当一个包完成后向外公布时，用户看到单元测试和文档的时候，会给他们一种踏实可靠的感觉。

#### 包描述文件与NPM

**包描述文件用于表达非代码相关的信息，它是一个JSON格式的文件——package.json**，位于包的根目录下，是包的重要组成部分** 。而NPM的所有行为都与包描述文件的字段息息相关。由于CommonJS包规范尚处于草案阶段，NPM在实践中做了一定的取舍，具体细节在后面会介绍到。

CommonJS为package.json文件定义了如下一些必需的字段。

- name。包名。规范定义它需要由小写的字母和数字组成，可以包含.、_和-，但不允许出现空格。包名必须是唯一的，以免对外公布时产生重名冲突的误解。除此之外，NPM还建议不要在包名中附带上node或js来重复标识它是JavaScript或Node模块。

- description。包简介。

- version。版本号。一个语义化的版本号，这在http://semver.org/上有详细定义，通常为major.minor.revision格式。该版本号十分重要，常常用于一些版本控制的场合。

- keywords。关键词数组，NPM中主要用来做分类搜索。一个好的关键词数组有利于用户快速找到你编写的包。

- maintainers。包维护者列表。每个维护者由name、email和web这3个属性组成。示例如下：

  ```
  "maintainers": [{ "name": "Jackson Tian", 
  "email": "shyvo1987@gmail.com", 
  "web": "http://html5ify.com" }]
  ```
  NPM通过该属性进行权限认证。

- contributors。贡献者列表。在开源社区中，为开源项目提供代码是经常出现的事情，如果名字能出现在知名项目的contributors列表中，是一件比较有荣誉感的事。列表中的第一个贡献应当是包的作者本人。它的格式与维护者列表相同。

- bugs。一个可以反馈bug的网页地址或邮件地址。

- licenses。当前包所使用的许可证列表，表示这个包可以在哪些许可证下使用。它的格式如下：

  ```
  "licenses": [{ "type": "GPLv2", "url": "http://www.example.com/licenses/gpl.html", }]
  ```
- repositories。托管源代码的位置列表，表明可以通过哪些方式和地址访问包的源代码。

- dependencies。使用当前包所需要依赖的包列表。这个属性十分重要，NPM会通过这个属性帮助自动加载依赖的包。

除了必选字段外，规范还定义了一部分可选字段，具体如下所示。

- homepage。当前包的网站地址。

- os。操作系统支持列表。这些操作系统的取值包括aix、freebsd、linux、macos、solaris、vxworks、windows。如果设置了列表为空，则不对操作系统做任何假设。

- cpu。CPU架构的支持列表，有效的架构名称有arm、mips、ppc、sparc、x86和x86_64。同os一样，如果列表为空，则不对CPU架构做任何假设。

- engine。支持的JavaScript引擎列表，有效的引擎取值包括ejs、flusspferd、gpsee、jsc、spidermonkey、narwhal、node和v8。builtin。标志当前包是否是内建在底层系统的标准组件。

- directories。包目录说明。implements。实现规范的列表。标志当前包实现了CommonJS的哪些规范。

- scripts。脚本说明对象。它主要被包管理器用来安装、编译、测试和卸载包。示例如下：

  ```
  "scripts": { "install": "install.js",
  "uninstall": "uninstall.js", "build": "build.js",
  "doc": "make-doc.js", "test": "test.js" }
  ```
包规范的定义可以帮助Node解决依赖包安装的问题，而NPM正是基于该规范进行了实现。

在包描述文件的规范中，NPM实际需要的字段主要有name、version、description、keywords、repositories、author、bin、main、scripts、engines、dependencies、devDependencies。

与包规范的区别在于多了author、bin、main和devDependencies这4个字段，下面补充说明一下。

- author。包作者。

- bin。一些包作者希望包可以作为命令行工具使用。配置好bin字段后，通过npm install package_name -g命令可以将脚本添加到执行路径中，之后可以在命令行中直接执行。前面的node-gyp即是这样安装的。通过-g命令安装的模块包称为全局模式。

- main。模块引入方法require()在引入包时，会优先检查这个字段，并将其作为包中其余模块的入口。如果不存在这个字段，require()方法会查找包目录下的index.js、index.node、index.json文件作为默认入口。

- devDependencies。一些模块只在开发时需要依赖。配置这个属性，可以提示包的后续开发者安装依赖包。

下面是知名框架express项目的package.json文件，具有一定的参考意义：

```
{
	"name": "express",
	"description": "Sinatra inspired web development framework",
	"version": "3.3.4",
	"author": "TJ Holowaychuk <tj@vision-media.ca>",
	"contributors": [{
			"name": "TJ Holowaychuk",
			"email": "tj@vision-media.ca"
		},
		{
			"name": "Aaron Heckmann",
			"email": "aaron.heckmann+github@gmail.com"
		},
		{
			"name": "Ciaran Jessup",
			"email": "ciaranj@gmail.com"
		},
		{
			"name": "Guillermo Rauch",
			"email": "rauchg@gmail.com"
		}
	],
	"dependencies": {
		"connect": "2.8.4",
		"commander": "1.2.0",
		"range-parser": "0.0.4",
		"mkdirp": "0.3.5",
		"cookie": "0.1.0",
		"buffer-crc32": "0.2.1",
		"fresh": "0.1.0",
		"methods": "0.0.1",
		"send": "0.1.3",
		"cookie-signature": "1.0.1",
		"debug": "*"
	},
	"devDependencies": {
		"ejs": "*",
		"mocha": "*",
		"jade": "0.30.0",
		"hjs": "*",
		"stylus": "*",
		"should": "*",
		"connect-redis": "*",
		"marked": "*",
		"supertest": "0.6.0"
	},
	"keywords": [
		"express", "framework",
		"sinatra", "web",
		"rest", "restful",
		"router", "app",
		"api"
	],
	"repository": "git://github.com/visionmedia/express",
	"main": "index",
	"bin": {
		"express": "./bin/express"
	},
	"scripts": {
		"prepublish": "npm prune",
		"test": "make test"
	},
	"engines": {
		"node": "*"
	}
}
```

#### NPM常用功能

CommonJS包规范是理论，NPM是其中的一种实践。NPM之于Node，相当于gem之于Ruby，pear之于PHP。对于Node而言，NPM帮助完成了第三方模块的发布、安装和依赖等。借助NPM，Node与第三方模块之间形成了很好的一个生态系统。

借助NPM，可以帮助用户快速安装和管理依赖包。

1. 查看帮助

在安装Node之后，执行npm -v命令可以查看当前NPM的版本：

```
$ npm -v
1.2.32
```
在不熟悉NPM的命令之前，可以直接执行NPM查看到帮助引导说明：

```
$ npm
Usage: npm <command>
where <command> is one of:
add-user, adduser, apihelp, author, bin, bugs, c, cache, completion, config, ddp, dedupe, deprecate, docs, edit,
explore, faq, find, find-dupes, get, help, help-search, home, i, info, init, install, isntall, issues, la, link,
list, ll, ln, login, ls, outdated, owner, pack, prefix, prune, publish, r, rb, rebuild, remove, restart, rm, root,
run-script, s, se, search, set, show, shrinkwrap, star, stars, start, stop, submodule, tag, test, tst, un,
uninstall, unlink, unpublish, unstar, up, update, version, view, whoami
npm <cmd> -h quick help on <cmd>
npm -l display full usage infonpm faq commonly asked questions
npm help <term> search for help on <term>npm help npm involved overview
Specify configs in the ini-formatted file:
/Users/jacksontian/.npmrcor on the command line via: npm <command> --key value
Config info can be viewed via: npm help config npm@1.2.32 /usr/local/lib/node_modules/npm
```
可以看到，帮助中列出了所有的命令，其中npm help <command>可以查看具体的命令说明。

2. 安装依赖包

安装依赖包是NPM最常见的用法，它的执行语句是npm install express。执行该命令后，NPM会在当前目录下创建node_modules目录，然后在node_modules目录下创建express目录，接着将包解压到这个目录下。

安装好依赖包后，直接在代码中调用require('express');即可引入该包。require()方法在做路径分析的时候会通过模块路径查找到express所在的位置。模块引入和包的安装这两个步骤是相辅相承的。

全局模式安装

如果包中含有命令行工具，那么需要执行npm install express -g命令进行全局模式安装。需要注意的是，全局模式并不是将一个模块包安装为一个全局包的意思，它并不意味着可以从任何地方通过require()来引用到它。

全局模式这个称谓其实并不精确，存在诸多误导。实际上，-g是将一个包安装为全局可用的可执行命令。它根据包描述文件中的bin字段配置，将实际脚本链接到与Node可执行文件相同的路径下：

```
"bin": {
  "express": "./bin/express"
},
```
事实上，通过全局模式安装的所有模块包都被安装进了一个统一的目录下，这个目录可以通过如下方式推算出来：

```
path.resolve(process.execPath, '..', '..', 'lib', 'node_modules');
```
如果Node可执行文件的位置是/usr/local/bin/node，那么模块目录就是/usr/local/lib/node_modules。最后，通过软链接的方式将bin字段配置的可执行文件链接到Node的可执行目录下。

从本地安装

对于一些没有发布到NPM上的包，或是因为网络原因导致无法直接安装的包，可以通过将包下载到本地，然后以本地安装。本地安装只需为NPM指明package.json文件所在的位置即可：它可以是一个包含package.json的存档文件，也可以是一个URL地址，也可以是一个目录下有package.json文件的目录位置。具体参数如下：

```
npm install <tarball file>
npm install <tarball url>
npm install <folder>
```

从非官方源安装

如果不能通过官方源安装，可以通过镜像源安装。在执行命令时，添加--registry=http://registry.url即可，示例如下：

```
npm install underscore --registry=http://registry.url
```
如果使用过程中几乎都采用镜像源安装，可以执行以下命令指定默认源：

```
npm config set registry http://registry.url
```

3. NPM钩子命令

另一个需要说明的是C/C++模块实际上是编译后才能使用的。package.json中scripts字段的提出就是让包在安装或者卸载等过程中提供钩子机制，示例如下：

```
"scripts": {
	"preinstall": "preinstall.js",
	"install": "install.js",
	"uninstall": "uninstall.js",
	"test": "test.js"
}
```
在以上字段中执行npm install <package>时，preinstall指向的脚本将会被加载执行，然后install指向的脚本会被执行。在执行npm uninstall <package>时，uninstall指向的脚本也许会做一些清理工作等。

当在一个具体的包目录下执行npm test时，将会运行test指向的脚本。一个优秀的包应当包含测试用例，并在package.json文件中配置好运行测试的命令，方便用户运行测试用例，以便检验包是否稳定可靠。

4. 发布包

为了将整个NPM的流程串联起来，这里将演示如何编写一个包，将其发布到NPM仓库中，并通过NPM安装回本地。

编写模块

模块的内容我们尽量保持简单，这里还是以sayHello作为例子，相关代码如下：

```
exports.sayHello = function () { return 'Hello, world.';
};
```
将这段代码保存为hello.js即可。

初始化包描述文件

package.json文件的内容尽管相对较多，但是实际发布一个包时并不需要一行一行编写。NPM提供的npm init命令会帮助你生成package.json文件，具体如下所示：

```
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sane defaults.

See `npm help json`for definitive documentation on these fieldsand exactly what they do .

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^ C at any time to quit.name: (module) hello_test_jackson
version: (0.0 .0) 0.0 .1 
description: A hello world package
entry point: (hello.js). / hello.js
test command:
git repository: keywords: Hello world
author: Jackson Tianlicense: (BSD) MIT
About to write to / Users / jacksontian / git / diveintonode / examples / 03 / module / package.json: {
	"name": "hello_test_jackson",
	"version": "0.0.1",
	"description": "A hello world package",
	"main": "./hello.js",
	"scripts": {
		"test": "echo \"Error: no test specified\" && exit 1"
	},
	"repository": "",
	"keywords": [
		"Hello", "world"
	],
	"author": "Jackson Tian",
	"license": "MIT"
}
Is this ok ? (yes) yes
npm WARN package.json hello_test_jackson @0 .0 .1 No README.md file found!
```
NPM通过提问式的交互逐个填入选项，最后生成预览的包描述文件。如果你满意，输入yes，此时会在目录下得到package.json文件。

注册包仓库账号

为了维护包，NPM必须要使用仓库账号才允许将包发布到仓库中。注册账号的命令是npm adduser。这也是一个提问式的交互过程，按顺序进行即可：

```
$ npm adduser
Username: (jacksontian)
Email: (shyvo1987@gmail.com)
```
上传包上传包的命令是npm publish <folder>。在刚刚创建的package.json文件所在的目录下，执行npm publish .开始上传包，相关代码如下：

```
$ npm publish .npm http PUT http://registry.npmjs.org/hello_test_jackson
npm http 201 http://registry.npmjs.org/hello_test_jacksonnpm http GET http://registry.npmjs.org/hello_test_jackson
npm http 200 http://registry.npmjs.org/hello_test_jacksonnpm http PUT http://registry.npmjs.org/hello_test_jackson/0.0.1/-tag/latest
npm http 201 http://registry.npmjs.org/hello_test_jackson/0.0.1/-tag/latestnpm http GET http://registry.npmjs.org/hello_test_jackson
npm http 200 http://registry.npmjs.org/hello_test_jacksonnpm http PUT http://registry.npmjs.org/hello_test_jackson/-/hello_test_jackson-0.0.1.tgz/-rev/2-2d64e0946b86687
8bb252f182070c1d5npm http 201
http://registry.npmjs.org/hello_test_jackson/-/hello_test_jackson-0.0.1.tgz/-rev/2-2d64e0946b86687 8bb252f182070c1d5
+ hello_test_jackson@0.0.1
```
在这个过程中，NPM会将目录打包为一个存档文件，然后上传到官方源仓库中。

安装包

为了体验和测试自己上传的包，可以换一个目录执行npm install hello_test_jackson安装它：

```
$ npm install hello_test_jackson --registry=http://registry.npmjs.org
npm http GET http://registry.npmjs.org/hello_test_jackson
npm http 200 http://registry.npmjs.org/hello_test_jackson
hello_test_jackson@0.0.1 ./node_modules/hello_test_jackson
```
管理包权限

通常，一个包只有一个人拥有权限进行发布。如果需要多人进行发布，可以使用npm owner命令帮助你管理包的所有者：

```
$ npm owner ls eventproxy
npm http GET https://registry.npmjs.org/eventproxy
npm http 200 https://registry.npmjs.org/eventproxy
jacksontian <shyvo1987@gmail.com>
```
使用这个命令，也可以添加包的拥有者，删除一个包的拥有者：

```
npm owner ls <package name>
npm owner add <user> <package name>
npm owner rm <user> <package name>
```

5. 分析包

在使用NPM的过程中，或许你不能确认当前目录下能否通过require()顺利引入想要的包，这时可以执行npm ls分析包。

这个命令可以为你分析出当前路径下能够通过模块路径找到的所有包，并生成依赖树，如下：

```
$ npm ls
/Users/jacksontian├─┬ connect@2.0.3
│ ├── crc@0.1.0│ ├── debug@0.6.0
│ ├── formidable@1.0.9│ ├── mime@1.2.4
│ └── qs@0.4.2├── hello_test_jackson@0.0.1
└── urllib@0.2.3
```

#### 局域NPM

在企业的内部应用中使用NPM与开源社区中使用有一定的差别。企业的限制在于，一方面需要享受到模块开发带来的低耦合和项目组织上的好处，另一方面却要考虑到模块保密性的问题。所以，通过NPM共享和发布存在潜在的风险。

为了同时能够享受到NPM上众多的包，同时对自己的包进行保密和限制，现有的解决方案就是企业搭建自己的NPM仓库。所幸，NPM自身是开源的，无论是它的服务器端和客户端。通过源代码搭建自己的仓库并不是什么秘密。

与镜像仓库不同的地方在于，企业局域NPM可以选择不同步官方源仓库中的包。下图为企业中混合使用官方仓库和局域仓库的示意图。

![混合使用官方仓库和局域仓库的示意图](https://graphbed.qiniu.songxingguo.com/Node-Module/%E6%B7%B7%E5%90%88%E4%BD%BF%E7%94%A8%E5%AE%98%E6%96%B9%E4%BB%93%E5%BA%93%E5%92%8C%E5%B1%80%E5%9F%9F%E4%BB%93%E5%BA%93%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

对于企业内部而言，私有的可重用模块可以打包到局域NPM仓库中，这样可以保持更新的中心化，不至于让各个小项目各自维护相同功能的模块，杜绝通过复制粘贴实现代码共享的行为。

#### NPM潜在问题

作为为模块和包服务的工具，NPM十分便捷。它实质上已经是一个包共享平台，所有人都可以贡献模块并将其打包分享到这个平台上，也可以在许可证（大多是MIT许可证）的允许下免费使用它们。NPM提供的这些便捷，将模块链接到一个共享平台上，缩短了贡献者与使用者之间的距离，这十分有利于模块的传播，进而也十分利于Node的推广。

Node选择了JavaScript，这门语言拥有极大的开发人员基数，具有强大的生产力；另一部分则是因为CommonJS规范和NPM，它们使得产品能够更好地组织、传播和使用。

潜在的问题在于，在NPM平台上，每个人都可以分享包到平台上，鉴于开发人员水平不一，上面的 **包的质量也良莠不齐** 。另一个问题则是， **Node代码可以运行在服务器端，需要考虑安全问题** 。

尽管NPM没有硬性的方式去评判一个包的质量和安全，好在开源社区也有它内在的健康发展机制，那就是 **口碑效应** ，其中NPM模块首页（ https://npmjs.org/ ）上的依赖榜可以说明模块的质量和可靠性。

第二个可以考查质量的地方是GitHub，NPM中大多的包都是通过GitHub托管的，模块项目的观察者数量和分支数量也能从侧面反映这个模块的 **可靠性** 和 **流行度** 。

第三个可以考量包质量的地方 **在于包中的测试用例和文档的状况** ，一个没有单元测试的包基本上是无法被信任的，没有文档的包，使用者使用时内心也是不踏实的。

总体而言，符合Kwalitee的模块要满足的条件与上述提及的考查点大致相同。

- 具备良好的测试。具备良好的文档（README、API）。
- 具备良好的测试覆盖率。具备良好的编码规范。
- 更多条件。

### 前后端共用模块

谈论了许多后端模块的具体实现后，现在我们围绕CommonJS规范再次回到前端模块上。JavaScript在Node出现之后，比别的编程语言多了一项优势，那就是一些模块可以在前后端实现共用，这是因为很多API在各个宿主环境下都提供。但是在实际情况中，前后端的环境是略有差别的。

#### 模块的侧重点

前后端JavaScript分别搁置在HTTP的两端，它们扮演的角色并不同。**浏览器端的JavaScript需要经历从同一个服务器端分发到多个客户端执行，而服务器端JavaScript则是相同的代码需要多次执行** 。

前者的瓶颈在于 **带宽** ，后者的瓶颈则在于 **CPU和内存等资源** 。前者需要 **通过网络加载代码** ，后者 **从磁盘中加载** ，两者的加载速度不在一个数量级上。

纵观Node的模块引入过程，几乎全都是同步的。尽管与Node强调异步的行为有些相反，但它是合理的。但是如果前端模块也采用同步的方式来引入，那将会在用户体验上造成很大的问题。UI在初始化过程中需要花费很多时间来等待脚本加载完成。

鉴于网络的原因，CommonJS为后端JavaScript制定的规范并不完全适合前端的应用场景。经过一段争执之后，**AMD规范** 最终在前端应用场景中胜出。它的全称是 **Asynchronous Module Definition** ，即是“ **异步模块定义** ”，详见https://github.com/amdjs/amdjs-api/wiki/AMD 。除此之外，还有玉伯定义的CMD规范。

####  AMD规范

AMD规范是CommonJS模块规范的一个延伸，它的模块定义如下：

```
define(id?, dependencies?, factory);
```
它的模块id和依赖是可选的，与Node模块相似的地方在于factory的内容就是实际代码的内容。下面的代码定义了一个简单的模块：

```
define(function() { 
  var exports = {};
  exports.sayHello = function() { 
    alert('Hello from module: ' + module.id);
  }; 
  return exports;
});
```
**不同之处在于AMD模块需要用define来明确定义一个模块，而在Node实现中是隐式包装的** ，它们的目的是进行作用域隔离，仅在需要的时候被引入，避免掉过去那种通过全局变量或者全局命名空间的方式，以免变量污染和不小心被修改。另一个区别则是 **内容需要通过返回的方式实现导出** 。

#### CMD规范

CMD规范由国内的玉伯提出，与AMD规范的主要区别在于定义模块和依赖引入的部分。AMD需要在声明模块的时候指定所有的依赖，通过形参传递依赖到模块内容中：

```
define(['dep1', 'dep2'], function (dep1, dep2) { 
  return function () {};
});
```
与AMD模块规范相比，**CMD模块更接近于Node对CommonJS规范的定义** ：

```
define(factory);
```
在依赖部分，CMD支持动态引入，示例如下：

```
define(function(require, exports, module) { 
  // The module code goes here
});
```
require、exports和module通过形参传递给模块，在需要依赖模块时，随时调用require()引入即可。

#### 兼容多种模块规范

为了让同一个模块可以运行在前后端，在写作过程中需要考虑兼容前端也实现了模块规范的环境。为了保持前后端的一致性，类库开发者需要将类库代码包装在一个闭包内。以下代码演示如何将hello()方法定义到不同的运行环境中，它能够兼容Node、AMD、CMD以及常见的浏览器环境中：

```js
;(function(name, definition) {
	// 检测上下文环境是否为AMD或CMD 
   var hasDefine = typeof define === 'function',
   // 检查上下文环境是否为 Node 
   hasExports = typeof module !== 'undefined' && module.exports;
	if (hasDefine) {
	 // AMD环境或CMD环境 
     define(definition);
	} else if (hasExports) { // 定义为普通Node模块
		module.exports = definition();
	} else {
		// 将模块的执行结果挂在window变量中，在浏览器中this指向window对象   
      this[name] = definition();
	}
})('hello', function() {
	var hello = function() {};
	return hello;
});
```
### 总结

CommonJS提出的规范均十分简单，但是现实意义却十分强大。Node通过模块规范，组织了自身的原生模块，弥补JavaScript弱结构性的问题，形成了稳定的结构，并向外提供服务。NPM通过对包规范的支持，有效地组织了第三方模块，这使得项目开发中的依赖问题得到很好的解决，并有效提供了分享和传播的平台，借助第三方开源力量，使得Node第三方模块的发展速度前所未有，这对于其他后端JavaScript语言实现而言是从未有过的。从一定的角度上讲，CommonJS规范帮助Node形成了它的骨骼。只有茁壮的根，才能培养出茂盛的枝叶，并成长为参天大树。正是这些底层的规范和实践，使得Node有序地发展着，摆脱掉过去JavaScript纷乱和被误解的局面，进而进化成良性的生态系统。