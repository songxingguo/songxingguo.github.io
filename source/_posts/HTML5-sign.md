title: 《HTML5秘籍》学习笔记-编写更有意义的标记
author: songxingguo
tags:
  - HTML
categories:
  - 前端技术
  - 读书笔记
date: 2018-07-25 08:50:00
---
### 编写更有意义的标记

  使用微数据（microdata）,解决文件级语义问题的前言标准，可以让网页和搜索结果的内容更加丰富。
  
  - #### 回顾语义元素
  
    从页面结构开始讨论语义是因为页面结构容易把握。绝大多数网站在规划布局时，都会使用为数不多的几种常见设计元素（页眉、页脚、侧边栏、菜单），结果网站的布局——除了外观装饰有所不同外——都很相似。
    
    <!-- more -->
    
    ![页面结构相关语义元素上](http://p9myzkds7.bkt.clouddn.com/HTML5/%E9%A1%B5%E9%9D%A2%E7%BB%93%E6%9E%84%E7%9B%B8%E5%85%B3%E8%AF%AD%E4%B9%89%E5%85%83%E7%B4%A0%E4%B8%8A.jpg)
   
    ![页面结构相关语义元素下](http://p9myzkds7.bkt.clouddn.com/HTML5/%E9%A1%B5%E9%9D%A2%E7%BB%93%E6%9E%84%E7%9B%B8%E5%85%B3%E8%AF%AD%E4%B9%89%E5%85%83%E7%B4%A0%E4%B8%8B.jpg)
   
    由于结构化的内容是又很多更小的内容片段构成的，而组织这些内容片段的方式没有一定之规。
   
    HTML5的策略是双管齐下。一方面，它 **只增强了很少几个文本级语义元素** ；另一方面，任何人都 **可以定义自己的信息** 。HTML5支持一个 **独立的微数据标准** 。这个标准为人们提供了扩展的可能性，任何人都可以定义自己的信息，在自己的页面中为信息添加标注。
   
    首先，来认识三个新的文本级的语义元素： `<time>` 、 `<output>` 和 `<mark>` 。
    
    - ##### 标注日期和时间
    
      网页中经常会出现日期和时间信息。有了 `<time>` 元素可以解决 没有标准的方式来标注日期的问题，通过它可以标注日期、时间或日期与时间的组合。先看个例子：
      
      ```
      The Party starts <time>2014-03-21</time>
      ```
          用 `<time>` 元素来标注日期似乎有点违反直觉，这是HTML5的古怪行为之一。更适合的元素显然应该是 `<datatime>`。
       
      `<time>` 元素在这儿扮演两个角色。首先，它 **表示日期和时间位于标记的哪个地方** 。其次，它 **以任何软件程序都能理解的方式提供日期和时间**。  
      
      前面的例子符合第二个角色的要求，使用了通用日期格式。
      
      可以采用任何格式来表示日期，只要你在datetime属性中提供计算机能看懂的通用格式日期就行，比如： 
      
      ```
      The party starts <time datetime="2014-03-21">March 21<sup>st</sup></time>
      ```
      在浏览器中的效果如下所示：
      
      ```
      The party starts March 21^st.
      ```
      对于时间部分， `<time>` 也有类似的规则，标准的时间格式如下：
      
      ```
      HH:MM
      ```
      首先是2位数的小时（24小时制），然后是2位数的分钟。比如：
      
      ```
      Parties starts <time datatime="2014-03-21 16:30">4:30 p.m.</time>
      ```
      最后，组合以上两个规则可以指定具体日期和时间，日期在前，空一个格，时间在后：
      
      ```
      The party starts <time datatime="2014-03-21 16:30">March 21<sup>st</sup> at 4:30 p.m.</time>
      ```
          最初 `<time>` 元素要求用一种稍有不同的格式组合日期和时间。2014-03-21T16:30的格式还可以用，因此在别人的网页也可能出现。
       
      在组合日期和时间的情况下，有时候还需要 **后缀时区信息** 。比如：纽约在东五区，即UTC-5:00。（关于时区的划分，请参见 http://en.wikipedia.org/wiki/Time_zone。）要表示纽约时间下午4:30，应该使用如下标记：
      
      ```
      The party starts <time datetime="2014-03-21 16:30-05:00">March 21<sup>st</sup> at 4:30 p.m.</time>
      ```
      这样，看你网页的人，能看到他们想看到的格式，而搜索机器人和其他软件则能看到它们可以处理的值，而且毫无歧义。
      
      另外， `<time>` 还有 **一个pubdate属性** 。如果当前内容（例如 `<time>` 元素所在的 `<article>` ）对应一个发表日期，可以使用这个属性。下面就是一个例子：
      
      ```
      Published on <time datetime="2014-03-21" pubdate>March 31,2014</time>.
      ```
          因为 `<time>`元素是纯粹是信息性的，没有任何附加样式，所以可以在任何浏览器中使用。不必担心兼容性问题。
        
    - ##### 使用 `<output>` 标注JavaScript返回值
      
      HTML5还包含一个语义元素 `<output>`,它能使 **某种JavaScript驱动页面更加清晰** 。实际上，这个元素是 **一个占位符** ，**用于展示一小段计算后的信息** 。
      
      通常的做法是给占位符指定一个ID属性，这样JavaScript代码就可以在计算时找到它。Web开发人员一般将 `<span>` 元素用作占位符，而唯一的问题是该元素不提供任何语义：
      
      ```
      <p>Your BMI: <span id="result"></span></p>
      ```
      以下则是使用HTML5的更有意义的版本：
      
      ```
      <p>Your BMI：<output id="result"></output></
      ```
      实际的JavaScript代码无需改变，因为它只是根据ID属性查找元素，不用考虑元素类型：
      ```
      var resultElement = document.getElementById("result");
      ```
      通常，这种页面都会采用一个 `<form>` 元素。在这个例子中，需要包含3个文本框，以便用户使用在其中输入信息：
      
      ```
      <form action="#" id="bmiCalculator">
        <label for="feet" inches>Height:</label>
        <input name="feet">feet</br>
        <input name="inches">inches<br>
        
        <labelfor="punds">Weight:</label>
        <input name="pounds">pounds<br><br>
        ...
      </form>
      ```
      可以为 `<output>` 元素添加form和for属性，前者的值是 **包含相关控件表单ID** ,后者的值是 **以空格分隔的相关控件的ID** 。比如下面的例子：
      
      ```
      <p>
       Your BMI: <output id="result" from="bniCalculator" for="feet inches punds" >
      </p>
      ```
      这些属性的唯一用处就是 **表明 `<output>` 从那些控件获取结果这一信息** 。
      如果其他人需要编辑你的页面， **那些属性可以帮他们理清思路** 。
      
      > **提示**
      可以在 http://prosetech.com/html5 找到所有示例页面。
      
    - ##### 使用 `<mark>` 标注突显文本
    
      `<mark>`  **用于标注一段文本** ， **这段文本会突出显示** 。在需要引用其他人的内容，而你想引起别人的注意，就可以使用 `<mark>` 元素：
      
       ```
        <p>In 2009, Facebook made a bold grab to own everyone's content,
      <em>forever</em>. This is the text they put in their terms of service:</p>

        <blockquote>
      You hereby grant Facebook an <mark>irrevocable, perpetual, non-exclusive, transferable, fully paid, worldwide license</mark> (with the right to sublicense) to <mark>use, copy, publish</mark>, stream, store, retain, publicly perform or display, transmit, scan, reformat, modify, edit, frame, translate, excerpt, adapt, create derivative works and distribute (through multiple tiers), <mark>any user content you post</mark> on or in connection with the Facebook Service or the promotion thereof subject only to your privacy settings or enable a user to post.
        </blockquote>

        <p>Fortunately, they've since backtracked and weakened the language considerably.
      Here's the relevant section today:</p>

        <blockquote>
      <mark>You own all of the content and information you post on Facebook</mark>, and you can control how it is shared through your privacy and application settings. In addition: 1. For content that is covered by intellectual property rights, like photos and videos ("IP content"), you</mark> specifically give us the following permission, subject to your privacy and application settings: <mark>you grant us a non-exclusive, transferable, sub-licensable, royalty-free, worldwide license to use any content that you post</mark> on or in connection with Facebook. <mark>This license ends when you delete your content or your account</mark> unless your content has been shared with others, and they have not deleted it.
        </blockquote>
       ```
       如下图3-2所示，浏览器会给 `<mark>` 中的文本添加黄色背景。
       
       ![mark语义元素](http://p9myzkds7.bkt.clouddn.com/HTML5/mark%E8%AF%AD%E4%B9%89%E5%85%83%E7%B4%A0.png)
       `图3-2`
       
       也可以使用 `<mark>` **标注重要的内容或关键字** ——就像搜索引擎在搜索结果中显示匹配的文本那样，还 **可以与 `<del>`（删除的文本）和 `<ins>`（插入的文本）组合使用** ,以标注文档的变化。
       
       `<mark>` 元素并不十分必要，HTML5规范认为它是一个语义元素，但实际上它更多地则是被用于表现目的。默认情况下，被标注的文本会带有浅黄色的背景（见图3-2），不过可以通过样式规则为它应用不同的样式。
       
       > **提示**
       应该坚持只用 `<mark>`（结合你想使用的任意CSS格式）来传达适当的语义。那就是 **使用 `<mark>`来吸引人注意那些变得很重要的文本** 。
       
       即使沿用默认的黄色背景，也应该为不支持HTML5的浏览器添加后备样式表。以下就是相应的样式规则：
       
       ```
       mark {
        bakground-color: yellow;
        color: black;
       }
       ```
  - #### 其他语义元素
  
    - ##### ARIA
     
       ARIA（Accessible Rich Internet Application, 无障碍富因特网应用）是一个还在制定中的标准， **它规定了在任意HTML元素上使用属性** ， **而通过这些属性可以为屏幕阅读器提供额外的信息** 。例如，ARIA中规定了一个role属性，表示所在元素的用途。下面就以一个表示页眉的 `<div>` 元素为例：
      
        ```
        <div class="header"
        ```
       通过为其指定值为banner的role属性，可以告诉屏幕阅读器它的用途是保存横幅广告：
        ```
        <div class="header" role="banner">
        ```
       HTML5为标注页眉提供了一个语义元素。因此，最适合的方式莫过于：
      
        ```
        <header role="banner">
        ```
       这个示例说明了两方面的问题: 首先，ARIA规定了一些推荐的角色名。（要了解所有角色名，请参考规范中相应部分： http://tinyurl.com/roles-aria 。）其次，部分ARIA与新HTML5语义元素重复——只是合情合理的，因为ARIA早于HTML5。不过重复也不是完全重复。
      
       ARIA还 **针对表单定义了属性** 。文本框的 **aria-required属性** 表示 **用户必须输入值** 。而文本框的 **aria-invalid属性** 表示 **当前值不正确** 。这些属性可以让屏幕阅读器用户，特别是视力不好的用户，不会因为看不到提示信息（比如必填字段旁的星号，或闪烁的红色错误图标）而受影响。
      
       如果想创造一个 **可以无障碍访问的网站** ，必须 **既要考虑ARIA,又要考虑HTML5** 。因为屏幕阅读器支持ARIA，不支持HTML5。
      
       > **注意**
       要了解有关ARIA（全名是WAI-ARIA, 因为它是由Web Accessibility Initiative 工作组负责制定的）的 更多信息，请参考其规范： http://www.w3.org/TR/wai-aria/ 。
      
    - ##### RDFa
    
      RDFa（Rrsource Description Framework, 资源描述框架） **也是一种使用属性向网页中嵌入详细信息的标准** 。RDFa有一个明显的优点;  **它是一个稳定不变的标准** 。RDFa也有两个明显的缺点。首先，RDFa最初是为XHTML而非HTML5设计的，因此关于严格的RDFa与松散的HTML5如何混写的争论一直不绝于耳。其次，RDFa很复杂，嵌入RDFa元数据后的网页比最初要长得多，而且显得很笨重。
      
      可以在Wikipedia上看到完整的介绍： http://en.wikipedia.org/wiki/RDFa 。
      
    - ##### 微格式
      
      **微格式** （Microformats）是 **一种在网页中嵌入元数据的简单而又比较合理的方式** 。它是一组统一的宽松的约定，这些约定 **有助于网页之间共享结构化信息** ，但 **又不会导致像采用RDFa那样复杂** 。
      
      微格式的应用的方式比较新颖，它们附加在通常用于添加样式的class属性上。你可以根据数据的类型，**使用某些标准样式名来标注数据** 。然后，其他程序可以读取你的标记，提取数据并 **通过检查class属性来确定数据的含义** 。
      
      
      
       
       
  
      
      
      
      