title: 《JavaScript 高级程序设计》学习笔记-DOM
author: songxingguo
tags:
  - JavaScript
categories:
  - 前端技术
  - 读书笔记
date: 2018-08-21 20:27:00
---
# DOM

DOM（文档对象模型）是针对 HTML 和 XML 文档的一个 API（应用程序编程接口）。DOM 描
绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。DOM 脱胎于
Netscape 及微软公司创始的 DHTML（动态 HTML），但现在它已经成为表现和操作页面标记的真正的跨
平台、语言中立的方式。
1998 年 10 月 DOM１级规范成为 W3C 的推荐标准，为基本的文档结构及查询提供了接口。本章主
要讨论与浏览器中的 HTML 页面相关的 DOM1 级的特性和应用，以及 JavaScript 对 DOM1 级的实现。
IE、Firefox、Safari、Chrome 和 Opera 都非常完善地实现了 DOM。

>注意，IE 中的所有 DOM 对象都是以 COM对象的形式实现的。这意味着 IE 中的
DOM 对象与原生 JavaScript 对象的行为或活动特点并不一致。本章将较多地谈及这些
差异。

<!-- more -->

## 节点层次

DOM 可以将任何 HTML 或 XML 文档描绘成一个由多层节点构成的结构。节点分为几种不同的类
型，每种类型分别表示文档中不同的信息及（或）标记。每个节点都拥有各自的特点、数据和方法，另
外也与其他节点存在某种关系。节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点
为根节点的树形结构。以下面的 HTML 为例：

```html
<html>
<head>
<title>Sample Page</title>
</head>
<body>
<p>Hello World!</p>
</body>
</html>
```
可以将这个简单的 HTML 文档表示为一个层次结构，如图 10-1 所示。
文档节点是每个文档的根节点。在这个例子中，文档节点只有一个子节点，即 <html> 元素，我们称之为文档元素。文档元素是文档的最外层元素，文档中的其他所有元素都包含在文档元素中。每个文
档只能有一个文档元素。在 HTML 页面中，文档元素始终都是 <html> 元素。在 XML 中，没有预定义
的元素，因此任何元素都可能成为文档元素。
每一段标记都可以通过树中的一个节点来表示：HTML 元素通过元素节点表示，特性（attribute）
通过特性节点表示，文档类型通过文档类型节点表示，而注释则通过注释节点表示。总共有 12 种节点
类型，这些类型都继承自一个基类型。
  
  ![图片]()
  
###  Node 类型

DOM1 级定义了一个 Node 接口，该接口将由 DOM 中的所有节点类型实现。这个 Node 接口在
JavaScript 中是作为 Node 类型实现的；除了 IE 之外，在其他所有浏览器中都可以访问到这个类型。
JavaScript 中的所有节点类型都继承自 Node 类型，因此所有节点类型都共享着相同的基本属性和方法。
每个节点都有一个 nodeType 属性，用于表明节点的类型。节点类型由在 Node 类型中定义的下列
12 个数值常量来表示，任何节点类型必居其一：

- Node.ELEMENT_NODE (1)；
- Node.ATTRIBUTE_NODE (2)；
- Node.TEXT_NODE (3)；
- Node.CDATA_SECTION_NODE (4)；
- Node.ENTITY_REFERENCE_NODE (5)；
- Node.ENTITY_NODE (6)；
- Node.PROCESSING_INSTRUCTION_NODE (7)；
- Node.COMMENT_NODE (8)；
- Node.DOCUMENT_NODE (9)；
- Node.DOCUMENT_TYPE_NODE (10)；
- Node.DOCUMENT_FRAGMENT_NODE (11)；
- Node.NOTATION_NODE (12)。

通过比较上面这些常量，可以很容易地确定节点的类型，例如：

```js
if (someNode.nodeType == Node.ELEMENT_NODE){ //在 IE 中无效
alert("Node is an element.");
}
```
这个例子比较了 someNode.nodeType 与 Node.ELEMENT_NODE 常量。如果二者相等，则意味着
someNode 确实是一个元素。然而，由于 IE 没有公开 Node 类型的构造函数，因此上面的代码在 IE 中
会导致错误。为了确保跨浏览器兼容，最好还是将 nodeType 属性与数字值进行比较，如下所示：

```js
if (someNode.nodeType == 1){ // 适用于所有浏览器
alert("Node is an element.");
}
```
并不是所有节点类型都受到 Web 浏览器的支持。开发人员最常用的就是元素和文本节点。本章后
面将详细讨论每个节点类型的受支持情况及使用方法。

