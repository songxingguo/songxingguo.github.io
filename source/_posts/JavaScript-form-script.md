title: 《JavaScript 高级程序设计》学习笔记-表单脚本
author: songxingguo
tags:
  - JavaScript
categories:
  - 读书笔记
date: 2018-08-27 15:49:00
---
# 表单脚本

JavaScript 最初的一个应用，就是分担服务器处理表单的责任，打破处处依赖服务器的局面。尽管目前的 Web 和 JavaScript 已经有了长足的发展，但 Web 表单的变化并不明显。由于 Web 表单没有为许多常见任务提供现成的解决手段，很多开发人员不仅会在验证表单时使用 JavaScirpt，而且还增强了一些标准表单控件的默认行为。

<!-- more -->

## 表单的基础知识

**在 HTML 中** ，**表单是由 `<form>` 元素来表示的** ，而 **在 JavaScript 中** ，**表单对应的则是 HTMLFormElement 类型** 。 **HTMLFormElement 继承了HTMLElement** ，**因而与其他 HTML 元素具有相同的默认属性** 。不过， HTMLFormElement 也有它自己下列独有的属性和方法。

- acceptCharset ：服务器能够处理的字符集；等价于 HTML 中的 accept-charset 特性。
- action ：接受请求的 URL；等价于 HTML 中的 action 特性。
- elements ：表单中所有控件的集合（ HTMLCollection ）。
- enctype ：请求的编码类型；等价于 HTML 中的 enctype 特性。
- length ：表单中控件的数量。
- method ：要发送的 HTTP 请求类型，通常是 "get" 或 "post" ；等价于 HTML 的 method 特性。
- name ：表单的名称；等价于 HTML 的 name 特性。
- reset() ：将所有表单域重置为默认值。
- submit() ：提交表单。
- target ：用于发送请求和接收响应的窗口名称；等价于 HTML 的 target 特性。

**取得 `<form>` 元素引用** 的方式有好几种。其中 **最常见的方式** 就是 **将它看成与其他元素一样** ，**并为其添加 id 特性** ，**然后再像下面这样使用 getElementById() 方法找到它** 。

```js
var form = document.getElementById("form1");
```
其次，**通过 document.forms 可以取得页面中所有的表单** 。在这个集合中，**可以通过数值索引或 name 值来取得特定的表单** ，如下面的例子所示。
  
```js
var firstForm = document.forms[0]; //取得页面中的第一个表单
var myForm = document.forms["form2"]; //取得页面中名称为"form2"的表单
```
另外，在较早的浏览器或者那些支持向后兼容的浏览器中，也会把每个设置了 name 特性的表单作为属性保存在 document 对象中。例如，**通过 document.form2 可以访问到名为 "form2" 的表单** 。不过，我们 **不推荐使用这种方式** ：一是 **容易出错** ，二是 **将来的浏览器可能会不支持** 。

注意，**可以同时为表单指定 id 和 name 属性** ，但 **它们的值不一定相同** 。

### 提交表单

**用户单击提交按钮或图像按钮时，就会提交表单** 。**使用 `<input>` 或 `<button>` 都可以定义提交按钮** ，**只要将其 type 特性的值设置为 "submit" 即可** ，而 **图像按钮则是通过将 `<input>` 的 type 特性值设置为 "image" 来定义的** 。因此，只要我们单击以下代码生成的按钮，就可以提交表单。
  
```js
<!-- 通用提交按钮 -->
<input type="submit" value="Submit Form">

<!-- 自定义提交按钮 -->
<button type="submit">Submit Form</button>

<!-- 图像按钮 -->
<input type="image" src="graphic.gif">
```
只要表单中存在上面列出的任何一种按钮，那么在相应表单控件拥有焦点的情况下，按回车键就可以提交该表单。（ textarea 是一个例外，在文本区中回车会换行。）**如果表单里没有提交按钮，按回车键不会提交表单** 。

**以这种方式提交表单时，浏览器会在将请求发送给服务器之前触发 submit 事件** 。这样，**我们就有机会验证表单数据，并据以决定是否允许表单提交** 。**阻止这个事件的默认行为就可以取消表单提交** 。例如，下列代码会阻止表单提交。

```js
var form = document.getElementById("myForm");
EventUtil.addHandler(form, "submit", function(event){

  //取得事件对象
  event = EventUtil.getEvent(event);
  
  //阻止默认事件
  EventUtil.preventDefault(event);
});
```
这里使用了第 13 章定义的 EventUtil 对象，以便跨浏览器处理事件。**调用 prevetnDefault() 方法阻止了表单提交** 。一般来说，**在表单数据无效而不能发送给服务器时，可以使用这一技术** 。

**在 JavaScript 中，以编程方式调用 submit() 方法也可以提交表单** 。而且，**这种方式无需表单包含提交按钮** ，**任何时候都可以正常提交表单** 。来看一个例子。

```js
var form = document.getElementById("myForm");

// 提交表单
form.submit();
```
**在以调用 submit() 方法的形式提交表单时** ，**不会触发 submit 事件** ，因此**要记得在调用此方法之前先验证表单数据** 。

提交表单时可能出现的最大问题，就是 **重复提交表单** 。在第一次提交表单后，如果长时间没有反应，用户可能会变得不耐烦。这时候，他们也许会反复单击提交按钮。结果往往很麻烦（因为服务器要处理重复的请求），或者会造成错误（如果用户是下订单，那么可能会多订好几份）。解决这一问题的办法有两个：**在第一次提交表单后就禁用提交按钮** ，或者 **利用 onsubmit 事件处理程序取消后续的表单提交操作** 。

### 重置表单

**在用户单击重置按钮时，表单会被重置** 。**使用 type 特性值为 "reset" 的 `<input>` 或 `<button>` 都可以创建重置按钮** ，如下面的例子所示。
  
```js
<!-- 通用重置按钮 -->
<input type="reset" value="Reset Form">

<!-- 自定义重置按钮 -->
<button type="reset">Reset Form</button>
```
这两个按钮都可以用来重置表单。在重置表单时，**所有表单字段都会恢复到页面刚加载完毕时的初始值** 。如果某个字段的初始值为空，就会恢复为空；而带有默认值的字段，也会恢复为默认值。

**用户单击重置按钮重置表单时，会触发 reset 事件** 。**利用这个机会，我们可以在必要时取消重置操作** 。例如，下面展示了阻止重置表单的代码。

```js
var form = document.getElementById("myForm");
EventUtil.addHandler(form, "reset", function(event){

  //取得事件对象
  event = EventUtil.getEvent(event);

  //阻止表单重置
  EventUtil.preventDefault(event);
});
```
与提交表单一样，也 **可以通过 JavaScript 来重置表单** ，如下面的例子所示。

```js
var form = document.getElementById("myForm");

// 重置表单
form.reset();
```
与调用 submit() 方法不同，**调用 reset() 方法会像单击重置按钮一样触发 reset 事件** 。

> 在 Web 表单设计中，重置表单通常意味着对已经填写的数据不满意。重置表单经常会导致用户摸不着头脑，如果意外地触发了表单重置事件，那么用户甚至会很恼火。事实上，重置表单的需求是很少见的。**更常见的做法是提供一个取消按钮** ，**让用户能够回到前一个页面** ，**而不是不分青红皂白地重置表单中的所有值** 。

### 表单字段

**可以像访问页面中的其他元素一样，使用原生 DOM 方法访问表单元素** 。此外，**每个表单都有elements 属性** ，该属性是 **表单中所有表单元素（字段）的集合** 。这个**elements 集合是一个有序列表** ，其中 **包含着表单中的所有字段** ，例如 `<input>` 、 `<textarea>` 、 `<button>` 和 `<fieldset>` 。**每个表单字段在 elements 集合中的顺序** ，**与它们出现在标记中的顺序相同** ，**可以按照位置和 name 特性来访问它们** 。下面来看一个例子。
  
```js
var form = document.getElementById("form1");

//取得表单中的第一个字段
var field1 = form.elements[0];

//取得名为"textbox1"的字段
var field2 = form.elements["textbox1"];

//取得表单中包含的字段的数量
var fieldCount = form.elements.length;
```
如果有多个表单控件都在使用一个 name （如单选按钮），那么就会返回以该 name 命名的一个 NodeList 。例如，以下面的 HTML 代码片段为例。

```html
<form method="post" id="myForm">
<ul>
<li><input type="radio" name="color" value="red">Red</li>
<li><input type="radio" name="color" value="green">Green</li>
<li><input type="radio" name="color" value="blue">Blue</li>
</ul>
</form>
```
在这个 HTML 表单中，有 3 个单选按钮，它们的 name 都是 "color" ，意味着这 3 个字段是一起的。在访问 elements["color"] 时，就会返回一个 **NodeList** ，其中 **包含这 3 个元素** ；不过，如果访问 elements[0] ，则只会返回第一个元素。来看下面的例子。

```js
var form = document.getElementById("myForm");

var colorFields = form.elements["color"];
alert(colorFields.length); //3

var firstColorField = colorFields[0];
var firstFormField = form.elements[0];
alert(firstColorField === firstFormField); //true
```
以上代码显示，通过 form.elements[0] 访问到的第一个表单字段，与包含在 form.elements["color"] 中的第一个元素相同。

> 也可以通过访问表单的属性来访问元素，例如 form[0] 可以取得第一个表单字段，而 form["color"] 则可以取得第一个命名字段。这些属性与通过 elements 集合访问到的元素是相同的。但是，我们应该尽可能使用 elements ，通过表单属性访问元素只是为了与旧浏览器向后兼容而保留的一种过渡方式。

