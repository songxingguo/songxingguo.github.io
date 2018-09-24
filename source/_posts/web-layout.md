title: 网页布局
author: songxingguo
tags: []
categories:
  - 前端技术
date: 2018-08-02 05:06:00
---
## 圣杯布局

### [圣杯布局效果](https://songxingguo.github.io/HuaQing/Flex%E5%B8%83%E5%B1%80/%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80.html)

<iframe src="https://songxingguo.github.io/HuaQing/Flex%E5%B8%83%E5%B1%80/%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80.html"  width="100%" height="500" frameborder="0" align="middle" ></iframe>

### HTML 结构

```html
<body class="HolyGrail">
  <header></header>
  <div class="body">
    <main class="content"></main>
    <nav class="nav"></nav>
    <aside class="ads"></aside>
  </div>
  <footer></footer>
</body>
```

### CSS 样式

```css
.HolyGrail {
	display: flex;
	min-height: 100vh;
	flex-direction: column;
	position: fixed;
	width: 100%;
}
header, footer {
	flex: 1;
	max-height: 60px;
	height: 5%;
	background-color: #666666;
}
.body {
	display: flex;
	flex: 1;
}
.content {
	flex: 1;
	background-color: #D6D6D6;
}
.nav, .ads {
	/* 两个边栏的宽度设为12em */
	flex: 0 0 12em;
	background-color: #FF6633;
}
.nav {
	/* 导航放到最左边 */
	order: -1;
	background-color: #77BBDD;
}
/*如果是小屏幕，躯干的三栏自动变为垂直叠加。*/
@media (max-width: 768px) {
	.body {
		flex-direction: column;
		flex: 1;
	}
	.nav, .ads, .content {
		flex: auto;
	}
}
```
### 完整代码

[圣杯布局](https://github.com/songxingguo/HuaQing/blob/master/Flex%E5%B8%83%E5%B1%80/%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80.html)


## 响应式页面

### [响应式页面效果](https://songxingguo.github.io/HuaQing/Flex%E5%B8%83%E5%B1%80/%E5%93%8D%E5%BA%94%E5%BC%8F%E9%A1%B5%E9%9D%A2.html)

<iframe src="https://songxingguo.github.io/HuaQing/Flex%E5%B8%83%E5%B1%80/%E5%93%8D%E5%BA%94%E5%BC%8F%E9%A1%B5%E9%9D%A2.html"  width="100%" height="180" frameborder="0" align="middle" ></iframe>

### HTML 结构

```html
<div class="flex-container">
  <header class="header">头部</header>
  <article class="main">
    <p>内容</p>
  </article>
  <aside class="aside aside1">边栏 1</aside>
  <aside class="aside aside2">边栏 2</aside>
  <footer class="footer">底部</footer>
</div>
```
### CSS 样式

```
* {
	text-align: center;
}
.flex-container {
    display: -webkit-flex;
    display: flex;  
    -webkit-flex-flow: row wrap;
    flex-flow: row wrap;
    font-weight: bold;
    text-align: center;
}

.flex-container > * {
    padding: 10px;
    flex: 1 100%;
}

.main {
    text-align: left;
    background: cornflowerblue;
}

.header {background: coral;}
.footer {background: lightgreen;}
.aside1 {background: moccasin;}
.aside2 {background: violet;}

@media all and (min-width: 600px) {
    .aside { flex: 1 auto; }
}

@media all and (min-width: 800px) {
    .main    { flex: 3 0px; }
    .aside1 { order: 1; } 
    .main    { order: 2; }
    .aside2 { order: 3; }
    .footer  { order: 4; }
}
```
### 完整代码

[响应式页面](https://github.com/songxingguo/HuaQing/blob/master/Flex%E5%B8%83%E5%B1%80/%E5%93%8D%E5%BA%94%E5%BC%8F%E9%A1%B5%E9%9D%A2.html)

> 如果要让代码在移动端生效需要加上：
```
 <meta charset="utf-8" name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
```