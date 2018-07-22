title: 《HTML5秘籍》学习笔记-用语义元素构造网页
author: songxingguo
tags:
  - HTML
categories:
  - 前端技术
  - 读书笔记
date: 2018-07-22 06:28:00
---
### 用语义元素构造网页

  为了能够把侧边栏与页眉分开，以及把广告条与菜单分开。HTML5为此引入了一组构造页面的新元素，这组元素能发挥 `<div>` 一样的作用，但是却能传达出更多的语义。
  
  - #### 语义元素
   
    - `<time>`元素
    
     ```
      Registration begins on <time>2014-11-25</time>.
     ```
     代码在网页中呈现的结果如下所示：
     
     ```
      Registration begins on 2014-11-25.
     ```
     `<time>` 元素没有任何内置的样式，元素中的文本和普通文本没有区别。
     
     设计 `<time>` 元素的用意是让它包含一小段信息。但大多数HTML5语义元素的用途是 **标识页面中的一个内容区块** 。比如， `<nav>` 元素用于标识一组导航链接。而 `<footer>` 元素用于标识通常放在页面底部的文脚（或页脚）。大概有十几个类似的新元素。

     所有语义元素都有一个显著的特点： **不真正做任何事**。相对来说， `<video>` 元素则囊括了在页面中充当视频播放器的全部能力。
     
     <!-- more -->
     
     使用这些新元素的理由：
     
     - ##### 容易修改和维护
        
       通过HTML5的语义元素，通过标记就可以传达出额外的结构化信息。

     - ##### 无障碍性
       
       现代Web设计的一个重要主题，就是让任何人都能无障碍的访问网页。换句话说，要让使用屏幕阅读器和其他辅助工具的人都能够在页面中自由导航。兼容HTML5的无障碍工具可以为残疾人士提供更好的上网体验。（仅举一个例子，有了 `<nav>` 元素，屏幕阅读器就能迅速返回导航区，进而找到网页链接。）
       
     - ##### 搜索引擎优化
     
       如果搜索引擎能更好的理解你的站点，那搜索者的查询就会越容易与你的内容匹配，因而你的网站列在搜索结果中的可能性也就越大。
       
     - ##### 未来功能
      
       新浏览器和Web编辑器工具一定会利用语义元素。比如，浏览器可以提供一个页面内容的纲要，以方便跳转到页面适当的区块。类似地，网页设计工具也能包含一些方便你构建或编辑菜单的功能，而方法就是组织你放在 `<nav>` 区块中的内容。
      
      最关键的问题在于，如果正确地使用了语义元素，就能够 **创建更加清晰的页面结构 ** ，就能 **适应未来的浏览器和Web设计工具的发展趋势** 。
      
