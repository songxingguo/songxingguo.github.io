title: 《HTML5秘籍》学习笔记-简介
author: songxingguo
tags:
  - ''
  - HTML
categories:
  - 前端技术
date: 2018-07-16 11:16:00
---
### HTML5简介

- #### HTML5的故事
  
  ![HTML5的故事](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%E7%9A%84%E6%95%85%E4%BA%8B.png)
  
 <!-- more -->
- #### HTML5三个主要原理

  - ##### 不破坏Web
  
    标准不该引入导致已有网页无法工作的改变。
    
    HTML5规范包括两部分。第一部分，面向Web开人员，要求摒弃过去的那些坏习惯和被摒弃的元素。第二部分,针对的是浏览器开发商，它们需要支持HTML中存在的一切，以做到向后兼容。
    
  - ##### 修补牛蹄子路
  
    牛蹄子路指的是高低不平但使用频率很高的路，通过它可以从一个地方到另一个地方。
  
    HTML5标准化了这些非官方（但广泛应用）的技术。
  
  - ##### 实用至上
  
    改变应该以实用为目的。改变越多，代价也就越大。
    
    HTML标准加入官方的支持，让功能在所有浏览器中都能一致的工作。
    
    让网站不依赖插件也能够提供视频、丰富的交互功能以及各种漂亮的效果。
 