- #### 共有的表单字段属性

  **除了 `<fieldset>` 元素之外，所有表单字段都拥有相同的一组属性** 。由于 **`<input>` 类型可以表示多种表单字段** ，因此 **有些属性只适用于某些字段** ，但 **还有一些属性是所有字段所共有的** 。表单字段共有的属性如下。

  - disabled ：布尔值，表示当前字段是否被禁用。
  - form ：指向当前字段所属表单的指针；只读。
  - name ：当前字段的名称。
  - readOnly ：布尔值，表示当前字段是否只读。
  - tabIndex ：表示当前字段的切换（tab）序号。
  - type ：当前字段的类型，如 "checkbox" 、 "radio" ，等等。
  - value ：当前字段将被提交给服务器的值。对文件字段来说，这个属性是只读的，包含着文件在计算机中的路径。

  除了 form 属性之外，**可以通过 JavaScript 动态修改其他任何属性** 。来看下面的例子：

  ```js
  var form = document.getElementById("myForm");
  var field = form.elements[0];

  //修改 value 属性
  field.value = "Another value";

  //检查 form 属性的值
  alert(field.form === form); //true

  //把焦点设置到当前字段
  field.focus();

  //禁用当前字段
  field.disabled = true;

  //修改 type 属性（不推荐，但对<input>来说是可行的）
  field.type = "checkbox";
  ```
  **能够动态修改表单字段属性** ，意味着 **我们可以在任何时候** ，**以任何方式来动态操作表单** 。例如，很多用户可能会重复单击表单的提交按钮。在涉及信用卡消费时，这就是个问题：因为会导致费用翻番。为此，最常见的解决方案，就是在第一次单击后就禁用提交按钮。只要侦听 submit 事件，并在该事件发生时禁用提交按钮即可。以下就是这样一个例子。

  ```js
  //避免多次提交表单
  EventUtil.addHandler(form, "submit", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);

    //取得提交按钮
    var btn = target.elements["submit-btn"];

    //禁用它
    btn.disabled = true;
  });
  ```
  **以上代码为表单的 submit 事件添加了一个事件处理程序** 。事件触发后，代码取得了提交按钮并将其 disabled 属性设置为 true 。注意，不能通过 onclick 事件处理程序来实现这个功能，原因是不同浏览器之间存在“时差”：有的浏览器会在触发表单的 submit 事件之前触发 click 事件，而有的浏览器则相反。对于先触发 click 事件的浏览器，意味着会在提交发生之前禁用按钮，结果永远都不会提交表单。因此，最好是通过 submit 事件来禁用提交按钮。不过，这种方式不适合表单中不包含提交按钮的情况；如前所述，只有在包含提交按钮的情况下，才有可能触发表单的 submit 事件。

  **除了 `<fieldset>` 之外** ，**所有表单字段都有 type 属性** 。**对于 `<input>` 元素** ，**这个值等于 HTML 特性 type 的值** 。对于其他元素，这个 type 属性的值如下表所列。

    ![其他元素的type属性](http://p9myzkds7.bkt.clouddn.com/JavaScript-form-script/%E5%85%B6%E4%BB%96%E5%85%83%E7%B4%A0%E7%9A%84type%E5%B1%9E%E6%80%A7.png)

  此外， **`<input>` 和 `<button>` 元素的 type 属性是可以动态修改的** ，而 **`<select>` 元素的 type 属性则是只读的** 。

- #### 共有的表单字段方法

  **每个表单字段都有两个方法： focus() 和 blur() **。其中， **focus() 方法** 用于 **将浏览器的焦点设置到表单字段** ，即 **激活表单字段** ，**使其可以响应键盘事件** 。例如，接收到焦点的文本框会显示插入符号，随时可以接收输入。**使用 focus() 方法** ，**可以将用户的注意力吸引到页面中的某个部位** 。例如，在页面加载完毕后，将焦点转移到表单中的第一个字段。为此，**可以侦听页面的 load 事件** ，**并在该事件发生时在表单的第一个字段上调用 focus() 方法** ，如下面的例子所示。

  ```js
  EventUtil.addHandler(window, "load", function(event){
    document.forms[0].elements[0].focus();
  });
  ```
  要注意的是，如果 **第一个表单字段是一个 `<input>` 元素** ，**且其 type 特性的值为 "hidden"** ，那么 **以上代码会导致错误** 。另外，**如果使用 CSS 的 display 和 visibility 属性隐藏了该字段** ，**同样也会导致错误** 。

  **HTML5 为表单字段** 新增了一个 **autofocus 属性** 。在支持这个属性的浏览器中，**只要设置这个属性** ，**不用 JavaScript 就能自动把焦点移动到相应字段** 。例如：

  ```html
  <input type="text" autofocus>
  ```
  **为了保证前面的代码在设置 autofocus 的浏览器中正常运行** ，必须 **先检测是否设置了该属性** ，如果 **设置了** ，就 **不用再调用 focus()了** 。

  ```js
  EventUtil.addHandler(window, "load", function(event){
    var element = document.forms[0].elements[0];
    if (element.autofocus !== true){
      element.focus(); console.log("JS focus");
    }
  });
  ```
  因为 **autofocus 是一个布尔值属性** ，所以 **在支持的浏览器中它的值应该是 true** 。（在不支持的浏览器中，它的值将是空字符串。）为此，**上面的代码只有在 autofocus 不等于 true 的情况下才会调用 focus()**  ，**从而保证向前兼容** 。支持 autofocus 属性的浏览器有 Firefox 4+、Safari 5+、Chrome 和 Opera 9.6。

  > **在默认情况下，只有表单字段可以获得焦点** 。对于其他元素而言，**如果先将其 tabIndex 属性设置为 -1** ，然后 **再调用 focus() 方法** ，**也可以让这些元素获得焦点** 。只有 Opera 不支持这种技术。

  与 focus() 方法相对的是 **blur() 方法** ，它的作用是 **从元素中移走焦点** 。**在调用 blur() 方法时，并不会把焦点转移到某个特定的元素上** ；**仅仅是将焦点从调用这个方法的元素上面移走而已** 。在早期  Web 开发中，那时候的表单字段还没有 readonly 特性，因此就可以使用 blur() 方法来创建只读字段。

  现在，虽然需要使用 blur() 的场合不多了，但必要时还可以使用的。用法如下：

  ```js
  document.forms[0].elements[0].blur();
  ```
- #### 共有的表单字段事件

**除了支持鼠标、键盘、更改和 HTML 事件之外**，**所有表单字段都支持下列 3 个事件** 。

- blur ：当前字段失去焦点时触发。
- change ：对于 `<input>` 和 `<textarea>` 元素，在它们失去焦点且 value 值改变时触发；对于 `<select>` 元素，在其选项改变时触发。
- focus ：当前字段获得焦点时触发。
  
**当用户改变了当前字段的焦点** ，或者 **我们调用了 blur() 或 focus() 方法** 时，**都可以触发 blur 和 focus 事件** 。**这两个事件在所有表单字段中都是相同的** 。但是，**change 事件在不同表单控件中触发的次数会有所不同** 。对于 **`<input>` 和 `<textarea>` 元素** ，**当它们从获得焦点到失去焦点且 value 值改变时** ，才会 **触发 change 事件** 。对于 **`<select>` 元素** ，只要 **用户选择了不同的选项** ，**就会触发 change 事件** ；换句话说，**不失去焦点也会触发 change 事件** 。

通常，**可以使用 focus 和 blur 事件来以某种方式改变用户界面** ，要么是 **向用户给出视觉提示** ，要么是 **向界面中添加额外的功能** （例如，为文本框显示一个下拉选项菜单）。而 **change 事件则经常用于验证用户在字段中输入的数据** 。例如，假设有一个文本框，我们只允许用户输入数值。此时，**可以利用 focus 事件修改文本框的背景颜色** ，**以便更清楚地表明这个字段获得了焦点** 。**可以利用 blur 事件恢复文本框的背景颜色** ，**利用 change 事件在用户输入了非数值字符时再次修改背景颜色** 。下面就给出了实现上述功能的代码。

```js
var textbox = document.forms[0].elements[0];
EventUtil.addHandler(textbox, "focus", function(event){
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);
  if (target.style.backgroundColor != "red"){
    target.style.backgroundColor = "yellow";
  }
});

EventUtil.addHandler(textbox, "blur", function(event){
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);
  if (/[^\d]/.test(target.value)){
    target.style.backgroundColor = "red";
  } else {
    target.style.backgroundColor = "";
  }
});

EventUtil.addHandler(textbox, "change", function(event){
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);
  if (/[^\d]/.test(target.value)){
    target.style.backgroundColor = "red";
  } else {
    target.style.backgroundColor = "";
  }
});
```
在此， **onfocus 事件处理程序将文本框的背景颜色修改为黄色**，**以清楚地表明当前字段已经激活** 。随后，**onblur 和 onchange 事件处理程序则会在发现非数值字符时，将文本框背景颜色修改为红色** 。为了测试用户输入的是不是非数值，这里针对文本框的 value 属性使用了简单的正则表达式。而且，为确保无论文本框的值如何变化，验证规则始终如一， onblur 和 onchange 事件处理程序中使用了相同的正则表达式。

>关于 blur 和 change 事件的关系，并没有严格的规定。在某些浏览器中， blur 事件会先于 change 事件发生；而在其他浏览器中，则恰好相反。为此，**不能假定这两个事件总会以某种顺序依次触发** ，这一点要特别注意。

## 文本框脚本

在 HTML 中，有两种方式来表现文本框：一种是 **使用 `<input>` 元素的单行文本框** ，另一种是 **使用 `<textarea>` 的多行文本框** 。这两个控件非常相似，而且多数时候的行为也差不多。不过，它们之间仍然存在一些重要的区别。

**要表现文本框，必须将 `<input>` 元素的 type 特性设置为 "text"** 。而通过 **设置 size 特性** ，可以 **指定文本框中能够显示的字符数** 。通过 **value 特性** ，可以 **设置文本框的初始值** ，而 **maxlength 特性** 则用于 **指定文本框可以接受的最大字符数** 。如果要创建一个文本框，让它能够显示 25 个字符，但输入不能超过 50 个字符，可以使用以下代码：

```html
<input type="text" size="25" maxlength="50" value="initial value">
```
相对而言，**`<textarea>` 元素则始终会呈现为一个多行文本框** 。**要指定文本框的大小，可以使用 rows 和 cols 特性** 。其中， **rows 特性** 指定的是 **文本框的字符行数** ，而 **cols 特性** 指定的是 **文本框的字符列数**（类似于 `<input>` 元素的 size 特性）。与 `<input>` 元素不同， **`<textarea>` 的初始值必须要放在 `<textarea>` 和 `</textarea>` 之间** ，如下面的例子所示。

```html
<textarea rows="25" cols="5">initial value</textarea>
```
另一个与 `<input>` 的区别在于，**不能在 HTML 中给 `<textarea>` 指定最大字符数** 。

无论这两种文本框在标记中有什么区别，但 **它们都会将用户输入的内容保存在 value 属性中** 。**可以通过这个属性读取和设置文本框的值** ，如下面的例子所示：
  
```js
var textbox = document.forms[0].elements["textbox1"];
alert(textbox.value);

textbox.value = "Some new value";
```
我们建议读者 **像上面这样使用 value 属性读取或设置文本框的值** ，**不建议使用标准的 DOM 方法** 。换句话说，**不要使用 setAttribute() 设置 `<input>` 元素的 value 特性** ，也 **不要去修改 `<textarea>` 元素的第一个子节点** 。原因很简单：**对 value 属性所作的修改** ，**不一定会反映在 DOM中** 。因此，在处理文本框的值时，最好不要使用 DOM 方法。

### 选择文本

上述两种文本框都支持 **select() 方法** ，这个方法用于 **选择文本框中的所有文本** 。在调用 select() 方法时，大多数浏览器（Opera 除外）都会将焦点设置到文本框中。这个方法不接受参数，可以在任何时候被调用。下面来看一个例子。

```js
var textbox = document.forms[0].elements["textbox1"];
textbox.select();
```
**在文本框获得焦点时选择其所有文本** ，这是一种非常常见的做法，特别是在文本框包含默认值的时候。因为这样做可以让用户不必一个一个地删除文本。下面展示了实现这一操作的代码。

```js
EventUtil.addHandler(textbox, "focus", function(event){
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);
  
  target.select();
});
```
将上面的代码应用到文本框之后，**只要文本框获得焦点** ，**就会选择其中所有的文本** 。这种技术能够较大幅度地提升表单的易用性。

- #### 选择（ select ）事件

  与 select() 方法对应的，是一个 **select 事件** 。**在选择了文本框中的文本时，就会触发 select 事件** 。不过，到底什么时候触发 select 事件，还会因浏览器而异。在 IE9+、Opera、Firefox、Chrome 和 Safari 中，只有用户选择了文本（而且要释放鼠标），才会触发 select 事件。而在 IE8 及更早版本中，只要用户选择了一个字母（不必释放鼠标），就会触发 select 事件。另外，**在调用 select() 方法时也会触发 select 事件** 。下面是一个简单的例子。

  ```js
  var textbox = document.forms[0].elements["textbox1"];
  EventUtil.addHandler(textbox, "select", function(event){
    var alert("Text selected" + textbox.value);
  });
  ```
- #### 取得选择的文本

  虽然通过 select 事件我们可以知道用户什么时候选择了文本，但仍然不知道用户选择了什么文本。HTML5 通过一些扩展方案解决了这个问题，以便更顺利地取得选择的文本。该规范采取的办法是添加两个属性： **selectionStart** 和 **selectionEnd** 。**这两个属性中保存的是基于 0 的数值** ，**表示所选择文本的范围** （即文本选区开头和结尾的偏移量）。因此，要 **取得用户在文本框中选择的文本** ，可以使用如下代码。

  ```js
  function getSelectedText(textbox){
     return textbox.value.substring(textbox.selectionStart, textbox.selectionEnd);
  }
  ```
  因为 **substring() 方法基于字符串的偏移量执行操作** ，所以 **将 selectionStart 和 selectionEnd 直接传给它就可以取得选中的文本** 。

  IE9+、Firefox、Safari、Chrome 和 Opera 都支持这两个属性。IE8 及之前版本不支持这两个属性，而是提供了另一种方案。

  IE8 及更早的版本中有一个 **document.selection 对象** ，其中 **保存着用户在整个文档范围内选择的文本信息** ；也就是说，无法确定用户选择的是页面中哪个部位的文本。不过，在与 select 事件一起使用的时候，可以假定是用户选择了文本框中的文本，因而触发了该事件。**要取得选择的文本** ，首先必须 **创建一个范围** （第 12 章讨论过），然后再 **将文本从其中提取出来** ，如下面的例子所示。

  ```js
  function getSelectedText(textbox){
    if (typeof textbox.selectionStart == "number"){
      return textbox.value.substring(textbox.selectionStart, textbox.selectionEnd);
    } else if (document.selection){
      return document.selection.createRange().text;
    }
  }
  ```
  这里修改了前面的函数，包括了在 IE 中取得选择文本的代码。注意，调用 document.selection 时，不需要考虑 textbox 参数。

- #### 选择部分文本

  HTML5 也为选择文本框中的部分文本提供了解决方案 ， 即最早由 Firefox 引入的**setSelectionRange() 方法** 。现在除 select() 方法之外，所有文本框都有一个 setSelectionRange() 方法。这个方法接收两个参数：**要选择的第一个字符的索引** 和 **要选择的最后一个字符之后的字符的索引** （类似于 substring() 方法的两个参数）。来看一个例子。

  ```js
  textbox.value = "Hello world!"

  //选择所有文本
  textbox.setSelectionRange(0, textbox.value.length); //"Hello world!"

  //选择前 3 个字符
  textbox.setSelectionRange(0, 3); //"Hel"

  //选择第 4 到第 6 个字符
  textbox.setSelectionRange(4, 7); //"o w"
  ```
  要看到选择的文本，必须在调用 setSelectionRange() 之前或之后立即将焦点设置到文本框。IE9、Firefox、Safari、Chrome和 Opera 支持这种方案。

  IE8 及更早版本支持使用范围（第 12 章讨论过）选择部分文本。要选择文本框中的部分文本，必须首先使用 IE 在所有文本框上提供的 **createTextRange() 方法创建一个范围** ，并 **将其放在恰当的位置上** 。然后，再 **使用 moveStart() 和 moveEnd() 这两个范围方法将范围移动到位** 。不过，在调用这两个方法以前，还 **必须使用 collapse() 将范围折叠到文本框的开始位置** 。此时， **moveStart() 将范围的起点和终点移动到了相同的位置** ，**只要再给 moveEnd() 传入要选择的字符总数即可** 。最后一步，就是 **使用范围的 select() 方法选择文本** ，如下面的例子所示。

  ```js
  textbox.value = "Hello world!";

  var range = textbox.createTextRange();

  //选择所有文本
  range.collapse(true);
  range.moveStart("character", 0);
  range.moveEnd("character", textbox.value.length); //"Hello world!"
  range.select();

  //选择前 3 个字符
  range.collapse(true);
  range.moveStart("character", 0);
  range.moveEnd("character", 3);
  range.select(); //"Hel"

  //选择第 4 到第 6 个字符
  range.collapse(true);
  range.moveStart("character", 4);
  range.moveEnd("character", 3);
  range.select(); //"o w"
  ```
  与在其他浏览器中一样，**要想在文本框中看到文本被选择的效果，必须让文本框获得焦点** 。

  为了 **实现跨浏览器编程** ，可以将上述两种方案组合起来，如下面的例子所示。

  ```js
  function selectText(textbox, startIndex, stopIndex){
    if (textbox.setSelectionRange){
      textbox.setSelectionRange(startIndex, stopIndex);
    } else if (textbox.createTextRange){
      var range = textbox.createTextRange();
      range.collapse(true);
      range.moveStart("character", startIndex);
      range.moveEnd("character", stopIndex - startIndex);
      range.select();
    }
    textbox.focus();
  }
  ```
  这个 **selectText() 函数** 接收三个参数：**要操作的文本框** 、**要选择文本中第一个字符的索引** 和 **要选择文本中最后一个字符之后的索引** 。首先，函数测试了文本框是否包含 setSelectionRange() 方法。

  如果有，则使用该方法。否则，检测文本框是否支持 createTextRange() 方法。如果支持，则通过创建范围来实现选择。最后一步，就是为文本框设置焦点，以便用户看到文本框中选择的文本。可以像下面这样使用 selectText() 方法。

  ```js
  textbox.value = "Hello world!"

  //选择所有文本
  selectText(textbox, 0, textbox.value.length); //"Hello world!"

  //选择前 3 个字符
  selectText(textbox, 0, 3); //"Hel"

  //选择第 4 到第 6 个字符
  selectText(textbox, 4, 7); //"o w"
  ```
  **选择部分文本的技术在实现高级文本输入框时很有用** ，例如提供自动完成建议的文本框就可以使用这种技术。

### 过滤输入

**我们经常会要求用户在文本框中输入特定的数据** ，**或者输入特定格式的数据** 。例如，必须包含某些字符，或者必须匹配某种模式。**由于文本框在默认情况下没有提供多少验证数据的手段** ，**因此必须使用 JavaScript 来完成此类过滤输入的操作** 。而综合运用事件和 DOM 手段，就可以 **将普通的文本框转换成能够理解用户输入数据的功能型控件** 。

- #### 屏蔽字符

  有时候，我们需要用户 **输入的文本中包含或不包含某些字符** 。例如，电话号码中不能包含非数值字符。如前所述，**响应向文本框中插入字符操作的是 keypress 事件** 。因此，可以通过阻止这个事件的默认行为来屏蔽此类字符。在极端的情况下，可以通过下列代码 **屏蔽所有按键操作** 。

  ```js
  EventUtil.addHandler(textbox, "keypress", function(event){
    event = EventUtil.getEvent(event);
    EventUtil.preventDefault(event);
  });
  ```
  运行以上代码后，由于所有按键操作都将被屏蔽，结果会导致文本框变成只读的。如果 **只想屏蔽特定的字符** ，则 **需要检测 keypress 事件对应的字符编码** ，然后 **再决定如何响应** 。例如，下列代码只允许用户输入数值。

  ```js
  EventUtil.addHandler(textbox, "keypress", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    var charCode = EventUtil.getCharCode(event);

    if (!/\d/.test(String.fromCharCode(charCode))){
      EventUtil.preventDefault(event);
    }
  });
  ```
  在这个例子中，我们 **使用 EventUtil.getCharCode() 实现了跨浏览器取得字符编码** 。然后，使用 **String.fromCharCode() 将字符编码转换成字符串** ，再 **使用正则表达式  /\d/ 来测试该字符串** ，从而 **确定用户输入的是不是数值** 。如果测试失败，那么就使用 **EventUtil.preventDefault() 屏蔽按键事件** 。结果，文本框就会忽略所有输入的非数值。

  虽然理论上只应该在用户按下字符键时才触发 keypress 事件，但有些浏览器也会对其他键触发此事件。Firefox 和 Safari（3.1 版本以前）会对向上键、向下键、退格键和删除键触发 keypress 事件；Safari 3.1 及更新版本则不会对这些键触发 keypress 事件。这意味着，**仅考虑到屏蔽不是数值的字符还不够** ，**还要避免屏蔽这些极为常用和必要的键** 。所幸的是，要检测这些键并不困难。在 Firefox 中，所有由非字符键触发的 keypress 事件对应的字符编码为 0，而在 Safari 3 以前的版本中，对应的字符编码全部为 8。为了让代码更通用，**只要不屏蔽那些字符编码小于 10 的键即可** 。故而，可以将上面的函数重写成如下所示。

  ```js
  EventUtil.addHandler(textbox, "keypress", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    var charCode = EventUtil.getCharCode(event);

    if (!/\d/.test(String.fromCharCode(charCode)) && charCode > 9){
      EventUtil.preventDefault(event);
    }
  });
  ```
  这样，我们的事件处理程序就可以适用所有浏览器了，**即可以屏蔽非数值字符** ，**但不屏蔽那些也会触发 keypress 事件的基本按键** 。

  除此之外，还有一个问题需要处理：**复制** 、**粘贴** 及 v其他操作还要用到 Ctrl 键** 。在除 IE 之外的所有浏览器中，**前面的代码也会屏蔽 Ctrl+C、Ctrl+V，以及其他使用 Ctrl 的组合键** 。因此，**最后还要添加一个检测条件** ，**以确保用户没有按下 Ctrl 键** ，如下面的例子所示。

  ```js
  EventUtil.addHandler(textbox, "keypress", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    var charCode = EventUtil.getCharCode(event);

    if (!/\d/.test(String.fromCharCode(charCode)) && charCode > 9 &&
    !event.ctrlKey){
      EventUtil.preventDefault(event);
    }
  });
  ```
  经过最后一点修改，就可以确保文本框的行为完全正常了。在这个例子的基础上加以修改和调整，就 **可以将同样的技术运用于放过和屏蔽任何输入文本框的字符** 。

- #### 操作剪贴板

  IE 是第一个支持与剪贴板相关事件，以及通过 JavaScript 访问剪贴板数据的浏览器。IE 的实现成为了事实上的标准，不仅 Safari 2、Chrome 和 Firefox 3 也都支持类似的事件和剪贴板访问（Opera 不支持通过 JavaScript 访问剪贴板），**HTML 5 后来也把剪贴板事件纳入了规范** 。下列就是 6 个剪贴板事件。

  - beforecopy ：在发生复制操作前触发。
  - copy ：在发生复制操作时触发。
  - beforecut ：在发生剪切操作前触发。
  - cut ：在发生剪切操作时触发。
  - beforepaste ：在发生粘贴操作前触发。
  - paste ：在发生粘贴操作时触发。

  由于没有针对剪贴板操作的标准，这些事件及相关对象会因浏览器而异。在 Safari、Chrome 和 Firefox 中， beforecopy 、 beforecut 和 beforepaste 事件只会在显示针对文本框的上下文菜单（预期将发生剪贴板事件）的情况下触发。但是，IE 则会在触发 copy 、 cut 和 paste 事件之前先行触发这些事件。

  **至于 copy 、 cut 和 paste 事件** ，**只要是在上下文菜单中选择了相应选项** ，或者 **使用了相应的键盘组合键** ，**所有浏览器都会触发它们** 。

  在实际的事件发生之前，**通过 beforecopy 、 beforecut 和 beforepaste 事件可以在向剪贴板发送数据** ，或者 **从剪贴板取得数据之前修改数据** 。不过，**取消这些事件并不会取消对剪贴板的操作** ——只有取消 copy 、 cut 和 paste 事件，才能阻止相应操作发生。

  要访问剪贴板中的数据，可以使用 clipboardData 对象：在 IE 中，这个对象是 window 对象的属性；而在 Firefox 4+、Safari 和 Chrome 中，这个对象是相应 event 对象的属性。但是，在 Firefox、Safari 和 Chorme 中，只有在处理剪贴板事件期间 clipboardData 对象才有效，这是为了防止对剪贴板的未授权访问；在 IE 中，则可以随时访问 clipboardData 对象。为了确保跨浏览器兼容性，最好只在发生剪贴板事件期间使用这个对象。

  这个 **clipboardData 对象** 有三个方法： **getData()** 、 **setData()** 和 **clearData()** 。其中， **getData() 用于从剪贴板中取得数据** ，它接受一个参数，即 **要取得的数据的格式** 。在 IE中，有两种数据格式： "text"和 "URL" 。在 Firefox、Safari 和 Chrome 中，这个参数是一种 MIME 类型；不过，可以用 "text" 代表 "text/plain" 。

  类似地， **setData() 方法** 的 **第一个参数也是数据类型** ，第二个参数是 **要放在剪贴板中的文本** 。对于第一个参数，IE 照样支持 "text" 和 "URL" ，而 Safari 和 Chrome 仍然只支持 MIME 类型。但是，与 getData() 方法不同的是，Safari 和 Chrome的 setData() 方法不能识别 "text" 类型。这两个浏览器在成功将文本放到剪贴板中后，都会返回 true ；否则，返回 false 。为了弥合这些差异，我们可以向 EventUtil 中再添加下列方法。

  ```js
  var EventUtil = {

    //省略的代码

    getClipboardText: function(event){
      var clipboardData = (event.clipboardData || window.clipboardData);
      return clipboardData.getData("text");
    },

    //省略的代码

    setClipboardText: function(event, value){
      if (event.clipboardData){
        return event.clipboardData.setData("text/plain", value);
      } else if (window.clipboardData){
        return window.clipboardData.setData("text", value);
      }
    },

    //省略的代码
  };
  ```
  这里的 **getClipboardText() 方法** 相对简单；它只要 **确定 clipboardData 对象的位置** ，然后再 **以 "text" 类型调用 getData() 方法** 即可。相应地， **setClipboardText() 方法** 则要稍微复杂一些。在 **取得 clipboardData 对象** 之后，需要 **根据不同的浏览器实现为 setData() 传入不同的类型** （对于 Safari 和 Chrome，是 "text/plain" ；对于 IE，是 "text" ）。

  **在需要确保粘贴到文本框中的文本中包含某些字符** ， **或者符合某种格式要求时** ，**能够访问剪贴板是非常有用的** 。例如，如果一个文本框只接受数值，那么就必须检测粘贴过来的值，以确保有效。在 **paste 事件** 中，**可以确定剪贴板中的值是否有效** ，**如果无效** ，就可以像下面示例中那样，**取消默认的行为** 。

  ```js
  EventUtil.addHandler(textbox, "paste", function(event){
    event = EventUtil.getEvent(event);
    var text = EventUtil.getClipboardText(event);
    if (!/^\d*$/.test(text)){
      EventUtil.preventDefault(event);
    }
  });
  ```
  在这里， onpaste 事件处理程序可以确保只有数值才会被粘贴到文本框中。如果剪贴板的值与正则表达式不匹配，则会取消粘贴操作。Firefox、Safari 和 Chrome 只允许在 onpaste 事件处理程序中访问 getData() 方法。

  由于并非所有浏览器都支持访问剪贴板，所以 **更简单的做法是屏蔽一或多个剪贴板操作** 。在支持 copy 、 cut 和 paste 事件的浏览器中（IE、Safari、Chrome 和 Firefox 3 及更高版本），很容易阻止这些事件的默认行为。在 Opera 中，则需要阻止那些会触发这些事件的按键操作，同时还要阻止在文本框中显示上下文菜单。

### 自动切换焦点

使用 JavaScript 可以从多个方面增强表单字段的易用性。其中，最常见的一种方式就是**在用户填写完当前字段时** ，**自动将焦点切换到下一个字段** 。通常，**在自动切换焦点之前，必须知道用户已经输入了既定长度的数据**（例如电话号码）。例如，美国的电话号码通常会分为三部分：区号、局号和另外 4 位数字。为取得完整的电话号码，很多网页中都会提供下列 3 个文本框：

```html
<input type="text" name="tel1" id="txtTel1" maxlength="3">
<input type="text" name="tel2" id="txtTel2" maxlength="3">
<input type="text" name="tel3" id="txtTel3" maxlength="4">
```

**为增强易用性** ，**同时加快数据输入** ，**可以在前一个文本框中的字符达到最大数量后** ，**自动将焦点切换到下一个文本框** 。换句话说，用户在第一个文本框中输入了 3 个数字之后，焦点就会切换到第二个文本框，再输入 3 个数字，焦点又会切换到第三个文本框。这种 **“自动切换焦点”的功能** ，可以通过下列代码实现：

```js
(function(){

  function tabForward(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    
    if (target.value.length == target.maxLength){
      var form = target.form;

      for (var i=0, len=form.elements.length; i < len; i++) {
        if (form.elements[i] == target) {
          if (form.elements[i+1]){
            form.elements[i+1].focus();
          } 
          return;
        }
      }
   }
}

var textbox1 = document.getElementById("txtTel1");
var textbox2 = document.getElementById("txtTel2");
var textbox3 = document.getElementById("txtTel3");

EventUtil.addHandler(textbox1, "keyup", tabForward);
EventUtil.addHandler(textbox2, "keyup", tabForward);
EventUtil.addHandler(textbox3, "keyup", tabForward);
})();
```
开始的 **tabForward() 函数** 是实现“自动切换焦点”的关键所在。这个函数 **通过比较用户输入的值与文本框的 maxlength 特性**v，**可以确定是否已经达到最大长度** 。**如果这两个值相等** （因为浏览器最终会强制它们相等，因此用户绝不会多输入字符），**则需要查找表单字段集合** ，**直至找到下一个文本框** 。

找到下一个文本框之后，则将焦点切换到该文本框。然后，我们把这个函数指定为每个文本框的 onkeyup 事件处理程序。由于 keyup 事件会在用户输入了新字符之后触发，所以此时是检测文本框中内容长度的最佳时机。这样一来，**用户在填写这个简单的表单时** ，**就不必再通过按制表键切换表单字段和提交表单了** 。

不过请记住，这些代码 **只适用于前面给出的标记** ，**而且没有考虑隐藏字段** 。

### HTML5 约束验证API

为了在将表单提交到服务器之前验证数据，HTML5 新增了一些功能。有了这些功能，**即便 JavaScript 被禁用或者由于种种原因未能加载** ，**也可以确保基本的验证** 。换句话说，**浏览器自己会根据标记中的规则执行验证** ，**然后自己显示适当的错误消息**（完全不用 JavaScript 插手）。当然，**这个功能只有在支持 HTML5 这部分内容的浏览器中才有效** ，这些浏览器有 Firefox 4+、Safari 5+、Chrome 和 Opera 10+。

**只有在某些情况下表单字段才能进行自动验证** 。具体来说，就是 **要在 HTML 标记中为特定的字段指定一些约束** ，然后 **浏览器才会自动执行表单验证**。

- #### 必填字段

  第一种情况是在表单字段中指定了 **required 属性** ，如下面的例子所示：

  ```html
  <input type="text" name="username" required>
  ```
  任何标注有 required 的字段，在提交表单时都不能空着。**这个属性适用于 `<input>` 、 `<textarea>` 和 `<select>` 字段**（Opera 11 及之前版本还不支持 `<select>` 的 required 属性）。**在 JavaScript 中** ，**通过对应的 required 属性，可以检查某个表单字段是否为必填字段** 。

  ```js
  var isUsernameRequired = document.forms[0].elements["username"].required;
  ```
  另外，使用下面这行代码可以 **测试浏览器是否支持 required 属性** 。

  ```js
  var isRequiredSupported = "required" in document.createElement("input");
  ```
  以上代码通过特性检测来确定新创建的 `<input>` 元素中是否存在 required 属性。

  对于空着的必填字段，不同浏览器有不同的处理方式。Firefox 4 和 Opera 11 会阻止表单提交并在相应字段下方弹出帮助框，而 Safari（5 之前）和 Chrome（9 之前）则什么也不做，而且也不阻止表单提交。

- #### 其他输入类型

  **HTML5 为 `<input>` 元素的 type 属性又增加了几个值** 。**这些新的类型不仅能反映数据类型的信息** ，而且 **还能提供一些默认的验证功能** 。其中， **"email" 和 "url" 是两个得到支持最多的类型** ，**各浏览器也都为它们增加了定制的验证机制** 。例如：

  ```html
  <input type="email" name ="email">
  <input type="url" name="homepage">
  ```
  顾名思义， **"email" 类型要求输入的文本必须符合电子邮件地址的模式** ，而 **"url" 类型要求输入的文本必须符合 URL 的模式** 。不过，本节前面提到的浏览器在恰当地匹配模式方面都存在问题。最明显的是 "-@-" 会被当成一个有效的电子邮件地址。浏览器开发商还在解决这些问题。

  **要检测浏览器是否支持这些新类型** ，**可以在 JavaScript 创建一个 `<input>` 元素** ，然后 **将 type 属性设置为 "email" 或 "url"**  ，最后 **再检测这个属性的值** 。**不支持它们的旧版本浏览器** 会 **自动将未知的值设置为 "text"**  ，而 **支持的浏览器** 则会 **返回正确的值** 。例如：

  ```js
  var input = document.createElement("input");
  input.type = "email";

  var isEmailSupported = (input.type == "email");
  ```
  要注意的是，如果不给 `<input>` 元素设置 required 属性，那么空文本框也会验证通过。另一方面，**设置特定的输入类型并不能阻止用户输入无效的值** ，**只是应用某些默认的验证而已** 。

- #### 数值范围

  **除了 "email" 和 "url" ，HTML5 还定义了另外几个输入元素** 。这几个元素都要求填写某种基于数字的值： **"number"** 、 **"range"** 、 **"datetime"** 、 **"datetime-local"** 、 **"date"** 、 **"month"** 、 **"week"** ，还有 **"time"** 。**浏览器对这几个类型的支持情况并不好** ，因此如果真想选用的话，要特别小心。目前，浏览器开发商主要关注更好的跨平台兼容性以及更多的逻辑功能。因此，本节介绍的内容某种程度上有些超前，不一定马上就能在实际开发中使用。

  **对所有这些数值类型的输入元素** ，**可以指定 min 属性**（最小的可能值）、 **max 属性**（最大的可能值）和 **step 属性**（从 min 到 max 的两个刻度间的差值）。例如，想让用户只能输入 0 到 100的值，而且这个值必须是 5 的倍数，可以这样写代码：

  ```js
  <input type="number" min="0" max="100" step="5" name="count">
  ```
  在不同的浏览器中，可能会也可能不会看到能够自动递增和递减的数值调节按钮（向上和向下按钮）。

  以上这些属性 **在 JavaScript 中都能通过对应的元素访问**（或修改）。此外，还有两个方法： **stepUp()** 和 **stepDown()** ，都接收一个可选的参数：**要在当前值基础上加上或减去的数值** 。（默认是加或减 1。）这两个方法还没有得到任何浏览器支持，但下面的例子演示了它们的用法。

  ```js
  input.stepUp(); //加 1
  input.stepUp(5); //加 5
  input.stepDown(); //减 1
  input.stepDown(10); //减 10
  ```
- #### 输入模式

  **HTML5 为文本字段新增了 pattern 属性** 。**这个属性的值是一个正则表达式** ，用于 **匹配文本框中的值** 。例如，如果只想允许在文本字段中输入数值，可以像下面的代码一样应用约束：

  ```html
  <input type="text" pattern="\d+" name="count">
  ```
  注意，**模式的开头和末尾不用加^和$符号**（假定已经有了）。**这两个符号表示输入的值必须从头到尾都与模式匹配** 。与其他输入类型相似，**指定 pattern 也不能阻止用户输入无效的文本** 。**这个模式应用给值，浏览器来判断值是有效，还是无效** 。**在 JavaScript 中可以通过 pattern 属性访问模式** 。

  ```js
  var pattern = document.forms[0].elements["count"].pattern;
  ```
  使用以下代码可以检测浏览器是否支持 pattern 属性。

  ```js
  var isPatternSupported = "pattern" in document.createElement("input");
  ```
- #### 检测有效性

  使用 **checkValidity() 方法** 可以 **检测表单中的某个字段是否有效** 。**所有表单字段都有个方法** ，**如果字段的值有效，这个方法返回 true** ，**否则返回 false** 。字段的值是否有效的判断依据是本节前面介绍过的那些约束。换句话说，**必填字段中如果没有值就是无效的** ，而 **字段中的值与 pattern 属性不匹配也是无效的** 。例如：

  ```js
  if (document.forms[0].elements[0].checkValidity()){
    //字段有效，继续
  } else {
    //字段无效
  }
  ```
  **要检测整个表单是否有效，可以在表单自身调用 checkValidity() 方法** 。**如果所有表单字段都有效** ，这个方法 **返回 true** ；**即使有一个字段无效** ，这个方法也会 **返回 false**  。

  ```js
  if(document.forms[0].checkValidity()){
    //表单有效，继续
  } else {
    //表单无效
  }
  ```
  与 **checkValidity() 方法简单地告诉你字段是否有效** 相比， **validity 属性则会告诉你为什么字段有效或无效** 。这个对象中包含一系列属性，**每个属性会返回一个布尔值** 。

  - customError ：如果设置了 setCustomValidity() ，则为 true ，否则返回 false 。
  - patternMismatch ：如果值与指定的 pattern 属性不匹配，返回 true 。
  - rangeOverflow ：如果值比 max 值大，返回 true 。
  - rangeUnderflow ：如果值比 min 值小，返回 true 。
  - stepMisMatch ：如果 min 和 max 之间的步长值不合理，返回 true 。
  - tooLong ：如果值的长度超过了 maxlength 属性指定的长度，返回 true 。有的浏览器（如Firefox 4）会自动约束字符数量，因此这个值可能永远都返回 false 。
  - typeMismatch ：如果值不是 "mail" 或 "url" 要求的格式，返回 true 。
  - valid ：如果这里的其他属性都是 false ，返回 true 。 checkValidity() 也要求相同的值。
  - valueMissing ：如果标注为 required 的字段中没有值，返回 true 。

  因此，**要想得到更具体的信息** ，就 **应该使用 validity 属性来检测表单的有效性** 。下面是一个例子。

  ```js
  if (input.validity && !input.validity.valid){
    if (input.validity.valueMissing){
      alert("Please specify a value.")
    } else if (input.validity.typeMismatch){
      alert("Please enter an email address.");
    } else {
      alert("Value is invalid.");
    }
  }
  ```
- #### 禁用验证

  **通过设置 novalidate 属性，可以告诉表单不进行验证** 。

  ```js
  <form method="post" action="signup.php" novalidate>
    <!--这里插入表单元素-->
  </form>
  ```
  **在 JavaScript 中使用 noValidate 属性可以取得或设置这个值** ，**如果这个属性存在** ，**值为 true** ，**如果不存在** ，**值为 false** 。

  ```js
  document.forms[0].noValidate = true; //禁用验证
  ```
  如果一个表单中有多个提交按钮，**为了指定点击某个提交按钮不必验证表单** ，**可以在相应的按钮上添加 formnovalidate 属性** 。

  ```html
  <form method="post" action="foo.php">
    <!--这里插入表单元素-->
    <input type="submit" value="Regular Submit">

    <input type="submit" formnovalidate name="btnNoValidate" value="Non-validating Submit">
  </form>
  ```
  在这个例子中，点击第一个提交按钮会像往常一样验证表单，而点击第二个按钮则会不经过验证而提交表单。**使用 JavaScript 也可以设置这个属性** 。

  ```js
  //禁用验证
  document.forms[0].elements["btnNoValidate"].formNoValidate = true;
  ```
## 选择框脚本

**选择框是通过 `<select>` 和 `<option>` 元素创建的** 。为了方便与这个控件交互，**除了所有表单字段共有的属性和方法外** ， HTMLSelectElement 类型还提供了下列属性和方法。

- add(newOption, relOption) ：向控件中插入新 `<option>` 元素，其位置在相关项（ relOption ）之前。
- multiple ：布尔值，表示是否允许多项选择；等价于 HTML 中的 multiple 特性。
- options ：控件中所有 `<option>` 元素的 HTMLCollection 。
- remove(index) ：移除给定位置的选项。
- selectedIndex ：基于 0 的选中项的索引，如果没有选中项，则值为 -1 。对于支持多选的控件，只保存选中项中第一项的索引。
- size ：选择框中可见的行数；等价于 HTML 中的 size 特性。选择框的 type 属性不是 "select-one" ，就是 "select-multiple" ，这取决于 HTML 代码中有没有 multiple 特性。选择框的 value 属性由当前选中项决定，相应规则如下。
-  如果没有选中的项，则选择框的 value 属性保存空字符串。
-  如果有一个选中项，而且该项的 value 特性已经在 HTML 中指定，则选择框的 value 属性等于选中项的 value 特性。即使 value 特性的值是空字符串，也同样遵循此条规则。
-  如果有一个选中项，但该项的 value 特性在 HTML 中未指定，则选择框的 value 属性等于该项的文本。
-  如果有多个选中项，则选择框的 value 属性将依据前两条规则取得第一个选中项的值。

以下面的选择框为例：
  
 ```html
<select name="location" id="selLocation">
  <option value="Sunnyvale, CA">Sunnyvale</option>
  <option value="Los Angeles, CA">Los Angeles</option>
  <option value="Mountain View, CA">Mountain View</option>
  <option value="">China</option>
  <option>Australia</option>
</select>
```
如果用户选择了其中第一项，则选择框的值就是 "Sunnyvale, CA" 。如果文本为 "China" 的选项被选中，则选择框的值就是一个空字符串，因为其 value 特性是空的。如果选择了最后一项，那么由于 `<option>` 中没有指定 value 特性，则选择框的值就是 "Australia" 。

在 DOM 中，每个 `<option>` 元素都有一个 HTMLOptionElement 对象表示。为便于访问数据，HTMLOptionElement 对象添加了下列属性：
  
- index ：当前选项在 options 集合中的索引。
- label ：当前选项的标签；等价于 HTML 中的 label 特性。
- selected ：布尔值，表示当前选项是否被选中。将这个属性设置为 true 可以选中当前选项。
- text ：选项的文本。
- value ：选项的值（等价于 HTML 中的 value 特性）。

**其中大部分属性的目的，都是为了方便对选项数据的访问** 。虽然也可以使用 **常规的 DOM 功能来访问这些信息** ，但 **效率是比较低的** ，如下面的例子所示：

```js
var selectbox = document.forms[0].elements["location"];

//不推荐
var text = selectbox.options[0].firstChild.nodeValue; //选项的文本
var value = selectbox.options[0].getAttribute("value"); //选项的值
```
以上代码使用标准 DOM 方法，取得了选择框中第一项的文本和值。可以与下面使用选项属性的代码作一比较：

```js
var selectbox = document.forms[0]. elements["location"];

// 推荐
var text = selectbox.options[0].text; // 选项的文本
var value = selectbox.options[0].value; // 选项的值
```
在操作选项时，我们建议 **最好是使用特定于选项的属性** ，因为 **所有浏览器都支持这些属性** 。在将表单控件作为 DOM 节点的情况下，实际的交互方式则会因浏览器而异。我们不推荐使用标准 DOM 技术修改 `<option>` 元素的文本或者值。

最后，我们还想提醒读者注意一点：**选择框的 change 事件与其他表单字段的 change 事件触发的条件不一样** 。**其他表单字段的 change 事件** 是在 **值被修改且焦点离开当前字段时触发** ，而 **选择框的 change 事件** 只要 **选中了选项就会触发** 。

> 不同浏览器下，选项的 value 属性返回什么值也存在差别。但是，在所有浏览器中， value 属性始终等于 value 特性。在未指定 value 特性的情况下，IE8会返回空字符串，而 IE9+、Safari、Firefox、Chrome 和 Opera 则会返回与 text 特性相同的值。

### 选择选项

对于 **只允许选择一项的选择框** ，访问选中项的最简单方式，就是 **使用选择框的 selectedIndex 属性** ，如下面的例子所示：

```js
var selectedOption = selectbox.options[selectbox.selectedIndex];
```
取得选中项之后，可以像下面这样显示该选项的信息：

```js
var selectedIndex = selectbox.selectedIndex;
var selectedOption = selectbox.options[selectedIndex];
alert("Selected index: " + selectedIndex + "\nSelected text: " + selectedOption.text + "\nSelected value: " + selectedOption.value);
```
这里，我们通过一个警告框显示了选中项的索引、文本和值。

**对于可以选择多项的选择框， selectedfIndex 属性就好像只允许选择一项一样** 。设置 selectedIndex 会导致取消以前的所有选项并选择指定的那一项，而读取 selectedIndex 则只会返回选中项中第一项的索引值。

另一种选择选项的方式，就是 **取得对某一项的引用** ，然后 **将其 selected 属性设置为 true** 。例如，下面的代码会选中选择框中的第一项：

```js
selectbox.options[0].selected = true;
```
与 selectedIndex 不同，在允许多选的选择框中设置选项的 selected 属性，不会取消对其他选中项的选择，因而可以动态选中任意多个项。但是，如果是在单选选择框中，修改某个选项的 selected 属性则会取消对其他选项的选择。需要注意的是，将 selected 属性设置为 false 对单选选择框没有影响。

实际上，**selected 属性的作用主要是确定用户选择了选择框中的哪一项** 。**要取得所有选中的项，可以循环遍历选项集合，然后测试每个选项的 selected 属性** 。来看下面的例子。

```js
function getSelectedOptions(selectbox){
  var result = new Array();
  var option = null;
  
  for (var i=0, len=selectbox.options.length; i < len; i++){
    option = selectbox.options[i];
    if (option.selected){
      result.push(option);
    }
  }
  
  return result;
}
```
这个函数可以返回给定选择框中选中项的一个数组。首先，创建一个将包含选中项的数组，然后使用 for 循环迭代所有选项，同时检测每一项的 selected 属性。如果有选项被选中，则将其添加到 result 数组中。最后，返回包含选中项的数组。下面是一个 **使用 getSelectedOptions() 函数取得选中项** 的示例。

```js
var selectbox = document.getElementById("selLocation");
var selectedOptions = getSelectedOptions(selectbox);
var message = "";

for (var i=0, len=selectedOptions.length; i < len; i++){
  message += "Selected index: " + selectedOptions[i].index + "\nSelected text: " + selectedOptions[i].text + "\nSelected value: " + selectedOptions[i].value + "\n\n";
}
alert(message);
```
在这个例子中，我们首先从一个选择框中取得了选中项。然后，使用 for 循环构建了一条消息，包含所有选中项的信息：每一项的索引、文本和值。这种技术适用于单选和多选选择框。

### 添加选项

**可以使用 JavaScript 动态创建选项，并将它们添加到选择框中** 。添加选项的方式有很多，第一种方式就是使用如下所示的 DOM方法。

```js
var newOption = document.createElement("option");
newOption.appendChild(document.createTextNode("Option text"));
newOption.setAttribute("value", "Option value");

selectbox.appendChild(newOption);
```
以上代码创建了一个新的 `<option>` 元素，然后为它添加了一个文本节点，并设置其 value 特性，最后将它添加到了选择框中。添加到选择框之后，用户立即就可以看到新选项。
  
第二种方式是 **使用 Option 构造函数来创建新选项** ，这个构造函数是 DOM 出现之前就有的，一直遗留到现在。 **Option 构造函数** 接受两个参数：**文本**（ text ）和 **值**（ value ）；**第二个参数可选** 。虽然这个构造函数会创建一个 Object 的实例，但兼容 DOM 的浏览器会返回一个 `<option>` 元素。换句话说，**在这种情况下，我们仍然可以使用 appendChild() 将新选项添加到选择框中** 。来看下面的例子。
  
```js
var newOption = new Option("Option text", "Option value");
selectbox.appendChild(newOption); //在 IE8 及之前版本中有问题
```
这种方式在除 IE 之外的浏览器中都可以使用。由于存在 bug，IE 在这种方式下不能正确设置新选项的文本。

第三种添加新选项的方式是 **使用选择框的 add() 方法** 。DOM 规定这个方法接受两个参数：**要添加的新选项** 和 **将位于新选项之后的选项** 。如果想在列表的最后添加一个选项，应该将第二个参数设置为 null 。在 IE对 add() 方法的实现中，第二个参数是可选的，而且如果指定，该参数必须是新选项之后选项的索引。兼容 DOM 的浏览器要求必须指定第二个参数，因此要想编写跨浏览器的代码，就不能只传入一个参数。这时候，为第二个参数传入 undefined ，就可以在所有浏览器中都将新选项插入到列表最后了。来看一个例子。

```js
var newOption = new Option("Option text", "Option value");
selectbox.add(newOption, undefined); //最佳方案
```
在 IE 和兼容 DOM 的浏览器中，上面的代码都可以正常使用。**如果你想将新选项添加到其他位置**（不是最后一个），**就应该使用标准的 DOM 技术和 insertBefore() 方法** 。

> 就和在 HTML 中一样，此时也不一定要为选项指定值。换句话说，只为 Option 构造函数传入一个参数（选项的文本）也没有问题。

### 移除选项

与添加选项类似，移除选项的方式也有很多种。首先，**可以使用 DOM 的 removeChild() 方法** ，**为其传入要移除的选项** ，如下面的例子所示：

```js
selectbox.removeChild(selectbox.options[0]); //移除第一个选项
```
其次，**可以使用选择框的 remove() 方法** 。这个方法接受一个参数，即 **要移除选项的索引** ，如下面的例子所示：

```js
selectbox.remove(0); //移除第一个选项
```
最后一种方式，**就是将相应选项设置为 null** 。这种方式也是 DOM 出现之前浏览器的遗留机制。例如：

```js
selectbox.options[0] = null; //移除第一个选项
```
**要清除选择框中所有的项，需要迭代所有选项并逐个移除它们** ，如下面的例子所示：

```
function clearSelectbox(selectbox){
  for(var i=0, len=selectbox.options.length; i < len; i++){
    selectbox.remove(i);
  }
}
```
这个函数每次只移除选择框中的第一个选项。**由于移除第一个选项后，所有后续选项都会自动向上移动一个位置** ，**因此重复移除第一个选项就可以移除所有选项了** 。

### 移动和重排选项

在 DOM 标准出现之前，将一个选择框中的选项移动到另一个选择框中是非常麻烦的。整个过程要涉及从第一个选择框中移除选项，然后以相同的文本和值创建新选项，最后再将新选项添加到第二个选择框中。而使用 DOM 的 appendChild() 方法，就可以将第一个选择框中的选项直接移动到第二个选择框中。我们知道，如果为 appendChild() 方法传入一个文档中已有的元素，那么就会先从该元素的父节点中移除它，再把它添加到指定的位置。下面的代码展示了 **将第一个选择框中的第一个选项移动到第二个选择框中的过程** 。

```js
var selectbox1 = document.getElementById("selLocations1");
var selectbox2 = document.getElementById("selLocations2");

selectbox2.appendChild(selectbox1.options[0]);
```
**移动选项与移除选项有一个共同之处** ，**即会重置每一个选项的 index 属性** 。

重排选项次序的过程也十分类似，最好的方式仍然是使用 DOM 方法。要将选择框中的某一项移动到特定位置，最合适的 DOM 方法就是 **insertBefore()** ； **appendChild()** 方法只适用于将选项添加到选择框的最后。要在选择框中向前移动一个选项的位置，可以使用以下代码：

```js
var optionToMove = selectbox.options[1];
selectbox.insertBefore(optionToMove, selectbox.options[optionToMove.index-1]);
```
以上代码首先选择了要移动的选项，然后将其插入到了排在它前面的选项之前。实际上，第二行代码对除第一个选项之外的其他选项是通用的。类似地，可以使用下列代码 **将选择框中的选项向后移动一个位置** 。

```js
var optionToMove = selectbox.options[1];
selectbox.insertBefore(optionToMove, selectbox.options[optionToMove.index+2]);
```
以上代码适用于选择框中的所有选项，包括最后一个选项。

> IE7 存在一个页面重绘问题，有时候会导致使用 DOM 方法重排的选项不能马上正确显示。

## 表单序列化

**随着 Ajax 的出现，表单序列化已经成为一种常见需求** （第 21 章将讨论 Ajax）。**在 JavaScript 中，可以利用表单字段的 type 属性，连同 name 和 value 属性一起实现对表单的序列化** 。在编写代码之前，有必须先搞清楚在表单提交期间，浏览器是怎样将数据发送给服务器的。

-  对表单字段的名称和值进行 URL 编码，使用和号（&）分隔。
-  不发送禁用的表单字段。
-  只发送勾选的复选框和单选按钮。
-  不发送 type 为 "reset" 和 "button" 的按钮。
-  多选选择框中的每个选中的值单独一个条目。
-  在单击提交按钮提交表单的情况下，也会发送提交按钮；否则，不发送提交按钮。也包括 type为 "image" 的 `<input>` 元素。
- `<select>` 元素的值，就是选中的 `<option>` 元素的 value 特性的值。如果 `<option>` 元素没有 value 特性，则是 `<option>` 元素的文本值。
  
在表单序列化过程中，一般不包含任何按钮字段，因为结果字符串很可能是通过其他方式提交的。除此之外的其他上述规则都应该遵循。以下就是 **实现表单序列化** 的代码。
  
```
function serialize(form){
  var parts = [],
  field = null,
    i,
    len,
    j,
    optLen,
    option,
    optValue;
  
  for (i=0, len=form.elements.length; i < len; i++){
    field = form.elements[i];
    
    switch(field.type){
      case "select-one":
      case "select-multiple":
      
        if (field.name.length){
          for (j=0, optLen = field.options.length; j < optLen; j++){
            option = field.options[j];
            if (option.selected){
              optValue = "";
              if (option.hasAttribute){
                optValue = (option.hasAttribute("value") ?
                option.value : option.text);
              } else {
                optValue = (option.attributes["value"].specified ?
                option.value : option.text);
              }
              parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(optValue));
            }
          }
        }
        break;
      
      case undefined: //字段集
      case "file": //文件输入
      case "submit": //提交按钮
      case "reset": //重置按钮
      case "button": //自定义按钮
        break;
        
      case "radio": //单选按钮
      case "checkbox": //复选框
        if (!field.checked){
          break;
        }
      /* 执行默认操作 */
      
      default:
        //不包含没有名字的表单字段
        if (field.name.length){
          parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(field.value));
        }
    }
  }
  return parts.join("&");
}
```
上面这个 serialize() 函数首先定义了一个名为 parts 的数组，用于保存将要创建的字符串的各个部分。然后，通过 for 循环迭代每个表单字段，并将其保存在 field 变量中。在获得了一个字段的引用之后，使用 switch 语句检测其 type 属性。序列化过程中最麻烦的就是 `<select>` 元素，它可能是单选框也可能是多选框。为此，需要遍历控件中的每一个选项，并在相应选项被选中的情况下向数组中添加一个值。对于单选框，只可能有一个选中项，而多选框则可能有零或多个选中项。这里的代码适用于这两种选择框，至于可选项的数量则是由浏览器控制的。在找到一个选中项之后，需要确定使用什么值。如果不存在 value 特性，或者虽然存在该特性，但值为空字符串，都要使用选项的文本来代替。为检查这个特性，在 DOM 兼容的浏览器中需要使用 hasAttribute() 方法，而在 IE 中需要使用特性的 specified 属性。

如果表单中包含 `<fieldset>` 元素，则该元素会出现在元素集合中，但没有 type 属性。因此，如果 type 属性未定义，则不需要对其进行序列化。同样，对于各种按钮以及文件输入字段也是如此（文件输入字段在表单提交过程中包含文件的内容；但是，这个字段是无法模仿的，序列化时一般都要忽略）。对于单选按钮和复选框，要检查其 checked 属性是否被设置为 false ，如果是则退出 switch 语句。如果 checked 属性为 true ，则继续执行 default 语句，即将当前字段的名称和值进行编码，然后添加到 parts 数组中。函数的最后一步，就是使用 join() 格式化整个字符串，也就是用和号来分隔每一个表单字段。

最后， serialize() 函数会以查询字符串的格式输出序列化之后的字符串。当然，要序列化成其他格式，也不是什么困难的事。

## 富文本编辑

富文本编辑，又称为 WYSIWYG（What You See Is What You Get，所见即所得）。**在网页中编辑富文本内容，是人们对 Web 应用程序最大的期待之一** 。虽然也没有规范，但在 IE 最早引入的这一功能基础上，已经出现了事实标准。而且，Opera、Safari、Chrome 和 Firefox 都已经支持这一功能。这一技术的本质，就是在页面中嵌入一个包含空 HTML 页面的 iframe 。通过设置 designMode 属性，这个空白的 HTML 页面可以被编辑，而编辑对象则是该页面 `<body>` 元素的 HTML 代码。 designMode 属性有两个可能的值： "off" （默认值）和 "on" 。在设置为 "on" 时，整个文档都会变得可以编辑（显示插入符号），然后就可以像使用字处理软件一样，通过键盘将文本内容加粗、变成斜体，等等。

**可以给 iframe 指定一个非常简单的 HTML 页面作为其内容来源** 。例如：
  
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Blank Page for Rich Text Editing</title>
  </head>
  <body>
  </body>
</html>
```
这个页面在 iframe 中可以像其他页面一样被加载。**要让它可以编辑，必须要将 designMode 设置为 "on"** ，但 **只有在页面完全加载之后才能设置这个属性** 。因此，在包含页面中，**需要使用 onload 事件处理程序来在恰当的时刻设置 designMode ** ，如下面的例子所示：

```html
<iframe name="richedit" style="height:100px;width:100px;" src="blank.htm"></iframe>

<script type="text/javascript">
  EventUtil.addHandler(window, "load", function(){
    frames["richedit"].document.designMode = "on";
  });
</script>
```
等到以上代码执行之后，你就会在页面中看到一个类似文本框的可编辑区字段。这个区字段具有与其他网页相同的默认样式；不过，**通过为空白页面应用 CSS 样式，可以修改可编辑区字段的外观** 。

### 使用 contenteditable 属性

另一种编辑富文本内容的方式是使用名为 **contenteditable 的特殊属性** ，这个属性也是由 IE 最早实现的。**可以把 contenteditable 属性应用给页面中的任何元素，然后用户立即就可以编辑该元素** 。

这种方法之所以受到欢迎，是因为它不需要 iframe 、空白页和 JavaScript，只要为元素设置 contenteditable 属性即可。

```html
<div class="editable" id="richedit" contenteditable></div>  
```
这样，元素中包含的任何文本内容就都可以编辑了，就好像这个元素变成了 <textarea> 元素一样。通过在这个元素上设置 contenteditable 属性，也能打开或关闭编辑模式。
  
```js
var div = document.getElementById("richedit");
div.contentEditable = "true";
```
contenteditable 属性有三个可能的值： **"true" 表示打开** 、 **"false" 表示关闭** ， **"inherit" 表示从父元素那里继承**（因为可以在 contenteditable 元素中创建或删除元素）。支持 contenteditable属性的元素有 IE、Firefox、Chrome、Safari 和 Opera。在移动设备上，支持 contenteditable 属性的浏览器有 iOS 5+中的 Safari 和 Android 3+中的 WebKit。

### 操作富文本

**与富文本编辑器交互的主要方式，就是使用 document.execCommand()** 。这个方法可以对文档执行预定义的命令，而且可以应用大多数格式。可以为 **document.execCommand() 方法** 传递 3 个参数：**要执行的命令名称** 、**表示浏览器是否应该为当前命令提供用户界面的一个布尔值** 和 **执行命令必须的一个值** （如果不需要值，则传递 null ）。**为了确保跨浏览器的兼容性，第二个参数应该始终设置为 false**  ，因为 Firefox 会在该参数为 true 时抛出错误。

不同浏览器支持的预定义命令也不一样。下表列出了那些被支持最多的命令。

![预定义命令](http://p9myzkds7.bkt.clouddn.com/JavaScript-form-script/%E9%A2%84%E5%AE%9A%E4%B9%89%E5%91%BD%E4%BB%A4.png)

![预定义命令（续）](http://p9myzkds7.bkt.clouddn.com/JavaScript-form-script/%E9%A2%84%E5%AE%9A%E4%B9%89%E5%91%BD%E4%BB%A4%EF%BC%88%E7%BB%AD%EF%BC%89.png)

其中，与剪贴板有关的命令在不同浏览器中的差异极大。Opera 根本没有实现任何剪贴板命令，而 Firefox 在默认情况下会禁用它们（必须修改用户的首选项来启用它们）。Safari 和 Chrome 实现了 cut 和 copy ，但没有实现 paste 。不过，即使不能通过 document.execCommand() 来执行这些命令，但却可以通过相应的快捷键来实现同样的操作。

**可以在任何时候使用这些命令来修改富文本区域的外观** ，如下面的例子所示。

```js
//转换粗体文本
frames["richedit"].document.execCommand("bold", false, null);

//转换斜体文本
frames["richedit"].document.execCommand("italic", false, null);

//创建指向 www.wrox.com 的链接
frames["richedit"].document.execCommand("createlink", false, "http://www.wrox.com");

//格式化为 1 级标题
frames["richedit"].document.execCommand("formatblock", false, "<h1>");
```
同样的方法也适用于页面中 contenteditable 属性为 "true" 的区块，**只要把对框架的引用替换成当前窗口的 document 对象即可** 。

```js
//转换粗体文本
document.execCommand("bold", false, null);

//转换斜体文本
document.execCommand("italic", false, null);

//创建指向 www.wrox.com 的链接
document.execCommand("createlink", false, "http://www.wrox.com");

//格式化为 1 级标题
document.execCommand("formatblock", false, "<h1>");
```
需要注意的是，虽然所有浏览器都支持这些命令，但这些命令所产生的 HTML 仍然有很大不同。例如，执行 bold 命令时，IE 和 Opera 会使用 `<strong>` 标签包围文本，Safari 和 Chrome 使用 `<b>` 标签，而 Firefox 则使用 `<span>` 标签。由于各个浏览器实现命令的方式不同，加上它们通过 innerHTML 实现转换的方式也不一样，因此不能指望富文本编辑器会产生一致的 HTML。

除了命令之外，还有一些与命令相关的方法。第一个方法就是 queryCommandEnabled() ，可以用它来检测是否可以针对当前选择的文本，或者当前插入字符所在位置执行某个命令。这个方法接收一个参数，即要检测的命令。如果当前编辑区域允许执行传入的命令，这个方法返回 true ，否则返回 false 。例如：

```js
var result = frames["richedit"].document.queryCommandEnabled("bold");
```
如果能够对当前选择的文本执行 "bold" 命令，以上代码会返回 true 。需要注意的是， queryCommandEnabled() 方法返回 true ，并不意味着实际上就可以执行相应命令，而只能说明对当前选择的文本执行相应命令是否合适。例如，Firefox 在默认情况下会禁用剪切操作，但执行 queryCommandEnabled("cut") 也可能会返回 true 。

另外， queryCommandState() 方法用于确定是否已将指定命令应用到了选择的文本。例如，要确定当前选择的文本是否已经转换成了粗体，可以使用如下代码。

```js
var isBold = frames["richedit"].document.queryCommandState("bold");
```
如果此前已经对选择的文本执行了 "bold" 命令，那么上面的代码会返回 true 。一些功能全面的富文本编辑器，正是利用这个方法来更新粗体、斜体等按钮的状态的。
最后一个方法是 queryCommandValue() ，用于取得执行命令时传入的值（即前面例子中传给 document.execCommand() 的第三个参数）。例如，在对一段文本应用 "fontsize" 命令时如果传入了 7 ，那么下面的代码就会返回 "7" ：

```js
var fontSize = frames["richedit"].document.queryCommandValue("fontsize");
```
通过这个方法可以确定某个命令是怎样应用到选择的文本的，可以据以确定再对其应用后续命令是否合适。

###  富文本选区

在富文本编辑器中，使用框架（ iframe ）的 getSelection() 方法，可以确定实际选择的文本。这个方法是 window 对象和 document 对象的属性，调用它会返回一个表示当前选择文本的 Selection 对象。每个 Selection 对象都有下列属性。

- anchorNode ：选区起点所在的节点。
- anchorOffset ：在到达选区起点位置之前跳过的 anchorNode 中的字符数量。
- focusNode ：选区终点所在的节点。
- focusOffset ： focusNode 中包含在选区之内的字符数量。
- isCollapsed ：布尔值，表示选区的起点和终点是否重合。
- rangeCount ：选区中包含的 DOM 范围的数量。

Selection 对象的这些属性并没有包含多少有用的信息。好在，该对象的下列方法提供了更多信息，并且支持对选区的操作。  

- addRange(range) ：将指定的 DOM 范围添加到选区中。
- collapse(node, offset) ：将选区折叠到指定节点中的相应的文本偏移位置。
- collapseToEnd() ：将选区折叠到终点位置。
- collapseToStart() ：将选区折叠到起点位置。
- containsNode(node) ：确定指定的节点是否包含在选区中。
- deleteFromDocument() ：从文档中删除选区中的文本，与 document.execCommand("delete",false, null) 命令的结果相同。
- extend(node, offset) ：通过将 focusNode 和 focusOffset 移动到指定的值来扩展选区。
- getRangeAt(index) ：返回索引对应的选区中的 DOM 范围。
- removeAllRanges() ：从选区中移除所有 DOM 范围。实际上，这样会移除选区，因为选区中至少要有一个范围。
- reomveRange(range) ：从选区中移除指定的 DOM 范围。
- selectAllChildren(node) ：清除选区并选择指定节点的所有子节点。
- toString() ：返回选区所包含的文本内容。

Selection 对象的这些方法都极为实用，它们利用了（第 12 章讨论过的）DOM 范围来管理选区。由于可以直接操作选择文本的 DOM 表现，因此访问 DOM范围与使用 execCommand() 相比，能够对富文本编辑器进行更加细化的控制。下面来看一个例子。
  
```js
var selection = frames["richedit"].getSelection();

//取得选择的文本
var selectedText = selection.toString();

//取得代表选区的范围
var range = selection.getRangeAt(0);

//突出显示选择的文本
var span = frames["richedit"].document.createElement("span");
span.style.backgroundColor = "yellow";
range.surroundContents(span);
```
以上代码会为富文本编辑器中被选择的文本添加黄色的背景。这里使用了默认选区中的 DOM 范围，通过 surroundContents() 方法将选区添加到了带有黄色背景的 <span> 元素中。
  
HTML5 将 getSelection() 方法纳入了标准，而且 IE9、Firefox、Safari、Chrome 和 Opera 8 都实现了它。由于历史原因，在 Firefox 3.6+中调用 document.getSelection() 会返回一个字符串。为此，可以在 Firefox 3.6+中改作调用 window.getSelection() ，从而返回 selection 对象。Firefox 8 修复了 document.getSelection() 的 bug，能返回与 window.getSelection() 相同的值。

IE8 及更早的版本不支持 DOM范围，但我们可以通过它支持的 selection 对象操作选择的文本。IE 中的 selection 对象是 document 的属性，本章前面曾经讨论过。要取得富文本编辑器中选择的文本，首先必须创建一个文本范围（请参考第 12 章中的相关内容），然后再像下面这样访问其 text 属性。
  
```js
var range = frames["richedit"].document.selection.createRange();
var selectedText = range.text;
```

虽然使用 IE 的文本范围来执行 HTML 操作并不像使用 DOM 范围那么可靠，但也不失为一种有效的途径。要像前面使用 DOM 范围那样实现相同的文本高亮效果，可以组合使用 htmlText 属性和 pasteHTML() 方法。

```js
var range = frames["richedit"].document.selection.createRange();
range.pasteHTML("<span style=\"background-color:yellow\"> " + range.htmlText + "</span>");
```
以上代码通过 htmlText 取得了当前选区中的 HTML，然后将其放在了一对 <span> 标签中，最后又使用 pasteHTML() 将结果重新插入到了选区中。

### 表单与富文本

由于富文本编辑是使用 iframe 而非表单控件实现的，因此从技术上说，富文本编辑器并不属于表单。换句话说，富文本编辑器中的 HTML 不会被自动提交给服务器，而需要我们手工来提取并提交 HTML。为此，通常可以添加一个隐藏的表单字段，让它的值等于从 iframe 中提取出的 HTML。具体来说，就是在提交表单之前，从 iframe 中提取出 HTML，并将其插入到隐藏的字段中。下面就是通过表单的 onsubmit 事件处理程序实现上述操作的代码。

```js
EventUtil.addHandler(form, "submit", function(event){
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);

  target.elements["comments"].value = frames["richedit"].document.body.innerHTML;
});
```
在此，我们通过文档主体的 innerHTML 属性取得了 iframe 中的 HTML，然后将其插入到了名为 "comments" 的表单字段中。这样可以确保恰好在提交表单之前填充 "comments" 字段。如果你想在代码中通过 submit() 来手工提交表单，那么一定不要忘记事先执行上面的操作。对于 contenteditable 元素，也可以执行类似操作。

```js
EventUtil.addHandler(form, "submit", function(event){
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);
  
  target.elements["comments"].value = document.getElementById("richedit").innerHTML;
});
```

## 小结

虽然 HTML 和 Web 应用自诞生以来已经发生了天翻地覆的变化，但 Web 表单相对却没有什么改变。使用 JavaScript 可以增强已有的表单字段，从而创造出新的功能，或者提升表单的易用性。为此，表单、表单字段都引入了相应的属性和方法，以便 JavaScript 使用。下面是本章介绍的几个概念。

- 可以使用一些标准或非标准的方法选择文本框中的全部或部分文本。
- 大多数浏览器都采用了 Firefox 操作选择文本的方式，但 IE 仍然坚持自己的实现。
- 在文本框的内容变化时，可以通过侦听键盘事件以及检测插入的字符，来允许或禁止用户输入某些字符。除 Opera 之外的所有浏览器都支持剪贴板事件，包括 copy 、 cut 和 paste 。其他浏览器在实现剪贴板事件时也可以分为几种不同的情况。
- IE、Firefox、Chrome 和 Safari 允许通过 JavaScript 访问剪贴板中的数据，而 Opera 不允许这种访问方式。
- 即使是 IE、Chrome 和 Safari，它们各自的实现方式也不相同。
- Firefox、Safari 和 Chrome 只允许在 paste 事件发生时读取剪贴板数据，而 IE 没有这个限制。
- Firefox、Safari 和 Chrome 只允许在发生剪贴板事件时访问与剪贴板相关的信息，而 IE 允许在任何时候访问相关信息。

在文本框内容必须限制为某些特定字符的情况下，就可以利用剪贴板事件来屏蔽通过粘贴向文本框中插入内容的操作。

选择框也是经常要通过 JavaScript 来控制的一个表单字段。由于有了 DOM，对选择框的操作比以前要方便多了。添加选项、移除选项、将选项从一个选择框移动到另一个选择框，甚至对选项进行排序等操作，都可以使用标准的 DOM技术来实现。

富文本编辑功能是通过一个包含空 HTML 文档的 iframe 元素来实现的。通过将空文档的 designMode 属性设置为 "on" ，就可以将该页面转换为可编辑状态，此时其表现如同字处理软件。另外，也可以将某个元素设置为 contenteditable 。在默认情况下，可以将字体加粗或者将文本转换为斜体，还可以使用剪贴板。JavaScript 通过使用 execCommand() 方法也可以实现相同的一些功能。另外，使用 queryCommandEnabled() 、 queryCommandState() 和 queryCommandValue() 方法则可以取得有关
文本选区的信息。由于以这种方式构建的富文本编辑器并不是一个表单字段，因此在将其内容提交给服务器之前，必须将 iframe 或 contenteditable 元素中的 HTML 复制到一个表单字段中。