- #### nodeName 和 nodeValue 属性

要了解节点的具体信息，可以使用 nodeName 和 nodeValue 这两个属性。这两个属性的值完全取
决于节点的类型。在使用这两个值以前，最好是像下面这样先检测一下节点的类型。

```js
if (someNode.nodeType == 1){
value = someNode.nodeName; //nodeName 的值是元素的标签名
}
```
在这个例子中，首先检查节点类型，看它是不是一个元素。如果是，则取得并保存 nodeName 的值。
对于元素节点， nodeName 中保存的始终都是元素的标签名，而 nodeValue 的值则始终为 null 。

- #### 节点关系

文档中所有的节点之间都存在这样或那样的关系。节点间的各种关系可以用传统的家族关系来描
述，相当于把文档树比喻成家谱。在 HTML 中，可以将 <body> 元素看成是 <html> 元素的子元素；相应
地，也就可以将 <html> 元素看成是 <body> 元素的父元素。而 <head> 元素，则可以看成是 <body> 元素
的同胞元素，因为它们都是同一个父元素 <html> 的直接子元素。
每个节点都有一个 childNodes 属性，其中保存着一个 NodeList 对象。 NodeList 是一种类数组
对象，用于保存一组有序的节点，可以通过位置来访问这些节点。请注意，虽然可以通过方括号语法来
访问 NodeList 的值，而且这个对象也有 length 属性，但它并不是 Array 的实例。 NodeList 对象的
独特之处在于，它实际上是基于 DOM 结构动态执行查询的结果，因此 DOM 结构的变化能够自动反映
在 NodeList 对象中。我们常说， NodeList 是有生命、有呼吸的对象，而不是在我们第一次访问它们
的某个瞬间拍摄下来的一张快照。
下面的例子展示了如何访问保存在 NodeList 中的节点——可以通过方括号，也可以使用 item()
方法。
  
```js
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
var count = someNode.childNodes.length;
```
无论使用方括号还是使用 item() 方法都没有问题，但使用方括号语法看起来与访问数组相似，因
此颇受一些开发人员的青睐。另外，要注意 length 属性表示的是访问 NodeList 的那一刻，其中包含
的节点数量。我们在本书前面介绍过，对 arguments 对象使用 Array.prototype.slice() 方法可以
将其转换为数组。而采用同样的方法，也可以将 NodeList 对象转换为数组。来看下面的例子：
//在 IE8 及之前版本中无效

```js
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
```
除 IE8 及更早版本之外，这行代码能在任何浏览器中运行。由于 IE8 及更早版本将 NodeList
实现为一个 COM 对象，而我们不能像使用 JScript 对象那样使用这种对象，因此上面的代码会导致
错误。要想在 IE 中将 NodeList 转换为数组，必须手动枚举所有成员。下列代码在所有浏览器中都
可以运行：

```js
function convertToArray(nodes){
var array = null;
try {
array = Array.prototype.slice.call(nodes, 0); //针对非 IE 浏览器
} catch (ex) {
array = new Array();
for (var i=0, len=nodes.length; i < len; i++){
array.push(nodes[i]);
}
}
return array;
}
```
这个 convertToArray() 函数首先尝试了创建数组的最简单方式。如果导致了错误（说明是在
IE8 及更早版本中），则通过 try-catch 块来捕获错误，然后手动创建数组。这是另一种检测怪癖的
形式。
每个节点都有一个 parentNode 属性，该属性指向文档树中的父节点。包含在 childNodes 列表中
的所有节点都具有相同的父节点，因此它们的 parentNode 属性都指向同一个节点。此外，包含在
childNodes 列表中的每个节点相互之间都是同胞节点。通过使用列表中每个节点的 previousSibling
和 nextSibling 属性，可以访问同一列表中的其他节点。列表中第一个节点的 previousSibling 属性
值为 null ，而列表中最后一个节点的 nextSibling 属性的值同样也为 null ，如下面的例子所示：

```js
if (someNode.nextSibling === null){
alert("Last node in the parent’s childNodes list.");
} else if (someNode.previousSibling === null){
alert("First node in the parent’s childNodes list.");
}
```
当然，如果列表中只有一个节点，那么该节点的 nextSibling 和 previousSibling 都为 null 。
父节点与其第一个和最后一个子节点之间也存在特殊关系。父节点的 firstChild 和 lastChild
属性分别指向其 childNodes 列表中的第一个和最后一个节点。其中， someNode.firstChild 的值
始 终 等 于 someNode.childNodes[0] ， 而 someNode.lastChild 的 值 始 终 等 于 someNode.
childNodes [someNode.childNodes.length-1] 。在只有一个子节点的情况下， firstChild 和
lastChild 指向同一个节点。如果没有子节点，那么 firstChild 和 lastChild 的值均为 null 。明
确这些关系能够对我们查找和访问文档结构中的节点提供极大的便利。图 10-2 形象地展示了上述关系。

