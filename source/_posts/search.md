title: 《大话数据结构》-读书笔记-查找
author: songxingguo
tags: []
categories:
  - 读书笔记
date: 2018-10-15 14:40:00
---
### 顺序查找

```c
/* 顺序查找，a为数组，n为要查找的数组长度， 
key为要查找的关键字 */
int Sequential_Search(int * a, int n, int key) {
  int i;
  for (i = 1; i <= n; i++) {
    if (a[i] == key) return i;
  }
  return 0;
}
```
<!--  more -->

### 有哨兵顺序查找

```c
/* 有哨兵顺序查找 */
int Sequential_Search2(int * a, int n, int key) {
  int i;
  /* 设置a[0]为关键字值，我们称之为“哨兵” */
  a[0] = key;
  /* 循环从数组尾部开始 */
  i = n;
  while (a[i] != key) {
    i--;
  }
  /* 返回0则说明查找失败 */
  return i;
}
```

### 有序表查找

#### 折半查找 

```
/* 折半查找 */
int Binary_Search(int * a, int n, int key) {
  int low,
  high,
  mid;
  /* 定义最低下标为记录首位 */
  low = 1;
  /* 定义最高下标为记录末位 */
  high = n;
  while (low <= high) {
    /* 折半 */
    mid = (low + high) / 2;
    /* 若查找值比中值小 */
    if (key < a[mid])
    /* 最高下标调整到中位下标小一位 */
    high = mid - 1;
    /* 若查找值比中值大 */
    else if (key > a[mid])
    /* 最低下标调整到中位下标大一位 */
    low = mid + 1;
    else
    /* 若相等则说明mid即为查找到的位置 */
    return mid;
  }
  return 0;
}
```
#### 斐波那契查找

```c
/* 斐波那契查找 */
int Fibonacci_Search(int * a, int n, int key) {
  int low,
  high,
  mid,
  i,
  k;
  /*定义最低下标为记录首位 */
  low = 1;
  /*定义最高下标为记录末位 */
  high = n;
  k = 0;
  /* 计算n位于斐波那契数列的位置 */
  while (n > F[k] - 1) k++;
  /* 将不满的数值补全 */
  for (i = n; i < F[k] - 1; i++) a[i] = a[n];
  while (low <= high) {
    /* 计算当前分隔的下标 */
    mid = low + F[k - 1] - 1;
    /* 若查找记录小于当前分隔记录 */
    if (key < a[mid]) {
      /* 最高下标调整到分隔下标mid-1处 */
      high = mid - 1;
      /* 斐波那契数列下标减一位 */
      k = k - 1;
    }
    /* 若查找记录大于当前分隔记录 */
    else if (key > a[mid]) {
      /* 最低下标调整到分隔下标mid+1处 */
      low = mid + 1;
      /* 斐波那契数列下标减两位 */
      k = k - 2;
    } else {
      if (mid <= n)
      /* 若相等则说明mid即为查找到的位置 */
      return mid;
      else
      /* 若mid>n说明是补全数值，返回n */
      return n;
    }
  }
  return 0;
}
```
### 线性索引查找

### 二叉排序树


首先我们提供一个二叉树的结构。 

```c
/* 二叉树的二叉链表结点结构定义 */
/* 结点结构 */
typedef struct BiTNode {
  /* 结点数据 */
  int data;
  /* 左右孩子指针 */
  struct BiTNode * lchild,
  *rchild;
}
BiTNode, *BiTree;
```
然后我们来看看二叉排序树的查找是如何实现的。 

```c
/* 递归查找二叉排序树T中是否存在key, */
/* 指针f指向T的双亲，其初始调用值为NULL */
/* 若查找成功，则指针p指向该数据元素结点，并 
返回TRUE */
/* 否则指针p指向查找路径上访问的最后一个结点 
并返回FALSE */
Status SearchBST(BiTree T, int key, BiTree f, BiTree * p) {
  /* 查找不成功 */
  if (!T) { * p = f;
    return FALSE;
  }
  /* 查找成功 */
  else if (key == T - >data) { * p = T;
    return TRUE;
  } else if (key < T - >data)
  /* 在左子树继续查找 */
  return SearchBST(T - >lchild, key, T, p);
  else
  /* 在右子树继续查找 */
  return SearchBST(T - >rchild, key, T, p);
}
```