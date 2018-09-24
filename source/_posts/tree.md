title: 树
author: songxingguo
tags: []
categories:
  - 数据结构
date: 2018-09-24 20:49:00
---
## 基本概念

- 树高：树根的高度就是树的高度。
- 树深：最深的叶结点的深度就是树的深度。
- 树深与树高的区别：树的深度是从根节点开始（其深度为1）自顶向下逐层累加的，而高度是从叶节点开始（其高度为1）自底向上逐层累加的。

> 对整棵树而言，树的高度和深度是相等的；对于树中相同深度的每个结	点来说，它们的高度不一定相同，这取决于每个结点下面的叶结点的深度。  
    
 - 树的度：树中各结点度的最大值称为该树的度，称度为m的树为m叉	树。
 - 度：结点分支的数目。
 
 <!-- more -->
 
## 二叉树
 
### 二叉树基本性质
 
- 若二叉树的层次从1开始, 则在二叉树的第 i 层最多有 2^（i-1）个结	点。(i>=1)
- 深度为k的二叉树最多有 2^k -1个结点。 (k >= 1)(2^0 + 2^1 + 2^2 + 2^3 + … + 2^(k-1) = 2^k－ 1 )
- 对任何一棵二叉树, 如果其叶结点个数为 n0,  度为2的非叶结点个数	为 n2, 则有 n0＝n2＋1。

证明：

- 结点总数为度为0的结点加上度为1的结点再加上度为2的结点：n = n0 + n1 + n2
- 另一方面，二叉树中一度结点有一个孩子，二度结点有二个孩子，根结点不是任何结点的孩子，因此，结点总数为：n = n1 + 2n2 + 1
- 两式相减，得到：n0 = n2 + 1    

### 特殊二叉树：

- #### 完全二叉树:（用数组存储，方便且不浪费空间）

  - 只有最后一层可以不满，且所有元素的靠左。
  - 若完全二叉树的深度为k ，则所有的叶子结点都出现在第k层或k-1	层。
  - 对于任一结点，如果其右子树的最大层次为l，则其左子树的最大层次为	l或l+1

- #### 满二叉树：

  - 一棵深度为k 且有2k-1个结点的二叉树。
  - 可以按“自上而下、自左至右”的原则进行连续编号。
  - 满二叉树的所有的支结点都有左、右子树。

> 关系：完全二叉树是满二叉树的一部分，而满二叉树是完全二叉树的特例。

- #### 数组存储：（i为下标，从i = 0开始）

  i>1时：双亲：(i -1) / 2 
  不超过元素总数时：左孩子：2 * i + 1，右孩子: 2 * i + 2
 
### 笔记
 
 ![二叉树遍历](http://p9myzkds7.bkt.clouddn.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E9%81%8D%E5%8E%86.png)
  
 ![层序遍历](http://p9myzkds7.bkt.clouddn.com/%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.png)
 
 ![前序遍历](http://p9myzkds7.bkt.clouddn.com/%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86.png)
 
 ![中序遍历](http://p9myzkds7.bkt.clouddn.com/%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86.png)
 
 ![树的遍历](http://p9myzkds7.bkt.clouddn.com/%E6%A0%91%E7%9A%84%E9%81%8D%E5%8E%86.png)
 
 ![二维数组](http://p9myzkds7.bkt.clouddn.com/%E4%BA%8C%E7%BB%B4%E6%95%B0%E7%BB%84.png)
 
 ![数组](http://p9myzkds7.bkt.clouddn.com/%E6%95%B0%E7%BB%84.png)
 
 ![遍历](http://p9myzkds7.bkt.clouddn.com/%E9%81%8D%E5%8E%86.png)

## 堆

### 笔记

![堆](http://p9myzkds7.bkt.clouddn.com/tree/%E5%A0%86.png)

![堆](http://p9myzkds7.bkt.clouddn.com/tree/%E5%A0%861.png)

![堆的创建](http://p9myzkds7.bkt.clouddn.com/tree/%E5%A0%86%E7%9A%84%E5%88%9B%E5%BB%BA.png)

![堆排序](http://p9myzkds7.bkt.clouddn.com/tree/%E5%A0%86%E6%8E%92%E5%BA%8F.png)

## 哈夫曼树

### 哈夫曼树基本性质

- 边带权，且左孩子为0，右孩子为1。
- 哈夫曼树为一棵二叉查找树，在任一结点，左孩子小于结点，右孩子大于结点。

### 生成哈夫曼树

- 每次选取两个最小的两数组成树，相加也是最小的。
- 从左至右，有小到大排，形成一棵二叉查找树。
- 为边加权，左孩子为0，右孩子为1。

### 哈夫曼编码

每个结点，按路径依次读取权

### 加权路径长度（weight path length）

![加权路径长度](http://p9myzkds7.bkt.clouddn.com/tree/%E5%8A%A0%E6%9D%83%E8%B7%AF%E5%BE%84%E9%95%BF%E5%BA%A6.png)

 WPL = 所有叶子节点的权 * 路径长度（即叶子到根结点边的条数）再取和。

### 笔记

![创建哈夫曼树](http://p9myzkds7.bkt.clouddn.com/tree/%E5%88%9B%E5%BB%BA%E5%93%88%E5%A4%AB%E6%9B%BC%E6%A0%91.png)

![哈夫曼编码](http://p9myzkds7.bkt.clouddn.com/tree/%E5%93%88%E5%A4%AB%E6%9B%BC%E7%BC%96%E7%A0%81.png)

![哈夫曼树](http://p9myzkds7.bkt.clouddn.com/tree/%E5%93%88%E5%A4%AB%E6%9B%BC%E6%A0%91.png)

## 树的存储

### 存储方式

- 顺序存储（数组）
- 二叉链表结构
- 三叉链表结构

### 二叉树

![二叉树的存储](http://p9myzkds7.bkt.clouddn.com/tree/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%AD%98%E5%82%A8.jpg)

![二叉树的存储](http://p9myzkds7.bkt.clouddn.com/tree/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%AD%98%E5%82%A81.jpg)

### 普通树

![孩子表](http://p9myzkds7.bkt.clouddn.com/tree/%E5%AD%A9%E5%AD%90%E8%A1%A8.jpg)

![双亲表](http://p9myzkds7.bkt.clouddn.com/tree/%E5%8F%8C%E4%BA%B2%E8%A1%A8.jpg)

![双亲孩子表](http://p9myzkds7.bkt.clouddn.com/tree/%E5%8F%8C%E4%BA%B2%E5%AD%A9%E5%AD%90%E8%A1%A8.jpg)
