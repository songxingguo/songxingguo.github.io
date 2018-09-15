title: 添加搜索
author: songxingguo
tags: []
categories:
  - 博客搭建
date: 2018-09-12 14:00:00
---
## 写在前面

下面展示的是使用 [swiftype](https://swiftype.com/) 实现站内搜索。

## 注册swiftype账号

可以在 [swiftype 官网](https://swiftype.com/) 中注册一个账号，就像下面这样。

![注册账号](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E6%B3%A8%E5%86%8C%E8%B4%A6%E5%8F%B7.png)

## 创建搜索引擎

点击下方的 `CREATE A NEW ENGINE` ，就可以创建一个搜索引擎。

![创建搜索引擎](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E5%88%9B%E5%BB%BA%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E.png)

### 输入网站地址

在下方输入框中输入要监听网站的地址。

![输入网站地址](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E8%BE%93%E5%85%A5%E7%BD%91%E7%AB%99%E7%9A%84%E5%9C%B0%E5%9D%80.png)

### 输入引擎名称

等待网址验证通过之后，按在下面的输入框中分别指定搜索引擎的名称和语言。

![输入引擎名称](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E8%BE%93%E5%85%A5%E5%BC%95%E6%93%8E%E5%90%8D%E7%A7%B0.png)

### 创建成功

下面就是创建成功的界面。

![创建成功](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E5%88%9B%E5%BB%BA%E6%88%90%E5%8A%9F.png)

## 安装搜索引擎

点击下方 `Install Search` 。

![安装搜索引擎](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E5%AE%89%E8%A3%85%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E.png)

### 自定义外观

搜索结果外观有下面三个样式，可以根据喜好选择一种外观。下面将一第一种外观为例。

![搜索结果](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E8%87%AA%E5%AE%9A%E4%B9%89%E5%A4%96%E8%A7%82.png)

然后可以设置结果中每个字段代表的内容。

![字段设置](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E5%AD%97%E6%AE%B5%E8%AE%BE%E7%BD%AE.png)

最后可以设置内容连接颜色以及高亮的颜色。

![颜色设置](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E9%A2%9C%E8%89%B2%E8%AE%BE%E7%BD%AE.png)

### 定义搜索框

swiftype 支持自定义搜索框，只要网页中添加一个 class 属性 `st-default-search-input` 便可实现。

![自定义搜索框](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E5%AE%9A%E4%B9%89%E6%90%9C%E7%B4%A2%E5%9F%9F.png)

当然如果嫌麻烦，也可以直接使用 swiftype 的默认搜索框，就像下面一样设置。

![默认搜索框](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E9%BB%98%E8%AE%A4%E6%90%9C%E7%B4%A2%E6%A1%86.png)

### 结果容器

结果容器也是支持自定义和默认两种方式，下面展示的是默认设置。

![结果容器](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E6%90%9C%E7%B4%A2%E5%AE%B9%E5%99%A8.png)

### 激活设置

最后，当所有都设置好了之后，我们就可以保存并激活我们的设置了。

![激活设置](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E6%BF%80%E6%B4%BB%E8%AE%BE%E7%BD%AE.png)

### 将代码加入到网页

当然，要想让搜索引擎在我们的网站中生效，还需要将下面代码加入到网页中，可以将代码放在 `</body>` 标签之前。

![将代加入网页](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E5%B0%86%E4%BB%A3%E7%A0%81%E5%86%99%E5%85%A5%E5%88%B0%E7%BD%91%E9%A1%B5%E4%B8%AD.png)

## 效果展示

下面就是默认搜索按钮位置和默认搜索结果的样式效果展示。

### 搜索按钮

![搜索按钮](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E6%90%9C%E7%B4%A2%E6%8C%89%E9%92%AE.png)

### 搜索结果

![搜索结果](http://p9myzkds7.bkt.clouddn.com/swiftype-search/%E6%90%9C%E7%B4%A2%E7%BB%93%E6%9E%9C.png)