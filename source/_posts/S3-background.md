title: CSS3 背景
author: songxingguo
tags:
  - CSS3
categories:
  - 前端技术
  - ''
date: 2019-01-28 16:26:00
---
## 背景的基本属性

背景主要包括 5 个属性：

- background-color：指定背景颜色。
- background-image：指定背景图片。
- background-repeat: 指定背景展示方式。
- background-attachment：指定背景图片是固定还是滚动。
- background-position：指定背景图片位置。

新增的背景属性：

- background-origin：指定绘制背景图片的起点。
- background-clip：指定背景图片的显示范围。
- background-size：指定背景图片的尺寸大小。
- background-break：指定内联元素的背景图片进行平铺时的循环方式。

<!-- more -->

## background-color 属性

语法：

```css
background-color: transparent || <color>
```
> 用来设置元素的背景色，其默认值为 “transparent”，不设置任何颜色情况下是透明色。

常用颜色格式：

- 颜色名：如 “red”；
- rgb 色：如 rgb(255, 0, 0) 或 rgb(100%, 0%, 0%)；
- hls 值：如 hsl(0, 100%, 50%)；
- 十六进制值：如 #ff0000；
- raba 色：如 rgba(255, 0, 0, 0, 3)；
- hsla 值：如 hsla(0, 100%, 50%, .0.5)；

## background-image 属性

语法：

```css
background-image: none || <url>
```
> 用来设置元素的背景图片，默认值为 “none”, `<url>` 是指背景图片的地址，可以是相对地址，也可以是绝对地址。

## background-repeat 属性

语法：

```css
background-repeat: repeat || repeat-x || repeat-y || no-repeat
```
> 用来设置元素背景图片在元素的盒模型中的铺放方式，其默认值为 “repeat”，也就是背景图片沿着元素的 X 轴和 Y 轴同时平铺。

## background-attachment 属性

语法：

```css
background-attachment: scroll || fixed
```
> 用来设置元素背景图片是否固定或随着页面的其余部分滚动，其默认值为 “scroll”, 表示背景图片会随着浏览器滚动条一起滚动。

> background-attachment 取值 “fixed”时，一般运用在 html 或 body 标签上，使用在其他标签上不能达到固定的效果。

## background-position 属性

语法：

```css
background-position: <percentgage> || <length> || [left|center|right] [,top|center|bottom]
```
> 用来设置元素背景图片的位置，其默认值为 (0, 0) || (0%, 0%) || (left top)。

背景图片相同的定位方式

![背景图片相同的定位方式](https://graphbed.qiniu.songxingguo.com/CSS3-background/%E8%83%8C%E6%99%AF%E5%9B%BE%E7%89%87%E7%9B%B8%E5%90%8C%E7%9A%84%E5%AE%9A%E4%BD%8D%E6%96%B9%E5%BC%8F.png)

## background-origin 属性

老语法：

```css
background-origin: padding || border || content
```
新语法：

```css
background-origin: padding-box || border-box || content-box
```

> 用来决定 background-position 属性的参考原点，即决定背景图片定位的起点。其默认值为元素左上角为坐标原点对背景图片进行背景定位。

三个属性的作用：

- **padding-box(padding)**：默认值，决定 background-position 起始位置从 padding 的外边缘（boder 的内边缘）开始显示背景图片。
- **border-box(border)**：默认值，决定 background-position 起始位置从 border 的外边缘开始显示背景图片。
- **content-box(content)**：默认值，决定 background-position 起始位置从 content 的外边缘（padding 的内边缘）开始显示背景图片。

## background-clip 属性

语法：

```css
background-clip: border-box || padding-box || content-box
```
> 用于定义背景图像的裁剪区域。

参数含义：

- **border-box** ：默认值，元素背景图像从元素的 border 区域向外裁剪，即元素边框之外的背景图片都将被裁剪掉。
- **padding-box** ：默认值，元素背景图像从元素的 padding 区域向外裁剪，即元素  padding 之外的背景图片都将被裁剪掉。
- **content-box** ：默认值，元素背景图像从元素的 content 区域向外裁剪，即元素  content 之外的背景图片都将被裁剪掉。

## background-size 属性

语法：

```css
background-size: auto  || <length> || <perentage> || cover || contain
```
> 用于指定背景图片的尺寸，可以控制背景图片在水平和垂直两个方向的缩放，也可以控制图片拉伸覆盖背景区域的方式，甚至还可以截取背景图片。

5 种属性的作用：

- auto： 默认值。将保持背景图片的原始高度和宽度。
- `<length>`：取具体的整数值(例如 px 值)，将改变背景图片的大小。
- `<percentage>`：取值为百分值，可以是 0% ~ 100%。此值是相对于 **元素的宽度** 来进行计算的，并不是根据背景图片的宽度来进行计算。
- cover：将背景图片放大，以适合铺满整个容器。但这种方法会致使背景图片失真。
- contain: 保持背景图像本身的宽高比例，将背景图像缩放到宽度或高度正好适应所定义背景容器的区域。

> 固定数值（length）和百分比值（percentage）时可以设置两个值，也可以设置一个值。只取一个值时，指定了背景图片的宽度，第二个值相当于 auto。

> background-size:conver 配合 background-position:center 常用来制作满屏背景效果（背景图片需要足够大）。

## background-break 属性

语法：

```css
background-break: bounding-box || each-box || continuous
```
> 用于指定内联元素背景图像进行平铺的循环方式。

3 个属性值：

- bounding-box：背景图像在整个内联元素中进行平铺。
- each-box：背景图像在行内中进行平铺。
- continuous：下一行的背景图像紧接着上一行中的图像继续平铺。

> 受限于浏览器的支持度，目前使用度极低。

## CSS3 多背景属性

语法：

```css
background ： [background-color] | [background-image] | [background-position][/background-size] | [background-repeat] | [background-attachment] | [background-clip] | [background-origin],...
```
可以把上面的缩写拆解成以下形式：

```css
background-image:url1,url2,...,urlN;
background-repeat : repeat1,repeat2,...,repeatN;
backround-position : position1,position2,...,positionN;
background-size : size1,size2,...,sizeN;
background-attachment : attachment1,attachment2,...,attachmentN;
background-clip : clip1,clip2,...,clipN;
background-origin : origin1,origin2,...,originN;
background-color : color;
```
> 用于层叠多张背景图片，摆脱对 Photoshop 等绘图工具的依赖。