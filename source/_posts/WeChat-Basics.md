title: 微信小程序基础学习
author: songxingguo
tags:
  - WeChat
categories:
  - 微信小程序
date: 2018-06-15 16:45:00
---
### 模板
- #### 模板的作用
   避免重复开发，提升效率。
   
   模板类似与React的组件思想提高重用性，但是模板只是只是包括视图和样式，并且两者需要分别引入对应的目标文件中。

- #### 模板的定义

 1. 创建模板文件
  
   ![模板文件](http://p9myzkds7.bkt.clouddn.com/%E6%A8%A1%E6%9D%BF%E6%96%87%E4%BB%B6.png)
 
   - 一个模板wxml文件中可以创建多个template，通过name进行区分。
   -  ES6的扩展语法：**...** , 可以用于将数据解构之后传给模板使用。

 2. .xml文件中定义模板
```
<template name="list-item">
   <view class="kind-list-item"></view>
</template>
```
<!-- more -->

- #### 模板的使用

 1. 在.wxml文件中引入模板的.wxml文件
  ```
   <import src="../../components/list-item/list-item.wxml" />
  ```
 2. 使用模板
  ```
   <template is="list-item" data="{{item}}"/>
  ```
 3. 在.wxss文件中引入模板的.wxss文件
 ```
 @import "../../components/list-item/list-item.wxss";
 ```  
---