- #### HTML5标记初体验

  - ##### 最简单的H5文档
  
    ```
    <!DOCTYPE html>
    <title>A Tiny HTML Document</title>
    <p>Let's rock the browser, HTML5 style.</p>
    ```
   开始是HTML5的文档声明，然后是页面标题和一些内容。在这里，内容是包含在一个段落中的文本。
    
   当然，HTML5允许没有关闭的标签（浏览器知道在文档后面关闭所有没有关闭的标签），HTML5标准也允许你省略 `<title>` 元素。
   
  - ##### 瘦骨嶙峋的H5文档
   
   ```
    <!DOCTYPE html>
    <head>
      <title>A Tiny HTML Document</title>
    </head>
    <body>
      <p>Let's rock the browser, HTML5 style.</p>
    </body>
    ```
    使用<head>和<body>来将关于页面的信息（头部）与页面的实际内容（主体）分开。在为页面添加脚本、样式表和元数据的时候，这种结构特别实用。
   
  - ##### 真正实用的H5文档
       ```
    <!DOCTYPE html>
    <html>
      <head>
        <title>A Tiny HTML Document</title>
      </head>
      <body>
        <p>Let's rock the browser, HTML5 style.</p>
      </body>
    </html>
    ```
    最后，选择用<html>元素来封装整个文档（不包含文档类型声明那一行）。
  
    注意： 使不使用  `<html>` 、 `<head>` 、 `<body>` 元素只代表一种风格。事实上，浏览器会自动假设页面中已经包含了这些元素。
    
  - ##### HTML5文档类型
   
    每个HTML5文档的第一行都必须是一个特定的文档类型声明。这个文档类型声明用于宣告后面的文档标记遵循那个标准。
   
    文档标记遵循HTML5标准：
    
     ```
     <!DOCTYPE html>
     ```
     XHTML1.0严格型的文档类型声明：
     ```
     <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
     ```
     HTML5的文档声明不包括官方规范的版本号。事实上，这个声明只表示当前页面是HTML页面。这与HTML5是一门活着的语言的远见是分不开的。换句话说，只要有新功能添加到HTML语言中，你在页面中都可以使用它们，而不必为此修改文档类型声明。

     要求保留文档类型声明，主要是由于历史原因。如果没有文档类型声明，那大多数浏览器将转换到一种混杂模式（quirk mode）。

     HTML5文档类型声明是最短的有效文档类型声明，因此它总能触发标准模式。

     虽然文档类型声明主要目的是告诉浏览器去做什么，但其他代理也可以检测该声明。比如： HTML5验证器、搜索引擎、设计工具，还有人-在想知道你当初在页面中想写什么样的标记时。

  - ##### 字符编码

     字符编码是一种标准，计算机根据它把文本转换成保存在文档中的字节序列（或者在打开文件时将字节序列转换成文本形式）。由于历史原因，现在的编码标准有很多种。

     但实际上英文网站今天都使用一种叫UTF-8的编码，这种编码简洁、转换速度快，而且支持任何你想要的非英文字符。

     一般来说，经过配置的Web服务器会告诉浏览器它提供的网页采用了什么编码。

     在HTML5文档中添加字符编码信息，只要在 `<head>` 区块的最开始处（如果没有添加 `<head>` 元素，则是紧跟在文档类型声明之后）添加相应的 `<meta>` 元素。

     ```
     <head>
       <meta charset="utf-8">
       <title>A Tiny HTML Document/title>
     </head>
     ```
  - ##### 页面语言

     指明网页中使用的自然语言是一种好习惯。有时候对搜索引擎可以通过它来筛选结果。
     给内容指定语言，可以在任何元素上使用lang属性，并为该属性指定相应语言的语言代码（比如， en表示英语）。各国语言代码可以在这里查到：http://people.w3.org/tishida/utils/subtags/ 。

     为整个页面添加语言说明的最简单方式，就是为 `<html>` 元素指定lang属性：

     ```
     <html lang="en">
     ```
     如果页面中包含多种语言的文本，可以为文本中不同的区块指定lang属性，指定 该区块中文本的语言。 

  - ##### 添加样式表

     指定想要使用的CSS样式表时，需要在HTML5文档的 `<head>` 区块中添加 `<link>` 元素， 例如： 

     ```
     <head>
       <meta charset="utf-8">
       <title>A Tiny HTML Document/title>
       <link href="styles.css" rel="stylesheet">
     </head>
     ```
     CSS是网页中唯一可用的样式表语言，所以网页中过去要求的type="text/css"属性就没有必要了。

  - ##### 添加JavaScript

     下面就是一个引入外部JavaScript代码的文件示例：

      ```
       <head>
         <meta charset="utf-8">
         <title>A Tiny HTML Document/title>
         <script src="script.js"></script>
       </head>
       ```
     没有必要加上language="JavaScript"属性，浏览器会假定你想要使用JavaScript，除非你想使用其他脚本语言。

     即使是引用外部JavaScript文件，也不能忘了后面的 `</script>` 标签。假如你不写这个标签或者使用空元素语法想缩短标记，页面将不会执行加载脚本。

     如果你在Inernet Explorer中花大量的时间测试包含JavaScript页面，还应该在 `<head>` 区块中包含一行特殊的注释，叫做Web标志（mark of the web）；这行注释要放在指定字符编码的元数据便签后面，如下所示：

     ```
       <head>
         <meta charset="utf-8">
         <!-- saved from url=(0014)about:internet -->
         <title>A Tiny HTML Document/title>
         <script src="script.js"></script>
       </head>
       ```
     这条注释告诉Internet Explorer将页面视为从远程网站上下载下来的。否则，IE会切换到一种特殊的锁定模式，弹出一条安全警告，在你点了“允许阻止内容”按钮后才会执行JavaScript代码。

     其他浏览器都会忽略这个“Web标志”注释，对远程站点和本地文件使用相同的安全设置。  

  - ##### 最终的HTML5文档

      ```
      <!DOCTYPE html>
      <html lang="en">    
        <head>
           <meta charset="utf-8">
           <title>A Tiny HTML Document/title>
           <link href="styles.css" rel="stylesheet">
           <script src="script.js"></script>
         </head>
         <body>
           <p>Let's rock the browser, HTML5 style.</p>
         </body>
      </html>
      ```
     虽然这不再是一个最短的HTML5文档，但以它为基础可以构建出任何网页。
 
