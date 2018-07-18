title: 《HTML5秘籍》学习笔记
author: songxingguo
tags:
  - ''
  - HTML
categories:
  - 前端技术
date: 2018-07-16 11:16:00
---
### 前言

### HTML5简介

- #### HTML5的故事
  
  ![HTML5的故事](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%E7%9A%84%E6%95%85%E4%BA%8B.png)
  
 <!-- more -->
- #### HTML5的三个主要原理

  - 不破坏Web
  
    标准不该引入导致已有网页无法工作的改变。
    
    HTML5规范包括两部分。第一部分，面向Web开人员，要求摒弃过去的那些坏习惯和被摒弃的元素。第二部分,针对的是浏览器开发商，它们需要支持HTML中存在的一切，以做到向后兼容。
    
  - 修补牛蹄子路
  
    牛蹄子路指的是高低不平但使用频率很高的路，通过它可以从一个地方到另一个地方。
  
    HTML5标准化了这些非官方（但广泛应用）的技术。
  
  - 实用至上
  
    改变应该以实用为目的。改变越多，代价也就越大。
    
    HTML标准加入官方的支持，让功能在所有浏览器中都能一致的工作。
    
    让网站不依赖插件也能够提供视频、丰富的交互功能以及各种漂亮的效果。
 
- #### HTML5标记初体验

  - 最简单的HTML5文档
  
    ```
    <!DOCTYPE html>
    <title>A Tiny HTML Document</title>
    <p>Let's rock the browser, HTML5 style.</p>
    ```
   开始是HTML5的文档声明，然后是页面标题和一些内容。在这里，内容是包含在一个段落中的文本。
    
   当然，HTML5允许没有关闭的标签（浏览器知道在文档后面关闭所有没有关闭的标签），HTML5标准也允许你省略 `<title>` 元素。
   
  - 瘦骨嶙峋的HTML5文档
   
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
   
  - 真正实用的HTML5文档
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
    
  - HTML5文档类型
   
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
   
    