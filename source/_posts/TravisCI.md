title: 使用 TravisCI 将代码部署到 Github 和 Coding
author: songxingguo
tags:
  - TravisCI
categories:
  - 博客搭建
date: 2018-05-19 16:09:00
---
## 写在前面

TravisCI 是在线部署工具，可以减少发布的重复操作，可以将代码同时发布到 Github 和 Coding 上。

## 配置文件

```
{
  "os": "linux",
  "dist": "trusty",
  "group": "stable",
  "script": [
    "hexo clean",
    "hexo g"
  ],
  "install": [
    "npm install"
  ],
  "node_js": "stable",
  "language": "node_js",
  "global_env": "GH_REF=github.com/songxingguo/songxingguo.github.io.git CO_REF=git.coding.net/songxingguo/songxingguo.coding.me.git",
  "after_script": [
    "cd ./public",
    "git init",
    "git config user.name \"songxingguo\"",
    "git config user.email \"1328989942@qq.com\"",
    "git add .",
    "git commit -m \"Update Blog By TravisCI With Build $TRAVIS_BUILD_NUMBER\"",
    "git push --force --quiet \"https://${GH_TOKEN}@${GH_REF}\" master:master",
    "git push --force --quiet \"https://songxingguo:${CO_TOKEN}@${CO_REF}\" master:master",
    "git tag v0.0.$TRAVIS_BUILD_NUMBER -a -m \"Auto Taged By TravisCI With Build $TRAVIS_BUILD_NUMBER\"",
    "git push --quiet \"https://${GH_TOKEN}@${GH_REF}\" master:master --tags",
    "git push --quiet \"https://songxingguo:${CO_TOKEN}@${CO_REF}\" master:master --tags"
  ]
}
```

<!-- more -->

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

## 参考文章

[手把手教你使用Travis CI自动部署你的Hexo博客到Github上](https://blog.csdn.net/woblog/article/details/51319364)
[基于Travis CI实现 Hexo 在 Github 和 Coding 的同步部署](https://blog.csdn.net/qinyuanpei/article/details/79388983)
[使用 Travis CI 自动更新 GitHub Pages](https://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)