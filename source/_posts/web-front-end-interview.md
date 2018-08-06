title: Web前端面试题目及详解汇总-HTML/CSS部分
author: songxingguo
tags: []
categories:
  - 找工作
date: 2018-08-04 03:02:00
---
## HTML/CSS部分

### 什么是盒模型

 > 在网页中，一个元素占有空间的大小构成，采用盒子作一个比喻来理解。

 - **盒模型的组成：** 由里向外由 **content** 、**padding**、 **border** 、**margin** 四部分组成。
 
 - **盒模型的两种标准：**  标准模型，IE模型。
 
 <!-- more -->
 
   - 两种标准的区别：
 
     在标准模型中，盒模型的 **宽高** 只是 **内容（content）的宽高** 。

     ![标准模型](http://p9myzkds7.bkt.clouddn.com/web-interview/%E6%A0%87%E5%87%86%E6%A8%A1%E5%9E%8B.png)

     在IE模型中盒模型的 **宽高** 是 **内容(content)+填充(padding)+边框(border)的总宽高** 。

     ![IE模型](http://p9myzkds7.bkt.clouddn.com/web-interview/IE%E6%A8%A1%E5%9E%8B.png)
 
   - CSS如何设置两种模型
  
     > 设置CSS3 的属性 box-sizing
     
     ```
     /* 标准模型 */
     box-sizing: content-box;

     /*IE模型*/
     box-sizing: border-box;
     
     /*继承父元素 box-sizing 属性的值*/
     box-sizing: inherit;
     ```

 - **边距重叠：** 两个 **相邻元素** ，如果 **上面的元素** 设置了 **margin-bottom** ，而 **下面的元素** 设置了 **margin-top** ，那么两者的 **margin将会重叠** ，并且将 **使用较大的margin作为最终的边距** 。
 
   - 避免方式：只设置 **margin-top** 或 **margin-bottom**  ，而不是将两者混合使用。
 
来自—— **[深入理解CSS盒模型](https://www.cnblogs.com/chengzp/p/cssbox.html)**
 
### HTML元素

  
  - **行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？**
  
  > 行内元素、块级元素、空(void)元素

    - **行内元素：** a、b、span、img、input、strong、select、label、em、button、textarea 。

    - **块级元素：** div、ul、li、dl、dt、dd、p、h1-h6、blockquote 。

    - **空元素：** 即没有内容的HTML元素，例如：br、meta、hr、link、input、img 。
  
  - **行内元素和块元素**
  
    - 两者的区别
    
      - 块级元素

        > **独占一行**, 垂直方向排列；默认情况下，其 **宽度自动填满其父元素宽度** ；块级元素 **可以设置width、height属性** ；**可以设置margin和padding属性** 。

        *注意，块级元素即使设置了宽度，仍然是独占一行。*

      - 行内元素

        > **不会独占一行** ，水平方向排列，相邻元素排列在同一行，直到排不下才会换行；其宽度  **随元素的内容而变化** ；行内元素 **设置width、height属性无效**（可以设置 **line-height** ） ；行内元素的  **margin** 和 **padding** 属性，**水平方向** 的 **padding-left** 、**padding-right** 、**margin-left** 、**margin-right** 都 **产生效果** ，但 **竖直方向** 的 **padding-top** 、**padding-bottom** 、**margin-top** 、**margin-bottom** 却 **不会产生边距效果** 。
     
    - CSS相关属性display
     
       ```
       /*块级元素*/
       display: block;
       
       /*行内元素*/
       display: inline;
       
       /*行内的块级元素,集块元素和行内元素的特点于一身*/
       display: inline-block;
       ```
       > 为兼容 IE，真正能用的 display 类型只有 **block** 、**inline** 和  **none** 三种。可以使用一种 **hack** - 触发 **行内元素** 的 **hasLayout** （触发hasLayout，就会让行内元素拥有块级元素的特性），来支持 **display: inline-block** ，然后通过 **vertical-aligin** 解决竖直对齐问题。
       ```
       display:inline-block;
       *display:inline;
       vertical-aligin: middle;
       ```

      *只能对行内元素实现 **display:inline-block**,而块级元素就不行。*
       
来自—— **《编写高质量代码：Web前端开发修炼之道》曹刘阳 （4.7.6 块级元素和行内元素的区别、4.7.7 display:inlne-block和hasLayout）**

### CSS实现居中

  > CSS的居中会遇到很多种情况，不同的情况使用的方法不同。

  - #### 水平居中

    - 文本、图片等 **行内元素** 的水平居中
    
     > 给 **父元素** 设置 **text-align：center** 。
     
     ```
     .parent  {
       text-align：center;
     }
     
     img {}
     ```
    - **确定宽度** 的 **块级元素** 的水平居中
    
     > **确定宽度的块级元素本身** 通过设置 **margin-left:auto** 和 **margin-right: auto** 来实现。
     
     ```
     .self {
       width: 200px;
       margin-left: auto;
       margin-right: auto；
     }
     ```
     或
     ```
     .self {
       width: 200px;
       margin: 0 auto;
     }
     ```
    - **不确定宽度** 的 **块级元素** 的水平居中
    
      > 不确定宽度的块级元素有三种方式可以实现居中。
    
      - 方法一： 使用 `table` 标签来实现不确定宽度的块级元素的水平居中
      
        > `table` 它本身并 **不是块级元素** ，如果不给它设定宽度的话，**它的宽度由内部元素的宽度“撑起”** ，但即使不设定它的宽度，仅 **对 `table`设置margin-left:auto 和 margin-right: auto就可以使 `table` 水平居中** ， **间接实现    `table` 内部元素的居中** 。
        
        ```
        table {
          margin: 0 auto;
        }
        ```
        *这种做法很巧妙，但 **缺点** 是 **增加了无语义标签** ，**加深了标签的嵌套层数** 。*
        
      - 方法二： 改变块级元素的 **display** 为 **inline类型**，然后使用 **text-aligin:center** 来实现居中。
      
        ```
         .parent {
           text-aligin: center;
         }

         .self {
           display: inline;
         }
        ```
        *它也 **存在一定问题** ：它将块级元素变成可行内元素，而 **行内元素比起块级元素少一些功能** ，比如设定长宽值，在某些特殊需求设置CSS设置中，这种方法可能会有一些限制。*
        
      - 方法三：通过给 **父元素** 设置 **float** ，然后 **父元素** 设置**position：relative** 和 **left:50%** , **子元素** 设置 **positionrelative** 和 **left:-50%** ;来实现水平居中。
      
         ```
         .parent {
           position: relative;
           left: 50%;
         }

         .self {
           position: relattive;
           left: -50%
         }
         ```
        *保留了块级元素，而且不会添加无语义标签，不增加嵌套深度，但它的 **缺点** 是设置了 **position:relative** ,带来了 **一定副作用** 。*
     
    - Flex

      可查看站内文章—— **Flex布局**
      
  - #### 竖直居中
  
    - **父元素高度不确定** 的 **文本** 、**图片** 、**块级元素** 的竖直居中
    
      > 通过给 **父容器** 设置 **相同的上下边距** 实现的。
      
      ```
      .parent {
        padding-top: 20px;
        padding-bottom: 20px;
      }
      ```
      或
      ```
      .parent {
        padding: 20px 0;
      }
      ```
    - **父元素高度确定** 的 **单行文本** 的竖直居中
    	
       > 通过给 **父元素** 设置 **line-height** 来实现的，**line-height值和父元素的高度值相同** 。
       
       ```
       parent {
         height: 100px;
         line-height: 100px;
       }
       ```
    - **父元素高度确定** 的 **多行文本** 、**图片** 、**块级元素的竖直居中**
      > 父元素高度确定的的多行文本、图片、块级元素的竖直居中有两种方法。

      - 方法一：使用用于垂直居中的 **属性vertical-align** ,但只有当 **父元素为td或th** 时，这个 **vertical-align属性才会生效** 。

        Fire和IE8及以上版本可以 设置块级元素的 **display** 类型为 **table-cell** ， **激活vertical-align属性** ，但是IE6和IE7并不支持dispaly:table-cell。
        > 虽然可以设置display:table-cell来模拟表格，但没办法跨浏览器兼容，那么就 **直接使用表格** 来实现。

        ```
        table {}

        .self {
          vertical-align: middle;
        }
        ```
        *方法一可以很好地实现竖直居中效果，且不会带来任何样式上副作用，但是它 **添加了无语义标签** ，并 **增加了嵌套深度** 。*
        
      - 方法二： 对支持display: table-cell的IE8和Firefox用dispaly:table-cell和vertical-align: middle来实现居中，对不支持display:table-cell的IE6和IE7，使用特定格式的hack。
      
        > 给不支持display:table-cell的IE6和IE7，通过给 **父子两层元素** 分别设置 **top:50%** 和 **top:-50%**来实现居中。
      
        ```
        .parent {
          display: table-cell;
          vertival-align: middle;
          *position: relative;
        }

        .vertical-parent {
          *position: absolute;
          *top:50%;
        }

        .vertical-self {
          *position: relative;
          *top:-50%;
        }
        ```
        *这种方法的好处是没有增加额外的标签，但它的 **缺点** 也很明显，一方面它 **使用了hack** ，**不利于维护** ，另一方面，它 **需要设置position：relative和posion：absolute** , **带来了副作用** 。*
     
    - Flex

      可查看站内文章—— **Flex布局**
         
来自—— **《编写高质量代码：Web前端开发修炼之道》曹刘阳 （4.7.9 居中）**


### 简述一下 href、src、rel、url 的区别

- **href**

  > **href** 是 **Hypertext Reference** 的缩写，表示 **超文本引用** 。用来建立 **当前元素** 和 **文档** 之间的链接，指向 **网络资源所在位置** 。
  
  常用的有：link、a。例如：
  
  ```
  <link href="reset.css" rel=”stylesheet“/>
  ```
  *浏览器会识别该文档为css文档，**并行下载该文档** ，并且 **不会停止对当前文档的处理** 。这也是建议使用link，而不采用@import加载css的原因。*
  
- **src**

  > **src** 是 **source** 的缩写，**src** 的内容是页面必不可少的一部分，是 **引入** 。**src** 指向的 **内容会嵌入到文档中当前标签所在的位置** ,指向 **外部资源的位置** 。
  
  常用的有：img、script、iframe。例如；
  
  ```
  <script src="script.js"></script>
  ```
  *当浏览器解析到该元素时，会 **暂停浏览器的渲染** ， **直到该资源加载完毕** 。这也是将js脚本放在底部而不是头部得原因。*

简而言之，**src** 用于 **替换当前元素** ；**href** 用于 **在当前文档和引用资源之间建立联系** 。也就是说src引用的路径是 **img自己的路径** ，href引用的路径是要 **跳转到的地方** 。

- **rel**

  > **rel** 是 **relationship** 的缩写。用于 **定义链接的文件和HTML文档之间的关系** 。**StyleSheet** 的意思就是 **样式调用** 。**设置或检索对象之间的链接目标的关系** 。如果没有值指出，rel属性的 **默认关系** 是一个 **空字符串** 。使用此属性 仅当 **href属性** 应用。
  
- **url**

  > **统一资源定位符** 是 对可以从互联网上得到的 **资源的位置** 和 **访问方法** 的一种简洁的表示，是 **互联网上标准资源的地址** 。互联网上的每个文件都有一个 **唯一** 的 **URL** ，它包含的信息指出 **文件的位置** 以及 **浏览器应该怎么处理它** 。
  
  基本URL包含 **模式**（或称 **协议** ）、 **服务器名称**（或 **IP地址** ）、 **路径** 和**文件名** 。
  
来自—— [HTML中href、src区别]、[rel、href、src、url的区别]

### 什么是CSS Hack?

> 针对不同的浏览器 /不同版本写相应的 **CSS** 的过程,就是 **CSS Hack** 。

**CSS Hack常见的有三种形式：**  **CSS属性Hack** 、 **CSS选择符Hack** 以及 **IE条件注释Hack** ， Hack主要针对IE浏览器。


- CSS属性Hack

  ```
  #test{
      color:#c30; /* For Firefox */
      color:red\0; /* For Opera */
      color:yellow\9; /* For IE8+ */
      *color:blue; /* For IE7 */
      _color:#ccc; /* For IE6 */
  }
  ```
- CSS选择符Hack
 
   ```
   * html .test{color:#090;}       /* For IE6 and earlier */
   * + html .test{color:#ff0;}     /* For IE7 */
   .test:lang(zh-cn){color:#f00;}  /* For IE8+ and not IE */
   .test:nth-child(1){color:#0ff;} /* For IE9+ and not IE */
   ```
 
- IE条件注释Hack
  ```
  <!--[if <keywords>? IE <version>?]>
      HTML代码块
  <![endif]-->
  ```

来自—— [史上最全的CSS hack方式一览]、[CSS hack大全]、[今天才知道css hack是什么]、[CSS Hack是什么意思？css hack有什么用？]、[你想知道的css hack知识全都帮你整理好了]、[CSS参考手册]

### 简述同步和异步的区别

> 同步是阻塞模式，异步是非阻塞模式。

- 同步
  
  > 使用者通过 **单个线程** 调用服务；该线程发送请求，在服务 **运行时阻塞**，并且 **等待响应** 。
 
  同步就是指一个进程在执行某个请求的时候，若该请求需要一段时间才能返回信息，那么这个 **进程将会一直等待** 下去，直到收到 **返回信息才继续执行下去** 。

- 异步
  
  > 使用者通过 **两个线程** 调用服务；一个线程 **发送请求** ，而另一个单独的线程 **接收响应** 。

  异步是指 **进程不需要一直等下去** ，而是 **继续执行下面的操作** ，不管其他进程的状态。当有 **消息返回时系统会通知进程进行处理** ，这样可以提高执行的效率。
  
来自—— [同步请求和异步请求的区别]、[简述同步和异步的区别]、[阻塞与非阻塞的区别]

### px、em和rem的区别

- px 

  > **px** 又称 **像素** （Pixel），**相对长度单位** 。像素 **px** 是 **相对于显示屏幕分辨率** 而言的。

- em
 
  > **em**，**相对长度单位** 。**em** 相对于 **当前对象内文本的字体尺寸** 。如当前对行内文本的 **字体尺寸未被人为设置** ，则 **相对于** 浏览器的 **默认字体尺寸**（16px）。
  
  *任意浏览器的默认字体高都是16px。所有未经调整的浏览器豆腐额：**1em=16px** 。但 **em** 的 **值并不是固定的** ；**em** 会 **继承父级元素** 的 **字体大小** 。*
  
- rem

  > **rem** 是 **root em** 的缩写（即 **根em**），**CSS3** 新增的一个 **相对单位**。区别在于使用 **rem** 为元素设定字体大小时，仍然是相对大小，但 **相对的是 HTML 根元素** （ [HTML文档的根元素是 html 元素] ）。
  
  *这个单位可谓集 **相对大小** 和 **绝对大小** 的 **优点** 于一身，通过它既可以做到 **只修改根元素** 就 **成比例地调整所有字体大小** ，又 **可以避免字体大小逐层复合的连锁反应** 。*
  
  目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于 **不支持** 它的 **浏览器** ，应对方法也很简单，就是 **多写一个绝对单位的声明** 。
  
  ```
  p {
    font-size: 14px;
    font-size: 875rem;
  }
  ```
 
如果你的用户群都是用 **最新版的浏览器** ，那 **推荐使用rem** ，如果要 **考虑兼容性**，那就 **使用px** ，或者 **两者同时使用** 。
 
[px,em,rem单位转换工具]

来自—— [彻底弄懂px,em和rem的区别]

### 什么叫优雅降级和渐进增强？

- **渐进增强**（progressive enhancement）：

  > 针对 **低版本浏览器** 进行 **构建页面** ，保证 **最基本** 的功能，然后再针对 **高级浏览器** 进行 **效果** 、**交互** 等 **改进** 和 **追加功能** 达到 **更好的用户体验** 。

- **优雅降级**（graceful degradation）：

  > 一开始就构建 **完整** 的 **功能** ，然后再针对 **低版本浏览器** 进行 **兼容** 。

 ---
- **渐进增强和优雅降级的区别：**

   1. **优雅降级** 是从 **复杂** 的现状 **开始** ，并试图 **减少用户体验** 的供给。

   2. **渐进增强** 则是从一个 **非常基础** 的，**能够起作用** 的 **版本** 开始，并 **不断扩充** ，以 **适应** 未来环境的 **需要** 。

   3.  **降级**（功能衰减）意味着 **往回看** ；而 **渐进增强** 则意味着 **朝前看** ，同时 **保证其根基处于安全地带** 。

来自—— [什么叫优雅降级和渐进增强？]

### 浏览器的内核分别是什么?

> 浏览器的内核是分为两个部分的，一是 **渲染引擎** (layout engineer或Rendering Engine)，另一个是 **JS引擎** 。现在JS引擎比较独立，内核更加倾向于说 **渲染引擎** 。

**渲染引擎**

负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

**JS引擎则** 

 解析和执行javascript来实现网页的动态效果。最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。

- 主流浏览器内核及其内核

  ![主流浏览器内核]

- 五大流浏览器内核及其代表

  - Trident内核

    > 代表作品是 **IE** ，因 **IE捆绑在Windows中** ，所以占有极高的份额，又称为**IE内核** 或 **MSHTML** ，此内核 **只能用于 Windows 平台** ，且 **不是开源的** 。代表作品还有 **腾讯**、**Maxthon**（遨游）、**360浏览器** 等。存在很多的兼容性问题。

  - Gecko内核

    > 代表作品是 **Firefox** （即火狐浏览器）。因火狐是最多的用户，故常被称为  **firefox** 内核,它是 **开源的** ，最大优势是 **跨平台**，在 **Microsoft Windows** 、**Linux** 、**MacOs X** 等主要操作系统中使用。

  - Webkit内核

    > 代表作品是 **Safari**、曾经的 **Chrome**。开源的项目。

  - Presto内核：

    > 代表作品是 **Opera** ，**Presto** 是由 **Opera Software** 开发的 **浏览器排版引擎** ，它是世界公认 **最快的渲染速度的引擎** 。在13年之后，Opera宣布加入谷歌阵营，弃用了 **Presto** 。

  - Blink内核

    > 由 **Google** 和 **Opera Software** 开发的 **浏览器排版引擎** ，2013年4月发布。现在 **Chrome** 内核是 **Blink** 。谷歌还开发了自己的 **JS引擎** ，**V8** ，**使JS运行速度极大地提高了** 。
   
来自—— [浏览器的内核分别是什么？]、[五大流浏览器内核及其代表]

### sessionStorage 、localStorage 和 cookie 之间的区别

> **sessionStorage** 、**localStorage** 和 **cookie** 都用于浏览器本地存储。

- sessionStorage

  > **HTML5 Web Storage API** 提供；可以方便的在 **Web** 请求之间 **保存数据** ；将数据保存在 **session** 对象中。所谓 **session** ，是指用户在 **浏览某个网站** 时，从 **进入网站** 到 **浏览器关闭** 所 **经过的这段时间** ，也就是用户 **浏览这个网站所花费的时间** 。**session** 对象可以用来 **保存在这段时间内所要求保存的任何数据** 。**sessionStorage** 为 **临时保存** 。
  
   **sessionStorage** 的 **生命周期** 是在仅在 **当前会话** 下有效。**sessionStorage** 的概念很特别，引入了一个 **“浏览器窗口”**的概念。**sessionStorage** 是在 **同源的同窗口**（或 **tab** ）中，**始终存在的数据** 。也就是说只要这个 **浏览器窗口没有关闭** ，即使 **刷新页面** 或 **进入同源另一页面** ，**数据仍然存在** 。**关闭窗口** 后，**sessionStorage** 即 **被销毁** 。同时 **“独立”** 打开的 **不同窗口**，即使是 **同一页面** ，**sessionStorage** 对象也是 **不同的** 。

- localStorage

  > 将数据保存在 **客户端本地的硬件设备** (通常指 **硬盘** ，也可以是 **其他硬件设备** )中，即使 **浏览器被关闭** 了，该 **数据仍然存在** ，**下次打开浏览器** 访问网站时 **仍然可以继续使用** 。**localStorage** 为 **永久保存** 。**localStorage** 的 **生命周期**是 **永久的** ，**关闭页面或浏览器** 之后**localStorage** 中的数据也 **不会消失** 。**localStorage** 除非 **主动删除数据** ，否则 **数据永远不会消失** 。

- cookie

  > **HTML5之前的本地存储** ;浏览器的 **缓存机制** 提供了可以将 **用户数据** 存储在 **客户端上的方式** ，可以利用 **cookie** , **session** 等跟 **服务端** 进行 **数据交互** 。

- session

  ![cookie、sessionStorage与lacalStrage的区别]
  
共同点：都是 **保存在浏览器端** ，且 **同源的** 。

区别：cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递；cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。

而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。

来自—— [cookies、sessionStorage和localStorage解释及区别]

### session和cookie之间的区别


### Web Storage与Cookie相比存在的优势

- 存储空间更大：cookie为4KB，而WebStorage是5MB；

- 节省网络流量：WebStorage不会传送到服务器，存储在本地的数据可以直接获取，也不会像cookie一样美词请求都会传送到服务器，所以减少了客户端和服务器端的交互，节省了网络流量；

- 对于那种只需要在用户浏览一组页面期间保存而关闭浏览器后就可以丢弃的数据，sessionStorage会非常方便；

- 快速显示：有的数据存储在WebStorage上，再加上浏览器本身的缓存。获取数据时可以从本地获取会比从服务器端获取快得多，所以速度更快；

- 安全性：WebStorage不会随着HTTP header发送到服务器端，所以安全性相对于cookie来说比较高一些，不会担心截获，但是仍然存在伪造问题；

- WebStorage提供了一些方法，数据操作比cookie方便；

   - setItem (key, value) ——  保存数据，以键值对的方式储存信息。
   - getItem (key) ——  获取数据，将键值传入，即可获取到对应的value值。

   - removeItem (key) ——  删除单个数据，根据键值移除对应的信息。

   - clear () ——  删除所有的数据

   - key (index) —— 获取某个索引的key
  
- 独立的存储空间：每个域（包括子域）有独立的存储空间，各个存储空间是完全独立的，因此不会造成数据混乱。  

来自—— [cookies、sessionStorage和localStorage解释及区别]


#### Ajax的优缺点及工作原理？

> **Ajax** 是 **Asynchronous Javascript And XML** （异步 JavaScript 和 XML）的缩写，是指一种创建交互式网页应用的网页开发技术。

- Ajax = 异步 JavaScript 和 XML（标准通用标记语言的子集）。
- Ajax 是一种用于创建快速动态网页的技术。
- Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。 [1] 
- 通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
- 传统的网页（不使用 Ajax）如果需要更新内容，必须重载整个网页页面。

*注意：Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。*

传统的Web应用交互由用户触发一个HTTP请求到服务器,服务器对其进行处理后再返回一个新的HTHL页到客户端, 每当服务器处理客户端提交的请求时,客户都只能空闲等待,并且哪怕只是一次很小的交互、只需从服务器端得到很简单的一个数据,都要返回一个完整的HTML页,而用户每次都要浪费时间和带宽去重新读取整个页面。这个做法浪费了许多带宽，由于每次应用的交互都需要向服务器发送请求，应用的响应时间就依赖于服务器的响应时间。这导致了用户界面的响应比本地应用慢得多。

与此不同，AJAX应用可以仅向服务器发送并取回必需的数据，它使用SOAP或其它一些基于XML的Web Service接口，并在客户端采用JavaScript处理来自服务器的响应。因为在服务器和浏览器之间交换的数据大量减少，结果我们就能看到响应更快的应用。同时很多的处理工作可以在发出请求的客户端机器上完成，所以Web服务器的处理时间也减少了。

优点：
1.减轻服务器的负担,按需取数据,最大程度的减少冗余请求

2.局部刷新页面,减少用户心理和实际的等待时间,带来更好的用户体验

3.基于xml标准化,并被广泛支持,不需安装插件等,进一步促进页面和数据的分离

缺点：
1.AJAX大量的使用了javascript和ajax引擎,这些取决于浏览器的支持.在编写的时候考虑对浏览器的兼容性.

2.AJAX只是局部刷新,所以页面的后退按钮是没有用的.

3.对流媒体还有移动设备的支持不是太好等

 ![AJAX工作原理]
 
 1.创建ajax对象（XMLHttpRequest/ActiveXObject(Microsoft.XMLHttp)）

2.判断数据传输方式(GET/POST)

3.打开链接 open()

4.发送 send()

5.当ajax对象完成第四步（onreadystatechange）数据接收完成，判断http响应状态（status）200-300之间或者304（缓存）执行回调函数。

AJAX的工作原理
Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。并不是所有的用户请求都提交给服务器,像—些数据验证和数据处理等都交给Ajax引擎自己来做, 只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。
Ajax其核心有JavaScript、XMLHTTPRequest、DOM对象组成，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。让我们来了解这几个对象。
(1).XMLHTTPRequest对象
Ajax的一个最大的特点是无需刷新页面便可向服务器传输或读写数据(又称无刷新更新页面),这一特点主要得益于XMLHTTP组件XMLHTTPRequest对象。
XMLHttpRequest 对象方法描述 

(2).JavaScript
JavaScript是一在浏览器中大量使用的编程语言。

(3).DOM Document Object Model
DOM是给HTML和XML文件使用的一组API。它提供了文件的结构表述，让你可以改变其中的內容及可见物。其本质是建立网页与Script或程序语言沟通的桥梁。所有WEB开发人员可操作及建立文件的属性、方法及事件都以对象来展现（例如，document就代表“文件本身“这个对像，table对象则代表HTML的表格对象等等）。这些对象可以由当今大多数的浏览器以Script来取用。一个用HTML或XHTML构建的网页也可以看作是一组结构化的数据，这些数据被封在DOM（Document Object Model）中，DOM提供了网页中各个对象的读写的支持。
(4).XML
可扩展的标记语言（Extensible Markup Language）具有一种开放的、可扩展的、可自描述的语言结构，它已经成为网上数据和文档传输的标准,用于其他应用程序交换数据 。
 
 来自—— [AJAX 简介]、[Ajax的优缺点及工作原理？]、[AJAX工作原理及其优缺点]
 
 ### 请指出document load和document ready的区别？
 
 共同点：这两种事件都代表的是页面文档加载时触发。

异同点：

ready 事件的触发，表示文档结构已经加载完成（不包含图片等非文字媒体文件）。

onload 事件的触发，表示页面包含图片等文件在内的所有元素都加载完成。

### 清除浮动有哪些方式？比较好的方式是哪一种？


　（1）父级div定义height。

　（2）结尾处加空div标签clear:both。

　（3）父级div定义伪类:after和zoom。

　（4）父级div定义overflow:hidden。

　（5）父级div定义overflow:auto。

　（6）父级div也浮动，需要定义宽度。

　（7）父级div定义display:table。

　（8）结尾处加br标签clear:both。
 
 38.清除浮动的几种方式，各自的优缺点？
1.使用空标签清除浮动 clear:both（理论上能清楚任何标签，，，增加无意义的标签）
2.使用overflow:auto（空标签元素清除浮动而不得不增加无意代码的弊端,,使用zoom:1用于兼容IE）
3.是用afert伪元素清除浮动(用于非IE浏览器)

　　比较好的是第3种方式，好多网站都这么用。
  
### Doctype作用？标准模式与兼容模式各有什么区别? 

告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。

(Q2) 标准模式的排版和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。

### HTML5 为什么只需要写<!DOCTYPE html> ？

　HTML5不基于 SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为（让浏览器按照它们应该的方式来运行）。

而HTML4.01基于SGML,所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。

### 页面导入样式时，使用link和@import有什么区别？

   （1）link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;

　　（2）页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;

　　（3）import是CSS2.1 提出的，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题。
  （1）、link属于XHTML标签，而@import是CSS提供的;

（2）、页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;

（3）、import只在IE5以上才能识别，而link是XHTML标签，无兼容问题;

（4）、link方式的样式的权重 高于@import的权重.


### CSS定义的权重

### CSS中margin和padding的区别

### 简述一下你对HTML语义化的理解？　　

用正确的标签做正确的事情。

　　html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析;

　　即使在没有样式CSS情况下也以一种文档格式显示，并且是容易阅读的;

　　搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO;

　　使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。
　
### 在html中，position取值有哪几种，默认值是什么？

取值：static、relative、fixed、absolute

默认值：static

### 前端页面由哪三层构成，分别是什么？作用是什么？

前端页面构成：结构层、表示层、行为层

结构层（structural layer）

由 HTML 或 XHTML之类的标记语言负责创建。标签，也就是那些出现在尖括号里的单词，对网页内容的语义含义做出了描述，但这些标签不包含任何关于如何显示有关内容的信息。例如，P标签表达了这样一种语义：“这是一个文本段。”

表示层（presentation layer）

由 CSS 负责创建。 CSS对“如何显示有关内容”的问题做出了回答。

行为层（behaviorlayer）

负责回答“内容应该如何对事件做出反应”这一问题。这是 Javascript 语言和 DOM主宰的领域。

### 如何居中一个浮动元素？

方式1:设置容器的浮动方式为相对定位，然后确定容器的宽高比如宽500 高 300 的层，然后设置层的外边距。

方式2：需要position:absolute;绝对定位。而层的定位点，使用外补丁margin负值的方法。负值的大小为层自身宽度高度除以二。

[来源](https://blog.csdn.net/dkh_321/article/details/79311446)


### 请简述HTML和XHTML最重要的4点不同？

  XHTML要求正确嵌套
  XHTML 所有元素必须关闭
  XHTML 区分大小写
  XHTML 属性值要用双引号
  XHTML 用 id 属性代替 name 属性
 XHTML 特殊字符的处理
 
 ### 怎么样从web前端方面优化性能？至少列举5点？
 
 
 ### .IE6的双倍边距BUG指的是什么？怎么解决？
 
 双边距：当块级元素有浮动样式的时候，给元素添加margin-left和margin-right样式，在ie6下就会出现双倍边距。

解决方案：给当前元素添加样式，使当前元素不为块，如：display:inline;display:list-item

###  如果制作一个访问量很大的网站，对css，js和图片应该怎么处理?

### 什么是同步异步(主观题，答案不唯一)?

### xhtml和html有什么区别？

HTML是一种基本的WEB网页设计语言，XHTML是一个基于XML的置标语言
最主要的不同：
XHTML 元素必须被正确地嵌套。
XHTML 元素必须被关闭。
标签名必须用小写字母。
XHTML 文档必须拥有根元素。

### CSS引入的方式有哪些?link和@import的区别是?
内联 内嵌 外链 导入
区别 ：同时加载
前者无兼容性，后者CSS2.1以下浏览器不支持
Link 支持使用javascript改变样式，后者不可

### CSS选择符有哪些？哪些属性可以继承？优先级算法如何计算？内联和important哪个优先级高？
（可以参考书上）
标签选择符 类选择符 id选择符
继承不如指定 Id>class>标签选择
后者优先级高

CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？ CSS3新增伪类有那些？

1.id选择器（ # myid）

2.类选择器（.myclassname）

3.标签选择器（div, h1, p）

4.相邻选择器（h1 + p）

5.子选择器（ul < li）

6.后代选择器（li a）

7.通配符选择器（ * ）

8.属性选择器（a[rel = "external"]）

9.伪类选择器（a: hover, li: nth - child）

可继承： font-sizefont-family color, UL LI DL DD DT;

不可继承 ：border paddingmargin width height ;

优先级就近原则，样式定义最近者为准;

载入样式以最后载入的定位为准;

优先级为:

!important >  id > class > tag 

important 比 内联优先级高

CSS3新增伪类举例：

p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。

p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。

p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。

p:only-child    选择属于其父元素的唯一子元素的每个 <p> 元素。

p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。

:enabled、:disabled控制表单控件的禁用状态。

:checked，单选框或复选框被选中。

### 31.写出几种IE6 BUG的解决方法？

1.双边距BUG float引起的使用display
2.3像素问题使用float引起的使用dislpay:inline-3px
3.超链接hover 点击后失效使用正确的书写顺序link visited hover active
4.Ie z-index问题给父级添加position:relative
5.Png 透明使用js代码改
6.Min-height 最小高度！Important解决’
7.select 在ie6下遮盖使用iframe嵌套
8.为什么没有办法定义1px左右的宽度容器（IE6默认的行高造成的，使用over:hidden,zoom:0.08 line-height:1px）

### <img>标签上title与alt属性的区别是什么？

Alt 当图片不显示是用文字代表。
Title 为该属性提供信息

### 33.描述css reset的作用和用途？
（可以参考书上）
Reset重置浏览器的css默认属性浏览器的品种不同，样式不同，然后重置，让他们统一

  因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。

·     当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。

*最简单的初始化方法就是： * {padding: 0;margin: 0;} （不建议）

   淘宝的样式初始化：

   body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li,pre, form, fieldset, legend, button, input, textarea, th, td { margin:0;padding:0; }

   body, button, input, select, textarea { font:12px/1.5tahoma, arial,\5b8b\4f53; }

   h1, h2, h3, h4, h5, h6{ font-size:100%; }

   address, cite, dfn, em, var { font-style:normal; }

   code, kbd, pre, samp { font-family:couriernew, courier, monospace; }

   small{ font-size:12px; }

   ul, ol { list-style:none; }

   a { text-decoration:none; }

   a:hover { text-decoration:underline; }

   sup { vertical-align:text-top; }

   sub{ vertical-align:text-bottom; }

   legend { color:#000; }

   fieldset, img { border:0; }

   button, input, select, textarea { font-size:100%; }

   table { border-collapse:collapse; border-spacing:0; }

### 解释css sprites，如何使用？

Css 精灵把一堆小的图片整合到一张大的图片上，减轻服务器对图片的请求数量

### 浏览器标准模式和怪异模式之间的区别是什么？

盒子模型渲染模式的不同
使用 window.top.document.compatMode 可显示为什么模式

### 你如何对网站的文件和资源进行优化？期待的解决方案包括：

文件合并
文件最小化/文件压缩
使用CDN托管
缓存的使用

·          （1）减少http请求次数：CSS Sprites, JS、CSS源码压缩、图片大小控制合适；网页Gzip，CDN托管，data缓存 ，图片服务器。

·         

·          （2）前端模板 JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存AJAX请求结果，每次操作本地变量，不用请求，减少请求次数

·         

·          （3）用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能。

·         

·          （4）当需要设置的样式很多时设置className而不是直接操作style。

·         

·          （5）少用全局变量、缓存DOM节点查找的结果。减少IO读取操作。

·         

·          （6）避免使用CSS Expression（css表达式)又称Dynamic properties(动态属性)。

·         

·          （7）图片预加载，将样式表放在顶部，将脚本放在底部  加上时间戳。

·         

·          （8）避免在页面的主体布局中使用table，table要等其中的内容完全下载之后才会显示出来，显示比div+css布局慢。

###  37.什么是语义化的HTML？
（可以参考书上 HTML5）
直观的认识标签对于搜索引擎的抓取有好处

### 经常遇到的浏览器的兼容性有哪些？原因，解决方法是什么，常用hack的技巧？

### 列出display的值，说明他们的作用。position的值， relative和absolute定位原点是？

 1.    block 象块类型元素一样显示。

 none 缺省值。向行内元素类型一样显示。

 inline-block 象行内元素一样显示，但其内容象块类型元素一样显示。

 list-item 象块类型元素一样显示，并添加样式列表标记。

 

 2.

 *absolute

       生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。

 *fixed （老IE不支持）

       生成绝对定位的元素，相对于浏览器窗口进行定位。

 *relative

       生成相对定位的元素，相对于其正常位置进行定位。

 * static  默认值。没有定位，元素出现在正常的流中

 *（忽略 top, bottom, left,right z-index 声明）。

 * inherit 规定从父元素继承 position 属性的值。
 
### iframe有那些缺点？

### 37.AMD（Modules/Asynchronous-Definition）、CMD（Common ModuleDefinition）规范区别？


(https://blog.csdn.net/gyq04551/article/details/55254359)
### 如何避免XSS？

### 平时如何管理项目？

### 工作中用过什么构建工具？

### 谈谈你对模块化的理解？

### 平时都用什么第三方框架？

### 10、谈谈你对预加载的理解？
11、缓存和预加载的区别是什么？
9、如何使用缓存？

### 12、图片如何压缩？

### 13、压缩文件有哪些方法？

### 14、如何区分静态页面和动态页面？

答：要区分这两个，最简单的方法就是看后缀了，动态网页网址中有两个标志性的符号“?”和“&”（有的可能没有&），这个问号和&就是用来带参数的。现在几乎爱所有的网页都是动态网页。

### 6、前台兼容性问题有哪些？

答：主要是常用浏览的（前端）API差异，渲染差异，等等

### 18、内存泄漏怎么理解？

### 19、微格式到底是做啥用？

答：是开放的数据格式，面向的是普通用户，任何用户可以透过简单的程序读取微格式内容。而不是像Flickr、Amazon、Google等提供特定的面向技术人员的API（一般基于XML-PRC、REST，相对复杂）。RSS具有微格式的部分优点，但限制还是比较多的，比如有限的元数据（标题、描述、URL等），不能更好地描述语义，不太容易与已存在的工具结合等。用微格式可以来聚合外部Blog，Flickr，YouTube，MapQuest，甚至MySpace里的内容。

微格式实际就是为现有的(X)HTML元素添加元数据和其他属性，增强语义

### 、如何缓存整个页面，在没有网络的时候可以来回的跳转？

### 22、CDN是啥？

### 23、浏览器一次可以从一个域名下请求多少资源？

### 24、什么是垃圾回收机制（GC）？
36、请描述你熟悉的语言的垃圾回收(GC)机制，他们对循环引用是如何处理的？如何查找内存泄漏(MemoryLeak)?

### 25、image和canvas在处理图片的时候有什么区别？

image是通过对象的形式描述图片的

canvas通过专门的API将图片绘制在画布上

### 26、简述移动开发的注意点,如何做好不同手机的适配,你以前的项目是怎么做的?

1、单独做移动端项目，采用百分比布局

2、采用响应式的方式做适配

### 28、http和tcp有什么区别？

TPC/IP协议是传输层协议，主要解决数据如何在网络中传输，是一种“经过三次握手”的可靠的传输方式；

HTTP协议即超文本传送协议(Hypertext Transfer Protocol )，是应用层协议，是Web联网的基础，也是手机联网常用的协议之一，HTTP协议是建立在TCP协议之上的一种应用。

### 35、设计模式有哪些？列举你在前端开发工作中自己应用到或者了解到其他框架所用到的设计模式？

单例、工厂、观察者、适配器、代理模式

### 16、什么是线程？进程和线程的关系是什么？

### 19、什么是MVVM框架？
[来源](https://www.jianshu.com/p/0e9a0d460f64)

### 20、利用@media screen实现网页布局的自适应。

### 22、前端安全方面有没有了解？XSS和CSRF如何攻防？

### 20、说说你对HTML5认识?（是什么,为什么）

### 21、什么是WebGL,它有什么优点?

### 22、说说你对SVG理解?

### CSS层叠是什么？介绍一下

### 对BFC规范的理解

BFC，块级格式化上下文，一个创建了新的BFC的盒子是独立布局的，盒子里面的子元素的样式不会影响到外面的元素。在同一个BFC中的两个毗邻的块级盒在垂直方向（和布局方向有关系）的margin会发生折叠。

（W3C CSS 2.1 规范中的一个概念,它决定了元素如何对其内容进行定位,以及与其他元素的关系和相互作用。）


[HTML中href、src区别]: https://blog.csdn.net/annsheshira23/article/details/51133709
[rel、href、src、url的区别]:https://blog.csdn.net/chengshaolei2012/article/details/72847770
[史上最全的CSS hack方式一览]:https://blog.csdn.net/freshlover/article/details/12132801
[CSS hack大全]:https://www.duitang.com/static/csshack.html
[今天才知道css hack是什么]:https://www.cnblogs.com/miniyk/p/3734664.html
[CSS Hack是什么意思？css hack有什么用？]:https://www.w3cschool.cn/css3/question-10231625.html
[你想知道的css hack知识全都帮你整理好了]:https://www.w3cschool.cn/css/css-hack.html
[CSS参考手册]:http://phpstudy.php.cn/css3/
[同步请求和异步请求的区别]: https://blog.csdn.net/goodshot/article/details/7244053
[简述同步和异步的区别]:https://www.cnblogs.com/Renyi-Fan/p/7808756.html#_label0_5
[阻塞与非阻塞的区别]:https://www.cnblogs.com/orez88/articles/2513460.html
[HTML文档的根元素是 html 元素]:https://blog.csdn.net/ixygj197875/article/details/79737953
[px,em,rem单位转换工具]:http://pxtoem.com/
[彻底弄懂px,em和rem的区别]:https://www.cnblogs.com/langee/p/6890362.html
[什么叫优雅降级和渐进增强？]:https://www.cnblogs.com/Renyi-Fan/p/7808756.html#_label0_7
[主流浏览器内核]:http://p9myzkds7.bkt.clouddn.com/web-interview/%E4%B8%BB%E6%B5%81%E6%B5%8F%E8%A7%88%E5%99%A8%E5%86%85%E6%A0%B8.png
[浏览器的内核分别是什么？]:https://www.cnblogs.com/maggie-pan/p/6391355.html
[五大流浏览器内核及其代表]:https://blog.csdn.net/u014753892/article/details/52713841
[cookie、sessionStorage与lacalStrage的区别]: http://p9myzkds7.bkt.clouddn.com/web-interview/cookie%E3%80%81sessionStorage%E4%B8%8ElacalStrage%E7%9A%84%E5%8C%BA%E5%88%AB.png
[cookies、sessionStorage和localStorage解释及区别]:https://www.cnblogs.com/pengc/p/8714475.html
[AJAX工作原理]:http://p9myzkds7.bkt.clouddn.com/web-interview/AJAX%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.gif
[AJAX 简介]:https://www.w3cschool.cn/ajax/nr583fns.html
[Ajax的优缺点及工作原理？]:https://www.cnblogs.com/wdlhao/p/8290436.html#_label3
[AJAX工作原理及其优缺点]:https://www.cnblogs.com/SanMaoSpace/archive/2013/06/15/3137180.html