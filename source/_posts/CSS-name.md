title: CSS 如何命名
author: songxingguo
tags:
  - CSS
categories:
  - 前端技术
date: 2019-03-14 14:16:00
---
BEM 规范[https://en.bem.info/methodology/css/]

BEM 代表块（Block），元素（Element），修饰符（Modifier）。

```html
< header  class = “header” > 
    <！ -
    `header__button`  - 块元素`header`;
    `button`  - 块;
    `button_theme_islands`  - 修饰符。
    - > 
    < button  class = “header__button button button_theme_islands” > ... </ button > 
</ header >
```
<!-- more -->

## 命令规则

block-name__elem-name_mod-name_mod-val

块名：

HTML

```html
<div class="menu">...</div>
```
CSS

```css
.menu { color: red; }
```

元素名：

HTML

```html
<div class="menu">
    ...
    <span class="menu__item"></span>
</div>
```
CSS

```css
.menu__item { color: red; }
```

块修饰名

menu_hidden

menu_theme_islands

HTML

```html
<div class="menu menu_hidden"> ... </div>
<div class="menu menu_theme_islands"> ... </div>
```
CSS

```css
.menu_hidden { display: none; }
.menu_theme_islands { color: green; }
```
元素修饰名

menu__item_visible

menu__item_type_radio

HTML

```html
<div class="menu">
    ...
    <span class="menu__item menu__item_visible menu__item_type_radio"> ... </span>
</div>
```
CSS

```css
.menu__item_visible {}
.menu__item_type_radio { color: blue; }
```

替代命令方案

双虚线样式

block-name__elem-name--mod-name--mod-val

驼峰命令

blockName-elemName_modName_modVal

React 

BlockName-ElemName_modName_modVal

无命名空间 

_available

## 命令

选择器的名称必须完整准确地描述它所代表的BEM实体。

```css
.button {}
 .button__icon {}
 .button__text {}
 .button_theme_islands {}
```

```html
< button  class = “button button_theme_islands” > 
    < span  class = “button__icon” > </ span >

    < span  class = “button__text” > ... </ span > 
</ button >
```

HTML

```html
<！ - `logo` block  - > 
< div  class = “logo logo_theme_islands” > 
    < img  src = “URL”  alt = “logo”  class = “logo__img” > 
</ div >

<！ - `user` block  - > 
< div  class = “user user_theme_islands” > 
    < img  src = “URL”  alt = “user-logo”  class = “user__img” >
  ...
</ div >
```

```css
.logo {}                   / *`logo`块的CSS类* /

.logo__img {}              / *`logo__img`元素的CSS类* /

.logo_theme_islands {}     / *`logo_theme_islands`修饰符的CSS类* /

.user {}                   / *`user`块的CSS类* /

.user__img {}              / *`user__img`元素的CSS类* /

.user_theme_islands {}     / *`user_theme_islands`修饰符的CSS类* /
```
## 修饰符

BEM修饰符设置块的外观，状态和行为。

HTML实现：

```html
< button  class = “button button_size_s” > ... </ button >
```
CSS实现：

```css
.button {
     font-family：Arial，sans-serif;
    text-align：center;
}

.button_size_s {
     font-size：13px ;
    line-height：24px ;
}

.button_size_m {
     font-size：15px ;
    行高：28px ;
}
```

## 混合


- 结合多个实体的行为和样式，无需复制代码。

- 将相同的格式应用于不同的HTML元素。

HTML实现：

```html
<！ - `header` block  - > 
< header  class = “header” > 
      < button  class = “button header__button” > ... </ button > 
</ header >
```
CSS按钮的实现：

```css
.button {
     font-family：Arial，sans-serif;
    text-align：center;
    边框：1px纯黑色;    / *框架* /
}
.header__button {
     margin：30px ;               / *填充* / 
    位置：相对;
}
```

HTML实现：

```html
< article  class = “article text” > ... </ article >

< footer  class = “footer” > 
    < div  class = “copyright text” > ... </ div > 
</ footer >
```
CSS实现：

```css
.text {
     font-family：Arial，sans-serif;
    font-size：14px ;
    颜色：＃000 ;
}
```

## 单一责任原则

每个CSS实现都必须承担单一责任。


责：外部几何和定位（让我们button通过header__button元素设置外部几何和块的定位）。

.header__button {
     margin：30px ;
    位置：相对;
}

## 开放/封闭原则

页面上的任何HTML元素都应该通过修饰符打开以进行扩展，但是对于更改是关闭的。

HTML实现：

```html
< button  class = “button” > ... </ button > 
< button  class = “button button_size_s” > ... </ button >
```
CSS实现：

```css
.button {
     font-family：Arial，sans-serif;
    text-align：center;
    font-size：11px ;
    行高：20px ;
}

.button_size_s {
     font-size：13px ;
    line-height：24px ;
}
```

## DRY

DRY（“不要重复自己”）是一种软件开发原则，旨在减少代码中的重复。

HTML实现：

```html
< button  class = “button button_theme_islands” > ... </ button > 
< button  class = “button button_theme_simple” > ... </ button >
```
CSS实现：

```css
.button {
     font-family：Arial，sans-serif;
    text-align：center;
}

.button_theme_islands {
     color：＃000 ;
    背景：#fff ;
}

.button_theme_simple {
     color：＃000 ;
    background：rgba（255,0,0,0.4）;
}
```

重要 DRY原则仅适用于页面功能相似的组件

## 重新定义级别

在项目中实施BEM原则：

- 抛开DOM模型并学习创建块。

- 不要使用ID选择器或标签选择器。

- 最小化嵌套选择器的数量。

- 使用CSS类命名约定以避免名称冲突，并使选择器名称尽可能地提供信息和清晰。

- 在块，元素和修饰符方面工作。

- 如果块似乎可能更改，则将块的CSS属性移动到修饰符。

- 使用混合物。

- 将代码分成小的独立部分，以便于使用单个块。

- 重复使用块。

常用 CSS 命名单词：

- head
- content
- foot

- header
- footer

- body
- item

- inner