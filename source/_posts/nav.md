title: 简单导航
author: songxingguo
tags: []
categories:
  - 前端技术
date: 2018-08-29 23:53:00
---
## 垂直导航

### HTML 结构

```html
<ul>
    <li class="active"><a href="#">首　　页</a></li>
    <li><a href="#">新闻快讯</a></li>
    <li><a href="#">产品展示</a></li>
    <li><a href="#">售后服务</a></li>
    <li><a href="#">联系我们</a></li>
</ul>
```

<!-- more -->

### CSS 样式
  
 ```css
/* reset */
* {
   margin: 0;
   padding: 0;
   font-size: 14px;
}
/* reset */
ul {
  list-style:  none; /* 清除列表元素样式 */
  width: 100px;
}
a {
  text-decoration: none; /* 去掉 a 标签下划线 */
  display: block; /* 将 a 标签变成块级元素 */
  height: 30px;
  line-height: 30px;
  width: 100px;
  background-color: #ccc;
  margin-bottom: 1px;
  /* padding-left: 10px; */
  text-indent: 10px; /* 文本缩略 与 padding 等价但总体少了 10px*/
}
a:hover {
  background-color: #f60;
  color: fff;
}
```
上面代码，首先将元素样式 reset, 然后清除 `ul` 元素的默认样式并设置宽度为 `100px`，紧接着设置 `a` 标签的下划线去掉，并将 `a` 标签变成块级元素，这样就可以为 `a` 标签设置高度和宽度了，然后使用 `padding-left` 或 `text-indent` 为文本设置居左的距离，但是由于 `padding-left` 会增加元素整体的宽度，因此 `text-indent` 属性来缩进文本更合适。最后再为 `a` 标签添加上 `hover` 效果就大功告成了。
  
  
## 水平导航

### HTML 结构

> 水平导航和垂直导航的 HTML 结构是完全一致的，只需要对 CSS 样式进行调整。

### CSS 样式

```css
/* reset */
* {
   margin: 0;
   padding: 0;
   font-size: 14px;
}
/* reset */
ul {
  list-style:  none; /* 清除列表元素样式 */
/*   width: 100px; */ /* 去掉 */
}
li {
  float: left; /* 添加浮动 */
}
a {
  text-decoration: none; /* 去掉 a 标签下划线 */
  display: block; /* 将 a 标签变成块级元素 */
  height: 30px;
  line-height: 30px;
  width: 100px;
  background-color: #ccc;
  margin-bottom: 1px;
  text-align: center; /* 文本水平居中对齐 */
}
a:hover {
  background-color: #f60;
  color: fff;
}
```
上面代码是在垂直导航的样式上，首先将 `li` 元素设置为浮动，然后将 `ul` 元素的宽度去掉，刷新页面导航就水平了。接着将 `a` 元素的 `text-indent` 属性去掉，并使用 `text-align` 属性将文字设置为水平居中。

## 写在后面

[效果展示](https://songxingguo.github.io/nav/)

[参考地址](https://www.imooc.com/video/42)
   