![图]()

在反映这些关系的所有属性当中， childNodes 属性与其他属性相比更方便一些，因为只须使用简
单的关系指针，就可以通过它访问文档树中的任何节点。另外， hasChildNodes() 也是一个非常有用
的方法，这个方法在节点包含一或多个子节点的情况下返回 true ；应该说，这是比查询 childNodes
列表的 length 属性更简单的方法。
所有节点都有的最后一个属性是 ownerDocument ，该属性指向表示整个文档的文档节点。这种关
系表示的是任何节点都属于它所在的文档，任何节点都不能同时存在于两个或更多个文档中。通过这个
属性，我们可以不必在节点层次中通过层层回溯到达顶端，而是可以直接访问文档节点。
虽然所有节点类型都继承自 Node ，但并不是每种节点都有子节点。本章后面将
会讨论不同节点类型之间的差异。

- #### 操作节点

因为关系指针都是只读的，所以 DOM 提供了一些操作节点的方法。其中，最常用的方法是
appendChild() ，用于向 childNodes 列表的末尾添加一个节点。添加节点后， childNodes 的新增
节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新。更新完成后， appendChild()
返回新增的节点。来看下面的例子：

```js
var returnedNode = someNode.appendChild(newNode);
alert(returnedNode == newNode); //true
alert(someNode.lastChild == newNode); //true
```
如果传入到 appendChild() 中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置
转移到新位置。即使可以将 DOM 树看成是由一系列指针连接起来的，但任何 DOM 节点也不能同时出
现在文档中的多个位置上。因此，如果在调用 appendChild() 时传入了父节点的第一个子节点，那么
该节点就会成为父节点的最后一个子节点，如下面的例子所示。

```js
//someNode 有多个子节点
var returnedNode = someNode.appendChild(someNode.firstChild);
alert(returnedNode == someNode.firstChild); //false
alert(returnedNode == someNode.lastChild); //true
```
如果需要把节点放在 childNodes 列表中某个特定的位置上，而不是放在末尾，那么可以使用
insertBefore() 方法。这个方法接受两个参数：要插入的节点和作为参照的节点。插入节点后，被插
入的节点会变成参照节点的前一个同胞节点（ previousSibling ），同时被方法返回。如果参照节点是
null ，则 insertBefore() 与 appendChild() 执行相同的操作，如下面的例子所示。

```js
//插入后成为最后一个子节点
returnedNode = someNode.insertBefore(newNode, null);
alert(newNode == someNode.lastChild); //true
```

```js
//插入后成为第一个子节点
var returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnedNode == newNode); //true
alert(newNode == someNode.firstChild); //true
```

```js
//插入到最后一个子节点前面
returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(newNode == someNode.childNodes[someNode.childNodes.length-2]); //true
```

前面介绍的 appendChild() 和 insertBefore() 方法都只插入节点，不会移除节点。而下面要介
绍的 replaceChild() 方法接受的两个参数是：要插入的节点和要替换的节点。要替换的节点将由这个
方法返回并从文档树中被移除，同时由要插入的节点占据其位置。来看下面的例子。

```js
//替换第一个子节点
var returnedNode = someNode.replaceChild(newNode, someNode.firstChild);

//替换最后一个子节点
returnedNode = someNode.replaceChild(newNode, someNode.lastChild);
```
在使用 replaceChild() 插入一个节点时，该节点的所有关系指针都会从被它替换的节点复制过
来。尽管从技术上讲，被替换的节点仍然还在文档中，但它在文档中已经没有了自己的位置。
如果只想移除而非替换节点，可以使用 removeChild() 方法。这个方法接受一个参数，即要移除
的节点。被移除的节点将成为方法的返回值，如下面的例子所示。

```js
//移除第一个子节点
var formerFirstChild = someNode.removeChild(someNode.firstChild);

//移除最后一个子节点
var formerLastChild = someNode.removeChild(someNode.lastChild);
```
与使用 replaceChild() 方法一样，通过 removeChild() 移除的节点仍然为文档所有，只不过在
文档中已经没有了自己的位置。
前面介绍的四个方法操作的都是某个节点的子节点，也就是说，要使用这几个方法必须先取得父节
点（使用 parentNode 属性）。另外，并不是所有类型的节点都有子节点，如果在不支持子节点的节点
上调用了这些方法，将会导致错误发生。

