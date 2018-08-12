title: 为 GitHub Pages  配置发布源
author: songxingguo
tags:
  - github
  - ''
categories:
  - 博客搭建
date: 2018-08-12 05:59:00
---
## 写在前面

 在 GitHub Pages 的 [帮助文档](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/) 中已经给我们详细说明了，GitHub Pages 的发布资源的 3 种方式的不同，以及发布的方法。而作为英语文盲的我，只有通过百度翻译才能勉强看懂，而下面我要做的就是将 GitHub Pages 的 3 中方式给出一个中文的说明，供以后需要的时候就不用再翻译了。
 
<!-- more -->
 
## 发布站点

 GitHub Pages 允许我们从 master 分支、gh-pages 分支 或 master分支下的 /docs 目录来发布我们的资源。
 
 而如果仓库名称为 `<username>.github.io` 或 `<orgname>.github.io` 就只能从 master 分支发布。
 

### 没有用户名命名仓库的默认源设置

发布源文件的默认设置取决于仓库的类型以及仓库中的分支。

如果仓库中没有 master 或没有 gh-pages 分支，则您的 GitHub Pages 发布源将设置为“ 无”，并且不会发布。

![不会发布](http://p9myzkds7.bkt.clouddn.com/github-pages/none-source-setting.png)

创建了 master 分支 或 gh-pages 分支，可以将其中一个设置为发布源，以便发布站点。

如果只有 master 或 gh-pages 分支，则将该分支设置为默认发布源。

### 从 master 或 gh-pages 发布资源

1. 在GitHub上，进入到代码仓库。

2. 点击仓库设置按钮。

 ![点击设置按钮](http://p9myzkds7.bkt.clouddn.com/github-pages/repo-actions-settings.png)

3. 选择 master 或 gh-pages 作为 GitHub Pages 发布源。
 
  ![选择发布源](http://p9myzkds7.bkt.clouddn.com/github-pages/select-gh-pages-or-master-as-source.png)

4. 点击保存按钮，保存发布源。
  
  ![保存发布源](http://p9myzkds7.bkt.clouddn.com/github-pages/click-save-next-to-source-selection.png)

### 从 master 分支 /docs 目录中发布资源

要从 master 分支上的 /docs 目录发布站点的源文件，您必须有 master 分支，并且您的仓库必须：

- 在根目录中有一个 /docs 文件夹。

- 不遵循存储库命名方案 `<username>.github.io` 或 `<orgname>.github.io` 。
   
GitHub Pages 将从该 /docs 目录中读取发布您网站的所有内容，包括 CNAME 文件。例如，当您通过 GitHub 页面设置编辑自定义域时，自定义域将写入 /docs/CNAME 。

>**提示：** 如果在启用后 /docs 从 master 分支中删除该文件夹，则您的站点将无法构建，并且您将收到丢失 /docs 文件夹的页面构建错误消息。

1. 在GitHub上，进入到代码仓库。

2. 在仓库 master 分支的根目录中创建一个名为 /docs 文件夹。

3. 点击仓库设置按钮。

  ![点击设置按钮](http://p9myzkds7.bkt.clouddn.com/github-pages/repo-actions-settings.png)

4. 选择 master 分支的 / docs 目录 作为 GitHub Pages 发布源。

 ![选择发布源](http://p9myzkds7.bkt.clouddn.com/github-pages/select-master-branch-docs-folder-as-source.png)

   >**提示：** 如果分支 /docs 上不存在该文件夹，则 master 分支 / docs 文件夹源设置将不会显示为选项 master。
   
5. 点击保存，选择单击保存主分支的文档文件夹。

   ![保存发布源](http://p9myzkds7.bkt.clouddn.com/github-pages/click-save-next-to-master-branch-docs-folder-source-selection.png)
   
   
## 问题及解决