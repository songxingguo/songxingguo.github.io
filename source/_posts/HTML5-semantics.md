title: 《HTML5秘籍》学习笔记-用语义元素构造网页
author: songxingguo
tags:
  - HTML
categories:
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
    
    ![页面结构示意图](https://graphbed.qiniu.songxingguo.com/Angular/%E9%A1%B5%E9%9D%A2%E7%BB%93%E6%9E%84.jpg)

  - ##### 添加插图

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

  - ##### 添加附注
   
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
   
- #### 对语义元素的支持情况
  
  所幸的是，HTML5的这些语义元素已经基本得到了所有现代浏览器的支持。最大绊脚石还是IE9之前的Internet Expolorer，包括仍然占很大比例的IE8.
  
  幸运的是，语义元素还是一个比较容易弥补的功能。只要让浏览器把它们当做普通的 `<div>` 元素就行了。
  
  - ##### 为语义元素添加样式
  
    浏览器在遇到不认识的元素时，会把它们 **当成内联（inline）元素** 。大多数HTML5语义元素（包括已经介绍的这些，但除 `<time>` 之外）都是 **块级元素** ，也就是 **需要在单独一行上来呈现它们** ，同时在 **它们与前后元素之间各添加一些空间** 。
    
    为解决浏览器不认识HTML5语义元素，只要在样式表中添加一条“超级规则”即可。下面就是一条为9个HTML5元素应用块级显示格式的规则：
    
    ```
    article, aside, figure, figcaption, footer, header, main, nav, section, summary {
      display: block;
    }
    ```
    这条规则 **对于能够识别HTML5元素的浏览器没有作用** ，因为它们的display属性已经被默认设成了block.而且这条规则 **也不影响我们已经为这些元素应用的样式** ，那些样式照样可以添加到它们身上。
    
  - ##### 使用H5“垫片”
  
    对于大多数浏览器而言，上面的技术可以解决兼容问题。但这里的“大多数”不包括IE8及更早的版本。换句话说，对于较早版本的IE,还要面临一个挑战：它们会拒绝给无法识别的元素应用样式。好在，有一个变通方案：通过JavaScript创建新元素，就可以骗过IE,让它识别外来元素。比如，下面的脚本可以让IE识别并为 `<header>` 元素应用样式：
    
     ``` 
     <script>
       document.createElement('header')
     </script>
     ```
     实际上，不用自己亲自写这些代码，因为已经有人写好了，只要拿过来用就可以了。
     要使用这个脚本，**只要在页面的 `<header>` 区块中引用它即可** ，就像这样：
     ```
     <head>
       <title>...<.title>
       <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"><script>
     </head>
     ```
     这行代码会从html5.goolecode.com这个Web服务器上取得脚本，然后在浏览器处理页面其余部分之前运行。

     这个脚本  **使用前面描述的JavaScript代码创建所有的新HTML5元素** ，然后还会 **为它们动态应用上面提到的样式** ，确保新元素能正确的显示为块元素。
  
     前面的html5.js脚本应该是有条件执行的—— **只有你使用旧版本Internet Explorer的情况下才会执行** 。为了避免不必要地加载JavaScript文件，可以像下面这样 **把引用脚本的代码放在IE的条件注释中：**

     ```
     <!--[if lt IE 9]>
       <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"><script>
     <![endif]-->
     ```
     这样，其他浏览器（IE9及更高版本）就会忽略这行脚本，为页面节省数毫秒的加载时间。

     也可以 **下载垫片脚本放在本地目录里** ，地址为 http://tinyurl.com/the-shiv 。

     **注意：** 可以在页面开头添加“Web标志”解决IE的安全提示条问题。
  
  - ##### 一站式解决方案

     **Modernizr内置了HTML5垫片脚本** ，因此 **会自动解决上述问题** ，不用再使用html5.js或者添加什么样式规则了。
  
- #### 使用语义元素设计站点

   - ##### 理解 `<header>`  
   
      对于 `<header>` 元素来说，又两种使用方式，但差别并不大。一种是 **用它标注内容的标题** ，另一种是 **用它标注网页的页眉** 。

      如果处理的是内容，那么除非必要，一般不必使用 `<header>` 。只有在内容标题还附带了其他信息的情况下，才有必要考虑 `<header>` 。也就是说，其中包含标题、摘要、发表日期、作者署名、图片、或子主题链接等很多内容，例如：

      ```
      <header>
        <h1>How the world Could End</h1>
        <p class="Tagline">Scenarios that spell the end of life as we know it </p>
        <p class="Byline"> by Ray N.Carnation</p>
      </header>
      ```
       不过，在为网站创建页眉的时候，人们多数会直接考虑 `<header>` 元素，即便这个页眉就是一个CSS控制的长盒子，里面啥都没有。
   
     **结论如下：** 网页中 **可以包含多个 `<header>` 元素（通常也应该如此）** ，即使相应的区块在页面中的角色不一样。
   
     在真实的网站中，可能几十个甚至更多个页面都会有相同的布局（以及相同的侧边栏）。访客点击页面的链接之后，唯一变化的地方就是主页面中的内容。
   
     使用开发网站的技术和技巧把网页变成网站：
   
      - 服务器端框架
      
        其实背后的思想都很简单，在浏览器请求某个页面时，Web服务器 **临时将页面的各个部分组装起来**，包括公共的元素（如导航条）和内容。这是目前最常用的一种技术，也是构建大型、专业网站的必由之路。以不同方式实现这种手段的技术不计其数，包括很早就出现的服务器端包含的功能，一些 **富Web应用平台**（如ASP.NET和PHP），还有 **内容管理系统**（如Drupal和WordPress）。
        
      - 页面模板
        
         Dreamwraver、Expression Web等功能强大的页面编辑器中就有页面模板功能。模板就是 **一个定义了页面结构并且包含页面中会重复出现的内容（像页眉和侧边栏）的页面** 。有了模板，就 **可以利用它来创建网站的所有页面** 。最关键的是，更新模板之后网页编辑器会自动更新使用该模板的所有页面。
   
    **站点标题结构：**
   
      随着HTML5获得胜利并统治Web,好像过多个 `<h1>` 的设计会越来越时髦。不过现在，很多开发人员为了让屏幕阅读器开心，仍然坚持只使用一个 `<h1>` 。
   
   - ##### 标注导航链接
     
     在传统的HTML网站中，可能会把整个侧边栏都放到一个 `<div>` 中。而在HTML5时代，则应该主要使用两个针对性更强的元素： `<aside>` 和 `<nav>` 。
     
     要说 `<aside>` 元素 ，倒是跟 `<header>` 元素有点像， `<aside>` 的含义也有些发散。可以用它来 **标注一段与正文相关的内容** ，也可以用它 **表示页面中一个完全独立的区块** ——作为页面主要内容的陪衬。
     
     而 `<nav>` 元素则 **用于包装一组链接** 。这些链接可以指向当前页面中的主题，也可以指向网站中的其他页面。多数页面中都会 **包含多个 `<nav>` 区块** 。但不是所有链接都需要 `<nav>` 区块——相反， `<nav>` 通常 **用于页面中最大最主要的导航区** 。
     
     事实上，如下图所示，至少有两个比较合理的方式来构造侧边栏。
     
      ![构造侧边栏](https://graphbed.qiniu.songxingguo.com/Angular/%E6%9E%84%E9%80%A0%E4%BE%A7%E8%BE%B9%E6%A0%8F.jpg)
     
      - 第一种，侧边栏这里的导航又长又复杂(比如都用上了可折叠的菜单形式），只是后面跟着一段小内容。
     
      - 第二种，侧边栏有多种用途，而没有哪一种用途是主要用途。
     
     以下就是构成侧边栏的标记，包含三个区块：
     
      ```
      <aside>
        <nav>
          <h2>Articles</h2>
          <ul>
            <li><a href="...">How The World Could End</a></li>
            <li><a href="...">Would Aliens Enslave or Eradicate Us?</a></li>
          </ul>
        </nav>
        
        <section>
          <h2>About Us</h2>
          <p>Apocalypse Today is a world leader in conspiracy theories ..."</p>
        </section>
        
        <div>
          <img src="ad.jpg" alt="Luckies cigarette ad: it's toasted">
        </div>
      </aside>
      ```
      看完这些代码，应该注意到如下关键点。
     
      - 标题（Articles和About Us）使用的是二级标题。这样，它们就自然位于网站的一级标题之下，从而方便屏幕阅读器无障碍的阅读标题。
     
      - 链接以 `<ul>` 和 `<li>` 元素组成的无序列表来标记。处理一组链接的最佳方式，也是最无障碍的方式，就是使用列表。不过，得通过样式表删除列表项默认的缩进和项目符号。
     
      - “About Us”包含在一个 `<section>` 元素中。`<section>` 适合任何以标题开头的内容区块。
     
      - 图片广告放在一个 `<div>`里。有了 `<di>` 区块在之间的关系就更加明确了。
     
     复杂的侧边栏可能会以 `<header>` 和 `<footer>` 元素作为开头和结尾，也可能包含多个 `<nav>` 区块——存档文章链接一个、新闻报道连接诶一个、友情链接一个或相关站点链接一个，等等。

     下面是侧边栏的样式表：

        ```
        aside .NavSidebar {
          position: absolute;
          top: 179px;
          left: 0px;
          paddding: 5px 15px 0px 15px;
          width: 203px;
          min-height: 1500px;
          background-color: #eee;
          font-size: small;
        }
        ```
     其他样式可以从 http://prosetech.com/html5 下载示例代码。

     **注意：** `<nav>` 通常会单独出现，有时候也会出现在 `<aside>` 中。其实，还有经常出现在顶部的 `<header>` 元素中。

     下面是整个页面布局关系：

     ![页面布局](https://graphbed.qiniu.songxingguo.com/Angular/%E9%A1%B5%E9%9D%A2%E5%B8%83%E5%B1%80.jpg)

         使用 `<details>` 和 `<summary>` 的折叠框,HTML5新添加了两个语义元素，用于辅助将折叠区块（通过单击区块标题能够显示或隐藏其中的内容）这种行为自动化。具体做法是可以把折叠的区块放在一个 `<details>` 元素中，而把标题放在一个 `<summary>` 元素中。最终结构类似如下这样：
    
         ```
         <details>
           <summary>Section #1</summary>
           <p>If you can see this content, the section is expanded</p>
         </details>
         ```
         **注意：** 对这两个元素支持的浏览器还很少。
   
   - ##### 理解区块
   
      区块元素 `<section>` 是应该最后考虑的语义元素。如果有一个带标题的内容块而其他语义元素都不合适，那么选择 `<section>` 通常比选择 `<div>` 更好些。

      可以放在 `<section>` 中的内容有以下几种。

      - 与页面主体内容并列显示的小内容块。
      - 独立性内容，但却不能用文章（ `<article>`）来描述，比如客户的购物记录或产品清单。
      - 分组内容——例如，新闻站点中的一组文章。
      - 比较长的文档中的一部分。

      最后， `<section>` 元素也会影响网页的纲要。

  - ##### 理解 `<footer>`

      `<footer>`与内容丰富的“胖” `<header>` 可谓天生一对。在 `<header>` 中 **不仅可以放副标题和作者署名，还可以添加图片、导航区域（使用 `<nav>` 元素）、以及任何有必要放在页面顶部的内容** 。

      但HTML5规定，只能在 `<footer>` 元素中放一些 **网站版权信息、作品来源、法律限制以及链接之类的信息** 。**不能** 在 `<footer>` 里面放太多链接、重要的内容或无关的内容，如广告、社交媒体按钮以及网站部件等。

      “胖” 页脚中经常会用到如下所示的一些花哨的技术。

       - 固定位置，这样就可以让页脚始终固定在浏览器窗口底部。

       - 关闭按钮，这样用户看完页脚内容后，单击它们就可以腾出页面空间。

       - 部分透明的背景，这样就可以透过页脚看到内容。

       - 动画，页脚在视图中弹出，或者滑入视图(例如，可以看看在 http://www.nytimes.com 上阅读到一篇文章最后时弹出的相关文章提示框）。

    如果站点中包含这种页脚，就要作出选择。比较简单的方法就是无视规定。可是，假如不想违反标准规定，那就调整一下标记。
    
    调整标记的关键在于从多余的内容中提取出标准的页脚来。这些内容看起来还是一个页脚，但在标记中，其他内容都不包含在 `<footer>` 元素中。以下就是“胖”页脚的实际结构：
     ```
     <div id="FatFooter">
       <!-- “胖” 页脚的内容 -->
       <img onclick="CloseBox()" src="close_icon.png" class="CloseButton">
       ...
       <footer>
         <!-- 标准页脚内容 -->
         <p>The views expressed on this site do not ... </p>
       </footer>
     </div>
     ```
    最外围的 `<div>` 没有特殊含义，它只负责把多余的内容和标准的页脚内容包装起来。同样，下面给出了应用的一些样式表规则：
    
     ```
     #FatFooter {
       position: fixed;
       bootom: 0px;
       height: 145px;
       width: 100%;
       background:#ECD672;
       border-top: thin solid black;
       font-size: small;
     }
     ```
    当然，也可以把页脚中其余内容放在一个 `<aside>` 元素中，从而清晰地表明其中内容属于一个独立区块，与页面中的其他内容无关。相应的标记结构类似如下所示：
    
      ```
       <div id="FatFooter">
         <aside>
           <!-- “胖” 页脚的内容 -->
           <img onclick="CloseBox()" src="close_icon.png" class="CloseButton">
           ...
         </aside>
         <footer>
           <!-- 标准页脚内容 -->
           <p>The views expressed on this site do not ... </p>
         </footer>
       </div>
      ```
    这里最重要的一点是没有把 `<footer>` 放到 `<aside>` 元素中。因为 `<footer>` 并不属于 `<aside>`, 而是整个网站的一部分。类似地，如果有一个 `<footer>` 属于某些内容，那么就应该将这个 `<footer>` 放在包含相应内容的元素中。
     
    **注意：** 如果一个元素看起来不适合你标注的内容，那就不要用。当然，，也可以上网去讨论，最好的一个去处就是 http://html5doctor.com 。

  - #####  标识主要内容

   `<main>` 元素用于标识网页中的主要内容。

    在启示录网站中，就用 `<main>` 包装了 `<article>`：
    
    ```
    <!DOCTYPE html>
    <html lang="en">
      <head>
        ...
      </head>
      
      <body>
        <header>
          ...
        </header>
        <aside>
          ...
        </aside>
        
        <main>
          <article>
            ...
          </article>
        </main>
        <footer>
          ...
        </footer>
      </body>
    </html>
    ```
    **不能** 把 `<main>` 元素嵌套到 `<article>`（或其他任何语义元素）里边。这是因为 `<main>` 元素的使命就是 **包裹页面中的主要内容** ，**而非标识文档中某个重要的部分** 。换句话说，一个页面中 **只能有一个 `<main>` 元素**。
    
    `<main>` 元素对屏幕阅读器很重要 ，有了它，屏幕阅读器就可以跳过那些次要的内容，比如页眉、导航菜单、广告、和侧边栏，直达内容。这个例子中 `<main>` 紧挨着 `<article>` ，这 **不** 是必须的。在复杂些的页面中，比如包含多个 `<article>` 元素时， `<main>` 应该包含所有 `<article>`,像这样：
    
    ```
    <main>
      <article>
        ...
      </article>
      
      <article>
        ...
      </article>
      
      <article>
        ...
      </article>
      
      ...
    </main>
    ```
    很明显，每个 `<article>` 都包含自己的内容，而 `<main>` 包含所有文章。
    
    总之一句话，就是 `<main>` **只应该包含主要内容，而不是上下左右的外部细节**。

- #### HTML5纲要

  HTML5定义了一组规则，**用于说明如何为网页创建文档纲要（document outline）** 。网页纲要能够提高很多便利：
  
   - 浏览器可以让你从纲要中的一处跳到另一处。
   - 设计工具可以让你通过在纲要视图中拖放来重排区块。
   - 搜索引擎可以使用纲要构建更好的页面预览。
   - 屏幕阅读器通过使用纲要可以引导视力障碍的用户在深度嵌套的区块和子区块中导航。
  
 - ##### 如何查看纲要

   目前，还没有浏览器实现HTML5纲要（或者提供一种查看方式）。不过，有几个工具填补了这个空白。
  
    - 在线HTML纲要生成器

      访问 http://gsnedders.html5.org/outliner/ ，告诉纲要生成器你想为那个网页生成纲要。可以通过三种方式提交网页：从本地电脑中上传、提供URL或直接在文本框中粘贴标记。

    - Chrome扩展

      在Chrome浏览器中查看网页纲要是，可以使用h5o插件分析纲要。访问 http://code.google.com/p/h5o 安装插件，然后在网上打开一个HTML5页面看一看。然后浏览器地址栏中会出现一个纲要图标，点击该图标就会显示页面的结构。

    - Opera扩展

      Chrome的h5o扩展也有一个针对Opera开发的版本，安装地址在 http://tinyurl.com/3k3ecdy 。

 - ##### 基本纲要

   要知道自己网页的纲要是什么样子，可以想象把页面中的所有内容都剥离，只 **剩下编号的标题元素（ `<h1>`、`<h2>`、`<h3>`等）中的文本**。然后，根据它们在 **标记中位置缩进标题** ，那么嵌套最深的标题在纲要中的缩进也最多。
  
   以最初的还没有应用HTML5元素之前的启示录文章页面的标记为例：

    ```
    <dody>
      <div class="Header">
        <h1>How World Could End</h1>
        ...
      </div>
      ...
      <h2>Mayan Doomsday</h2>
      ...
      <h2>Robot Takeover</h2>
      ...
      <h2>Unexplained Singularity</h2>
      ...
      <h2>Runaway Climate Change</h2>
      ...
      <h2>Global Epidenmic</h2>
      
      <div class="Footer">
        ...
      </div>
    </body>
    ```
   这个简单的结构会生成如下所示的纲要：
   ```
   1.How World Could End
    1.Mayan Doomsday
    2.Robot Takeover
    3.Unexplained Singularity
    4.Runaway Climate Change
    5.Global Epidenmic
   ```
   两个级别的标题（ `<h1>`和`<h2>`）就创建两级的纲要。**这种对应关系与文字处理软件中的纲要功能类似** 。比如，可以在微软Word的文档结构图窗格中看到类似的纲要。
   换个例子，对于下面的标记：
   ```
   <h1>Level-1 Heading</h1>
   <h2>Level-2 Heading</h2>
   <h2>Level-2 Heading</h2>
   <h3>Level-3 Heading</h3>
   <h2>Level-2 Heading</h2>
   ```
   会生成如下所示的纲要：
   ```
   1.一级标题
     2.二级标题
     2.二级标题
       1.三级标题
     3.二级标题
   ```
   最后，纲要算法也很聪明，**能够自动忽略跳过的级别** 。例如，如果写的标记有点不太规范，从 `<h1>` 直接就跳到了 `<h3>`：
   
   ```
   <h1>Level-1 Heading</h1>
   <h2>Level-2 Heading</h2>
   <h1>Level-1 Heading</h1>
   <h3>Level-3 Heading</h3>
   <h2>Level-2 Heading</h2>
   ```
   那会得到如下纲要：
   ```
   1.一级标题
     1.二级标题
   2.一级标题
     1.三级标题
     2.二级标题
   ```
   本来的三级标题与二级标题放在同一层次上了，正是由于这种纲要算法可以消除网页标题级别不完美匹配的问题。
   
  - ##### 分块元素
  
    分块元素（sectioning element）是指那些 **在页面中创建新的、嵌套纲要的元素** 。这些元素有 `<article>` 、 `<aside>` 、 `<nav>` 和 `<section>` 。要理解分块元素的作用，可以想象一个包含两个 `<article>` 元素的页面。因为 `<article>` 是一个分块元素，所以 **这个页面中（至少）有三个纲要：整个页面有一个纲要、每篇文章有一个嵌套的纲要 **。
   
    为了进一步弄清楚到底是什么情况，我们来看一看用HTML5改造之后的启示录文章页面：

    ```
    <dody>
      <article>
        <header>
          <h1>How World Could End</h1>
          ...
        </header>
        
        <div class="Content">
          ...
          <h2>Mayan Doomsday</h2>
          ...
          <h2>Robot Takeover</h2>
          ...
          <h2>Unexplained Singularity</h2>
          ...
          <h2>Runaway Climate Change</h2>
          ...
          <h2>Global Epidenmic</h2>
          ...
        </div>
      </article>
      
      <footer>
        ...
      </footer>
    </body>
    ```
    将这些标记复制到 http://gsnedders.html5.org/outliner/ 中，可以看到下面的结果：
    ```
    1.Untitled Section
      1.How World Could End
        1.Mayan Doomsday
        2.Robot Takeover
        3.Unexplained Singularity
        4.Runaway Climate Change
        5.Global Epidenmic
    ```
    这个纲要开始于 **一个无标题的区块（Untitled Section）**，也就是 **最上层的 `<body>` 元素** 。然后，`<article>` 元素开始一个新的嵌套的纲要，其中包含一个 `<h1>` 元素和几个 `<h2>` 元素。
    
    有时候，Untitled Section意味着一个错误。尽管 `<aside>` 和 `<nav>` 元素可以不带标题，但 `<article>` 和 `<section>` 则万万不可。
    
    现在，再看一个复杂一点的例子。比如带有导航侧边栏的启示录站：
       http://prosetech.com/html5/Chapter%2002/FatFooter.html 。
    把相应的链接放到纲要生成器中，可以得到如下结果：
    ```
    1.Apocalypse Today
      1.Untitled Section
        1.Articles
        2.About Us
      2.How the World Could End
        1.Scenarios that spell the end of life as we know
        2.Mayan Doomsday
        3.Robot Takeover
        4.Untitled Section
        5.Unexplained Singularity
        6.Runaway Climate Change
        7.Global Epidemic
      3.Untitled Section
    ```
    这一次，标记包含三个分块元素，因此就有 **三个嵌套的纲要：**一个属于侧边栏，一个属于文章，另外一个属于页脚。此外，也有 **三个无标题区块** ，但都是合乎规范的-第一个是属于侧边栏的 `<aside>` 元素，第二个是文章中醒目引文的 `<aside>` 元素，第三个是页脚“胖”页脚部分的 `<aside>` 元素。
    
    除了分块元素之外，还有一些元素被称作 **区块根** （section root）。这些元素不是从已有纲要向下分支，而是 **产生自己的纲要，但不会出现在包含页面的主纲要视图中** 。比如包含网页内容的 `<body>` 元素就是一个区块根，这当然合理。但HTML5还把下列元素视为区块根：`<blockquote>` 、`<td>`、 `<fieldset>` 、 `<figure>` 和 `<details>` 。
    
    分块元素对复杂页面的意义：
    
    分块对联合（syndication）和聚合（aggregation）都有很大的意义，这两种方式都是从一个页面取得内容，然后注入到另一个页面的技术。
    
    在HTML5中，只要把这篇文章嵌套在一个 `<article>` 元素中，那么抓过来的内容就会变成它自己嵌套纲要的一部分。而这个纲要可以从任意级别的标题开始，几级标题都无所谓。
    
    **最后的结论是：** HTML5有一个讲究逻辑的纲要机制，让组合文档变得更容易。在这种纲要机制下，标题的位置变得更加重要，而实际的级别则没有那么重要了。
    
  - ##### 解决一个纲要问题
   
     有时候也会出现一个问题。比如：你创建了一下页面结构：

     ```
     <body>
       <article>
         <h1>Natural Wonders to Visit Before You Die</h1>
         ...
         <h2>In North America</h2>
         ...
         <h3>The Grand Canyon</h3>
         ...
         <h3>Yellowstone National Park</h3>
         ...
         <h2>In the Rest of the World</h2>
         ...
         <aside>...</aside>
         ...
         <h3>Galapagos Islands</h3>
         ...
         <h3>The Swisss Alps</h3>
         ...
       </article>
     </body>
     ```
     然后，你觉得这个页面的纲要应该是这样的：
     ```
     1.Untitled Section for the <body>
       1.Natural Wonders to Visit Before You Die
         1.In North America
          1.The Grand Canyon
          2.Yellowstone National Park
       2.In the Rest of the World
       3.Untitled Section for the <aside>
          1.Galapagos Islands
          2.The Swisss Alps
     ```
     但实际上纲要确实这样的：
     ```
     1. Untitled Section for the <body>
       1.Natural Wonders to Visit Before You Die
         1.In North America
           1.The Grand Canyon
           2.Yellowstone National Park
        2.In the Rest of the World
        3.Untitled Section for the <aside>
        4.Galapagos Islands
        5.The Swisss Alps
     ```
     不知怎么地， `<h2>` 后面多出来的那个 `<aside>` 把后面的两个 `<h3>` 元素都带了出来，让它们在逻辑上跟 `<h2>` 元素排在了同一层次上。这显然不是想要的结果。
     
    为解决这个问题，首先需要理解HTML5的纲要机制，即每次遇到编号标题元素（ `<h1>`、 `<h2>`、`<h3>` ，等等），只要该元素 **不在某个区块顶部 **，就会为它 **自动创建一个新的区块** 。
    
    对这个例子来说，纲要机制对一开头的 `<h1>` 元素什么都不做，因为它位于 `<article>` 区块的顶部。但纲要算法却会为接下来的 `<h2>` 和 `<h3>` 创建新的区块，就好像你写了如下标记一样：
    
    ```
     <body>
       <article>
         <h1>Natural Wonders to Visit Before You Die</h1>
         ...
         <section>
           <h2>In North America</h2>
           ...
           <section>
             <h3>The Grand Canyon</h3>
             ...
           </section>
           <section>
             <h3>Yellowstone National Park</h3>
             ...
           </section>
         </section>
         
         <section>
           <h2>In the Rest of the World</h2>
           ...
         </section>
         <aside>...</aside>
         ...
         <section>
           <h3>Galapagos Islands</h3>
           ...
         </section>
         <section>
           <h3>The Swisss Alps</h3>
           ...
         </section>
       </article>
     </body>
    ```
    多数情况下，这些自动创建的区块不是问题。事实上，这些区块通常还很有用，因为这样可以确保为正确编号的标题仍然能排在正确的纲要级别上。但这样做的代价就是偶尔会出现小差错，就像我们这个例子。
    
    而这个问题的原因就是因为 **碰到了 `<aside>` 元素，而关闭了为 `<h2>` 创建的子区块** 。
    
    为了纠正这个问题，需要通过自己定义区块和子区块，来代替纲要机制的自动创建行为。在标记中 **明确为该 `<h2>` 定义一个区块即可解决问题：**
    
    ```
     <body>
       <article>
         <h1>Natural Wonders to Visit Before You Die</h1>
         ...
         <h2>In North America</h2>
         ...
         <h3>The Grand Canyon</h3>
         ...
         <h3>Yellowstone National Park</h3>
         ...
         <section>
           <h2>In the Rest of the World</h2>
           ...
           <aside>...</aside>
           ...
           <h3>Galapagos Islands</h3>
           ...
           <h3>The Swisss Alps</h3>
           ...
         </section>
       </article>
     </body>
    ```
    这样，纲要算法就不必再为第二个 `<h2>` 自动创建区块了，因而也就避免了在它发现 `<aside>` 时去关闭该区块的风险。
    
    修改后的纲要：
    ```
     1.Untitled Section for the <body>
       1.Natural Wonders to Visit Before You Die
         1.In North America
           1.The Grand Canyon
           2.Yellowstone National Park
        2.In the Rest of the World
           1.Untitled Section for the <aside>
           2.Galapagos Islands
           3.The Swisss Alps
    ```
    另一个办法是用 `<div>` 元素替换 `<aside>` 。因为 `<div>` 不是分块元素，因此不会导致前面的区块意外关闭。
    
    假如不小心在 **两个不同级别的标题之间插入了一个分块元素 **，那就要检查一下纲要，看这样干是不是说得过去。
    
    最好是把HTML5的纲要机制当成一种 **质量保障工具** ，它有时候可以帮上你的忙。通过在纲要生成器中查看自己的标记，有可能 **发现其他问题导致的错误**，而且能够 **确保你正确地使用语义元素** 。


​    
​    