- #### 其他方法

有两个方法是所有类型的节点都有的。第一个就是 cloneNode() ，用于创建调用这个方法的节点
的一个完全相同的副本。 cloneNode() 方法接受一个布尔值参数，表示是否执行深复制。在参数为 true
的情况下，执行深复制，也就是复制节点及其整个子节点树；在参数为 false 的情况下，执行浅复制，
即只复制节点本身。复制后返回的节点副本属于文档所有，但并没有为它指定父节点。因此，这个节点
副本就成为了一个“孤儿”，除非通过 appendChild() 、 insertBefore() 或 replaceChild() 将它添加到文档中。例如，假设有下面的 HTML 代码。

```html
<ul>
<li>item 1</li>
<li>item 2</li>
<li>item 3</li>
</ul>
```
如果我们已经将 <ul> 元素的引用保存在了变量 myList 中，那么通常下列代码就可以看出使用
cloneNode() 方法的两种模式。
 
```js
var deepList = myList.cloneNode(true);
alert(deepList.childNodes.length); //3（IE < 9）或 7（其他浏览器）
var shallowList = myList.cloneNode(false);
alert(shallowList.childNodes.length); //0
```
在这个例子中， deepList 中保存着一个对 myList 执行深复制得到的副本。因此， deepList 中
包含 3 个列表项，每个列表项中都包含文本。而变量 shallowList 中保存着对 myList 执行浅复制得
到的副本，因此它不包含子节点。 deepList.childNodes.length 中的差异主要是因为 IE8 及更早版
本与其他浏览器处理空白字符的方式不一样。IE9 之前的版本不会为空白符创建节点。
cloneNode() 方法不会复制添加到 DOM 节点中的 JavaScript 属性，例如事件处
理程序等。这个方法只复制特性、（在明确指定的情况下也复制）子节点，其他一切
都不会复制。IE 在此存在一个 bug，即它会复制事件处理程序，所以我们建议在复制
之前最好先移除事件处理程序。
我们要介绍的最后一个方法是 normalize() ，这个方法唯一的作用就是处理文档树中的文本节点。
由于解析器的实现或 DOM 操作等原因，可能会出现文本节点不包含文本，或者接连出现两个文本节点
的情况。当在某个节点上调用这个方法时，就会在该节点的后代节点中查找上述两种情况。如果找到了
空文本节点，则删除它；如果找到相邻的文本节点，则将它们合并为一个文本节点。本章后面还将进一
步讨论这个方法。

###  Document 类型
JavaScript 通过 Document 类型表示文档。在浏览器中， document 对象是 HTMLDocument （继承
自 Document 类型）的一个实例，表示整个 HTML 页面。而且， document 对象是 window 对象的一个
属性，因此可以将其作为全局对象来访问。 Document 节点具有下列特征：

- nodeType 的值为 9；
- nodeName 的值为 "#document" ；
- nodeValue 的值为 null ；
- parentNode 的值为 null ；
- ownerDocument 的值为  null ；
-  其子节点可能是一个 DocumentType （最多一个）、 Element （最多一个）、 ProcessingInstruction或 Comment 。

ocument 类型可以表示 HTML 页面或者其他基于 XML 的文档。不过，最常见的应用还是作为
HTMLDocument 实例的 document 对象。通过这个文档对象，不仅可以取得与页面有关的信息，而且还
能操作页面的外观及其底层结构。
> 在 Firefox、Safari、Chrome 和 Opera 中，可以通过脚本访问 Document 类型的构
造函数和原型。但在所有浏览器中都可以访问 HTMLDocument 类型的构造函数和原型，
包括 IE8及后续版本。

- #### 文档的子节点

虽然 DOM 标准规定 Document 节点的子节点可以是 DocumentType 、 Element 、 ProcessingIn-
struction 或 Comment ，但还有两个内置的访问其子节点的快捷方式。第一个就是 documentElement
属性，该属性始终指向 HTML 页面中的 <html> 元素。另一个就是通过 childNodes 列表访问文档元素，
但通过 documentElement 属性则能更快捷、更直接地访问该元素。以下面这个简单的页面为例。
  
```html
<html>
<body>
</body>
</html>
```
这个页面在经过浏览器解析后，其文档中只包含一个子节点，即 <html> 元素。可以通过
documentElement 或 childNodes 列表来访问这个元素，如下所示。
  
