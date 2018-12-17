title: '配置多个 SSH Key '
author: songxingguo
tags:
  - git
categories: []
date: 2018-12-17 11:40:00
---
## 写在前面

通过配置一个config文件来解决个人的 github 和 公司内部的 gitlab 冲突的问题。

<!-- more -->

## git 常用配置

### 配置全局的用户名和邮箱

```
git config --global user.name "xxx"
git config --global user.email "xxx.mail@xxx.com"
```
使用如下命令可以取消全局设置：

```
git config --global --unset user.name
git config --global --unset user.email
```
## 管理生成的多个SSH Key

### 查看本地公钥

进入任意目录的 `git bash` 执行下面代码：

```
cat ~/.ssh/id_rsa.pub
```
`id_rsa.pub` 可以是任意文件名。

### 生成 SSH 密钥

```
ssh-keygen -t rsa -C “xxx.mail@xxx.com”
```
windows 下文件存放的位置在： `/c/Users/admin/.ssh/id_rsa.pub`，其中 `admin` 为自定义的用户名。

执行命令后，可以使用默认值（适用于只有一个 SSH Key 的时候），也可以自定义配置（适用于有多个 SSH Key的时候）。

#### 使用默认值

这个指令会要求你提供一个位置和文件名去存放键值对和密码，你可以点击Enter键去使用默认值。

![使用默认配置](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E4%BD%BF%E7%94%A8%E9%BB%98%E8%AE%A4%E5%80%BC.webp)

![默认生成文件](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E9%BB%98%E8%AE%A4%E6%96%87%E4%BB%B6.webp)

#### 自定义配置

1. 首先可以根据不同的代码托管平台命名不同的文件，如 github 为 id_rsa_github, gitlab 为 id_rsa_gitlab。

2. 然后根据个人需要确定是否给密钥设置密码。

![自定义配置](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%96%87%E4%BB%B6%E5%90%8D.webp)

![自定义文件](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E8%87%AA%E5%AE%9A%E4%B9%89%E6%96%87%E4%BB%B6.webp)
	
    
### ssh-agent 管理多个SSH Key

ssh-agent就是一个密钥管理器，运行ssh-agent以后，使用ssh-add将私钥交给ssh-agent保管，其他程序需要身份验证的时候可以将验证申请交给ssh-agent来完成整个认证过程。

```
ssh-add ~/.ssh/id_rsa_github
ssh-add ~/.ssh/id_rsa_gitlab
```

#### 将私钥交给 ssh-agent 管理

进入用户名目录下的.ssh目录，打开git bash，执行如下命令

```
exec ssh-agent bash
eval ssh-agent -s
ssh-add ./id_rsa_github
ssh-add ./id_rsa_gitlab
```
![添加github](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B7%BB%E5%8A%A0github.webp)

![添加gitlab](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B7%BB%E5%8A%A0gitlab.webp)

#### 添加私钥命令错误

在执行上面的添加私钥命令时，如果出现如下错误：

![添加私钥错误](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B7%BB%E5%8A%A0%E7%A7%81%E9%92%A5%E5%91%BD%E4%BB%A4%E9%94%99%E8%AF%AFwebp.webp)

解决方法如下：

1. 输入如下命令查看已开启的ssh-agent线程

  ```
  ps aux | grep ssh
  ```
  ![查看已开启的ssh-agent线程](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%9F%A5%E7%9C%8B%E5%B7%B2%E5%BC%80%E5%90%AF%E7%9A%84ssh-agent%E7%BA%BF%E7%A8%8B.webp)

2. 执行如下命令杀死线程：

  ```
  kill -9 线程号`
  ```
  ![杀死线程](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%9D%80%E6%AD%BB%E7%BA%BF%E7%A8%8B.webp)

#### 添加配置文件 

1. 创建config文件，将文件创建在【.ssh】目录下

  - 在windows下新建一个txt文本，然后将名字改成config（包括.txt后缀）。
  - 在git bash下,直接touch config即可创建一个config文件。

2. 编辑config文件,修改如下内容：

  ```
  # gitlab
  Host gitool.glanway.com
  HostName gitool.glanway.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_gitlab
  User xxx

  # github
  Host github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_github
  User xxx
  ```
  配置文件说明：

  ```
  HostName                      #这里填你们公司的git网址即可
  IdentityFile                  #这里是id_rsa的地址
  PreferredAuthentications      #配置登录时用什么权限认证--可设置publickey,password publickey,keyboard-interactive等
  User                          #配置使用用户名
  ```
## 将公钥添加到 github 和 gitlab 上

### 添加到 github

![添加到 github](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B7%BB%E5%8A%A0%E5%88%B0github.png)

![添加到 github（续）](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B7%BB%E5%8A%A0%E5%88%B0%20github%EF%BC%88%E7%BB%AD%EF%BC%89.png)

### 添加到gitlab

打开gitlab,找到Profile Settings-->SSH Keys--->Add SSH Key,并把上一步中复制的内容粘贴到Key所对应的文本框，在Title对应的文本框中给这个sshkey设置一个名字，点击Add key按钮。

![添加到 gitlab 上](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B7%BB%E5%8A%A0%E5%88%B0gitlab%E4%B8%8A.png)

## 测试

```
ssh -T git@github.com          #测试github
ssh -T git@gitool.glanway.com   #测试gitlab
```

看到如下输出表示配置成功

![测试](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%B5%8B%E8%AF%95.webp)

## sourceTree 添加 ssh key

### 配置SourceTree 的 SSH 客户的为：OpenSSH 

1. 工具->选项 

  ![工具-选项](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E5%B7%A5%E5%85%B7-%E9%80%89%E9%A1%B9.png)

2. 设置 OpenSSH，并选择 .ssh 目录下的 id_rsa 私钥。

  ![选择私钥](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E9%80%89%E6%8B%A9%E7%A7%81%E9%92%A5.png)
  
### SourceTree 来检出 git 项目 

![检出 git 项目](https://graphbed.qiniu.songxingguo.com/SSH-Key/%E6%A3%80%E5%87%BAgit%E9%A1%B9%E7%9B%AE.png)

## 写在后面

参考地址：

[sourceTree 添加 ssh key 方法](https://blog.csdn.net/tengdazhang770960436/article/details/54171911)

[win10生成SSH keys](https://www.cnblogs.com/xiaoCong2016/p/6623243.html)

[Git安装及SSH Key管理之Windows篇](https://www.jianshu.com/p/a3b4f61d4747)

[GitLab配置ssh key](http://www.cnblogs.com/hafiz/p/8146324.html)