title: 响应式导航
author: songxingguo
tags:
  - HTML
  - CSS
categories:
  - 开发者手册
date: 2018-08-12 14:56:00
---
## 单层导航

### HTML结构

```html
<div role="navigation" id="nav" class="opened">
    <ul>
        <li class="active"><a href="#">首页</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">项目</a></li>
        <li><a href="#">博客</a></li>
    </ul>
</div>
```
<!-- more -->

效果：

![HTML结构效果](https://graphbed.qiniu.songxingguo.com/Navigation/HTML%E7%BB%93%E6%9E%84%E6%95%88%E6%9E%9C.png)

> 实例中导航菜单的 HTML 结构中元素 `<ul>` 用来定位导航菜单，.active 表示当前活动的导航项。

### 简单的菜单样式

```css
#nav {
    position: absolute;
    width: 24%;
    top: 2em;
    left: 0
}
#nav ul {
    display: block;
    width: 100%;
    list-style: none
}
#nav li {
    width: 100%;
    display: block
}
#nav a {
    color: #aaa;
    font-weight: 700;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    -webkit-transition: background .3s ease;
    -moz-transition: background .3s ease;
    transition: background .3s ease;
    text-shadow: 0 -1px rgba(0,0,0,.5);
    border-bottom: 1px solid rgba(0,0,0,.2);
    border-top: 1px solid rgba(255,255,255,.1);
    display: block;
    padding: .6em 2em;
    width: 100%
}
#nav a:hover {
    background: rgba(255,255,255,.1)
}
#nav .active a {
    color: #fff;
    background: rgba(0,0,0,.3)
}
#nav li:first-child a {
    border-top: 0
}
#nav li:last-child a {
    border-bottom: 0
}
```
效果;

![简单样式](https://graphbed.qiniu.songxingguo.com/Navigation/%E7%AE%80%E5%8D%95%E6%A0%B7%E5%BC%8F.png)

### 媒体查询

```css
@media screen and (max-width: 40em) {
    .js #nav {
        clip:rect(0 0 0 0);
        max-height: 0;
        position: absolute;
        display: block;
        overflow: hidden
    }
    #nav {
        top: 0;
        width: 100%;
        position: relative
    }
    #nav.opened {
        max-height: 9999px
    }
    #nav a:hover {
        background: transparent
    }
    #nav .active a:hover {
        color: #fff;
        background: rgba(0,0,0,.3)
    }
    #toggle {
        -webkit-touch-callout: none;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        display: block;
        width: 70px;
        height: 55px;
        float: right;
        margin: 0 -2em 1em 0;
        text-indent: -9999px;
        overflow: hidden;
        background: #444 url("hamburger.gif") no-repeat 50% 33%
    }
    .main {
        -webkit-overflow-scrolling: auto;
        padding: 0 2em 2em;
        border-radius: 0;
        box-shadow: none;
        position: relative;
        width: 100%;
        overflow: hidden
    }
    .main::-webkit-scrollbar {
        background-color: transparent
    }
}
@media screen and (-webkit-min-device-pixel-ratio:1.3),screen and (min--moz-device-pixel-ratio:1.3),screen and (-o-min-device-pixel-ratio:2 / 1),screen and (min-device-pixel-ratio:1.3),screen and (min-resolution: 192dpi),screen and (min-resolution:2dppx) {
    body {
        -webkit-background-size: 200px 200px;
        -moz-background-size: 200px 200px;
        -o-background-size: 200px 200px;
        background-size: 200px 200px
    }
    #toggle {
        background-image: url("hamburger-retina.gif");
        -webkit-background-size: 100px 100px;
        -moz-background-size: 100px 100px;
        -o-background-size: 100px 100px;
        background-size: 100px 100px
    }
}
@media screen and (min-width: 76em) {
    #nav {
        width:18em
    }
 
    .main {
        width: auto;
        left: 18em
    }
}
```

![媒体查询](https://graphbed.qiniu.songxingguo.com/Navigation/%E5%AA%92%E4%BD%93%E6%9F%A5%E8%AF%A2.png)

### 基础样式

```css
body,div,h1,h2,p,ol,ul,li {
    margin: 0;
    padding: 0;
    border: 0
}
 
@-webkit-viewport {
    width: device-width
}
 
@-moz-viewport {
    width: device-width
}
 
@-ms-viewport {
    width: device-width
}
 
@-o-viewport {
    width: device-width
}
 
@viewport {
    width: device-width
}
 
html,body {
    min-height: 100%
}
 
body {
    min-width: 290px;
    -webkit-font-smoothing: antialiased;
    -webkit-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
    text-size-adjust: 100%;
    background: #444;
    color: #666;
    font: 400 100%/1.5 "Helvetica Neue",Helvetica,Arial,sans-serif
}
 
h1 {
    font-size: 3em;
    line-height: 1;
    color: #222;
    margin-bottom: .5em;
    float: left;
    width: 100%
}
 
h2 {
    float: left;
    width: 100%;
    font-size: 1.5em;
    color: #222;
    margin: 1em 0 .5em
}
 
p {
    float: left;
    width: 100%;
    margin-bottom: 1em
}
 
p.intro {
    font-size: 1.25em;
    color: #555;
    font-weight: 700
}
 
a {
    color: #f4f4f4;
    text-decoration: none
}
 
a:active,a:hover {
    outline: 0
}
 
.main {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    -webkit-overflow-scrolling: touch;
    padding: 3em 4em;
    position: fixed;
    overflow: hidden;
    overflow-y: scroll;
    border-top-left-radius: 5px;
    box-shadow: 0 0 15px rgba(0,0,0,.6);
    top: .8em;
    right: 0;
    bottom: 0;
    width: 76%;
    background: #fff
}
 
.main::-webkit-scrollbar {
    -webkit-appearance: none;
    background-color: rgba(0,0,0,.15);
    width: 8px;
    height: 8px
}
 
.main::-webkit-scrollbar-thumb {
    border-radius: 0;
    background-color: rgba(0,0,0,.4)
}
 
#toggle {
    display: none
}
```
效果：

![基础样式](https://graphbed.qiniu.songxingguo.com/Navigation/%E5%9F%BA%E7%A1%80%E6%A0%B7%E5%BC%8F.png)

## 多层导航

### HTML 结构

```html
<div class="subNavBox">
    <div class="subNav currentDd currentDt">
        新闻中心
    </div>
    <ul class="navContent " style="display:block">
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">新闻管理</a></li>
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">新闻管理</a></li>
    </ul>
    <div class="subNav">
        关于我们
    </div>
    <ul class="navContent">
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">新闻管理</a></li>
    </ul>
    <div class="subNav">
        业务系统
    </div>
    <ul class="navContent">
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">新闻列表</a></li>
        <li><a href="#">我的关注</a></li>
        <li><a href="#">新闻管理</a></li>
    </ul>
    <div class="subNav">
        招商联盟
    </div>
    <ul class="navContent">
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">新闻管理</a></li>
        <li><a href="#">添加新闻</a></li>
        <li><a href="#">新闻管理</a></li>
    </ul>
</div>
```
效果：

![多层导航 HTML 结构](https://graphbed.qiniu.songxingguo.com/Navigation/%E5%A4%9A%E5%B1%82%E5%AF%BC%E8%88%AAHTML%E7%BB%93%E6%9E%84.png)

### 多层导航样式

```css
.subNavBox {
    width: 200px;
    border: solid 1px #e5e3da;
    margin: 100px auto;
}
.subNav {
    border-bottom: solid 1px #e5e3da;
    cursor: pointer;
    font-weight: bold;
    font-size: 14px;
    color: #999;
    line-height: 28px;
    padding-left: 10px;
    background: url(../images/jiantou1.jpg) no-repeat;
    background-position: 95% 50%
}
.subNav:hover {
    color: #277fc2;
}
.currentDd {
    color: #277fc2
}
.currentDt {
    background-image: url(../images/jiantou.jpg);
}
.navContent {
    display: none;
    border-bottom: solid 1px #e5e3da;
}
.navContent li a {
    display: block;
    width: 200px;
    heighr: 28px;
    text-align: center;
    font-size: 14px;
    line-height: 28px;
    color: #333
}
.navContent li a:hover {
    color: #fff;
    background-color: #277fc2
}
```

效果：

![多层导航样式](https://graphbed.qiniu.songxingguo.com/Navigation/%E5%A4%9A%E5%B1%82%E5%AF%BC%E8%88%AA%E6%A0%B7%E5%BC%8F.png)

### 重置默认样式

```css
/*reset*/
body, div, dl, dt, dd, ul, ol, li, h1, h2, h3, h4, h5, h6, pre, code, form, fieldset, legend, input, textarea, p, blockquote, th, td { margin: 0; padding: 0; font-family:"Î¢ÈíÑÅºÚ"}
table { border-collapse: collapse; border-spacing: 0; }
fieldset, img { border: 0; }
address, caption, cite, code, dfn, em, strong, th, var { font-style: normal; font-weight: normal; }
ol, ul { list-style: none; }
caption, th { text-align: left; }
h1, h2, h3, h4, h5, h6 { font-size: 100%; font-weight: normal; }
q:before, q:after { content: ''; }
abbr, acronym { border: 0; font-variant: normal; }
sup { vertical-align: text-top; }
sub { vertical-align: text-bottom; }
input:focus, textarea:focus, select:focus { outline: none; }
select, input { vertical-align: middle; }
legend { color: #000; }
.clean:before, .clean:after, .clearfix:before, .clearfix:after { content: ""; display: table; }
.clean:after, .clearfix:after { clear: both; }
.clean, .clearfix { zoom: 1; }
.clear { clear: both; }
.fl { float: left; }
.fr { float: right; }
.break { word-wrap: break-word; width: inherit; }
.linkhidden { text-indent: -9999em; overflow: hidden; }
.hidden { display: none; }
a{ text-decoration:none;}
/*reset*/
```
效果：

![重置默认样式](https://graphbed.qiniu.songxingguo.com/Navigation/%E9%87%8D%E7%BD%AE%E9%BB%98%E8%AE%A4%E6%A0%B7%E5%BC%8F.png)

### 添加动态效果

```js
$(function() {
    $(".subNav").click(function() {
        $(this).toggleClass("currentDd").siblings(".subNav").removeClass("currentDd")
        $(this).toggleClass("currentDt").siblings(".subNav").removeClass("currentDt")


        $(this).next(".navContent").slideToggle(500).siblings(".navContent").slideUp(500);
    });
});
```
效果：

![动态效果](https://graphbed.qiniu.songxingguo.com/Navigation/%E5%8A%A8%E6%80%81%E6%95%88%E6%9E%9C%20.png)


## 导航效果

[效果展示](https://songxingguo.github.io/nav/)
