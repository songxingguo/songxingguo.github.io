title: 使用 TravisCI 将代码部署到 Github 和 Coding
author: songxingguo
tags:
  - TravisCI
  - 博客搭建
categories:
  - 开发者手册
date: 2018-05-19 16:09:00
---
## 写在前面

TravisCI 是在线部署工具，可以减少发布的重复操作，可以将代码同时发布到 Github 和 Coding 上。

## 配置文件

```yml
language: node_js
node_js: 
  - 8.12.0

# S: Build Lifecycle
install:
  - npm install
  - npm install hexo-helper-live2d
  - npm install live2d-widget-model-z16

before_script:
 # - npm install -g gulp

script:
  - hexo clean
  - hexo g

after_script:
  - cd ./public
  - git init
  - git config user.name "songxingguo"
  - git config user.email "1328989942@qq.com"
  - git add .
  - git commit -m "Update Blog By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
  # Coding Pages
  - git push --force --quiet "https://songxingguo:${CO_TOKEN}@${CO_REF}" master:master
  # Add Tag
  - git tag v0.0.$TRAVIS_BUILD_NUMBER -a -m "Auto Taged By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --quiet "https://${GH_TOKEN}@${GH_REF}" master:master --tags
  # Coding Pages
  - git push --quiet "https://songxingguo:${CO_TOKEN}@${CO_REF}" master:master --tags
# E: Build LifeCycle

branches:
  only:
    - hexo
env:
 global:
  # Github Pages
  - GH_REF: github.com/songxingguo/songxingguo.github.io.git
  # Coding Pages
  - CO_REF: git.coding.net/songxingguo/songxingguo.coding.me.git
```

<!-- more -->

## 部署流程

### 添加仓库

[登录 travis-ci](https://travis-ci.com/account/repositories)

> 登录 travis-ci， 从 github 添加仓库到 travis-ci。

![添加仓库](https://graphbed.qiniu.songxingguo.com/TravisCI/%E6%B7%BB%E5%8A%A0%E4%BB%93%E5%BA%93.png)

![选择仓库](https://graphbed.qiniu.songxingguo.com/TravisCI/%E9%80%89%E6%8B%A9%E4%BB%93%E5%BA%93.png)

### 配置仓库

#### 生成 Access Token

[添加 Access Token](https://github.com/settings/tokens)

> 登录 github ，进入 Settings Developer settings 添加 Access Token。

![添加 Access Token](https://graphbed.qiniu.songxingguo.com/TravisCI/%E6%B7%BB%E5%8A%A0%20Access%20Token.png)

权限选择

![权限选择](https://graphbed.qiniu.songxingguo.com/TravisCI/%E6%9D%83%E9%99%90%E9%80%89%E6%8B%A9.png)

#### 添加 Acess Token

[添加 Acess Token](https://travis-ci.com/songxingguo)

![配置仓库](https://graphbed.qiniu.songxingguo.com/TravisCI/%E9%85%8D%E7%BD%AE%E4%BB%93%E5%BA%93.png)

添加 Acess Token 到 TrivisCI

![添加 Acess Token 到 TrivisCI](https://graphbed.qiniu.songxingguo.com/TravisCI/%E6%B7%BB%E5%8A%A0%20Acess%20Token%20%E5%88%B0%20TrivisCI.png)

### 添加配置文件

> 添加 `.travis.yml` 文件。

```yml
language: node_js
node_js:
  - 8.12.0

# S: Build Lifecycle
install:
  - npm install

before_script:
# - npm install -g gulp

script:
  - npm run build

after_script:
  - cd ./dist
  - git init
  - git config user.name "songxingguo"
  - git config user.email "1328989942@qq.com"
  - git add .
  - git commit -m "Update Blog By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:gh-pages
  # Add Tag
  - git tag v0.0.$TRAVIS_BUILD_NUMBER -a -m "Auto Taged By TravisCI With Build $TRAVIS_BUILD_NUMBER"
  # Github Pages
  - git push --quiet "https://${GH_TOKEN}@${GH_REF}" master:gh-pages --tags
# E: Build LifeCycle

branches:
  only:
    - master
env:
  global:
    # Github Pages
    - GH_REF: github.com/songxingguo/vue-todoList.git
```
配置说明：

![配置说明](https://graphbed.qiniu.songxingguo.com/TravisCI/%E9%85%8D%E7%BD%AE%E8%AF%B4%E6%98%8E%20.png)

## 存在的问题

### index 文件为空

- **原因：**

  index 文件为空,是因为主题代码没有提交到代码托管上。
  
- **解决方式：**

  将主题文件中的 `.git` 文件删除，并将主题文件考出项目文件夹后提交代码，然后再将主题代码拷贝回项目文件，然后再次提交 代码。

### 图片发布后无法显示

- **原因：**

  图片路径问题。

- **解决方式：**

  使用 [七牛云](https://www.qiniu.com/) 作为图床，提供图片链接。
  
### ORGANIZATIONS 仓库如何授权获取

![ORGANIZATIONS 仓库授权获取](https://graphbed.qiniu.songxingguo.com/TravisCI/ORGANIZATIONS%20%E6%8E%88%E6%9D%83.png)

### 免密使用 github API 的两种方式

#### deploy-keys 

> deploy-keys SSH key 的形式，将公钥放在 github 上，进行授权验证。

![配置 Deploy keys](https://graphbed.qiniu.songxingguo.com/TravisCI/%E9%85%8D%E7%BD%AE%20Deploy%20keys.png)

#### Personal access tokens

> 用于添加到请求路径中，进行免密请求，和具体的用户无关。

```git
git push --quiet "https://${GH_TOKEN}@${GH_REF}" master:gh-pages --tags
```

![配置 access tokens](https://graphbed.qiniu.songxingguo.com/TravisCI/%E6%B7%BB%E5%8A%A0%20Access%20Token.png)

### travis org 和 com 的不同

 Public repositories appear in https://travis-ci.org/profile and private ones in https://magnum.travis-ci.com/profile. 

## 参考文章

[手把手教你使用Travis CI自动部署你的Hexo博客到Github上](https://blog.csdn.net/woblog/article/details/51319364)
[基于Travis CI实现 Hexo 在 Github 和 Coding 的同步部署](https://blog.csdn.net/qinyuanpei/article/details/79388983)
[使用 Travis CI 自动更新 GitHub Pages](https://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)
