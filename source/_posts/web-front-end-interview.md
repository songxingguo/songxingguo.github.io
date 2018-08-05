title: Web前端面试题目及详解汇总
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
     box-sizing:content-box;

     /*IE模型*/
     box-sizing:border-box;
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

        > **独占一行** ；默认情况下，其 **宽度自动填满其父元素宽度** ；块级元素 **可以设置width、height属性** ；**可以设置margin和padding属性** 。

        *注意，块级元素即使设置了宽度，仍然是独占一行。*

      - 行内元素

        > **不会独占一行** ，相邻元素排列在同一行，直到排不下才会换行；其宽度  **随元素的内容而变化** ；行内元素 **设置width、height属性无效** ；行内元素的  **margin** 和 **padding** 属性，**水平方向** 的 **padding-left** 、**padding-right** 、**margin-left** 、**margin-right** 都 **产生效果** ，但 **竖直方向** 的 **padding-top** 、**padding-bottom** 、**margin-top** 、**margin-bottom** 却 **不会产生边距效果** 。
     
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


[HTML中href、src区别]: https://blog.csdn.net/annsheshira23/article/details/51133709
[rel、href、src、url的区别]:https://blog.csdn.net/chengshaolei2012/article/details/72847770
[史上最全的CSS hack方式一览]:https://blog.csdn.net/freshlover/article/details/12132801
[CSS hack大全]:https://www.duitang.com/static/csshack.html
[今天才知道css hack是什么]:https://www.cnblogs.com/miniyk/p/3734664.html
[CSS Hack是什么意思？css hack有什么用？]:https://www.w3cschool.cn/css3/question-10231625.html
[你想知道的css hack知识全都帮你整理好了]:https://www.w3cschool.cn/css/css-hack.html
[CSS参考手册]:http://phpstudy.php.cn/css3/