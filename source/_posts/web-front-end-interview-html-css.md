title: Web前端面试题目及详解汇总-HTML/CSS部分
author: songxingguo
tags: 
  - 找工作
categories:
  - 备忘录
date: 2018-08-06 14:56:00
---
## HTML/CSS部分

### 什么是盒模型

 > 在网页中，一个元素占有空间的大小构成，采用盒子作一个比喻来理解。

 - **盒模型的组成：** 由里向外由 **content** 、**padding**、 **border** 、**margin** 四部分组成。

 - **盒模型的两种标准：**  标准模型，IE模型。

 <!-- more -->

   - 两种标准的区别：

     在标准模型中，盒模型的 **宽高** 只是 **内容（content）的宽高** 。

     ![标准模型](https://graphbed.qiniu.songxingguo.com/web-interview/%E6%A0%87%E5%87%86%E6%A8%A1%E5%9E%8B.png)

     在IE模型中盒模型的 **宽高** 是 **内容(content)+填充(padding)+边框(border)的总宽高** 。

     ![IE模型](https://graphbed.qiniu.songxingguo.com/web-interview/IE%E6%A8%A1%E5%9E%8B.png)

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

来自—— **[深入理解CSS盒模型]**

[深入理解CSS盒模型]:https://www.cnblogs.com/chengzp/p/cssbox.html

### HTML元素

  - 元素分类 

   ![元素分类]

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

[元素分类]:http://p9myzkds7.bkt.clouddn.com/web-interview/%E5%85%83%E7%B4%A0%E5%88%86%E7%B1%BB.png

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
        
    > 只有一个元素属于inline或是inline-block水平，其身上的vertical-align属性才会起作用。虽然vertical-align属性只会在inline-block水平的元素上期作用，但是其影响到的元素涉及到inline属性的元素。inline水平元素受vertical-align属性而位置改变等不是因为其对vertical-align属性敏感或起作用，而是受制于整个line box的变化而不得不变化的。[为什么我的vertical-align属性不起作用](https://www.zhangxinxu.com/wordpress/2010/05/%E6%88%91%E5%AF%B9css-vertical-align%E7%9A%84%E4%B8%80%E4%BA%9B%E7%90%86%E8%A7%A3%E4%B8%8E%E8%AE%A4%E8%AF%86%EF%BC%88%E4%B8%80%EF%BC%89/)
    
    - Flex

      可查看站内文章—— **Flex布局**
      
- #### absoute 居中
 
   - 高度或宽度确定的水平和垂直居中
   
   ```css
   .element {
     width: 300px;
     height: 200px;
     position: absolute;
     left: %;
     top: 50%;
     margin-left: -150px;
     margin-top: -100px;
   }
   ```
  - 利用 **绝对定位元素的流体特性** 和  **margin:auto 的自动分配特性** 实现居中 （IE 8 开始支持）。
  
    ```css
    .element {
      width: 300px;
      height: 200px;
      position: absolute;
      left: 50%;
      top: 50%;
      margin: auto;
    }
    ```
- #### 居中布局

  - 水平居中

    - 行内元素: text-align: center
    - 块级元素: margin: 0 auto
    - absolute + transform
    - flex + justify-content: center

  - 垂直居中

    - line-height: height
    - absolute + transform
    - flex + align-items: center
    - table

  - 水平垂直居中

    - absolute + transform
    - flex + justify-content + align-items

来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.7.9 居中）**，**张鑫：CSS世界[M].人民邮电出版社，2017.4.（6.8.3 absolute 的 margin: auto 居中 ）**


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

[HTML中href、src区别]: https://blog.csdn.net/annsheshira23/article/details/51133709
[rel、href、src、url的区别]:https://blog.csdn.net/chengshaolei2012/article/details/72847770

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

[史上最全的CSS hack方式一览]:https://blog.csdn.net/freshlover/article/details/12132801
[CSS hack大全]:https://www.duitang.com/static/csshack.html
[今天才知道css hack是什么]:https://www.cnblogs.com/miniyk/p/3734664.html
[CSS Hack是什么意思？css hack有什么用？]:https://www.w3cschool.cn/css3/question-10231625.html
[你想知道的css hack知识全都帮你整理好了]:https://www.w3cschool.cn/css/css-hack.html
[CSS参考手册]:http://phpstudy.php.cn/css3/

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

[HTML文档的根元素是 html 元素]:https://blog.csdn.net/ixygj197875/article/details/79737953
[px,em,rem单位转换工具]:http://pxtoem.com/
[彻底弄懂px,em和rem的区别]:https://www.cnblogs.com/langee/p/6890362.html

### 在HTML中，position取值有哪几种，默认值是什么？

> **position** 的 **5** 种属性值：**relative** 、**absolute** 、**fixed** 、**static**（默认值） 、**inherit** 。

- **简述**
![position属性值]

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

[position属性值]:http://p9myzkds7.bkt.clouddn.com/web-interview/position%E5%B1%9E%E6%80%A7%E5%80%BC.png

### 清除浮动有哪些方式？比较好的方式是哪一种？

- **浮动的定义**

  > 定义了 **浮动属性的框** 可以 **左右移动** ， **直到它的外边缘** 与 **包含框** 或 **另一个浮动元素的框的边框** 相碰为止。**浮动框** 是 **不存在文档的普通流中**  ，因此 **文档的普通流中的块框** 表现的就 **如同浮动框不存在一般** 。如果 **包含框宽度不够** ，即 **无法同时容纳几个浮动元素** ，那么**浮动块** 将 **向下移动** ，**直到有充足的空间** ，可以 **放下浮动框** 为止。如果 **浮动元素的高度不同** ，那么它们向**下移动时** 可能 **会被其他浮动元素“卡住”** 。

- **float 属性**

  > **float** 的 **4** 种属性值：**left** 、**right** 、**none**（默认值） 、**inherit** 。

<!--| 值      |    描述| 
| :-------- | :--------| 
| left	| 元素 **向左浮动** 。 | 
| right	| 元素 **向右浮动** 。 |
| none	| **默认值** 。元素 **不浮动** ，并会 **显示在其在文本中出现的位置** 。 | 
| inherit	| 规定应该 **从父元素继承 float 属性的值** 。 |-->

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

     <!-- | 值      |    描述| 
| :-------- | :--------| 
		left	| 在 **左侧不允许** 浮动元素。 | 
		right	| 在 **右侧不允许** 浮动元素。 |
| both  | 在 **左右两侧均不允许**  浮动元素。 |
		none	| **默认值** 。**允许** 浮动元素出现在两侧。 | 
		inherit	| 规定应该 **从父元素继承 clear 属性的值**。 | -->

  3. 父级 div 定义： overflow： hidden （创建父级 BFC）。 
  
  4. 在浮动元素后面增加 `<br/>` 标签（ :after / <br> : clear: both）。
  
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

来自—— **Web 前端开发实战教程（2014版）V1.8[M].教材用书，2014.（3.9.4 浮动与清除浮动）**、[CSS float 属性]、[CSS样式----浮动（图文详解）]

[float属性值]:http://p9myzkds7.bkt.clouddn.com/web-interview/float%E5%B1%9E%E6%80%A7%E5%80%BC.png
[clear属性值]:http://p9myzkds7.bkt.clouddn.com/web-interview/clear%E5%B1%9E%E6%80%A7%E5%80%BC.png
[CSS样式----浮动（图文详解）]:https://www.cnblogs.com/smyhvae/p/7297736.html
[CSS float 属性]:http://www.w3school.com.cn/cssref/pr_class_float.asp

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

> **通配符** 的权重是 **0** ，**HTML 标签** 和 **伪对象** 的权重是 **1** ，**class**、 **伪类**、**属性** 的权重都是 **10** ，**id** 的权重是 **100**, **important** 的权重是 **1000** 。（**该部分内容还有待证实**）


_所以从样式选择器看权重优先级是（由高到低排列）：important > 内嵌样式 > id > class > 标签 | 伪类 | 属性选择 > 伪对象 > 继承 > 通配符。_（**该部分内容还有待证实**）

<!-- important的权重为1,0,0,0
ID的权重为0,1,0,0
类的权重为0,0,1,0
标签的权重为0,0,0,1
伪类的权重为0,0,1,0
属性的权重为0,0,1,0
伪对象的权重为0,0,0,1
通配符的权重为0,0,0,0 -->

_例如 **p** 的权重是 **1** ，“ **div em** ”的权重是 **1+1=2** ，“ **strong .demo** ”的权重是 **1+10=11** ,“ **#test .red** ”的权重是 **100+10=110** 。_

> 如果 **选择符权重相同** ，那么样式会遵循 **就近原则** ，哪个选择符 **最后定义** ，**就采用哪个选择符的样式** 。

_**注意：** 使用 **子选择器** ，会 **增加 CSS 选择符的权重** ，CSS选择符的 **权重越高** ，样式 **越不易被覆盖** ，**越容易对其他选择符产生影响** 。所以，除非确定HTML结构非常稳定，一定不会再修改了，否则 **尽量不要使用子选择器** 。为了保证样式容易被覆盖，提高可维护性， **CSS选择符权重需要尽可能低** 。新添加class比使用子选择器更利于维护。_

来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.5 低权重原则— 避免滥用子选择器）**、[CSS 选择器权重计算规则]

[CSS 选择器权重计算规则]:https://www.cnblogs.com/dq-Leung/p/4213375.html


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

### 描述css reset的作用和用途？

**什么 CSS reset 呢？**

> **HTML 标签** 在浏览器里 **有默认的样式** ，例如 p 标签有上下边距，strong 标签有字体加粗样式，em 标签有字体倾斜样式。**不同的浏览器的默认样式之间会有差别** ，例如 ul 默认带有缩进的样式，在 IE 下，它的缩进是通过 margin 实现的，而在 Firefox 下，它的缩进却是由 padding 实现的。在切换页面的时候，**浏览器的默认样式往往会给我们带来麻烦** ，**影响开发效率** 。现在流行的 **解决办法是** 一开始就 **将浏览器的默认样式全部去掉** ，更确切的说，应该 **通过重新定义标签的样式** ，“ **覆盖** ”掉浏览器提供的 **默认样式** ，这就是 **CSS ret** 。

**CSS reset 的使用**

> 将常用的标签显式地罗列出来，避免使用 “ * ”。

YUI（雅虎的前端框架,详见：http://developer.yahoo.com/yui/ ）CSS reset
```
/*CSS ret*/
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre, 

form,fieldset,input,textarea,p,blockquote,th,td { 
    padding: 0; 
    margin: 0; 
} 
table { 
    border-collapse: collapse; 
    border-spacing: 0; 
} 
fieldset,img { 
    border: 0; 
} 
address,caption,cite,code,dfn,em,strong,th,var { 
    font-weight: normal; 
    font-style: normal; 
} 
ol,ul { 
    list-style: none; 
} 
caption,th { 
    text-align: left; 
} 
h1,h2,h3,h4,h5,h6 { 
    font-weight: normal; 
    font-size: 100%; 
} 
q:before,q:after { 
    content:”; 
} 
abbr,acronym { 
    border: 0; 
}
```
来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.3 推荐的base.css）**

### 简述一下你对HTML语义化的理解？什么是语义化的HTML？　

> 使用 **语义化元素** 书写网页，让网页传达出额外的 **结构化信息** 。

- **什么是语义元素呢？**

> 语义元素 **为它们标注的内容赋予额外的含义** ，**让网页的结构更加清晰** ，语义元素都有一个显著的特点： **不真正做任何事** 。

- **为什么要使用语义化元素呢？**

  - 容易修改和维护

    通过 **HTML5的语义元素** ，通过标记就可以传达出 **额外的结构化信息** 。

  - 无障碍性

    现代Web设计的一个重要主题，就是 **让任何人都能无障碍的访问网页** 。换句话说，要让使用屏幕阅读器和其他辅助工具的人都能够在页面中自由导航。兼容HTML5的无障碍工具可以为残疾人士提供更好的上网体验。（仅举一个例子，有了 `<nav>` 元素，屏幕阅读器就能迅速返回导航区，进而找到网页链接。）

    _Web无障碍性的最佳实践，可以访问WAI（Web Accessibility Initiative, Web无障碍倡议）的网站：www.w3.org/WAI 。或者，要了解通过屏幕阅读器上网是一种什么样的的感觉（同时理解为什么标题要排列适当），可以看看YouTube的这段视频：http://tinyurl.com/6bu4pe 。_

  - 搜索引擎优化

    如果搜索引擎能更好的理解你的站点，那 **搜索者的查询就会越容易与你的内容匹配** ，因而你的网站列在搜索结果中的可能性也就越大。

  - 未来功能

    **新浏览器**和 **Web编辑器工具** 一定会 **利用语义元素** 。比如，浏览器可以提供一个页面内容的纲要，以方便跳转到页面适当的区块。类似地，网页设计工具也能包含一些方便你构建或编辑菜单的功能，而方法就是组织你放在 `<nav>` 区块中的内容。

  _最关键的问题在于，如果正确地使用了语义元素，就能够 **创建更加清晰的页面结构 ** ，就能 **适应未来的浏览器和Web设计工具的发展趋势** 。_

来自—— **（美）麦克唐纳（Matthew MacDonald，M.）著；李松峰，朱魏，刘帅译.HTML5秘籍[M].人民邮电出版社，2017.10.（2.1 语义元素）**

### 说说你对HTML5认识?

- **什么是HTML5**

> HTML5 是下一代的 HTML。HTML5 将成为 HTML、XHTML 以及 HTML DOM 的新标准。HTML 的上一个版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变。HTML5 仍处于完善之中。然而，大部分现代浏览器已经具备了某些 HTML5 支持。

> **HTML5 添加了许多新的语法特征** ，其中包括 `<video>` 、`<audio>` ，和 `<canvas>`   元素，同时集成了 SVG 内容。这些元素是 **为了更容易的在网页中添加和处理多媒体和图片内容而添加的** ，其它新的元素包括 `<section>`，`<article>` , `<header>`, 和 `<nav>`,是 **为了丰富文档的数据内容** 。新的属性的添加也是为了同样的目的。同时也 **有一些属性和元素被移除掉了** 。一些元素，像 `<a>`, 和 `<menu>`  **被修改，重新定义或标准化了** 。同属 **APIs** 和 **DOM**  已经成为 **HTML** 中的 **基础部分** 了。**HTML5** 还定义了 **处理非法文档的具体细节** ，使得 **所有浏览器和客户端程序** 能够 **一致地** 处理语法错误。

- **HTML5三个主要原理**

  - 不破坏Web
  
    **标准不该引入导致已有网页无法工作的改变。**
    
    **HTML5规范** 包括 **两部分** 。第一部分，**面向Web开人员** ，要求 **摒弃过去的那些坏习惯和被摒弃的元素** 。第二部分,针对的是 **浏览器开发商** ，它们需要 **支持HTML中存在的一切** ，以做到 **向后兼容** 。
    
  - 修补牛蹄子路
  
    **牛蹄子路** 指的是 **高低不平** 但 **使用频率很高的路** ，通过它可以从一个地方到另一个地方。
  
    **HTML5标准化了这些非官方（但广泛应用）的技术。**
  
  - 实用至上
  
    改变应该以实用为目的。改变越多，代价也就越大。
    
    **HTML标准加入官方的支持** ，让 **功能** 在 **所有浏览器** 中 **都能一致的工作** 。
    
    让网站 **不依赖插件** 也能够提供 **视频** 、**丰富的交互功能** 以及 **各种漂亮的效果** 。
  
- **HTML5的规则**

  - 新特性应该基于 HTML、CSS、DOM 以及 JavaScript。
  
  - 减少对外部插件的需求（比如 Adobe Flash、Microsoft Silverlight, 与 Oracle JavaFX 的需求）。
  
  - 更优秀的错误处理。
  
  - 更多取代脚本的标记。
  
  - HTML5 应该独立于设备。
  
  - 开发进程应对公众透明。
  
- **HTML5的新特性**

  - 用于绘画的 canvas 元素。
  
  - 用于媒介回放的 video 和 audio 元素。
  
  - 对本地离线存储的更好的支持。
  
  - 新的特殊内容元素，比如 article、footer、header、nav、section。
  
  - 新的表单控件，比如 calendar、date、time、email、url、search。

  详细内容可查看站内文章—— **HTML5的新特性**

来自—— **Web 前端开发实战教程（2014版）V1.8[M].教材用书，2014.（8.1 什么是HTML5）**、**（美）麦克唐纳（Matthew MacDonald，M.）著；李松峰，朱魏，刘帅译.HTML5秘籍[M].人民邮电出版社，2017.10.（1.2 HTML5的三个主要原理）**、[HTML 5 简介]

[HTML 5 简介]:http://www.w3school.com.cn/html5/html_5_intro.asp


### 列出display的值，说明他们的作用？

 > display 属性规定元素应该生成的框的类型。

 - **说明**

  这个属性用于定义建立布局时元素生成的显示框类型。对于 HTML 等文档类型，如果使用 display 不谨慎会很危险，因为可能违反 HTML 中已经定义的显示层次结构。对于 XML，由于 XML 没有内置的这种层次结构，所有 display 是绝对必要的。

  _注释：CSS2 中有值 compact 和 marker，不过由于缺乏广泛的支持，已经从 CSS2.1 中去除了。_

- **可能的值**

  ![display属性值](https://graphbed.qiniu.songxingguo.com/web-interview/display%E7%9A%84%E5%B1%9E%E6%80%A7%E5%80%BC.png)

	!-- | 值	       | 描述       |
| :--------    | :-------- |
	none	       | 此元素 **不会被显示** 。|
	block	       | 此元素将显示为 **块级元素** ，**此元素前后会带有换行符**。|
	inline	       | **默认** 。此元素会被显示为**内联元素** ，**元素前后没有换行符**。|
|inline-block  | **行内块元素**。（CSS2.1 新增的值）|
|list-item     | 此元素会作为 **列表** 显示。|
	run-in	       | 此元素会根据 **上下文** 作为 **块级元素** 或 **内联元素** 显示。|
|compact       | CSS 中有值 compact，不过由于缺乏广泛支持，**已经从 CSS2.1 中删除**。|
	marker	       | CSS 中有值 marker，不过由于缺乏广泛支持，**已经从 CSS2.1 中删除**。|
|table         | 此元素会作为 **块级表格** 来显示（类似 `<table>`），表格前后带有换行符。|
|inline-table  | 此元素会作为 **内联表格** 来显示（类似 `<table>`），表格前后没有换行符。|
|table-row-group| 此元素会作为 **一个或多个行的分组** 来显示（类似 `<tbody>`）。|
|table-header-group | 此元素会作为 **一个或多个行的分组来** 显示（类似 `<thead>`）。|
	table-footer-group	|此元素会作为 **一个或多个行的分组** 来显示（类似 `<tfoot>`）。|
	table-row	  | 此元素会作为一个 **表格行** 显示（类似 `<tr>`）。|
	table-column-group	 |此元素会作为 **一个或多个列的分组** 来显示（类似 `<colgroup>`）。|
	table-column |	此元素会作为一个 **单元格列** 显示（类似 `<col>`）|
	table-cell	  | 此元素会作为一个 **表格单元格** 显示（类似 `<td>` 和 `<th>`）。|
|table-caption | 此元素会作为一个 **表格标题** 显示（类似 `<caption>`）。|
	inherit	  | 规定应该 **从父元素继承 display 属性的值** 。| -->

来自—— [CSS display 属性]

[CSS display 属性]: http://www.w3school.com.cn/cssref/pr_class_display.asp

### CSS选择符有哪些？哪些属性可以继承？优先级算法如何计算？内联和important哪个优先级高？

- 为文档应用样式的 3 种方式

  - 内联样式（Inline Style）
  
    > 只需直接在HTML元素里加入style参数，在style的内容就是需要设置是CSS的属性和值。并且内联样式只对所在的标签有效。
  
      ```
      <p style="font-family:'宋体';color:#000;font-size；12px;">
            内联样式只对P标签有效
      </p>
      ```

     _**style** 参数虽然可以 **使用于任意 body 内的元素**（包括 body 本身），但是却 **不能** 使用在 **basefont** 、**param** 和 **script** 中。_

     _**内联样式** 虽然使用起来非常的 **方便** 、**直观** ，但是因为它是在 HTML 中直接使用，**与 HTML 文档结构有强耦合**  ，所以使得 **页面的样式调整和页面的结构无法分离开** ，因此也给维护人员 **修改页面样式带来了极大的不方便** 。_


  - 内部样式（Embedding Style）

  > 通常 **定义在HTML文档中 `<head>``</head>` 标签中** ，或者 **定义在 `<head>`与`<body>`标签之间** 。其实，style 元素和 script 元素相同，是可以定义在HTML文档中的任意位置。但碍于规范，最后还是定义在head元素内，便于统一的管理。内部样式和内联样式一样，均是使用 style 参数实现，不同之处，内部样式 **使用在 `<style>` 与 `</style>` 标签之间** ，HTML 的各种元素样式规则均定义在 style 标签间。

  ```
  <head>
    <style type="text/css">
      p {
        font-family: "宋体";
        font-size: 12px;
        color: #000000;
      }
    </style>
  </head>
  <body>
    <p>内部样式</p>
    <p>内部样式</p>
    <p>内部样式</p>
    <p>所有p元素的样式均相同</p>
  </body>
  ```
 _上面的 type="text/css" 是用来指明 **文档属性为 CSS 样式表** ，也可以省略不写。HTML 文档中的 **所有 p 元素的样式全部统一** ，均为 12 像素黑色字体。这就是 **内联样式与内部样式最大的区别** 。_

 _内部样式实际上是 **将 CSS 样式写在了 HTML 中** ，与 HTML 有强耦合，那么就需要一个文档一个文档的修改，增加了维护人员的工作量。所以 **对于较大较复杂的网页，最好还是不要使用内部样式** 。但 **相较于简单的网页** ，使用内部样式就显得更加的 **方便** 、**直观** 。而且，**对于网页进行调试** ，查看效果时，我们也可以使用内部样式。因为这样更加的 **快捷** 。_

 - 外部样式（Link Style）

 > 外部样式表，是 **一种保存在外部的 CSS 样式表文件** ，是扩展名为 **.css**  的外部文件。外部样式方式同导入样式表的方式相似。它们都是 **单独存放在外的** ，**使用时** ，只需 **在 HTML 文档中引入该样式表** 。要在 HTML 文档中引入外部样式表，只需在 head 元素中使用 **link 元素** ， link 元素的值为要引入的样式表的名称。

 ```
 <head>
   <link rel="stylesheet" type="text/css" href="style.css"/>
 </head>
 ```
 _可选的 type 属性用来指定媒体类型为 text/css, **text-css** 是一个 **层叠样式表** ，**它允许浏览器忽略它们不支持的样式表类型** 。 **rel 属性** 用来 **定义外部的文件与 HTML 文档之间的关系** ，这里的 rel="stylesheet" 定义一个固定的或者首选的样式。需要注意的是，外部样式表不能包含任何的 HTML 元素，外部样式表只能 **由样式规则或声明组成** 。_

 **层叠顺序：(优先级由高到低）**

 > **内联样式** > **内部样式** > **外部样式表** > **浏览器中的样式声明**（缺省值）

来自—— **Web 前端开发实战教程（2014版）V1.8[M].教材用书，2014.（3.3 为文档应用样式的 3 种方式）**

### IE6的双倍边距BUG指的是什么？怎么解决？

> 在 **IE6**  下，如果对元素 **设置了浮动** ，同时 **又设置了 margin-left 或者 margin-right** ，**margin值会加倍** 。

**解决方法：**

> 解决这个 Bug 的方法就是设置 **display:inline** 。

来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（4.3 推荐的base.css）**

### 前端页面由哪三层构成，分别是什么？作用是什么？

> 最权威、最详细的关于Web标准的信息，可以访问官方网站：https://www.w3.org/

> **Web 标准**由 **一系列标准组合而成** ，其核心理念就是将网页的 **结构** （HTML 或 XHTML）、**样式** （CSS）和 **行为**（JavaScript） 分离开来，所以它可以分为三大部分：**结构标准** 、**样式标准** 和 **行为标准** 。**结构标准** 包括 **XML 标准** 、**XHTML 标准**、**HTML 标准**；**样式标准** 主要是指 **CSS 标准**；**行为标准** 主要包括 **DOM 标准** 和 **ECMAScript 标准** 。

_**一个符合标准的网页** ，标签中的 **标签名** 应该 **全部都是小写的** ，**属性** 要 **加上引号**，**样式和行为不再夹杂在标签中** ，而应该分别单独存放在样式文件和脚本文件中。理想状态下，**网页源代码由三部分组成** ：**html文件** 、**.css文件** 和 **.js文件** 。**标签中混有样式和行为的写法是不推荐的** 。_

来自—— **曹刘阳.编写高质量代码：Web前端开发修炼之道[M].机械工业出版社，2016.6.（1.2 Web标准— 结构、样式和行为的分离）**

### CSS中margin和padding的区别

- **内边距**（padding）
  
  元素的内边距在边框和内容区之间，控制该区域最简单的属性是 padding 属性。padding 属性允许接受 **长度值** 或 **百分比值** ，但 **不允许** 使用 **负值** 。
  可以按照上、右、下、左的顺序（顺时针）**分别设置各边的内边距** ，各边均可以使用不同的单位或百分比：
  
  ```
  h1 {
    padding: 8px 20% 0.5em 8pt;
  }
  ```
  也可以通过使用 padding-top、padding-right、padding-bottom、padding-left四个单独的属性，**分别设置上、右、下、左内边距** ：
  
  ```
  h1 {
    padding-top: 8px;
    padding-right: 20%;
    padding-bottom: 0.5em;
    padding-left: 8pt;
  }
  ```
  可以为元素的内边距设置百分数值。而百分数值是 **相对于其父元素的 width 计算的** 。所以，如果父元素的 width 改变，它们也会改变。
  
  特别注意：上下内边距与左右内边距一致；即 **上下内边距的百分数** 会相对于 **父元素宽度设置** ，而 **不是相对于高度** 。
  
- 边框（border）

 元素的边框是 **围绕元素内容和内边距** 的 **一条或多条线** 。使用 border 属性来定义。在HTML中，使用表格来创建文本周围的边框，但是通过使用 border 属性，可以创建出效果出色的边框，并且 **可以应用于任何元素** 。每个边框有 3 个方面：宽度、样式以及颜色。

 CSS 规范指出，**边框绘制在“元素的背景之上”** 。这很重要，因为有些边框是“间断的”（例如点线边框或虚线边框），元素的背景应当出现在边框的可见部分之间。CSS2指出背景只延伸到内边距，而不是边框。后来CSS2.1进行了更正：**元素背景是内容、内边距和边框区的背景** 。大多数浏览器都遵循 CSS2.1 定义，不过一些较老的浏览器可能会有不同的表现。

  - 边框样式

    使用 border-style 属性用于 **设置元素所有边框样式** ，或者 **单独地为各边设置边框样式** 。**只有当这个值不是 none 时边框才可能出现** 。

    为一个边框定义多个样式：

    ```
    .aside {
      border-style: solid dotted dashed double;
    }
    ```
    为元素框的某一个边设置边框样式：

    ```
    .aside {
      border-top-style: solid;
      border-right-style； dotted;
      border-bottom-style: ddashed;
      border-left-style: double;
    }
    ```
   - **可能的值**
  
     ![border-style属性值](https://graphbed.qiniu.songxingguo.com/web-interview/border-style%E5%B1%9E%E6%80%A7%E5%80%BC.png)
   
     _最不可预测的边框样式是 double。它定义为两条线的宽度再加上这两条线之间的空间等于 border-width 值。不过，CSS 规范并没有说其中一条线是否比另一条粗或者两条线是否应该是一样的粗，也没有指出线之间的空间是否应当比线粗。所有这些都有用户代理决定，创作人员对这个决定没有任何影响。_
   
     _**特别注意：** 所有主流的浏览器都支持 border-style 属性，但任何版本的 Internet Explorer（包括 IE8）都不支持属性值“inherit” 或 “hidden”。_
  
  - 边框的宽度
   
    通过设置 border-width 属性为边框指定宽度，border-width 简写属性 **为元素所有边框设置** ，或者 **单独地为各边框设置宽度** ，但 **只有当边框样式不是 none 时才起作用** ，**如果边框样式是 none** ，**边框宽度** 实际上 **会被重置为 0** ，**不允许指定负长度值** 。
   
    为边框指定宽度有两种方法：可以指定 **长度值** ，比如 2px 或 0.1em；或者  **使用 3 个关键字之一** ，它们分别是 **thin** 、 **medium**（默认值）和 **thick** 。
   
    _**特别注意：** CSS 没有定义 3 个关键字的具体宽度，所以一个用户代理可能把 thin 、medium 和 thick 分别设置为等于 5px、3px 和 2px, 而另一个用户代理则分别设置为 3px、 2px 和 1px。_
   
    按照 top-right-bottom-left的顺序设置元素的各边边框：
   
     ```
     p {
       border-style: solid;
       border-width: 15px 5px 15px 5px;
     }
     ```
     也可以简写为（这种写法称为 **值复制** ）：

     ```
     p {
       border-style: solid;
       border-width: 15px 5px;
     }
     ```
     可以分别设置边框各边的宽度：

     ```
     p {
       border-style: solid;
       border-top-width: 15px;
       border-right-width: 5px;
       border-bottom-width: 15px;
       border-left-width: 5px;
     }
     ```
 - 边框的颜色
   
   设置边框的颜色非常简单，CSS使用一个简单的 border-color 属性，它一次可以最多接受 4 个颜色值。border-color 属性是一个简写属性，可以 **设置一个元素的所有边框中可见部分的颜色** ，或者 **为 4 个边分别设置不同的颜色** 。但要记住，**边框的样式不能为 none 或 hidden** ，**否则边框不会出现** 。
   
   **特别注意：** 请始终 **把 border-style 属性声明到 border-color 属性之前** ，元素必须在改变其颜色之前获的边框。
   
   可以使用任何类型的颜色值，例如可以是 **命名颜色** ，也可以是 **十六进制** 和 **RGB 值** ：
   
   ```
   p {
     border-style: solid;
     border-color: blue rgb(25%，35%， 45%) #909090 red;
   }
   ```
   如果 **颜色值少于 4 个** ，**值复制就会起作用** 。例如下面的规则声明了段落的上边框是蓝色，左右边框是红色。
   
   ```
   p {
     border-style: solid;
     border-color: blue red;
   }
   ```
   另外还有一些单边边框颜色属性，它们的原理与单边样式和宽度属性相同：
   
   ```
   p {
     border-style: solid;
     border-top-color: blue;
     border-right-color: rgb(25%, 35%, 45%);
     border-bottom-color: #909090;
     borer-left-color: red;
   }
   ```
   
- **外边距**（margin）

   **围绕在元素边框的空白区域是外边距** ，设置外边距会 **在元素外创建额外的“空白”** 。而设置外边距最简单的方法就是使用 margin 属性，这个属性接受 **任何长度单位** 、**百分数值** 甚至 **负值** ，可以是 **像素** 、**英寸** 、**毫米**  或 **em** 。这个简写属性可以设置为 auto，但更常见的做法是设置一个元素所有边距的宽度，或者各边上外边距的宽度。
   
   **边距重叠：** **块级元素** 的 **垂直相邻外边距会合并** ，并且将 **使用较大的外边距作为最终的边距** 。而 **行内元素实际上不占上下外边距** 。**行内元素的左右边距不会合并** 。同样地，**浮动元素的外边距也不会合并** 。允许 **指定负的外边距值** ，不过使用时要小心。   
   
     为 h1 元素的四个边分别定义了不同你的外边距：

     ```
     h1 {
       margin: 10px 20px 30px 40px;
     }
     ```
    外边距的设置与内边距的设置相同，这些值的顺序是从上外边距（top）开始围着元素顺时针旋转的：
   
    top -> right -> bottom -> left
   
    另外，还可以为margin设置一个百分比数值：

     ```
     h1 {
       margin: 10%;
     }
     ```
    百分数是 **相对于父元素的 width 计算的** ，上面这个例子为 h1 元素 设置的外边距是其父元素 width 的 10% 。
   
    **margin 的默认值是 0** ， 所以如果没有为 margin 声明一个值，就不会出现外边距。但是，在实际中，浏览器许多元素已经提供了预定样式，外边距也不例外。例如，在支持 CSS 的浏览器中，外边距会为每个段落元素的上面和下面生成“空行”。因此，如果没有为 h1 元素声明外边距，浏览器可能会自己应用一个外边距。当然，**只要你特别做了声明** ，**就会覆盖默认样式** 。
   
  下面将讲解 **如何使用值复制** 。有时，会输入一些重复的值：

     ```
     p {
       margin: 10px 15px 10px 15px;
     }
   ```
  **通过值复制，可以不必重复的输入这对数字** ，上面的规则可以简写如下：
   
   ```
     p {
       margin: 10px 15px;
     }
     ```
  这两个值可以 取代 前面 4 个值。这是如何做到的呢？CSS定义了一些规则，允许为外边距指定少于 4 个值。**规则如下** ：

   - **如果缺少左外边距的值，则使用右外边距的值。**
   
   - **如果缺少下外边距的值，则使用上外边距的值。**
   
   - **如果缺少右外边距是值，则使用上外边距的值。**

  利用这个简单的机制，**只需要指定必要的值，而不必全部应用 4 个值** 。但如果只是希望控制元素的单边上的外边距，就得使用如下单外边距属性：

    ```
    p {
      margin-top: 10px;
      margin-right: 15px;
      margin-bottom: 10px;
      margin-left: 15px;
    }
    ```
  _NetScape 和 IE 对 body 标签定义的默认边距（margin）值是 8px,因此 Opera 不是这样。相反地，OPera 将内部填充（padding）的默认值定义为 8px，因此如果希望对整个网站的边缘部分进行调整，并将之正确显示于 Opera 中，那么必须对 body 的 padding 进行自定义。_

来自—— **Web 前端开发实战教程（2014版）V1.8[M].教材用书，2014.（3.7 理解盒模型）**

### `<img>` 标签上title与alt属性的区别是什么？

- alt属性

  > 在你的 **图片** 因为某种原因 **不能加载时** 在 **页面显示的提示信息** ，**它会直接输出在原本加载图片的地方**。

- title属性

  > 在 **鼠标悬停在该图片上时显的小提示** ，鼠标离开就没有了，**有点类似   jQuery 的 hover** ，另外，**HTML 的绝大多数标签都支持 title 属性** ，**title** 属性就是 **专门做提示信息的** 。

_虽然 alt 也有 title 的功能，但是只是在低版本的ie浏览器才支持，高版本及标准浏览器不支持这个功能了。_

来自—— [HTML语言中img标签的alt属性和title属性的作用与区别]

[HTML语言中img标签的alt属性和title属性的作用与区别]: https://zhidao.baidu.com/question/263730425037686525.html

### 页面导入样式时，使用link和@import有什么区别？

> link 和 @import 是导入外部样式的两种方式。

- link 语法

  ```
  <link rel="stylesheet" type="text/css" href="css文件" media="all">
  ```
  _这个代码中有一个media，是用来制作响应式网页的，media=“all” 是用于所有设备， media=“screen” 用于电脑屏幕，平板电脑、智能手机场景，举个例子：_
  
   ```
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <title>Examples</title>
  <meta name="description" content="">
  <meta name="keywords" content="">
  <link href="" rel="stylesheet">
  <style>
      @media screen and (max-width:800px){
          body{
              background-color: yellow;
          }
      }
  </style>
  </head>
  <body>
     <h1> hello</h1>
  </body>
  </html>
   ```
   _这段代码表示当屏幕的宽度小于等于800像素的时候，页面背景是黄色。_
  
- @import 语法

  ```
  <style type="text/css" media="screen">
    @import url("css文件");
  </style>
  ```

**两者的区别：**

- 从属关系   

  > **link** 是 XHTML 标签，除了 **可以引用css样式外还可以定义RSS等事物** ，但 **@import** 是 css 标签，**只能引用 css 样式** 。

- 加载的顺序 
  
  > **link** 在页面加载的 **同时加载** （需要等待），而 **@import** 是 **在页面内容加载完成之后加载的** 。

- 兼容性的区别 

  > **link** 是 XHTML标签，**没有兼容问题** ，而 **@import** 是在 css2.1 提出来的，**低版本的浏览器不支持** 。

- DOM可控性的区别 

  > **link** 支持使用JavaScript 控制 DOM去 改变样式 ，@import **不支持** 。

- 权重的区别

  > **link 引入的样式权重大于 @import 引入的样式。** 换句话说就是在 link 标签中引入的 css 文件中使用 @import 时，相同样式将被该 css 文件本身的样式覆盖。

_建议最好不要使用 @improt，如果 @import 加载的样式比较大，出现加载延迟，就可能会发生闪屏的问题。_

来自—— [link和@import区别]、[link 与@import的区别]、[link 和 @import区别]

[link和@import区别]: https://www.jianshu.com/p/0010338621b9
[link 与@import的区别]: https://blog.csdn.net/YANGJIAHAO666/article/details/55254735
[link 和 @import区别]: https://blog.csdn.net/qq_41047322/article/details/81179254

### 如何居中一个浮动元素？

- 宽度不固定的浮动元素

  > 

  下面是 HTML 代码：

    ```
     <div class="outerbox">
      <div class="innerbox">我是浮动的</div>
     </div>
    ```
   CSS 代码：
  
    ```
    .outerbox{
      float:left; 
      position:relative; 
      left:50%; 
    } 
    .innerbox{ 	
      float:left; 
      position:relative; 
      right:50%; 
    }
    ```
   ![浮动元素居中](https://graphbed.qiniu.songxingguo.com/web-interview/%E6%B5%AE%E5%8A%A8%E5%85%83%E7%B4%A0%E5%B1%85%E4%B8%AD)
  
- 宽度固定的浮动元素

   > 在浮动元素外部包裹一层 div，将外层的 div 居中，浮动元素也就居中了。
  
    下面是 HTML 代码：

    ```
    <div class="outerbox">
      <div>我是浮动的</div>
    </div>
    ```
    CSS 代码：

    ```
    .outerbox{
      background-color:pink; /*方便看效果 */  
      width:500px ; 
      height:300px; /*高度可以不设*/
      margin: -150px 0 0 -250px; /*使用marin向左移动250px，保证元素居中*/
      position:relative;   /*相对定位*/
      left:50%;
      top:50%;
    }
    ```
   ![固定宽带的浮动元素居中](https://graphbed.qiniu.songxingguo.com/%E6%B5%AE%E5%8A%A8%E5%85%83%E7%B4%A0%E5%B1%85%E4%B8%AD-%E5%AE%BD%E5%B8%A6%E5%9B%BA%E5%AE%9A)

- 让绝对定位的元素水平居中对齐 

   ```
   .center{
     position: absolute; /*绝对定位*/
     width: 500px;
     height:300px;
     background: red;
     margin: 0 auto; /*水平居中*/
     left: 0; /*此处不能省略，且为0*/
     right: 0; /*此处不能省略，且为0*/
   }
   ```
   
**水平居中的主要属性有:**

  - text-alin:center;

  - margin:0 auto;

  - position:relative|absolute; left:50%;


来自—— [Web前端面试指导(十四)：如何居中一个元素（正常、绝对定位、浮动元素）?]

[Web前端面试指导(十四)：如何居中一个元素（正常、绝对定位、浮动元素）?]:https://blog.csdn.net/lxcao/article/details/52670724

### 利用@media screen实现网页布局的自适应。

- 了解 Media Queries

  >**Media Queries** ，其作用就是 **允许添加表达式用以媒体查询** ，**以此来选择不同的样式表** ，**从而自动适应不同的屏幕分辨率** 。

  _**CSS2** 里面虽然支持 @media 属性，但是能实现的 **功能比较少** ，一般 **只用做打印的时候做特殊定义的CSS** ，我们不去讨论。_

  _**CSS3** 的 @media 属性在 CSS3 里面已经演变成一种 **media queries**（媒体查询/匹配）了,在 CSS3 里面，**可以用查询语句来匹配各种类型的屏幕** 。_

- @media 与 @media screen 区别

  > @media screen 的 CSS 在打印设备里是无效的，而 @media 在打印设备里是有效的，如果css需要用在打印设备里，那么就用 @media ，否则，就用@media screen。

  _不过这只是笼统的做法，其实如果把“screen”换为“print”，写为@media print，那么该css就可用到打印设备上了，但要注意，@media print声明的css只能在打印设备上有效。_
  
- Media Queries工作方式:

  在media属性里：

  - **screen** 是媒体类型里的一种，CSS2.1定义了10种媒体类型。
  
  - **and** 被称为关键字，其他关键字还包括 **not** (排除某种设备)，**only** (限定某种设备)。
  
  - (min-width: 400px) 就是 **媒体特性** ，其被放置在一对圆括号中。完整的特性参看相关的Media features部分。

  1. 一种是直接在link中判断设备的尺寸，然后引用不同的css文件：

      ```
      <link rel="stylesheet" type="text/css" href="styleA.css" media="screen and (min-width: 400px)">
      ```
        意思是当屏幕的宽度大于等于 400px 的时候，应用 styleA.css。

      ```
      <link rel="stylesheet" type="text/css" href="styleB.css"  media="screen and (min-width: 600px) and (max-width: 800px)">
      ```
        意思是当屏幕的宽度大于 600 小于 800 时，应用 styleB.css。

  2. 另一种方式，即是直接写在 style 标签里：

      ```
      @media screen and (max-width: 600px) { /*当屏幕尺寸小于600px时，应用下面的CSS样式*/
        .class {
          background: #ccc;
        }
      }
      ```
        写法是前面加 @media，其它跟 link 里的 media 属性相同。


来自—— [利用@media与@media screen进行响应式布局]、[CSS3 @media 查询]、[利用@media screen实现网页布局的自适应]

[利用@media与@media screen进行响应式布局]:http://www.511yj.com/media-media-screen.html
[CSS3 @media 查询]:http://www.runoob.com/cssref/css3-pr-mediaquery.html
[利用@media screen实现网页布局的自适应]:https://www.cnblogs.com/xcxc/p/4531846.html

### 对BFC规范的理解

- 常见定位方案

  > 常见的定位方案，定位方案是控制元素的布局，有三种常见方案。

  - 普通流 (normal flow)

    > 在普通流中，元素按照其在 HTML 中的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则会被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在 HTML 文档中的位置决定。

  - 浮动 (float)

    > 在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。

  - 绝对定位 (absolute positioning)

    > 在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响，而元素具体的位置由绝对定位的坐标决定。

- BFC 概念

  > Formatting context(格式化上下文) 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。BFC 和 IFC 都是常见的 FC。分别叫做 Block Fomatting Context 和 Inline Formatting Context。

  **那么 BFC 是什么呢？**

  > BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。
  
  具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

   通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。
  
- 触发 BFC

 只要元素满足下面任一条件即可触发 BFC 特性：

 -  body 根元素
  
 - 浮动元素：float 除 none 以外的值
  
 - 绝对定位元素：position (absolute、fixed)
  
 - display 为 inline-block、table-cells、flex
  
 -  overflow 除了 visible 以外的值 (hidden、auto、scroll)
 
- BFC 特性及应用

 1. 同一个 BFC 下外边距会发生折叠；

    > 这不是 CSS 的 bug，我们可以理解为一种规范，如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。
    
    > 盒子垂直方向的距离由margin决定，属于同一个BFC的两个相邻Box的上下margin会发生重叠；
  
 2. BFC 可以包含浮动的元素（清除浮动）；

    > 浮动的元素会脱离普通文档流，由于容器内元素浮动，脱离了文档流，所以容器只剩下 2px 的边距高度。如果使触发容器的 BFC，那么容器将会包裹着浮动元素。

 3. BFC 可以阻止元素被浮动元素覆盖；

    > 这个方法可以用来实现两列自适应布局，效果不错，这时候左边的宽度固定，右边的内容自适应宽度(去掉上面右边内容的宽度)。
    
    > BFC的区域不会与float重叠。
    
 4. 内部的盒子会在垂直方向，一个个地放置；

 5. 每个元素的左边，与包含的盒子的左边相接触，即使存在浮动也是如此；

 6. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也如此；

 7. 计算BFC的高度时，浮动元素也参与计算。
 
- 触发条件

  - 根元素
  - positon: absolute/fixed
  - display: inline-block / table
  - float 元素
  - ovevflow !== visible
 
- 规则

  - 属于同一个 BFC 的两个相邻 Box 垂直排列
  - 属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
  - BFC 中子元素不会超出他的包含块
  - BFC 的区域不会与 float 的元素区域重叠
  - 计算 BFC 的高度时，浮动子元素也参与计算
  - 文字层不会被浮动层覆盖，环绕于周围

来自—— [10 分钟理解 BFC 原理]、[浅谈BFC和IFC]、[前端精选文摘：BFC 神奇背后的原理]、[什么是BFC]

[10 分钟理解 BFC 原理]:https://blog.csdn.net/jiaojsun/article/details/76408215
[浅谈BFC和IFC]:https://blog.csdn.net/mevicky/article/details/47008939
[前端精选文摘：BFC 神奇背后的原理]:https://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html
[什么是BFC]:https://www.cnblogs.com/libin-1/p/7098468.html

### 层叠上下文

**触发条件**

- 根层叠上下文(html)
- position
- css3属性
  - flex
  - transform
  - opacticy
  - filter
  - will-change
  - -webkit-overflow-scrolling
  
**层叠等级**

> 层叠上下文在z轴上的排序。

![层叠等级](https://graphbed.qiniu.songxingguo.com/web-front-end-interview-html-css/168e9d9f3a1d368b)

来自—— [层叠上下文]

[层叠上下文]:https://juejin.im/post/5c64d15d6fb9a049d37f9c20

### 说说你对SVG理解?

> SVG 是使用 XML 来描述二维图形和绘图程序的语言。

- 什么是SVG？

  - SVG 指可伸缩矢量图形 (Scalable Vector Graphics)。
  - SVG 用来定义用于网络的基于矢量的图形。
  - SVG 使用 XML 格式定义图形。
  - SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失。
  - SVG 是万维网联盟的标准。
  - SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体。

 _在 2003 年一月，SVG 1.1 被确立为 W3C 标准。参与定义 SVG 的组织有：太阳微系统、Adobe、苹果公司、IBM 以及柯达。_

- 与其他图像格式相比，使用 SVG 的优势在于：

  - SVG 可被非常多的工具读取和修改（比如记事本）
  - SVG 与 JPEG 和 GIF 图像比起来，尺寸更小，且可压缩性更强。
  - SVG 是可伸缩的
  - SVG 图像可在任何的分辨率下被高质量地打印
  - SVG 可在图像质量不下降的情况下被放大
  - SVG 图像中的文本是可选的，同时也是可搜索的（很适合制作地图）
  - SVG 可以与 Java 技术一起运行
  - SVG 是开放的标准
  - SVG 文件是纯粹的 XML
  
  SVG 的主要竞争者是 Flash。

  > 与 Flash 相比，SVG 最大的优势是与其他标准（比如 XSL 和 DOM）相兼容。而 Flash 则是未开源的私有技术。
  
- 查看 SVG 文件

  今天，**所有浏览器均支持 SVG 文件** ，**不过需要安装插件的 Internet Explorer 除外** 。插件是免费的，比如 [Adobe SVG Viewer]。

来自—— [SVG 简介]、[深入理解 SVG 系列（一） —— SVG 基础]、[SVG入门理解]、[svg文档]

[SVG 简介]:http://www.w3school.com.cn/svg/svg_intro.asp
[深入理解 SVG 系列（一） —— SVG 基础]:http://www.cnblogs.com/thelongmarch/p/9332889.html
[SVG入门理解]: https://blog.csdn.net/wf19930209/article/details/80938259
[svg文档]:https://developer.mozilla.org/zh-CN/docs/Web/SVG/Element/svg
[Adobe SVG Viewer]:https://www.adobe.com/devnet/svg.html


### 请简述HTML和XHTML最重要的4点不同？

> XHTML 是更严谨更纯净的 HTML 版本。XHTML 是 HTML 与 XML（扩展标记语言）的结合物。XHTML 包含了所有与 XML 语法结合的 HTML 4.01 元素。

- XHTML 是什么？

  - XHTML 指可扩展超文本标签语言（EXtensible HyperText Markup Language）。
  - XHTML 的目标是取代 HTML。
  - XHTML 与 HTML 4.01 几乎是相同的。
  - XHTML 是更严格更纯净的 HTML 版本。
  - XHTML 是作为一种 XML 应用被重新定义的 HTML。
  - XHTML 是一个 W3C 标准。
  
    > XHTML 于2000年的1月26日成为 W3C 标准。W3C 将 XHTML 定义为最新的HTML版本。XHTML 将逐渐取代 HTML。
  
- 所有新的浏览器都支持 XHTML

  > XHTML 与 HTML 4.01 兼容。所有新的浏览器都支持 XHTML。

- 为什么要使用XHTML？

  - 我们认为万维网上的许多页面都包含着糟糕的 HTML 代码。

    下面的 HTML 代码仍然可以工作得很好，即使它没有遵守 HTML 规则：

      ```
      <html>
      <head>
      <title>This is bad HTML</title>
      <body>
      <h1>Bad HTML
      </body>
      ```



  - 今天的市场中存在着不同的浏览器技术，某些浏览器运行在计算机中，某些浏览器则运行在移动电话和手持设备上。而后者没有能力和手段来解释糟糕的标记语言。

  - XML 是一种标记化语言，其中所有的东西都要被正确的标记，以产生形式良好的文档。XML 用来描述数据，而 HTML 则用来显示数据。因此，通过把 HTML 和 XML 各自的长处加以结合，我们得到了在现在和未来都能派上用场的标记语言 - XHTML。

  - XHTML 可以被所有的支持 XML 的设备读取，同时在其余的浏览器升级至支持 XML 之前，XHTML 使我们有能力编写出拥有良好结构的文档，这些文档可以很好地工作于所有的浏览器，并且可以向后兼容。

- XHTML 与 HTML 之间的差异

  - 最主要的不同：

    - XHTML 元素必须被正确地嵌套。
    
      在 HTML 中，某些元素可以像这样彼此不正确地嵌套：
      
      ```
      <b><i>This text is bold and italic</b></i>
      ```
      在 XHTML 中，所有的元素必须像这样彼此正确地嵌套：
      
      ```
      <b><i>This text is bold and italic</i></b>
      ```
      **提示：** 在嵌套列表中一个容易犯的错误，是忘记内部列表必须位于 li 元 素中，就像下面这样：

      这是错误的：
      
      ```
      <ul>
        <li>Coffee</li>
        <li>Tea
          <ul>
            <li>Black tea</li>
            <li>Green tea</li>
          </ul>
        <li>Milk</li>
      </ul>
      ```
      这是正确的：
      
      ```
      <ul>
        <li>Coffee</li>
        <li>Tea
          <ul>
            <li>Black tea</li>
            <li>Green tea</li>
          </ul>
        </li>
        <li>Milk</li>
      </ul>
      ```
      注意：在正确代码的例子中，我们在 `</ul>` 之后插入了一个 `</li>` 标签。
      
    - XHTML 元素必须被关闭。
    
      **非空标签必须使用结束标签**

      这是错误的：

      ```
      <p>This is a paragraph
      <p>This is another paragraph
      ```
      这是正确的：

      ```
      <p>This is a paragraph</p>
      <p>This is another paragraph</p>
      ```
      **空标签也必须被关闭**
      
      > 空标签也必须使用结束标签，或者其开始标签必须使用/>结尾。
      
      这是错误的：
      
      ```
      A break: <br>
      A horizontal rule: <hr>
      An image: <img src="happy.gif" alt="Happy face">
      ```
      这是正确的：
      ```
      A break: <br />
      A horizontal rule: <hr />
      An image: <img src="happy.gif" alt="Happy face" />
      ```
    - 标签名必须用小写字母。
    
      > XHTML 规范定义：标签名和属性对大小写敏感。
      
      这是错误的：
      ```
      <BODY>
      <P>This is a paragraph</P>
      </BODY>
      ```
      这是正确的：
      ```
      <body>
      <p>This is a paragraph</p>
      </body>
      ```
    - XHTML 文档必须拥有根元素。
    
     > 所有的 XHTML 元素必须被嵌套于 `<html>` 根元素中。其余所有的元素均可有子元素。子元素必须是成对的且被嵌套在其父元素之中。基本的文档结构如下：

      ```
      <html>
      <head> ... </head>
      <body> ... </body>
      </html>
      ```

  更多的 XHTML 语法规则：

    - 属性名称必须小写。
    - 属性值必须加引号。
    - 属性不能简写。
    - 用 Id 属性代替 name 属性。
    - XHTML DTD 定义了强制使用的 HTML 元素。

来自—— [XHTML 教程]

[XHTML 教程]:http://www.w3school.com.cn/xhtml/index.asp

### 什么是WebGL,它有什么优点?

> WebGL 是一项用来在网页上绘制和渲染三维图形并允许用户与之交互的技术。同时，WebGL ( Web 图形库)是一种 JavaScript API，用于在任何兼容的 Web 浏览器中呈现交互式 3D 和 2D 图形，而无需使用插件。WebGL 通过引入一个与 OpenGL ES2.0 紧密相符合的 API，可以在 HTML5 `<canvas>` 元素中使用。


- WebGL有什么优势？

  - webGL内嵌在浏览器中，不需要安装其他插件就可以直接在浏览器中使用。
  - webGL不需要搭建开发环境，文本编辑器就可以开发。
  - 轻松跨平台。
  - webGL基于并继承开源openGL。
  
- 测试当前浏览器是否支持WebGL

  ```
  // 创建一个canvas节点
  <canvas id="canvas" width="400" height="400"></canvas>
  // 判断canvas对象中是否有WebGL上下文
  var canvas = document.getElementById('canvas');
  var gl = canvas.getContext('webgl');
  if(!gl) {
      console.log('当前浏览器版本不支持webgl，请升级或切换浏览器');
  }
  ```
来自—— [WebGL学习(1) — 浏览器支持测试]、[WebGL中文网]、[WebGL技术学习之路]

[WebGL学习(1) — 浏览器支持测试]:https://www.jianshu.com/p/de01dce980d5
[WebGL中文网]:http://www.hewebgl.com/
[WebGL技术学习之路]:https://blog.csdn.net/happyduoduo1/article/details/51831775


### CSS重排和重绘与CSS3动画

CSS3动画时，以下四种情形绘制的效率较高，分别是：

- 改变位置
- 改变大小
- 旋转
- 改变透明度

#### CSS的图层的概念（Chrome浏览器）

浏览器在渲染一个页面时，会将页面分为很多个图层，图层有大有小，每个图层上有一个或多个节点。在渲染DOM的时候，浏览器所做的工作实际上是：

1. 获取DOM后分割为多个图层。
2. 对每个图层的节点计算样式结果（Recalculate style--样式重计算）。
3. 为每个节点生成图形和位置（Layout--回流和重布局）。
4. 将每个节点绘制填充到图层位图中（Paint Setup和Paint--重绘）。
5. 图层作为纹理上传至GPU。
6. 符合多个图层到页面上生成最终屏幕图像（Composite Layers--图层重组）。

Chrome中满足以下任意情况就会创建图层：

- 3D或透视变换（perspective transform）CSS属性
- 使用加速视频解码的`<video>`节点
- 拥有3D（WebGL）上下文或加速的2D上下文的`<canvas>`节点
- 混合插件（如Flash）
- 对自己的opacity做CSS动画或使用一个动画webkit变换的元素
- 拥有加速CSS过滤器的元素
- 元素有一个包含复合层的后代节点（一个元素拥有一个子元素，该子元素在自己的层里）
- 元素有一个z-index较低且包含一个复合层的兄弟元素（换句话说就是该元素在复合层上面渲染）

需要注意的是，如果图层中某个元素需要重绘，那么整个图层都需要重绘。

#### 层和CSS动画

简化一下上述过程，每一帧动画浏览器可能需要做如下工作：

1. 计算需要被加载到节点上的样式结果（Recalculate style--样式重计算）
2. 为每个节点生成图形和位置（Layout--回流和重布局）
3. 将每个节点填充到图层中（Paint Setup和Paint--重绘）
4. 组合图层到页面上（Composite Layers--图层重组）

如果我们需要使得动画的性能提高，需要做的就是减少浏览器在动画运行时所需要做的工作。最好的情况是，改变的属性仅仅印象图层的组合，变换（transform）和透明度（opacity）就属于这种情况。

现代浏览器如Chrome，Firefox，Safari和Opera都对变换和透明度采用硬件加速，但IE10+不是很确定是否硬件加速。

#### 触发重布局的属性

有些节点，当你改变他时，会需要重新布局（这也意味着需要重新计算其他被影响的节点的位置和大小）。这种情况下，被影响的DOM树越大（可见节点），重绘所需要的时间就会越长，而渲染一帧动画的时间也相应变长。所以需要尽力避免这些属性。

##### 一些常用的改变时会触发重布局的属性：

盒子模型相关属性会触发重布局：

* width
* height
* padding
* margin
* display
* border-width
* border
* min-height

定位属性及浮动也会触发重布局：

* top
* bottom
* left
* right
* position
* float
* clear

改变节点内部文字结构也会触发重布局：

* text-align
* overflow-y
* font-weight
* overflow
* font-family
* line-height
* vertival-align
* white-space
* font-size

这么多常用属性都会触发重布局，可以看到，他们的特点就是可能 **修改整个节点的大小或位置** ，所以会触发重布局。

#### 别使用CSS类名做状态标记

如果在网页中使用CSS的类来对节点做状态标记，当这些节点的状态标记类修改时，将会触发节点的重绘和重布局。所以在节点上使用CSS类来做状态比较是代价很昂贵的。

#### 触发重绘的属性

修改时只触发重绘的属性有：

* color
* border-style
* border-radius
* visibility
* text-decoration
* background
* background-image
* background-position
* background-repeat
* background-size
* outline-color
* outline
* outline-style
* outline-width
* box-shadow

这样可以看到，这些属性都不会修改节点的大小和位置，自然不会触发重布局，但是 **节点内部的渲染效果进行了改变** ，所以只需要重绘就可以了。

##### 手机就算重绘也很慢

在重绘时，这些节点会被加载到GPU中进行重绘，这对移动设备如手机的影响还是很大的。因为CPU不如台式机或笔记本电脑，所以绘画巫妖的时间更长。而且CPU与GPU之间的有较大的带宽限制，所以纹理的上传需要一定时间。

#### 触发图层重组的属性

##### 透明度竟然不会触发重绘？

需要注意的是，上面那些触发重绘的属性里面没有opacity（透明度），很奇怪不是吗？实际上透明度的改变后，GPU在绘画时只是简单的降低之前已经画好的纹理的alpha值来达到效果，并不需要整体的重绘。不过这个前提是这个被修改opacity本身必须是一个图层，如果图层下还有其他节点，GPU也会将他们透明化。

##### 强迫浏览器创建图层

在Blink和WebKit的浏览器中，一当一个节点被设定了透明度的相关过渡效果或动画时，浏览器会将其作为一个单独的图层，但很多开发者使用translateZ(0)或者translate3d(0,0,0)去使浏览器创建图层。这种方式可以消除在动画开始之前的图层创建时间，使得动画尽快开始（创建图层和绘制图层还是比较慢的），而且不会随着抗锯齿而导出突变。不过这种方法需要节制，否则会因为创建过多的图层导致崩溃。

##### Chrome中的抗锯齿

Chrome中，非根图层以及透明图层使用grayscale antialiasing而不是subpixel antialiasing，如果抗锯齿方法变化，这个效果将会非常显著。如果你打算预处理一个节点而不打算等到动画开始，可以通过这种强迫浏览器创建图层的方式进行。

##### transform变换是你的选择

我们通过节点的transform可以修改节点的位置、旋转、大小等。我们平常会使用left和top属性来修改节点的位置，但正如上面所述，left和top会触发重布局，修改时的代价相当大。取而代之的更好方法是使用translate，这个不会触发重布局。

来自——[前端知识点分析——重排和重绘]、[重绘和重排]、[前端性能优化（CSS动画篇）]

[前端知识点分析——重排和重绘]:https://blog.csdn.net/sophia_little/article/details/79613990
[重绘和重排]:https://blog.csdn.net/baidu_28479651/article/details/76228216

### JS动画和CSS3动画的比较

#### JS动画

缺点：JavaScript在浏览器的主线程中运行，而其中还有很多其他需要运行的JavaScript、样式计算、布局、绘制等对其干扰。这也就导致了线程可能出现阻塞，从而造成丢帧的情况。

优点：JavaScript的动画与CSS预先定义好的动画不同，可以在其动画过程中对其进行控制：开始、暂停、回放、中止、取消都是可以做到的。而且一些动画效果，比如视差滚动效果，只有JavaScript能够完成

#### CSS动画

缺点：缺乏强大的控制能力。而且很难以有意义的方式结合到一起，使得动画变得复杂且易于出问题。

优点：浏览器可以对动画进行优化。它必要时可以创建图层，然后在主线程之外运行。

#### 前瞻

Google目前正在探究通过JS的多线程（Web Workers）来提供更好的动画效果，而不会触发重布局及样式重计算。

#### 结论

动画给予了页面丰富的视觉体验。我们应该尽力避免使用会触发重布局和重绘的属性，以免失帧。最好提前申明动画，这样能让浏览器提前对动画进行优化。由于GPU的参与，现在用来做动画的最好属性是如下几个：

* opacity
* translate
* rotate
* scale

也许会有一些新的方式使得可以使用JavaScript做出更好的没有限制的动画，而且不用担心主线程的阻塞问题。但在那之前，还是好好考虑下如何做出流畅的动画吧。

### css布局（圣杯布局、双飞翼布局、垂直居中、水平居中）

来自——[六种CSS经典三栏布局方案]、[CSS布局说——可能是最全的]、[CSS经典布局]、[CSS布局十八般武艺都在这里了]、[史上最全Html和CSS布局技巧]、[CSS 常见布局方式]

[六种CSS经典三栏布局方案]:http://www.php.cn/css-tutorial-382041.html
[CSS布局说——可能是最全的]:https://segmentfault.com/a/1190000011358507
[CSS经典布局]:https://blog.csdn.net/Sourcemyx/article/details/79126069
[CSS布局十八般武艺都在这里了]:https://segmentfault.com/a/1190000008789039
[史上最全Html和CSS布局技巧]:https://blog.csdn.net/qq_33834489/article/details/79111172
[CSS 常见布局方式]:https://www.sohu.com/a/168143624_274163

#### CSS3 动画（性能、3D硬件加速）

来自——[前端性能优化（CSS动画篇）]、[css3动画与js动画的区别]、[前端制作动画的几种方式（css3，js）]、[css3动画和JS+DOM动画和JS+canvas动画比较]、[Javascript&CSS3动画]、[css3动画性能优化]、[GPU（图形处理器）]

[前端性能优化（CSS动画篇）]:https://segmentfault.com/a/1190000000490328
[css3动画与js动画的区别]:https://www.cnblogs.com/shuaishuaidejun/p/7444711.html)
[前端制作动画的几种方式（css3，js）]:https://www.cnblogs.com/zhangwenjiajessy/p/6154703.html
[css3动画和JS+DOM动画和JS+canvas动画比较]:https://www.cnblogs.com/ricoliu/p/6361018.html
[Javascript&CSS3动画]:https://www.cnblogs.com/coco1s/category/807452.html
[css3动画性能优化]:https://www.cnblogs.com/sowhite/p/6524689.html
[GPU（图形处理器）]:https://baike.baidu.com/item/%E5%9B%BE%E5%BD%A2%E5%A4%84%E7%90%86%E5%99%A8/8694767?fromtitle=gpu&amp;fromid=105524&amp;fr=aladdin

### CSS 性能


来自——[css优化，js优化以及web性能优化]、[提高 web 应用性能之 CSS 性能调优]、[前端开发规范(三、CSS性能优化)]

[css优化，js优化以及web性能优化]:https://blog.csdn.net/lululul123/article/details/76167861
[提高 web 应用性能之 CSS 性能调优]:https://www.ibm.com/developerworks/cn/web/1109_zhouxiang_optcss/index.html
[前端开发规范(三、CSS性能优化)]:https://baijiahao.baidu.com/s?id=1603253323416106525&amp;wfr=spider&amp;for=pc

### 浏览器渲染页面

来自——[浏览器渲染原理与过程]、[浏览器如何渲染页面？]、[浏览器渲染页面过程与页面优化]

[浏览器渲染原理与过程]:https://www.imooc.com/article/40004
[浏览器如何渲染页面？]:https://blog.csdn.net/jnshu_it/article/details/77150545
[浏览器渲染页面过程与页面优化]:https://segmentfault.com/a/1190000010298038


### 移动端适配 

来自——[H5移动端适配总结]、[移动端前端适配方案对比]、[前端移动端适配总结]

[H5移动端适配总结]:https://www.jianshu.com/p/64877ce6e893
[移动端前端适配方案对比]:https://blog.csdn.net/sinat_17775997/article/details/62416605
[前端移动端适配总结]:https://segmentfault.com/a/1190000011586301

### CSS 响应式设计


来自——[响应式 Web 设计-菜鸟教程]

[响应式 Web 设计-菜鸟教程]:http://www.runoob.com/css/css-rwd-viewport.html

### CSS 属性

- box-sizing（content-box、border-box、inerit）
- display （none、inline、inline-block、block）
- position（relative、absoulate、fixed、static、sticky）


### less、sass 预处理器

来自——[浅谈CSS预处理器SASS、LESS、Stylus]、[sass和less，优秀的前端样式预处理器]

[浅谈CSS预处理器SASS、LESS、Stylus]:https://blog.csdn.net/zhouziyu2011/article/details/67646875

[sass和less，优秀的前端样式预处理器]:https://blog.csdn.net/minidrupal/article/details/29701103

### CSS3新特

[CSS3新特](https://songxingguo.github.io/2018/09/24/CSS3/#CSS3-%E6%A8%A1%E5%9D%97)

### HTML 行内、块级

[HTML 行内、块级](https://www.songxingguo.com/2018/08/06/web-front-end-interview-html-css/#HTML%E5%85%83%E7%B4%A0)

### BFC（独立的布局空间）

[对BFC规范的理解](https://www.songxingguo.com/2018/08/06/web-front-end-interview-html-css/)

### HTML5

[HTML5教程](https://www.songxingguo.com/2018/09/23/HTML5/)

### CSS3新增伪类

[同步请求和异步请求的区别]: https://blog.csdn.net/goodshot/article/details/7244053
[简述同步和异步的区别]:https://www.cnblogs.com/Renyi-Fan/p/7808756.html#_label0_5
[阻塞与非阻塞的区别]:https://www.cnblogs.com/orez88/articles/2513460.html
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
