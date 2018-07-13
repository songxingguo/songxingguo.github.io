title: Windows 10  常见问题处理
author: songxingguo
tags:
  - Windows
categories:
  - 操作系统
date: 2018-07-13 18:23:00
---
### 解决文件正在使用（进程占用）问题

该问题解决分为两步，第一步，找到进程号；第二步，杀死它。

- 复制被占用的文件路径
  
 复制路径：
 
 ![复制文件路径](http://p9myzkds7.bkt.clouddn.com/windows/%E5%A4%8D%E5%88%B6%E8%A2%AB%E5%8D%A0%E7%94%A8%E7%9A%84%E8%B7%AF%E5%BE%84.png)
 
<!-- more -->

- 查找进程号

  1、打开 `任务管理器`, 点击 `性能`，再点击 `打开资源监视器`。
  
  ![打开资源监视器](http://p9myzkds7.bkt.clouddn.com/windows/%E6%89%93%E5%BC%80%E8%B5%84%E6%BA%90%E7%9B%91%E8%A7%86%E5%99%A8.png)
 
  2、点击 `关联句柄`，在搜索处 `粘贴文件路径`，`回车搜索`，出现被占用的进程号。
  
  ![查找进程号](http://p9myzkds7.bkt.clouddn.com/windows/%E6%9F%A5%E6%89%BE%E8%A2%AB%E5%8D%A0%E7%94%A8%E7%9A%84%E8%BF%9B%E7%A8%8B%E5%8F%B7.png)

- 杀死进程号

  打开刚才的 `任务管理器`， 点击 `服务`，在服务中找到上面 `搜索的进程号`，`选中该进程`，鼠标 `右键`，然后点击 `停止` 杀死该进程。
  
  ![停止进程](http://p9myzkds7.bkt.clouddn.com/windows/%E5%81%9C%E6%AD%A2%E8%BF%9B%E7%A8%8B.png)
  
  