- #### HTML5语法
 
 - ##### 放松的规则
 
   HTML5不区分大小写，因此类似下面这样的标记是没有问题的：
      ```
      <P>Capital and lowercase letters <EM> don't metter</eM> in tag names.</p>
      ```
   HTML5还允许省略关闭空元素（void element）的斜杆；所谓空元素，就是不会嵌套内容的元素，如`<img>`(图像）、 `<br>`(换行）或 `<hr>`(水平线）。以下是三中添加换行的等价方式：
     ```
     I cannot <br />
     move backward<br>
     or forward.<br/>
     I am caught
     ```
   HTML5也修改了属性语法规则。属性值中只要不包含受限的字符（比如>、=或空格），就可以不加引号。下面这个 `<img>` 元素就利用了这点：
     ```
     <img alt="Horsehead Nebula" src=Horsehead01.jpg>
     ```
   只有属性名没有属性值也可以。虽然XHTML要求必须采用如下冗余的语法将复选框设置为选中状态：
     ```
     <input type="checkbox" checked="checked" />
     ```
   但现在可以只包含属性名，回到HTML4.01时代的传统短语法形式：
     ```
     <input type="checkbox" checked>
     ```
   如果能做到如下几点（也是尽管不是必须遵循的）， 基本上就可以算作良好的HTML5风格了。
     - 包含可选的 `<html>`、 `<body>` 和 `<head>`元素。要给页面定义自然语言`<html>`是最理想的地方；而 `<body>` 和 `<head>` 有助于将页面内容与其他页面信息分离。

     - 标签全部小写（如用 `<p>` 而非 `<P>`）。虽然不是必须这么做，但是这种形式很常见，输入起来要轻松容易很多，而且并不会让人触目惊心。

     - 为属性值加引号。加引号是为了防止不经意间犯错。要知道，没有引号的话，一个无效字符就能破坏整个页面。

     - 不加斜杆、使用短语法属性。
  
  - ##### HTML5验证

    以下是HTML5验证器会关注的一些可能的问题：

       - 缺少必要元素（例如 `<title>` 元素）；
       - 有开始标签但没有结束标签；
       - 标签嵌套又错误；
       - 不包含必要属性的标签（例如没有src属性的 `<img>` 元素）；
       - 元素或内容放错了地方（例如把文本直接放在了`<head>` 区块中）。
      
    可以使用在线验证工具，下面是W3C标准组织提供的流行的验证器，在浏览器中，打开http://validator.w3.org   
      
  - ##### XHTML的回归

    XHTML 强制要求的规则要么仍具有指导意义（例如，元素要正确嵌套），要么仍然是一种得到支持的可选约定（例如，空元素可以包含结束的斜杆）。

    将一个HTML5文档转换成XHTMl5文档（器本质是给HTML5加上了XML的限制），必须在 `<html>` 元素中明确添加XHTML命名空间、关闭每个元素、所有标签都要小写等。下面就是一个XHTML5文档示例：

      ```
      <!DOCTYPE html>
      <html lang="en"  xmlns="htttp://www.w3.org/1999/xhtml">    
        <head>
           <meta charset="utf-8">
           <title>A Tiny HTML Document/title>
           <link href="styles.css" rel="stylesheet">
           <script src="script.js"></script>
         </head>
         <body>
           <p>Let's rock the browser, HTML5 style.</p>
         </body>
      </html>
      ```
    可以使用 http://validator.w3.org/nu 进行验证。
   
- #### HTML5元素家族
 
  - ##### 新增的元素
  
    ![新增元素](http://p9myzkds7.bkt.clouddn.com/HTML5/%E6%96%B0%E5%A2%9E%E5%85%83%E7%B4%A0.jpg)
  
  - ##### 删除元素
  
   剔除的少量元素，浏览器仍然可以得到浏览器的支持，但任何遵循规范的HTML5验证器都会敏感地查出它们的藏身之所，并给出错误提示。
   
   HTML5沿袭了表示不欢迎表现性元素的思想（最初萌发于XHTML）。所谓表现性元素，指的是那些仅仅是为网页添加样式的元素。
   
   HTML5埋葬了已经摒弃的HTML框架，`<iframe>` 元素侥幸得以保留，主要是Web应用中经常利用 `<iframe>` 实现一些集成任务，比如在网页中包含YouTube窗口、广告和谷歌搜索框等。
   
   还有另外一些元素，由于冗余或容易导致误会等原因也被剔除了，比如 `<acronym>` （代之为 `<abbr>`）和 `<applet>` （因为 `<object>` 更好）。
   
   完整的元素列表，可参考 http://dev.w3.org/html5/markup/ 。
   
  - ##### 改变元素
   
    
  