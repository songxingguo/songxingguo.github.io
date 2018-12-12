title: 微信小程序云开发
author: songxingguo
tags:
  - WeChat
categories: []
date: 2018-11-25 10:33:00
---
## 写在前面

[视频地址](https://cloud.tencent.com/developer/edu/learn-100005-1244/3154)

[官方文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html)


## 云开发简介

- 云函数：在云端运行的代码，微信私有协议天然鉴权，开发者只需编写自身业务逻辑代码

- 数据库：一个既可在小程序前端操作，也能在云函数中读写的 JSON 数据库

- 存储：在小程序前端直接上传/下载云端文件，在云开发控制台可视化管理

![云开发](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E5%BC%80%E5%8F%91.png)

### 云开发数据库

云开发提供了一个 JSON 数据库，顾名思义，数据库中的每条记录都是一个 JSON 格式的对象。一个数据库可以有多个集合（相当于关系型数据中的表），集合可看做一个 JSON 数组，数组中的每个对象就是一条记录，记录的格式是 JSON 对象。

关系型数据库和 JSON 数据库的概念对应关系如下表：

![关系型数据库和 JSON 数据库的概念对应关系](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E5%85%B3%E7%B3%BB%E5%9E%8B%E6%95%B0%E6%8D%AE%E5%BA%93%E5%92%8C%20JSON%20%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E6%A6%82%E5%BF%B5%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB.png)

以下是一个示例的集合数据，假设我们有一个 books 集合存放了图书记录，其中有两本书：

```json
[
  {
    _id: 'Wzh76lk5_O_dt0vO',
    title: 'The Catcher in the Rye',
    author: 'J. D. Salinger',
    characters: [
      'Holden Caulfield',
      'Stradlater',
      'Mr. Antolini'
    ],
    publishInfo: {
      year: 1951,
      country: 'United States'
    }
  },
  {
    _id: 'Wzia0lk5_O_dt0vR',
    _openid: 'ohl4L0Rnhq7vmmbT_DaNQa4ePaz0',
    title: 'The Lady of the Camellias',
    author: 'Alexandre Dumas fils',
    characters: [
      'Marguerite Gautier',
      'Armand Duval',
      'Prudence',
      'Count de Varville'
    ],
    publishInfo: {
      year: 1848,
      country: 'France'
    }
  }
]
```

### 云函数

- 云端运行一段代码

- 可获取天然可信任的用户登录态

- 支持 Node,js 环境

## 开发环境配置

- 使用云开发模板

- 手动添加支持

  - project.config.json 中增加了字段 cloudfunctionRoot 用于指定存放云函数的目录

  - 在 app.json / game.json 中增加字段 "cloud": true

## 微信登录实现

### 传统微信登录流程

![传统微信登录流程](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BC%A0%E7%BB%9F%E5%BE%AE%E4%BF%A1%E7%99%BB%E5%BD%95%E6%B5%81%E7%A8%8B.jpg)

![传统微信登录流程](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BC%A0%E7%BB%9F%E5%BE%AE%E4%BF%A1%E7%99%BB%E5%BD%95%E6%B5%81%E7%A8%8B.png)

#### 传统微信登录流程

![小程序云开发.微信登录](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E4%BA%91%E5%BC%80%E5%8F%91.%E5%BE%AE%E4%BF%A1%E7%99%BB%E5%BD%95.png)


在用户管理中会显示使用云能力的小程序的访问用户列表，默认以访问时间倒叙排列，访问时间的触发点是在小程序端调用 wx.cloud.init 方法，且其中的 traceUser 参数传值为 true。例：

```js
wx.cloud.init({
  traceUser: true
})
```
![options 提供的可选配置](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/options%20%E6%8F%90%E4%BE%9B%E7%9A%84%E5%8F%AF%E9%80%89%E9%85%8D%E7%BD%AE.png)

![微信登录](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E5%BE%AE%E4%BF%A1%E7%99%BB%E5%BD%95.png)

### 云函数端

```js
exports.main = (event, context) => {
  console.log(event)
  console.log(context)

  // 可执行其他自定义逻辑
  // console.log 的内容可以在云开发云函数调用日志查看

  return {
    openid: event.userInfo.openId,
  }
}
```

## 云开发与 Serverless

![无服务器](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/Serverless.png)

## 数据库 API

![云数据库](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E6%95%B0%E6%8D%AE%E5%BA%93.png)

## 文件存储 API

![文件存储](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8.png)

## 云函数 API

![云函数](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E5%87%BD%E6%95%B0.png)

## 服务端 API
