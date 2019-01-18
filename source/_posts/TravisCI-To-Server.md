title: 使用 TravisCI 将代码部署到服务器
author: songxingguo
tags:
  - TravisCI
categories:
  - 前端技术
date: 2019-01-18 16:27:00
---
## 写在前面

实现该功能的初衷是为了实现 push 代码的时候就能够自动推送到远端的服务器，持续部署 Node 应用。

系统环境：ubantu 。

> 该功能 **只能在 linux 环境** 上进行，否则后续加密的私钥在 TravisCI 服务器无法解密。

<!-- more -->

## 基本原理

首先在本地电脑生成 SSH 密钥，然后将公钥放到服务器，将 **私钥加密** 后放到 TravisCI 服务器上，TravisCI 服务器在发布代码的时候使用公钥登录。

> 所谓"公钥登录"，原理很简单，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码。

## 实现步骤

### 生成 SSH 密钥

打开命令行工具，在任意目录输入如下命令：

```
ssh-keygen -t rsa -C "1328989942@qq.com"
```

### 将公钥放置到服务器

拷贝公钥到指定服务器

```
ssh-copy-id root@47.100.232.2
```

### 将私钥加密后推送到代码仓库（TravisCI 服务器）

#### 安装并登陆 TravisCI

`travis login` 后需添加参数 `--org`。

> 只有通过 `travis-ci.org` 登录，后续加密过程中 TravisCI 网页上才会有生成的环境变量。

```
gem install travis
travis login --org 
```

[登录网页版](https://travis-ci.org/)

设置仓库

![设置仓库](https://graphbed.qiniu.songxingguo.com/TravisCI-To-Server/%E8%AE%BE%E7%BD%AE%E4%BB%93%E5%BA%93.png)

#### 加密私钥

首先新建 .travis.yml，然后进入 **项目目录** ， 接着执行 `travis encrypt-file` 命令进行加密。

```
travis encrypt-file ssh_key --add
```

该命令会在 TravisCI 网页中添加两个环境变量，并且生成一个加密后的文件。

![两个环境变量](https://graphbed.qiniu.songxingguo.com/TravisCI-To-Server/%E4%B8%A4%E4%B8%AA%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F.png)

> `ssh_key` 为私钥的位置，当前命令是把 **私钥放在项目根目录下** 的。

> 添加 --add 参数，该命令会在 .teavis.yml 中自动添加如下解密命令。
 ```
  - openssl aes-256-cbc -K $encrypted_4629cddb74de_key -iv $encrypted_4629cddb74de_iv
    -in id_rsa.enc -out ~/.ssh/id_rsa -d
```

#### 配置 .travis.yml并推送到代码仓库

```yml
language: node_js
node_js:
  - 8.12.0
before_install:
  - openssl aes-256-cbc -K $encrypted_4629cddb74de_key -iv $encrypted_4629cddb74de_iv
    -in id_rsa.enc -out ~/.ssh/id_rsa -d # 解密命令，添加 --add 参数时, 自动添加
  - chmod 600 ~/.ssh/id_rsa # 降低 id_rsa 文件的权限
  - echo -e "Host 47.100.232.2\n\tStrictHostKeyChecking no\n" >> ~/.ssh/config # 将生产服务器地址加入到测试机的信任列表中
install:
  - npm install
script:
  - npm run release
after_success:
  - npm prune --production # 删除devDependencies
  - tar -zcvf learning.tar.bz2 * # 打包并压缩代码
  - scp learning.tar.bz2 ${REMOTE_SERVER}:~/ # 复制到生产服务器上
  - ssh ${REMOTE_SERVER} 'mkdir -p learning && tar -zxvf learning.tar.bz2 -C
    learning' # 解压
  - ssh ${REMOTE_SERVER} 'cd learning && npm install && npm run prd' # 重启pm2
branches:
  only:
    - master
env:
  global:
    - GH_REF: github.com/songxingguo/learning.git
    - REMOTE_SERVER: root@47.100.232.2
```

> 注意： **不要** 把私钥推送到了代码仓库。

## 写在最后

[Travis CI用来持续集成你的项目](http://www.cnblogs.com/zqzjs/p/6119750.html)

[利用travis-ci持续部署nodejs应用](https://cnodejs.org/topic/5885f19c171f3bc843f6017e)

[SSH原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)

[Encrypting Files](https://docs.travis-ci.com/user/encrypting-files)