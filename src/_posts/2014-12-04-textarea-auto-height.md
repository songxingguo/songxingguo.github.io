---
title: textarea高度自适应
categories:
  - JavaScript
  - 前端杂烩
tags:
  - 问答
  - textarea
  - 自适应
date: 2014-12-04 00:00:00
---


问题：textarea数据是数据库直接输出填充，请问如何做到自适应?

回答：

```javascript
var tt = document.getElementsByTagName("textarea").item(0),
    len = tt.value.length,
    width = tt.clientWidth,
    style = (window.getComputedStyle
                ||function(a){return a.currentStyle})(tt, null),
    fs = parseInt(style['font-size']) || 12,
    lh = parseInt(style['line-height']) || fs * 1.2;

tt.style.height = Math.ceil(len / (width / fs)) * lh + "px";
```

这里需要注意的是：

- line-height 可能是 normal/inherit 之类的值，所以最好加上一个默认值
- 上面算法适合等宽字体
- 防止计算误差，使用 Math.ceil 函数，比较靠谱的方式是 Math.ceil(len / (width / fs) - 1)，可以少一行~

当然，最简单的方式莫过于：

```javascript
tt.onpropertychange = tt.oninput = function(){
    this.style.height = this.scrollHeight;
};
```
    
虽然 css 也可以做到，但效果并不是很好：

```css
.tt {
    overflow-x: hide;
    overflow-y: visible;
    width: 300px;
}
```

找个有 textarea 的页面，打开控制台，输入字符，验证下吧~
