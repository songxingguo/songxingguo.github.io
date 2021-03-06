title: Hexo主题修改
author: songxingguo
tags:
  - Hexo
  - 博客搭建
categories:
  - 开发者手册
date: 2018-06-06 21:58:00
---
-  ### 前提

 &emsp;&emsp;已经使用Hexo搭建一个博客

-  ### 选取自己喜欢的主题
 &emsp;&emsp;在Hexo [主题列表](https://hexo.io/themes/) 中选取一个自己喜欢的主题，

 &emsp;&emsp;我这儿选取的是 hiker 主题

 <!-- more -->

- ### 将主题clone到themes目录下

 - 进入博客根目录，将代码clone到 **themes/hiker** 目录下
  ```
 git clone https://github.com/iTimeTraveler/hexo-theme-hiker.git themes/hiker
  ```
   效果：
 ![clone主题](https://graphbed.qiniu.songxingguo.com/clone-hiker.png)

 - 然后进入主题文件目录下，执行 **npm install** 安装主题依赖的资源
    ```
    cd themes/Hiker
    npm install
    ```
      效果：
 ![安装依赖](https://graphbed.qiniu.songxingguo.com/%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96.png)


-  ### 修改博客_config.yml文件

 - 打开博客的—config.yml文件，找到 **theme** 字段并修改为 **hiker**
 ![修改主题配置](https://graphbed.qiniu.songxingguo.com/%E4%B8%BB%E9%A2%98%E4%BF%AE%E6%94%B9%E4%B8%BAhiker.png)

- ### 重新启动服务

 - 进入博客根目录，重新启动服务
  ```
  cd ../../
  hexo clean 
  hexo g
  hexo s
  ```
   ![重启服务](https://graphbed.qiniu.songxingguo.com/%E9%87%8D%E5%90%AFHexo%E6%9C%8D%E5%8A%A1.png)

 - 点击 <http://localhost:4000> 查看
 ![hiker效果](https://graphbed.qiniu.songxingguo.com/hiker%E6%95%88%E6%9E%9C.png)
