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

<!-- more -->

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

### 传统开发模式

![传统开发模式](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BC%A0%E7%BB%9F%E5%BC%80%E5%8F%91%E6%A8%A1%E5%BC%8F.png)

### 云开发模式

![云开发模式](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E5%BC%80%E5%8F%91%E6%A8%A1%E5%BC%8F.png)

### Serverless分类

![Serverless分类](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/Serverless%E5%88%86%E7%B1%BB.png)

### Serverless 的优点

- 运营成本更低

- 降低开发成本

- 拓展能力强

- 管理简单

### Serverless 的优势

- 快速接入

- 快速实现

- 无感运维

## 数据库 API 的解读

### 云开发数据库概念

![云开发数据库概念](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E5%BC%80%E5%8F%91%E6%95%B0%E6%8D%AE%E5%BA%93%E6%A6%82%E5%BF%B5.png)


### 数据库 API

![云数据库](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E6%95%B0%E6%8D%AE%E5%BA%93.png)

#### database

> 获取数据库的引用。

以下调用获取默认环境的数据库的引用：

```js
const db = wx.cloud.database()
```
假设有一个环境名为 test，用做测试环境，那么可以如下获取测试环境数据库：

```js
const testDB = wx.cloud.database({
  env: 'test'
})
```
#### serverDate

> 构造一个服务端时间的引用。可用于查询条件、更新字段值或新增记录时的字段值。

新增记录时设置字段为服务端时间：

```js
const db = wx.cloud.database()
db.collection('todos').add({
  description: 'eat an apple',
  createTime: db.serverDate()
})
```
更新字段为服务端时间往后一小时：

```js
const db = wx.cloud.database()
db.collection('todos').doc('my-todo-id').update({
  due: db.serverDate({
    offset: 60 * 60 * 1000
  })
})
```
#### Geo

> 构造一个基于地理位置的引用。

```js
const db = wx.cloud.database()
db.collection('todos').add({
  description: 'eat an apple',
  location: db.Geo.Point(113, 23)
})
```
### 数据库命令 API

> 获取数据库查询及更新指令。

```js
const _ = db.command
```

![Command查询指令](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/Command%E6%9F%A5%E8%AF%A2%E6%8C%87%E4%BB%A4.png)

![Command更新指令](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/Command%E6%9B%B4%E6%96%B0%E6%8C%87%E4%BB%A4.png)

### 数据库集合 API

> 获取集合的引用。

```js
onst db = wx.cloud.database()
const todosCollection = db.collection('todos')
```
![Collection API](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/Collection%20API.png)

### 数据库文档 API 

> 获取对一个记录的引用。

![Document API](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/Document%20API.png)

## 文件存储 API 的解读

![文件存储](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8.png)

### 小程序端上传文件

wx.cloud.uploadFile

> 将本地资源上传至云存储空间，如果上传至同一路径则是覆盖。

Promise 风格

```js
const cloud = require('wx-server-sdk')
const fs = require('fs')
const path = require('path')

exports.main = async (event, context) => {
  const fileStream = fs.createReadStream(path.join(__dirname, 'demo.jpg'))
  return await cloud.uploadFile({
    cloudPath: 'demo.jpg',
    fileContent: fileStream,
  })
}
```
### 小程序端下载文件

wx.cloud.downloadFile

> 从云存储空间下载文件

Promise 风格

```js
const cloud = require('wx-server-sdk')

exports.main = async (event, context) => {
  const fileID = 'xxxx'
  const res = await cloud.downloadFile({
    fileID,
  })
  const buffer = res.fileContent
  return buffer.toString('utf8')
}
```
### 获取临时文件链接

wx.cloud.getTempFileURL

> 用云文件 ID 换取真实链接，可自定义有效期，默认一天且最大不超过一天。

Promise 风格

```js
const cloud = require('wx-server-sdk')

exports.main = async (event, context) => {
  const fileList = ['cloud://xxx', 'cloud://yyy']
  const result = await cloud.getTempFileURL({
    fileList,
  })
  return result.fileList
}
```
### 小程序端删除文件

wx.cloud.deleteFile

> 从云存储空间删除文件。

```js
const cloud = require('wx-server-sdk')

exports.main = async (event, context) => {
  const fileIDs = ['xxx', 'xxx']
  const result = await cloud.deleteFile({
    fileList: fileIDs,
  })
  return result.fileList
}
```
### 小程序组件对云文件 ID 的支持

![小程序组件对云文件 ID 的支持](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%BB%84%E4%BB%B6%E5%AF%B9%E4%BA%91%E6%96%87%E4%BB%B6%20ID%20%E7%9A%84%E6%94%AF%E6%8C%81.png)

## 云函数 API 的解读

![云函数](https://graphbed.qiniu.songxingguo.com/WeChatMinApp/%E4%BA%91%E5%87%BD%E6%95%B0.png)

### 调用云函数

wx.cloud.callFunction

```js
const cloud = require('wx-server-sdk')
exports.main = async (event, context) => {
  const res = await cloud.callFunction({
    // 要调用的云函数名称
    name: 'add',
    // 传递给云函数的参数
    data: {
      x: 1,
      y: 2,
    }
  })
  return res.result
}
```
 
## 服务端 API 的解读

### 服务端 API 和小程序 API 的异同

- 服务端 API 仅支持 Promise 风格

- 服务端 API 可以进行批量 update 和  remove

### 服务端 API 使用注意事项

- 服务端 API 使用前需先调用 wx.cloud.init 方法

- 通过 cloud.init 传入的 env 可以设置不同的环境

### 云函数获取用户信息

> 云开发的云函数的独特优势在于与微信登录鉴权的无缝整合。当小程序端调用云函数时，云函数的传入参数中会被注入小程序端用户的 openid，开发者无需校验 openid 的正确性，因为微信已经完成了这部分鉴权，开发者可以直接使用该 openid。与 openid 一起同时注入云函数的还有小程序的 appid。

```js
exports.main = (event, context) => {
  return {
    openid: event.userInfo,
  }
}
```
### 云函数返回结果

在云函数中处理一些异步操作时，需要在异步操作完成后，再返回结果给到调用方。此时我们可以通过在云函数中返回一个 Promise 的方法来完成。

```js
exports.main = async (event, context) => new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(event.a + event.b)
  }, 3000)
})
``