- #### 改造传统的HTML页面
 
   要了解和熟悉新的语义元素（包括学习如何使用它们来构造页面），最好的方式莫过于拿一个经典的HTML文档作为例子，然后把HTML5的一些新鲜营养充实进去。例子访问 http://prosetech.com/html5 。
   
   - ##### 构造页面的老办法
   
    这个示例页面使用的是HTML最佳实践，没有任何通过标记来格式化的痕迹。没有粗体或斜体元素，没有嵌入的样式。总之，这是一个格式非常规范的页面， **所有样式均来自外部样式表** 。
    
    ```
    <div class="header">
      <h1>How the  World Could End</h1>
      <p class="Teaser">Scenarios that spell the end of life as we know it</p>
      <p class="Byline">by Ray N.Carnation</p>
    </div>
    
    <div class="content">
      <p><span class="LeadIn">Right now</span>, you're probably ...</p>
      <p>...</p>
      
      <h2>Mayna Doomsday</h2>
      <p>Skeptics suggest...</p>
      ...
    </div>
    
    <div class="footer">
      <p class="Disclaimer">These apocalyptic predictions ...</p>
      <p>
        <a href="AboutUs.html">About Us</a>
        ...
      </p> 
      <p>Copyright @ 2014</p>
    </div>
    ```
    编写规范的传统HTML页面中，通过使用 `<div>` 和 `<span>` 元素，已经把大部分工作移交给了样式表。通过 `<span>` 可以为 **处在其他元素中的少量文本添加样式 **，而通过 `<div>` 不仅可以为 **整个内容区块添加样式** ，还可以 **构建起整个页面结构** 。
    
    因为使用了 `<div>` 元素，所以添加样式很容易。使用下列规则为页眉及其中内容添加样式。
    
    ```
    /* 为<div>添加样式，使其具有页眉的外观（蓝色带边框）*/
    .Header {
      background-color: #7695FE;
      boder:thin #336699 solid;
      padding: 10px;
      margin: 10px;
      text-align: center;
    }
    
    /* 为页眉中的 <h1> 添加样式(这是文章的标题）*/
    .Header h1 {
      margin: 0px;
      color: white;
      font-size: xx-large;
    }
    
    /* 为页眉中的子标题添加样式 */
    .Header .Teaser {
      margin: 0px;
      font-weight: bold;
    }
    
    /* 为页面中的署名行添加样式 */
    .Header .Byline {
      font-style: italic;
      font-size: small;
      margin: 0px;
    }
    ```
    
  - ##### 使用H5构造页面
  
   `<div>` 目前依旧是Web设计中的必备元素，它是 **一个直观、多用途的容器** ，可以通过它 **为页面中的任何区块应用样式** 。但 `<div>` 的问题在于，它本身 **不反映与页面相关的任何信息** 。
    
   要通过HTML5改进这种情况，可以把 `<div>` 替换成更具有描述性的语义元素。这些语义元素的行为与 `<div>` 元素类似：它们 **仅包含一组标记，除此之外没有其他作用** ，可以将它 **作为“格式挂钩”来为页面应用样式** 。不过，除此之外，它们还会 **为页面添加一点语义** 。
   
   下面代码是对传统页面的简单修改之后的结果，删除了两个 `<div>` 元素，但添加了两个HTML5的语义元素：`<header>` 和 `<footer>` 。
    
    ```
    <header class="header">
      <h1>How the  World Could End</h1>
      <p class="Teaser">Scenarios that spell the end of life as we know it</p>
      <p class="Byline">by Ray N.Carnation</p>
    </header>
    
    <div class="content">
      <p><span class="LeadIn">Right now</span>, you're probably ...</p>
      <p>...</p>
      
      <h2>Mayna Doomsday</h2>
      <p>Skeptics suggest...</p>
      ...
    </div>
    
    <footer class="footer">
      <p class="Disclaimer">These apocalyptic predictions ...</p>
      <p>
        <a href="AboutUs.html">About Us</a>
        ...
      </p> 
      <p>Copyright @ 2014</p>
    </footer>
    ```
  如果修改一个大型网站，可以考虑用HTML5中相应的 **语义元素来包装已有的 `<div>` 元素** 。
  
  不管怎么说，这里的类名还是有点多余。可以变成这样：
  
  ```
  <header>
     <h1>How the  World Could End</h1>
     <p class="Teaser">Scenarios that spell the end of life as we know it</p>
     <p class="Byline">by Ray N.Carnation</p>
   </header>
  ```
  但这样修改后，就得修改样式表规则，直接 **通过元素名来应用样式** 。
  
  下面是修改之后的为 `<header>`及其包含的所有元素应用样式规则：
      
    ```
    /* 为<header>添加样式，使其具有页眉的外观（蓝色带边框）*/
    header {
      ...
    }
    
    /* 为<header>中的 <h1> 添加样式(这是文章的标题）*/
    header h1 {
      ...
    }
    
    /* 为<header>中的子标题添加样式 */
    header .Teaser {
      ...
    }
    
    /* 为<header>中的署名行添加样式 */
    header .Byline {
      ...
    }
    ```
  HTML5页面经常会混合各种语义元素和更通用的 `<div>` 容器。
  
  **注意：** 上面的网页在IE9之前的Internet Explorer 中无法显示。
  
  最后，还有一个元素有必要用在示例页面中。那就是HTML5的 `<article>` 元素，这个元素 **表示一个完整的、自成一体的内容块** ，比如博客文章或新闻报道。 `<article>` 元素应该包含所有相关的内容，包括标题、作者署名以及正文。添加了 `<article>` 元素之后的页面结构就变成如下所示：
  
  ```
  <article>
    <header>
      <h1>How the  World Could End</h1>
      ...
    </header>
    
    <div class="content">
      <p><span class="LeadIn">Right now</span>, you're probably ...</p>
      <p>...</p>
      
      <h2>Mayna Doomsday</h2>
      <p>Skeptics suggest...</p>
      ...
    </div>
  </article>
  
  <footer>
    <p class="Disclaimer">These apocalyptic predictions ...</p>
    ...
  </footer>
  ```
  最终的页面结构示意图：
  
  ![页面结构示意图](http://p9myzkds7.bkt.clouddn.com/Angular/%E9%A1%B5%E9%9D%A2%E7%BB%93%E6%9E%84.jpg)
  
  - ##### 用 `<figure>` 添加插图
  
   HTML5规范建议我们将插图想象成一本书中的附图；换句话说，**插图虽独立于文本，但文本中会提到它** 。
   
   一般来说，插图应该是浮动的；换句话说，应该把它 **放在相关文本旁边的一个比较近便的位置上** ，而不要把它们锁定在特定的词或元素旁边。而且，插图通常 **还会有与之相伴的浮动图题** 。
   
   下面是个这篇文章添加插图的HTML标记。
   
   ```
   <p><span class="LeadIn">Right now</span>, you're probably ...</p>
   <div class="FloatFigure">
     <img src="human_skull.jpg" alt="Human skull">
     <p>Will you be the last person standing if one of these apocalyptic scenarios play out?</p>
   </div>
   
   <p>But don't get too smug ...</p>
   ```
   下面是给出的样式规则：
   
   ```
   /* 为插图应用样式 */
   .FloatFigure {
     float: left;
     margin-left: 0px; 
     margin-top: opx;
     margin-right: 20px;
     margin-bottom: 0px;
   }
   
   /* 为题图应用样式 */
   .FloatFigure p {
     max-width: 200px;
     font-size: small;
     font-style: italic;
     margin-bottom: 5px;
   }
   ```
   HTML5专门量身打造了一个了一个新的语义元素为内容添加插图，下面是用 `<figure>` 改造后的HTML5文档：
   
   ```
   <figure class="FloatFigure">
     <img src="human_skull.jpg" alt="Human skull">
     <figcaption>Will you be the last person standing if one of these apocalyptic scenarios play out?</figcaption>
   </figure>
   ```
   而题图是放在 `<figure>` 中的 `<figcaption>` 元素里的。
   
   当然，给插图和图题应用什么样式，把它们放在什么位置上，还是由你说了算。使用新元素之后，插图还是那样，但不同的是标注插图的标记现在的含义已经非常明确了。（随便在提个醒，`<figcaption>` 元素不是只能包含文本——任何HTML元素都可以，比如链接、小图标等。）
   
   因为图题中经常包含了对图片的完整说明，所以alt文本就显得有点多余了。这种情况下，可以把 `<img>` 元素中的alt属性删除：
   
   ```
   <figure class="FloatFigure">
     <img src="human_skull.jpg">
     <figcaption>A human skull lies on the sand</figcaption>
   </figure>
   ```
   这里要小心的是，不能把alt文本设成空字符串。因为这意味着你的图片纯粹是装饰用的，屏幕阅读器会忽略不读。
   
   - ##### 用 `<aside>` 添加附注
   
    新的 `<aside>` 元素 **表示与它周围的文本没有密切关系的内容** 。这就是说，你可以像在印刷品中使用附录栏一样使用 `<aside>` 元素，可以通过它 **介绍另一个相关的话题** ，或者 **对主文档中提出的某个观点进行解释** 。另外，也可以用 `<aside>` 来盛放广告、相关内容链接，甚至醒目引文（pull quote）。
    
    当然，使用熟悉的 `<div>` 元素也可以创造出这种效果，但用 `<aside>` 元素包装同样的内容，可以让标记更富有意义：
    
    ```
    <p>... (in a suitably robotic voice) "who' s your daddy now?"</p>
    
    <aside class="PullQuote">
      <img src=-"quotes_start.png" alt="Quote">
      We don't know how the universe started, so we can't be sure it won't just end, maybe today.
      <img src="quotes_end.png" alt="End quote">
    </aside>
    
    <h2>Unexplained Singularity</h2>
    ```
    下面给出了相应的样式：
    
     ```
     .PullQuote {
       float: right;
       max-width: 300px;
       border-top: thin black solid;
       border-bottom: thick black solid;
       font-size: 30px;
       line-height: 130%;
       font-style: italic;
       padding-top: 5px;
       padding-bottom: 5px;
       margin-left: 15px;
       margin-bottom: 10px;
     }
     
     .PullQuote img {
       vertical-align: bottom;
     }
     ```
    **语义元素的来历：可以看看这里： https://developers.google.com/webmasters/state-of-the-web/ 。**
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  