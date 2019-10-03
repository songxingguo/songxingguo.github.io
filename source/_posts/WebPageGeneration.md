title: 网页生成
author: songxingguo
tags: []
categories:
  - 开发者手册
date: 2019-04-17 12:11:00
---
### 网页生成过程

![网页生成](https://graphbed.qiniu.songxingguo.com/WebPageGeneration/%E7%BD%91%E9%A1%B5%E7%94%9F%E6%88%90-%E6%A6%82%E8%BF%B0.png)

![网页生成](https://graphbed.qiniu.songxingguo.com/WebPageGeneration/%E7%BD%91%E9%A1%B5%E7%94%9F%E6%88%90.png)

<!-- more -->

网页的生成过程，大致可以分成五步。
1. HTML代码转化成DOM
2. CSS代码转化成CSSOM（CSS Object Model）
3. 结合DOM和CSSOM，生成一棵渲染树（包含每个节点的视觉信息）
4. 生成布局（layout），即将所有渲染树的所有节点进行平面合成
5. 将布局绘制（paint）在屏幕上

### 参考链接

[网页性能管理详解](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)
