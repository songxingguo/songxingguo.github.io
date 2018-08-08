title: Web前端面试题目及详解汇总-HTML/CSS部分
author: songxingguo
date: 2018-08-06 14:56:36
tags:
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

 - **边距重叠：** **块级元素** 的 **垂直相邻外边距会合并** ，并且将 **使用较大的外边距作为最终的边距** 。
 
   - 避免方式：只设置 **margin-top** 或 **margin-bottom**  ，而不是将两者混合使用。
 
来自—— **[深入理解CSS盒模型](https://www.cnblogs.com/chengzp/p/cssbox.html)**
 
### HTML元素

  - 元素分类 

   ![元素分类](http://p9myzkds7.bkt.clouddn.com/web-interview/%E5%85%83%E7%B4%A0%E5%88%86%E7%B1%BB.png)

  
  - **行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？**
  
  > 行内元素、块级元素、空(void)元素

    - **行内元素：** a、b、span、img、input、strong、select、label、em、button、textarea 。

    - **块级元素：** div、ul、li、dl、dt、dd、p、h1-h6、blockquote 。

    - **空元素：** 即没有内容的HTML元素，例如：br、meta、hr、link、input、img 。
  
  - **行内元素和块元素**
  
    - 两者的区别
    
      - 块级元素

        > **独占一行**, 垂直方向排列；默认情况下，其 **宽度自动填满其父元素宽度** ；块级元素 **可以设置 width 、height 属性** ；**可以设置 margin 和padding 属性** 。

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
       
来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.7.6 块级元素和行内元素的区别，4.7.7 display:inlne-block和hasLayout）**



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
         
来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.7.9 居中）**


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

### 在HTML中，position取值有哪几种，默认值是什么？

> **position** 的 **5** 种属性值：**relative** 、**absolute** 、**fixed** 、**static**（默认值） 、**inherit** 。

- **简述**
![position属性值](http://p9myzkds7.bkt.clouddn.com/web-interview/position%E5%B1%9E%E6%80%A7%E5%80%BC.png)

<!-- | 值|    描述|
| :-------- | :--------|
| relative	| **相对定位**，**相对于** 自己在 **z-index:0 层** 的 **位置** ；**left** 、**top** 、**right** 、**bottom** 和 **z-index** 属性 **激活** 。 | 
| absolute	| **绝对定位**，**相对于** 自己 **最近的** 一个设置了 **position:relative** 或  **position: absolute** 的 **祖先元素** 或 **相对于 body 元素** ；**left** 、**top** 、**right** 、**bottom** 和 **z-index** 属性 **激活** ；隐式地改变 **display 类型** 为 **inline-block** 。 | 
| fixed	| **绝对定位**，相对于 **浏览器窗口** 进行定位；**left** 、**top** 、**right** 、**bottom** 和 **z-index** 属性 **激活**。| 
| static	| **默认值** 。**没有定位** ，元素出现在正常的流中；**left** 、**top** 、**right** 、**bottom** 和 **z-index** 属性 **未激活** 。 | 
| inherit	| 从父元素继承 **position** 属性的值 。 |  -->

- **详细描述**

   >_**position: relative** 和 **position:absolute**  可改变元素在 **文档流** 中的 **位置** 。可以让元素激活 **left** 、**top** 、**right** 、**bottom** 和 **z-index**（默认在 **z-index:0** 这一层） 属性（默认情况下，这些属性未激活，设置了也无效）。_

  **普通流**

  >*默认情况下，**所有元素** 都是在 **z-index:0** 这一层。元素根据自己的 **dispaly类型** 、**长宽** 、**内外边距** 等属性 **顺序排列** 在 **z-index:0**  这一层里，这就是 **文档流** 。设置 **position:relative** 或  **position: absolute** 会让元素“ **浮** ”起来，也就是 **z-index** 值 **大于 0** ，它会 **改变** 正常情况下的 **文档流** 。*
  
  > **CSS** 有三种基本的定位机制，分别是 **普通流** 、**浮动** 以及 **绝对定位** 。**除非特殊定义** ，不然 **任何框** 都应该 **遵循文档流的定位规则** ，换而言之，**普通流的元素的位置** 由其 **在HTML文档中的位置** 决定。

  **position:relative**

  >*不同的是 **position:relative** 会 **保留** 自己在 **z-index:0 层**  的 **占位** ，**left**  、**top** 、**right** 、**bottom** 值是 **相对于** 自己在 **z-index:0 层** 的 **位置** 。虽然它的实际位置可能因为设置了 **left**  、**top** 、**right** 、**bottom** 值而 **偏离原来在文档流中的位置** ，但 **对其他仍然在z-index:0层的元素位置不会造成影响** 。*

  **position:absolute**

  >*而 **position:absolute** 会 **完全脱离文档流** ，**不再在z-index:0层保留占位符** ，其 **left**  、**top** 、**right** 、**bottom** 值是 **相对于** 自己 **最近的** 一个设置了 **position:relative** 或  **position: absolute** 的 **祖先元素的** , 如果 **祖先元素全都没有设置**  **position:relative** 或  **position: absolute** ，那么就 **相对于 body 元素** 。*

  **float**

  >_**float** 属性也能 **改变文档流** ，不同的是，**float** 属性 **不会让元素“上浮”到另一个 z-index 层**，它仍然 **让元素在 z-index:0 层排列** ，**float** 不像 **position:relative** 和 **position:absolute** 那样，它 **不能** 通过 **left**  、**top** 、**right** 、**bottom** 属性 **精确地控制元素的坐标** ，它只能通过 **float:left** 和 **float:right** 来控制元素 **在同层里“左浮”和“右浮”** 。**float 会改变正常的文档流排列** ，**影响到周围元素** 。_

  **隐式改变display类型**
  
  >_**position:absolute**和 **float** 会 **隐式地改变display类型** ，不论之前是什么类型的元素（ **display:none** 除外），只要设置了 **position:absolute** 、**float:lef** 或 **float:right** 中任意一个，都会让元素以 **display：inline-block** 的方式显示：**可以设置长宽** ，**默认宽度并不占满父元素** 。就算我们 **显式地** 设置 **display:inline** 或者 **display：block** ，也 **仍然无效** （ **float** 在 **IE 6** 下的 **双倍边距bug** 就是利用 **添加 display:inline 来解决的** ）。值得注意的是，**position:relative** 却 **不会隐式改变display的类型** 。_
  
来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.7.8 relative、absolute 和 float）**

### 清除浮动有哪些方式？比较好的方式是哪一种？

- **浮动的定义**

  > 定义了 **浮动属性的框** 可以 **左右移动** ， **直到它的外边缘** 与 **包含框** 或 **另一个浮动元素的框的边框** 相碰为止。**浮动框** 是 **不存在文档的普通流中**  ，因此 **文档的普通流中的块框** 表现的就 **如同浮动框不存在一般** 。如果 **包含框宽度不够** ，即 **无法同时容纳几个浮动元素** ，那么**浮动块** 将 **向下移动** ，**直到有充足的空间** ，可以 **放下浮动框** 为止。如果 **浮动元素的高度不同** ，那么它们向**下移动时** 可能 **会被其他浮动元素“卡住”** 。

- **float 属性**

  > **float** 的 **4** 种属性值：**left** 、**right** 、**none**（默认值） 、**inherit** 。

  ![float属性值](http://p9myzkds7.bkt.clouddn.com/web-interview/float%E5%B1%9E%E6%80%A7%E5%80%BC.png)

<!-- | 值      |    描述| 
| :-------- | :--------| 
| left	| 元素 **向左浮动** 。 | 
| right	| 元素 **向右浮动** 。 |
| none	| **默认值** 。元素 **不浮动** ，并会 **显示在其在文本中出现的位置** 。 | 
| inherit	| 规定应该 **从父元素继承 float 属性的值** 。 | -->

- **浮动的“副作用”**

 1. 背景无法显示。
 
    > 如果对 **父级元素** 设置了 **CSS背景颜色**（background）或是 **CSS背景图片**（background-image）,由于 **浮动的产生** ，**父级元素不能被撑开**，所以导致 **CSS背景无法显示** 。
   
 2. 边框不能撑开。
 
    > 如果 **父级元素** 设置了 **CSS边框属性**（border）,由于 **子级里使用了 float 属性** 而 **产生了浮动** 。而 **父级不能被撑开** ，因此 **导致边框不能随内容而被撑开** 。
    
 3. margin、padding 属性的值不能正确显示。
 
    > **浮动** 会导致 **父级子级之间** 设置的 **padding** 、**margin** 属性值 **不能正确显示** 。特别是 **上下边的 padding 和 margin 值** 。
   
- 清除浮动

  1. 对父级设置合适的CSS高度。
    
     > **浮动** 导致 **父级元素不能被撑开** 。
     
  2. 使用 clear: both 清除浮动。
  
     > clear属性规定元素的哪一侧不允许其他的浮动元素。如果声明左侧或者右侧不允许浮动，会使元素的上下边框边界刚好在该边上浮动元素的下外边距边距之下。
     
     ![clear属性值](http://p9myzkds7.bkt.clouddn.com/web-interview/clear%E5%B1%9E%E6%80%A7%E5%80%BC.png)
     
     <!-- | 值      |    描述| 
| :-------- | :--------| 
| left	| 在 **左侧不允许** 浮动元素。 | 
| right	| 在 **右侧不允许** 浮动元素。 |
| both  | 在 **左右两侧均不允许**  浮动元素。 |
| none	| **默认值** 。**允许** 浮动元素出现在两侧。 | 
| inherit	| 规定应该 **从父元素继承 clear 属性的值**。 | -->

  3. 父级 div 定义： overflow： hidden 。 
  
  4. 在浮动元素后面增加 `<br/>` 标签。
  
  5. 在父级元素加上 .clearfix 类（常用）
  
     > 其 **原理** 就是 **使用 CSS 伪类** ，在 **元素末尾** 加 **一个带有浮动功能的标签** 。
  
     ```
     .clearfix:after {
       visibility: hidden;
       display: block;
       font-size: 0;
       content: "";
       clear: both;
       height: 0;
     }
     .clearfix {
       display: inline-table;
     }
     
     /* Hides from IE-mac \*/
     *html .clearfix {
       height: 1%;
     }
     .clearfix {
       dispaly: block;
     }
     /* End hide from IE-mac */
     ```

来自—— [CSS float 属性]、[CSS样式----浮动（图文详解）]、 **Web 前端开发实战教程（2014版）V1.8[M].教材用书，2014.（3.9.4 浮动与清除浮动）**

### 文档类型声明（DOCTYPE）作用？

> **文档类型声明** 用于 **宣告后面的文档标记遵循哪个标准** 。

- *声明构成**

  **html**  + 顶级元素可用性 + " **注册** // **组织** // **类型标签** // **定义语言** " + "**URL**"。
  
    ```
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0//EN"> 
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Strict//EN">
    ```
- **文档定义类型**

  HTML 4.01 规定的 3 种主要文档定义类型：

    - 严格型（Strict）: 所有标记必须符合 XHTML标准。

      ```
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
      ```
    - 过渡型（Transitional）: 能兼容之前的HTML代码。

      ```
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
      ```

    - 框架型（Frameset）: 能兼容XHTML不推荐的框架。

      ```
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org//TR/html4/frameset.dtd">
      ```

  XHTML 1.0 规定的 3 种主要文档定义类型：

    - 严格型（Strict）：文档中不允许使用任何表现样式的标识和属性，例如 `<br>` 。

    ```
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    ```
    - 过渡型（Transitional）：允许你继续使用HTML4.01的标识(但是要符合xhtml的写法) 。

    ```
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    ```
    - 框架型（Frameset）：框架页类型。网页使用框架结构时，声明此类型。

    ```
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
    ```
  HTML5文档定义类型：

    ```
    <! DOCTYPE html>
    ```
    
 来自—— **Web 前端开发实战教程（2014版）V1.8[M].教材用书，2014.（2.2.1 文档定义）** 、[<!DOCTYPE html PUBLIC……>的组成解释]、[HTML文档类型 DTD]
 
[HTML文档类型 DTD]:https://blog.csdn.net/lihaikuo666/article/details/81011830
[<!DOCTYPE html PUBLIC……>的组成解释]:https://www.jianshu.com/p/d3e08ab627ae


### 标准模式与兼容模式各有什么区别? 

> **DOCTYPE** **不存在** 或 **格式不正确** 会导致文档以 **兼容模式** 呈现 。

**两种模式**

- 标准模式（Standards, 也就是严格呈现模式）

  > 用于呈现遵循最新标准的网页。
  
  标准模式的排版 和 JS运作模式都是以该浏览器支持的最高标准运行。
  
- 兼容模式（Quirks, 也就是松散呈现模式或者怪异模式）

  > 用于呈现为传统浏览器而设计的网页。 

  兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。简单来说就是尽可能的显示能显示的东西给用户看。

**具体区别**

  - 盒模型。
  
    >在 **严格模式** 中 ：**width** 是内容宽度 ，元素真正的宽度 = **width** ;在 **兼容模式** 中 ：**width** = width + padding + border。

  - **兼容模式** 下可设置 **百分比的高度** 和 **行内元素的高宽**。
  
    > 在 **标准模式** 下，给 **span** 等行内元素设置 **wdith** 和 **height** 都 **不会生效** ，而在 **兼容模式** 下，则 **会生效** 。
    
    > 在标准模式下，一个元素的高度是由其包含的内容来决定的，如果父元素没有设置高度，子元素设置一个百分比的高度是无效的。

  -  用 **margin:0 auto** 设置水平居中在 **IE** 下会失效。
  
     > 使用 **margin:0 auto** 在 **标准模式** 下 **可以使元素水平居中** ，但在 **兼容模式** 下却 **会失效**（用 **text-align** 属性解决）。
     ```
      body{
        text-align:center;
      }
      #content{
         text-align:left;
      }
     ```
  - **兼容模式** 下 **Table** 中的 **字体属性不能继承上层的设置** ，**white-space:pre**会失效，设置图片的 **padding** 会失效。
  
来自—— [Doctype作用？标准模式与兼容模式各有什么区别?]

[Doctype作用？标准模式与兼容模式各有什么区别?]:https://www.cnblogs.com/coldfishdt/p/6533120.html

### HTML5 为什么只需要写<!DOCTYPE html> ？

> **HTML 4.01** 基于 **SGML** ，**需要** 对 **DTD** 进行引用，才能 **告知浏览器文档所使用的文档类型** 。**HTML 5** 不基于 **SGML** ，因此 **不需要** 对 **DTD** 进行引用，但是需要 **DOCTYPE**  来 **规范浏览器的行为** 。

> **HTML5** 文档类型声明 **不包含官方规范版本号** （即HTML5中的5）。事实上，这声明 **仅仅表示当前页面是HTML页面** 。只要有 **新功能** 添加到 **HTML** 语言中，在页面中就 **可以使用** 它们，而 **不必** 为此 **修改文档类型声明** 。

**文档类型定义是什么？**

> 文档类型定义（ DTD: Document Type Definition ）是 W3C 所发布的，用于描述文档的内容和结构和验证文档的合法性。告诉浏览器你的这个 HTML，是根据那个标准写的，解析的时候用哪个标准解析。[DTD教程]

**SGML是什么？**

> SGML 是标准通用标记语言，简单的说，就是比 HTML , XML 更老的标准，这两者都是由SGML 发展而来的。但是，HTML5 不是的。


**保留文档类型声明的原因：**

>要求 **保留文档类型声明** ，主要是由于 **历史原因** 。如果 **没有文档类型声明** ，那 **大多数浏览器** （包括Internet Explorer 和 Firefox）将转换到一种 **混杂模式**（quirk mode）。在这种模式下，**浏览器会尝试根据有点不那么正常的规则呈现网页** （那些规则是在浏览器的老版本中使用的），并且 **不同浏览器的混杂模式也不一样** 。

> 而 **添加了文档类型声明** 后，浏览器就知道你想要使用更严格的 **标准模式**（standard mode）,在这种模式下，**所有浏览器** 都会 **以一致的格式和布局来显示网页** 。浏览器不关心你使用的是哪种文档类型（个别情况下还是有些例外），**只要它检查到你有某种文档类型声明就好** 。HTML5的文档类型声明是 **最短的有效文档类型声明** ，因此它 **总能触发标准模式** 。

来自—— **（美）麦克唐纳（Matthew MacDonald，M.）著；李松峰，朱魏，刘帅译.HTML5秘籍[M].人民邮电出版社，2017.10.（1.3.1 HTML5文档类型）**

[DTD教程]:http://www.runoob.com/dtd/dtd-tutorial.html

### CSS层叠是什么？CSS定义的权重

**CSS设置的样式** 是可以 **层叠** 的，如代码下代码所示：

```
<style type="text/CSS">
span{
  font-size: 40px;
}
.test{
  color: red;
}
</style>
<span class="test">1234567890</sapn>
```
_“ **1234567890** ”既 可得到“ **font-size:40px** ”的样式，又可以得到“ **color:red **”的样式。_

**CSS层叠样式冲突** 的情况：

```
<style type="text/CSS">
span{
  font-size: 40px;
  color: green;
}
.test{
  color: red;
}
</style>
<span class="test">1234567890</sapn>
```
_“ **1234567890** ”的颜色会是什么呢？这就涉及 **CSS的选择符的权重** 问题了。_

> **CSS的选择符** 是 **有权重的** ，当 **不同选择符的样式** 设置 **有冲突** 时，会采用 **权重高的选择符** 设置的样式。

**权重规则** 

> **HTML 标签** 的权重是 **1** ，**class** 的权重是 **10** ，**id** 的权重是  **100**。

_例如 **p** 的权重是 **1** ，“ **div em** ”的权重是 **1+1=2** ，“ **strong .demo** ”的权重是 **1+10=11** ,“ **#test .red** ”的权重是 **100+10=110** 。_

> 如果 **选择符权重相同** ，那么样式会遵循 **就近原则** ，哪个选择符 **最后定义** ，**就采用哪个选择符的样式** 。

_**注意：** 使用 **子选择器** ，会 **增加 CSS 选择符的权重** ，CSS选择符的 **权重越高** ，样式 **越不易被覆盖** ，**越容易对其他选择符产生影响** 。所以，除非确定HTML结构非常稳定，一定不会再修改了，否则 **尽量不要使用子选择器** 。为了保证样式容易被覆盖，提高可维护性， **CSS选择符权重需要尽可能低** 。新添加class比使用子选择器更利于维护。_

来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.5 低权重原则— 避免滥用子选择器）**

### 解释css sprite，如何使用？

>  **图片翻转技术** 将 **多张图片合并为一张** ，然后利用 **background-position 属性** 来 **展示我们需要的部分** 。按照这种思路，后来衍生出的另一种技术将这种思路发挥到了极致 — **将网站的多张背景图片合并到一张大图片上** ，这便是 **CSS sprite** 。

_不仅是 **解决滑过状态时背景图片出现空白的问题** ，而且将多张图片合并成一张大图，会**大大减少网页的HTTP请求数** ，**减小服务器压力** 。_

**CSS sprite技术** 看似简单，其实 **不容易掌握**，主要有如下原因：

- 它能合并的 **只能是用于背景的图片** ，对于 **`<img src="" />` 设置的图片** ，是 **不能** 合并到CSS sprite大图中的，如果合并这些图片会影响到页面的可读性。

- 对于 **横向和纵向都平铺的图片** ，也 **不能使用CSS sprite** ；如果是 **横向平的** ，只能 **将所有横向平铺的图合成一张大图** ，只能 **竖直排列**，不能水平排列；如果是 **纵向平铺的** ，我们只能 **将所有纵向平铺的图合并成一张大图** ，只能 **水平排列** ，不能竖直排列。

  _图片如何排列能够尽量紧凑，同时保证不会影响扩展性。这点是CSS sprite技术最困难也是最具挑战性的地方。_

- **CSS sprite技术** 通过 **position** 进行定位，这对于图片的位置精确程度要求非常高，一方面 **在制作网页时** ，我们 **需要精确测量坐标** ，还 **需要考虑如何让图案尽可能密集的排列** ，这 **影响开发速度** ；另一方面，**大图中每个小图的坐标值都不可轻易改动** ，因为 **每改动一个小图** ，都 **可能影响到周边的其他图片** ，这 **大大降低了可维护性** 。

**注意：**

_**CSS sprite** 的最大好处是 **减少了HTTP请求次数** ，**减轻服务器的压力** ，但它却需要付出“ **降低开发效率** ”和“ **增大维护难度** ”的代价。_
_对于 **流量不大的网站** 来说，**CSS sprite** 带来的 **好处并不明显** ，而它付出的 **代价却很大** ，其实并不划算。所以，**是否使用CSS sprite主要取决于网站流量** 。_

来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.6 CSS sprite）**

### CSS中margin和padding的区别

- **外边距**（margin）

   **边距重叠：** **块级元素** 的 **垂直相邻外边距会合并** ，并且将 **使用较大的外边距作为最终的边距** 。而行内元素实际上不占上下外边距。行内元素的左右边距不会合并。同样地，浮动元素的外边距也不会合并。允许指定负的外边距值，不过使用时要小心。


### 页面导入样式时，使用link和@import有什么区别？




### 简述一下你对HTML语义化的理解？37.什么是语义化的HTML？　


### 前端页面由哪三层构成，分别是什么？作用是什么？

### 如何居中一个浮动元素？

### 请简述HTML和XHTML最重要的4点不同？

### .IE6的双倍边距BUG指的是什么？怎么解决？

### CSS选择符有哪些？哪些属性可以继承？优先级算法如何计算？内联和important哪个优先级高？


### `<img>` 标签上title与alt属性的区别是什么？

### 33.描述css reset的作用和用途？



### 列出display的值，说明他们的作用。position的值， relative和absolute定位原点是？
 
### 20、利用@media screen实现网页布局的自适应。


### 20、说说你对HTML5认识?（是什么,为什么）

### 21、什么是WebGL,它有什么优点?

### 22、说说你对SVG理解?

### 对BFC规范的理解


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
[CSS样式----浮动（图文详解）]:https://www.cnblogs.com/smyhvae/p/7297736.html
[CSS float 属性]:http://www.w3school.com.cn/cssref/pr_class_float.asp