```js
var html = document.documentElement; //取得对<html>的引用
alert(html === document.childNodes[0]); //true
alert(html === document.firstChild); //true
```
这个例子说明， documentElement 、 firstChild 和 childNodes[0] 的值相同，都指向 <html>
元素。
作为 HTMLDocument 的实例， document 对象还有一个 body 属性，直接指向 <body> 元素。因为开
发人员经常要使用这个元素，所以 document.body 在 JavaScript代码中出现的频率非常高，其用法如下。
var body = document.body; //取得对<body>的引用
所有浏览器都支持 document.documentElement 和 document.body 属性。
Document 另一个可能的子节点是 DocumentType 。通常将 <!DOCTYPE> 标签看成一个与文档其他
部分不同的实体，可以通过 doctype 属性（在浏览器中是 document.doctype ）来访问它的信息。
var doctype = document.doctype; //取得对<!DOCTYPE>的引用
浏览器对 document.doctype 的支持差别很大，可以给出如下总结。
  
-  IE8 及之前版本：如果存在文档类型声明，会将其错误地解释为一个注释并把它当作 Comment
节点；而 document.doctype 的值始终为 null 。
-  IE9+及 Firefox：如果存在文档类型声明，则将其作为文档的第一个子节点； document.doctype
是一个 DocumentType 节点，也可以通过 document.firstChild 或 document.childNodes[0]
访问同一个节点。
-  Safari、Chrome和 Opera：如果存在文档类型声明，则将其解析，但不作为文档的子节点。 docu-
ment.doctype 是一个 DocumentType 节点，但该节点不会出现在 document.childNodes 中。

由于浏览器对 document.doctype 的支持不一致，因此这个属性的用处很有限。
从技术上说，出现在 <html> 元素外部的注释应该算是文档的子节点。然而，不同的浏览器在是否
解析这些注释以及能否正确处理它们等方面，也存在很大差异。以下面简单的 HTML 页面为例。
  
```html
<!--第一条注释 -->
<html>
<body>
</body>
</html>
<!--第二条注释 -->
```
看起来这个页面应该有 3 个子节点：注释、 <html> 元素、注释。从逻辑上讲，我们会认为
document.childNodes 中应该包含与这 3 个节点对应的 3 项。但是，现实中的浏览器在处理位于
<html> 外部的注释方面存在如下差异。
  
-  IE8 及之前版本、Safari 3.1 及更高版本、Opera 和 Chrome 只为第一条注释创建节点，不为第二
条注释创建节点。结果，第一条注释就会成为 document.childNodes 中的第一个子节点。
-  IE9 及更高版本会将第一条注释创建为 document.childNodes 中的一个注释节点，也会将第
二条注释创建为 document.childNodes 中的注释子节点。
-  Firefox 以及 Safari 3.1 之前的版本会完全忽略这两条注释。
同样，浏览器间的这种不一致性也导致了位于 <html> 元素外部的注释没有什么用处。
多数情况下，我们都用不着在 document 对象上调用 appendChild() 、 removeChild() 和
replaceChild() 方法，因为文档类型（如果存在的话）是只读的，而且它只能有一个元素子节点（该
节点通常早就已经存在了）。
  
- #### 文档信息
作为 HTMLDocument 的一个实例， document 对象还有一些标准的 Document 对象所没有的属性。
这些属性提供了 document 对象所表现的网页的一些信息。其中第一个属性就是 title ，包含着
<title> 元素中的文本——显示在浏览器窗口的标题栏或标签页上。通过这个属性可以取得当前页面的
标题，也可以修改当前页面的标题并反映在浏览器的标题栏中。修改 title 属性的值不会改变 <title>
元素。来看下面的例子。
//取得文档标题
  
```js
var originalTitle = document.title;
//设置文档标题
document.title = "New page title";
```
接下来要介绍的 3 个属性都与对网页的请求有关，它们是 URL 、 domain 和 referrer 。 URL 属性
中包含页面完整的 URL（即地址栏中显示的 URL）， domain 属性中只包含页面的域名，而 referrer
属性中则保存着链接到当前页面的那个页面的 URL。在没有来源页面的情况下， referrer 属性中可能
会包含空字符串。所有这些信息都存在于请求的 HTTP 头部，只不过是通过这些属性让我们能够在
JavaScrip 中访问它们而已，如下面的例子所示。

```js
//取得完整的 URL
var url = document.URL;
//取得域名