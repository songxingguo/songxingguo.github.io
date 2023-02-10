---
title: 使用Github Action部署静态网站
urlname: tfub27hk86lsdrpb
date: '2023-01-15 12:05:37 +0000'
tags: []
categories: []
---

## 生成 token

1. 进入 Github 生成[授权 token 的页面](https://github.com/settings/apps)，创建一个 token，修改名称并选择所有仓库。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FpT1FXfdY0l7BGM5m1bJf8CpX2vJ.png)

<!-- more -->

2. 将仓库权限设置中的 Content 项，设置为读和写。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fq0KQlr3rLfbLvpcG3eeBuR3obCM.png)

3. 最后点击确认生成 token，并**复制**，**注意 ⚠️ 刷新之后就没有办法复制了**。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fv9aHq1-NNLTVgSUqUFagqiuXnKq.png)

## 设置环境变量

1. 进入到**项目**设置页面，选择添加[Actions 的环境变量](https://github.com/songxingguo/resume/settings/secrets/actions)。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FrIY5Jtbgpf4f6dVbJw05tgPrnLU.png)

2. 输入变量名称`ACCESS_TOKEN`，密钥一栏粘贴刚才复制的 Token，点击添加就完成了。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fl6OpNuIfRyDofKm-aKoQSfW8LAr.png)

## 项目中添加配置文件

1. 在项目的更目录下创建`.github/workflows/static.yml`。

```javascript
name: GitHub Actions Build and Deploy
on:
  push:
    branches:
      - main #触发自动化部署的分支
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️ #从触发分支检出代码
        uses: actions/checkout@v3
        with:
          persist-credentials: false

      - name: Install and Build 🔧 #安装依赖并且打包静态文件
        run: |
          npm i
          npm run build
      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
          BRANCH: gh-pages #部署的分支
          FOLDER: dist #打包后静态文件所在的位置
          CLEAN: true
```

2. 然后提交代码就开始自动部署了。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FtBuIbPHuD4YxSJImJg7yFsektPM.png)

## 扩展阅读
