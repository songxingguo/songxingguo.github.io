title: Web前端面试题目汇总
author: songxingguo
tags: []
categories:
  - 找工作
date: 2018-08-04 03:02:00
---
## HTML/CSS部分

### 什么是盒模型

 > 在网页中，一个元素占有空间的大小构成，采用盒子作一个比喻来理解。

 - **盒模型的组成：** 由里向外由 **content** 、**padding**、 **border** 、**margin** 四部分组成。
 
 - **盒模型的两种标准：**  标准模型，IE模型。
 
   - 两种标准的区别：
 
     在标准模型中，盒模型的 **宽高** 只是 **内容（content）的宽高** 。

     ![标准模型](http://p9myzkds7.bkt.clouddn.com/web-interview/%E6%A0%87%E5%87%86%E6%A8%A1%E5%9E%8B.png)

     在IE模型中盒模型的 **宽高** 是 **内容(content)+填充(padding)+边框(border)的总宽高** 。

     ![IE模型](http://p9myzkds7.bkt.clouddn.com/web-interview/IE%E6%A8%A1%E5%9E%8B.png)
 
   - CSS如何设置两种模型
  
     CSS3 的属性 box-sizing
     
     ```
     /* 标准模型 */
     box-sizing:content-box;

     /*IE模型*/
     box-sizing:border-box;
     ```

 - **边距重叠：** 两个 **相邻元素** ，如果 **上面的元素** 设置了 **margin-bottom** ，而 **下面的元素** 设置了 **margin-top** ，那么两者的 **margin将会重叠** ，并且将 **使用较大的margin作为最终的边距** 。
 
   - 避免方式：只设置 **margin-top** 或 **margin-bottom**  ，而不是将两者混合使用。
 
 [深入理解CSS盒模型](https://www.cnblogs.com/chengzp/p/cssbox.html)
 
### HTML元素构成

  > 行内元素、块级元素、空(void)元素
  
  - **行内元素：** a、b、span、img、input、strong、select、label、em、button、textarea 。
  
  - **块级元素：** div、ul、li、dl、dt、dd、p、h1-h6、blockquote 。
  
  - **空元素：** 即没有内容的HTML元素，例如：br、meta、hr、link、input、img 。

### CSS实现水平居中

- 对于有宽度的元素
 
  使用margin


### CSS实现垂直居中