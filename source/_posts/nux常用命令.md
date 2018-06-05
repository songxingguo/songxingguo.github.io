title: Linux常用命令
author: songxingguo
tags:
  - 'Linux '
categories:
  - 常用命令
date: 2018-06-04 15:21:00
---
编辑文件
```bash
# vim example.js
```

输入 i，进入编辑模式，将以下项目文件内容粘贴到文件中。使用Esc按钮，退出编辑模式，输入:wq，回车，保存文件内容并退出。

创建文件

```bash
# touch example.js
```
<!-- more-->
删除文件

```bash
# rm example.js
```
新建文件夹
```bash
mkdir test
```
拷贝文件夹
如将/test1目录下的file1复制到/test3目录，并将文件名改为file2,可输入以下命令：
```bash
cp /test1/file1 /test3/file2
```
移动文件夹
如将/test1目录下的file1复制到/test3 目录，并将文件名改为file2,可输入以下命令：
```bash
mv /test1/file1 /test3/file2
```
如果是移动文件夹下的所有文件的话就可以文件夹后面跟上 /* 
```bash
mv /data/new/* /data/old/
```


~代表你的/home/用户明目录
假设你的用户名是x，那么~/就是/home/x/
.是代表此目录本身，但是一般可以不写
所以cd ~/. 和cd ~ 和cd ~/效果是一样的

配置node环境

1.wget命令下载Node.js安装包。

```bash
 wget https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz
```
2.解压文件。
```bash
tar xvf node-v6.9.5-linux-x64.tar.xz
```
3.创建软链接，使node和npm命令全局有效。
```bash
ln -s /root/node-v6.9.5-linux-x64/bin/node /usr/local/bin/node
ln -s /root/node-v6.9.5-linux-x64/bin/npm /usr/local/bin/npm
```
4.查看node、npm版本。
```bash
```

查看文件夹中的文件
```bash
ls -l
```
回到根目录
```bash
../..
```