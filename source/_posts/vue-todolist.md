title: Vue-TodoList项目实战
author: songxingguo
tags:
  - Vue
categories: []
date: 2018-12-06 16:02:00
---
## 写在前面

[课程地址](https://www.imooc.com/video/16402)

## 前端开发

## 网络优化

- 减少http请求

- 压缩静态文件

- 使用浏览器长缓存

- 应用浏览编写

- 加快访问速度

<!-- more -->

## 工程化

- 搭建前端工程

- 网络优化

- API 定制

- Node.js

## 项目配置

### 项目初始化

1. cnpm init 

2. cnpm i vue vue-loader webpack css-loader vue-template-compiler style-loader file-loader url-loader less-loader less  --save

3. cnpm i webpack-dev-server cross-env -D

4. cnpm i html-webpack-plugin -D

### 项目结构

```
vue-todolist 
  -- src 
   -- app.vue
   -- index.js
  -- package.json
  -- webpack.config.js
```
#### app.vue

```vue
<template>
 <div id="text">{{ text }}</div>
</template>

<script>
 export  default {
     data() {
         return {
             text: '你好啊，Vue!'
         }
     }
 }
</script>

<style>
  #text {
      color: red;
  }
</style>
```

### Vue 项目开发

```
cnpm i postcss-loader postcss-load-config autoprefixer babel-loader babel-core --save
```

```
cnpm i babel-preset-env babel-plugin-transform-vue-jsx --save
```

### 问题

1. vue-template-compiler must be installed as a peer dependency, or a compatible compiler implementation must be passed via options.

  [参考地址](https://segmentfault.com/q/1010000015986575)

  解决方法： npm install vue-template-compiler -D

2. Module parse failed: Unexpected character '#' (16:0) You may need an appropriate loader to handle this file type.

  [参考地址](https://segmentfault.com/q/1010000014770071?sort=created)

  解决方法： npm install style-leader --save
  
    ```
      {
          test: /\.css$/,
              use: ['style-loader','css-loader']
      }
    ```