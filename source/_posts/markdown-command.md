title: Markdown常用命令
author: songxingguo
tags:
  - 'Markdown '
  - ''
categories:
  - 常用命令
date: 2018-07-29 05:16:00
---
# Markdown常用命令

> 用于展示常用的Markdown命令，采用 **示例** 和 **代码** 的形式展示，部分语法会给出 **说明** 。

<!-- more -->

## 介绍Markdown

### 标题
  
  - #### Atx方式
    
    # 一级标题
    ## 二级标题
    ### 三级标题
    #### 四级标题
    ##### 五级标题
    ###### 六级标题
    
    ```
    # 一级标题
    ## 二级标题
    ### 三级标题
    #### 四级标题
    ##### 五级标题
    ###### 六级标题
    ```
  - #### Setext方式
  
    大标题
    ===
    小标题
    ---
    
    ```
    大标题
    ===
    小标题
    ---
    ```
    
### 列表

>符号之后的空格不能少，-+*效果一样，但不能混合使用，因混合是嵌套列表，内容可超长
 
 - #### 无序列表
  
   - 无序列表1
   - 无序列表2
   - 无序列表3
   
   ```
   - 无序列表1
   - 无序列表2
   - 无序列表3
   ```
 - #### 有序列表
 
   1. 有序列表1
   2. 有序列表2
   3. 有序列表3
   
   ```
   1. 有序列表1
   2. 有序列表2
   3. 有序列表3
   ```
  
 - #### 嵌套列表
 
   - 嵌套列表1
     + 嵌套列表2
     * 嵌套列表3
   - 嵌套列表4
   
   ```
   - 嵌套列表1
     + 嵌套列表2
     * 嵌套列表3
   - 嵌套列表4
   ```
   
### 引用
 
  > 这里是引用
   >>这里是引用中的引用
   
  ```
  > 这里是引用
   >>这里是引用中的引用
  ```

### 图片与链接

  - #### 插入连接
   
   [阿有的博客](https://www.songxingguo.com/)
   
   ```
   [阿有的博客](https://www.songxingguo.com/)
   ```
   ##### 自动连接
    
   <1328989942@qq.com>
   
   ```
   <1328989942@qq.com>
   ```
   
  - #### 插入图片
  
   ![这里是图片](http://pajsphiyq.bkt.clouddn.com/images/mine/mine-bg.jpg)
   
   ```
   ![这里是图片](http://pajsphiyq.bkt.clouddn.com/images/mine/mine-bg.jpg)
   ```
   ##### 设置图片宽度和高度
   
   <img src="http://pajsphiyq.bkt.clouddn.com/images/mine/mine-bg.jpg" width=256 height=auto alt="这里是设置宽度和高度的图片" />
   
   ```
   <img src="http://pajsphiyq.bkt.clouddn.com/images/mine/mine-bg.jpg" width=256 height=auto alt="这里是设置宽度和高度的图片" />
   ```

### 粗体与斜体
  
  - #### 粗体
  
   **这里是粗体**
   
   ```
   **这里是粗体**
   ```
  
  - #### 斜体
  
   *这里是斜体*
   
   ```
   *这里是斜体*
   ```
   
   
### 代码框

 ```
<!DOCTYPE html>
<html>
  <head>
    <title>A Tiny HTML Document</title>
  </head>
  <body>
    <p>Let's rock the browser, HTML5 style.</p>
  </body>
</html>
 ```
 
 ### 分割线

  ***
  
  ```
  ***
  ```
 
### 注释

```
<!-- 注释 -->
```

### 段落

  半方大的空白&ensp;或&#8194;看，飞碟
  全方大的空白&emsp;或&#8195;看，飞碟
  不断行的空白格&nbsp;或&#160;看，飞碟
  &emsp;&emsp;段落从此开始。
  
  ```
  半方大的空白&ensp;或&#8194;看，飞碟
  全方大的空白&emsp;或&#8195;看，飞碟
  不断行的空白格&nbsp;或&#160;看，飞碟
  &emsp;&emsp;段落从此开始。
  ```

### 标签

  `HTML`、`CSS`、`JavaScript`
  
  ```
  `HTML`、`CSS`、`JavaScript`
  ```
  
  
### 字体、字号、颜色

  <font face="黑体">我是黑体字</font>
  <font face="微软雅黑">我是微软雅黑</font>
  <font face="STCAIYUN">我是华文彩云</font>
  <font color=#0099ff size=12 face="黑体">黑体</font>
  <font color=#00ffff size=3>null</font>
  <font color=gray size=5>gray</font>
  
  ```
  <font face="黑体">我是黑体字</font>
  <font face="微软雅黑">我是微软雅黑</font>
  <font face="STCAIYUN">我是华文彩云</font>
  <font color=#0099ff size=12 face="黑体">黑体</font>
  <font color=#00ffff size=3>null</font>
  <font color=gray size=5>gray</font>
  ```
  
### 表格

项目      | 价格
--------   | ---
Computer   | $1600
Phone     | $12
Pipe      | $1

```
项目      | 价格
--------   | ---
Computer   | $1600
Phone     | $12
Pipe      | $1
```

### 生成目录

>该命令在本文档中是不能正确显示的。

```
[TOC]  
```

### 链接引用

> 可以用于将链接统一放在一起，方便管理。

[这里是链接][link]

[link]: https://www.songxingguo.com/

```
[这里是链接][link]

[link]: https://www.songxingguo.com/
```

 - #### 参考文献
 
  >该命令在本文档中是不能正确显示的。
  
   ```
   参考文献[^demo]
   [^demo]: http://marxi.co/client_en
   ```
   
## 相关文章阅读

 - [认识与入门 Markdown](https://sspai.com/post/25137)
 - [Markdown 语法 示例 字体 字号 颜色](https://blog.csdn.net/u011419965/article/details/50536937)
 - [Welcome to Marxico](http://marxi.co/#fn:demo)
 - [MarkDown 图片大小问题](https://blog.csdn.net/yhl_leo/article/details/50099843)
 - [Markdown 语法说明 (简体中文版)](http://wowubuntu.com/markdown/#list)
 - [Markdown 语法说明 (英文版)](https://daringfireball.net/projects/markdown/syntax)