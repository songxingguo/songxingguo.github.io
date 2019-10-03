title: Flex布局
tags:
  - Flex
categories:
  - 开发者手册
author: songxingguo
date: 2018-06-20 14:37:00
---
### 基本概念

- Flex布局分为：父容器和子容器，主轴和交叉轴。

- 关于Flex的属性一共有 **13** 个，分别是：

 1.  justify-content
 2.  align-items
 3.  align-content
 4.  flex-direction
 5.  flex-wrap
 6.  flex-flow
 7.  flex-basis
 8.  flex-grow
 9.  flex-shrink
 10. flex
 11. order
 12. algn-self
 13. display

<!-- more -->
- 按作用的对象（子容器和父容器 ）可以分为下面的内容：

 ![按作用的对象进行分类](https://graphbed.qiniu.songxingguo.com/flex%E6%8C%89%E5%AE%B9%E5%99%A8%E5%88%86%E7%B1%BB.png)

- 按属性的功能，我们大致可以把属性分为 **4** 大类，分别是：

  - 元素排列（justify-content、align-items、algn-self, align-content）
  - 换行方式 （flex-direction、 flex-wrap、flex-flow）
  - 伸缩方式 （flex-basis、flex-grow、flex-shrink、flex）
  - 主轴排序 （order）

 下面从 **4** 个大类分别介绍每个属性。

----

### 元素排列

元素排列共有三个属性，分别是：

 - #### justify-content 

  设置子容器沿主轴排列, 用于定义如何 **沿着主轴方向排列子容器**。

   - 位置排列
   
     - flex-start：起始端对齐
     - flex-end：末尾段对齐
     - center：居中对齐
     
   - 分布排列
     - space-around：**元素周围添加间距** ，子容器沿主轴均匀分布，位于首尾两端的子容器到父容器的距 离是子容器间距的一半（**子容器和父容器之间的距离是子容器之间距离的一半**）。
     
     - space-between：**元素之间添加间距** ，子容器**沿主轴均匀分布**，位于首尾两端的**子容器与父容器相切**。

- #### align-items

   设置子容器如何沿交叉轴排列,用于定义如何 **`单行` 沿着交叉轴向分配子容器的间距**。
  
  - 位置排列
  
     - flex-start：起始端对齐
     - flex-end：末尾段对齐
     - center：居中对齐
     
  - 基线对齐
   
    - baseline： baseline 默认是指 **首行文字**，即 first baseline，所有 **子容器向基线对齐**，**交叉轴起点到元素基线距离最大的子容器** 将会 **与交叉轴起始端相切以确定基线**。
    
  - 拉伸排列
  
    - stretch：子容器 **沿交叉轴方向的尺寸** 拉伸至 **与父容器一致**。
    
- #### algn-self
  
     单独设置子容器如何沿交叉轴排列。每个子容器也可以单独定义沿交叉轴排列的方式，此属性的可选值与父容器 align-items 属性完全一致，如果两者同时设置则以子容器的 align-self 属性为准。
     
- #### align-content 

   多行沿交叉轴对齐, 用于子容器 `多行` 排列时，设置行与行之间的对齐方式。属性和上面所说的类似，只是 **align-content** 是 **行与行之间** 的排列，而 **justify-content** 、**align-items** 、 **algn-self** 是 **子容器之间** 的排列。
   
   - 位置排列
     
     - flex-start：起始端对齐
     - flex-end：末尾段对齐
     - center：居中对齐
     
   - 分布排列 
   
     - space-around：等边距均匀分布
     - space-between：等间距均匀分布
     - stretch：拉伸对齐
     
---
### 换行方式

  - #### flex-direction 
  
    主轴的方向,用于决定主轴的方向，交叉轴的方向由主轴确定。（其他属性都要参照主轴确定）
    
    - 主轴
    
        主轴的起始端由 flex-start 表示，末尾段由 flex-end 表示。不同的主
        轴方向对应的起始端、末尾段的位置也不相同。
        
      - 向右：flex-direction: row
      - 向下：flex-direction: column
      - 向左：flex-direction: row-reverse
      - 向上：flex-direction: column-reverse
      
    - 交叉轴
    
      主轴沿逆时针方向旋转 90° 就得到了交叉轴，交叉轴的起始端和末尾段也由      你flex-start 和 flex-end 表示。
   
  - #### flex-wrap
  
    用于决定子容器是否换行排列，不但可以顺序换行而且支持逆序换行。
    
    - nowrap：不换行
    
    - wrap：换行
    
    - wrap-reverse：逆序换行
    
     逆序换行是指沿着交叉轴的反方向换行。
    
  - #### flex-flow
  
     flow 即流向，也就是子容器沿着哪个方向流动，流动到终点是否允许换行。相当于 flex-direction 与 flex-wrap 的组合。
     
---
### 伸缩方式

  - #### flex-basis

    设置基准大小，表示在不伸缩的情况下子容器的原始尺寸。**主轴为横向时代表宽度，主轴为纵向时代表高度**。
    
  - #### flex-grow
   
     设置扩展比例,子容器弹性伸展的比例。剩余空间按比例分配给子容器。
     
  - #### flex-shrink
   
     设置收缩比例, 子容器弹性收缩的比例。超出的部分按比例从给子容器中减去。
     
  - #### flex
   
     主轴上如何伸缩，是上面三个属性的缩写，允许 1 - 3 个值连用，子容器是有弹性的（flex 即弹性），它们会自动填充剩余空间，子容器的伸缩比例由 flex 属性确定。
     
     - 无单位数字（如：1, 2, 3）
     - 有单位数字（如：15px，30px，60px）
     - none 关键字。
     
     子容器会按照 flex 定义的尺寸比例自动伸缩，如果取值为 none 则不伸缩。
     
---
### 主轴排序

   - #### order
   
     设置排列顺序,改变子容器的排列顺序，覆盖 HTML 代码中的顺序，默认值为 0，可以为负值，数值越小排列越靠前。
     
---
### 回顾
以上就是 flex 布局的全部属性，一共 12 个，父容器、子容器各 6 个，可以随时通过下图进行回顾。

  ![按作用的对象进行分类](https://graphbed.qiniu.songxingguo.com/flex%E6%8C%89%E5%AE%B9%E5%99%A8%E5%88%86%E7%B1%BB.png)

---
### 学习误区

  1. Flex布局没有水平轴和垂直轴的概念，只有主轴和交叉轴，主轴可以是水平的也可以是水平的也可以是垂直的，这要根据具体的 **flex-direction** 属性设置来决定。
  
  2. 在每两个嵌套元素之间都可以使用flex布局，并且flex布局的元素也可以嵌套使用。

---
### 感谢

参考文章:

[一劳永逸的搞定 flex 布局 - osimly](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb)

[Flex 布局教程：语法篇 - 阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[Flex 布局教程：实例篇 - 阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
