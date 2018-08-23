title: 《HTML5秘籍》学习笔记-HTML5简介
author: songxingguo
tags:
  - HTML
categories:
  - 读书笔记
date: 2018-07-16 11:16:00
---
### HTML5简介

- #### HTML5的故事
  
  ![HTML5的故事](http://p9myzkds7.bkt.clouddn.com/HTML5/HTML5%E6%95%85%E4%BA%8B.png)
  
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
  
    **注意：**  使不使用  `<html>` 、 `<head>` 、 `<body>` 元素只代表一种风格。事实上，浏览器会自动假设页面中已经包含了这些元素。
    
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
           <title>A Tiny HTML Document</title>
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
           <title>A Tiny HTML Document</title>
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
  
  - ##### 删除的元素
  
   剔除的少量元素，浏览器仍然可以得到浏览器的支持，但任何遵循规范的HTML5验证器都会敏感地查出它们的藏身之所，并给出错误提示。
   
   HTML5沿袭了表示不欢迎表现性元素的思想（最初萌发于XHTML）。所谓表现性元素，指的是那些仅仅是为网页添加样式的元素。
   
   HTML5埋葬了已经摒弃的HTML框架，`<iframe>` 元素侥幸得以保留，主要是Web应用中经常利用 `<iframe>` 实现一些集成任务，比如在网页中包含YouTube窗口、广告和谷歌搜索框等。
   
   还有另外一些元素，由于冗余或容易导致误会等原因也被剔除了，比如 `<acronym>` （代之为 `<abbr>`）和 `<applet>` （因为 `<object>` 更好）。
   
   完整的元素列表，可参考 http://dev.w3.org/html5/markup/ 。
   
 - ##### 改变的元素
  
   - `<small>` 元素
  
     HTML5将一些旧元素用于新的目的。例如， `<small>` 元素的用途不再是减小文本字体大小，而是表示“附属细则”（small print）,比如页面底部没人想让你看到的那些法律条款。
    
     放在 `<small>` 元素中的文本仍然照常显示，只不过字体稍小一点。
    
     **注意：** 一种看法是认为它最大限度的向后兼容，另一种看法认为它会导致旧网页中相应元素的语义变化。
    
   - `<hr>` 元素
    
     另一个改变的元素是 `<hr>` (horizontal rule, 水平线)，用于在两个区域间画一条线。在HTML5中 `<hr>` 表示主题的转换，即从一个主题变为另一个主题。默认格式还在，只不过又赋予了新的含义。
     
   - `<s>` 元素
  
     不仅仅是给文本加一条删除线，而它现在表示不再准确或不相关的内容。
    
   - 粗体和斜体
    
      HTML中最常用的两个表示粗体和斜体的元素 `<b>` 和 `<i>` 部分被 `<strong>` 和 `<em>` 元素取代。

      其背后的思想是停止从格式（粗体和斜体）的角度来看问题，而是要换成使用具有真实逻辑含义（重要或重音）的元素。

      但 `<b>` 和 `<i>` 这两个标签仍然作为XHTML新引入的两个标签的简写形式存在着。

      下面分别说明：
        - 使用 `<strong>` 表示重要的文本内容，也就是那些需要在周围文本中突出出来的文本。
        - 使用 `<b>` 表示应该用粗体表示的文本，但该文本并不比其他文本重要。比如,关键字、产品名称等所有需要用粗体表示的文本都可以用这个标签。  
        - 使用 `<em>` 表示重读的文本，也就是在朗读的时候要大声读出来。
        - 使用 `<i>` 表示应该用斜体表示的文本，但该文本并不比其他文本更重要。比如，外文单词、技术术语等所需要使用斜体表示的文本都可以用这个标签。
      
  - ##### 调整的元素
   
    - `<address>` 元素
    
      提供HTML文档作者的联系信息，比如邮件地址或网站链接：
      
      ```
      <address>
        <a href="mailto:songxingguo.com">Xingguo Song</a>
      </address>
      ```
    - `<cite>` 元素
     
       引用某些作品，去标注人名已经不对了。
       
       ```
       <cite> HTML5 秘籍 </cite>
       ```
    - `<a>` 元素
     
       HTML以前的版本允许用 `<a>` 元素来标注可以单击的文本或图像。而在HTML5中，可以在 `<a>` 元素中放置任何东西（所有文本都会变成蓝色并带有下划线，而图像则会产生蓝色的边框）。Web浏览器支持这种做法已经有很多年了，HTML5只不过是把这种行为写进了规范。
     
    - `<ol>` 元素
     
        有序列表有了reversed属性，只是目前不是所有浏览器都支持。
        
  - ##### 标准化的元素
   
      - `<embed>` 元素
      
        向页面中加入插件的通用方法。
       
      - `<wbr>` 元素
         
          表示可以在某处换行，如果词太长了，一行放不下，那浏览器就会在<wbr>标注的地方断行（只有一行容不下来了才会断行）。
       
      - `<nobr>` 元素 
         
         HTML5认为 `<nobr>` 已经不合时宜，可以在CSS中使用white-space属性，将它的值设定为nowarp。

- #### 今天开始用HTML5

  - ##### 对付旧版本浏览器
    
    **平稳退化：**
    
       为老的浏览器提供替代内容（比如使用Flash插件的视频播放器）。
       
    **借助JavaScript:**
    
      HTML5中的某些功能完全可以使用优秀的JavaScript库实现（最差的情况，完全通过手工编写JavaScript代码也可以写出来）。

  - ##### 了解浏览器支持情况
    
    在浏览器中打开http://caniuse.com 。
    
    红色代表不支持，浅绿色表示支持，橄榄绿色表示部分支持，灰色表示不确定（一般是因为浏览器的当前版本还在开发过程中，相应功能还没有加入）。
    
    ![HTML5浏览器支持情况](http://p9myzkds7.bkt.clouddn.com/HTML5%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81%E6%83%85%E5%86%B5.png)
    `上图为HTML5浏览器支持情况`
    
  - ##### 浏览器装机情况统计
     
    可以查看流行的流量跟踪站点GlobalStats。
  
    在浏览器中打开http://gs.statcounter.com （国内只有翻墙才能访问）。
     
    打开网站可以看到一个折线图，显示着最近几年常用的浏览器，可以选择不同的条件显示统计不同的数据（比如，可以统计国内的浏览器使用情况）。
    
    ![国内浏览器使用情况](http://p9myzkds7.bkt.clouddn.com/HTML5/%E5%9B%BD%E5%86%85%E6%B5%8F%E8%A7%88%E5%99%A8%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5.png)
    `上图为国内浏览器使用情况`
    
  - ##### 通过Modernizr检测功能
  
    加载页面，通过JavaScript脚本检测某个具体的功能是否可用，然后，可以对用户进行提示（最衰的方法）、平稳退化到没有那么花哨的版本（稍好一些），或者采用其他方法实现浏览器本该支持的HTML5功能（最佳）。
    
    Modernizr是一个小巧、持续更新的工具，专门用于测试浏览器对很多HTML5及相关功能的支持情况。
    
    在刘浏览器中打开 http://modernizr.com/download ，下载完整版Moderniznr，并将js引入到 `<head>` 区块中。
    
  - ##### 使用“腻子脚本”填补功能缺陷
  
    Moderniznr可以帮你找出浏览器支持上的缺陷。它会在某个功能不可用时提醒你，但除此之外不会帮你弥补这些缺陷。而这就是腻子脚本（polyfill）的用途所在。腻子脚本就是一大堆五花八门的技术，目的是填平旧浏览器对HTML5支持上的缺陷。
    
    不过，腻子脚本并不完美。有些脚本依赖的技术同样得不到普遍的支持。
    
     腻子脚本的完整集合，页面地址为：http://tinyurl.com/polyfill 。