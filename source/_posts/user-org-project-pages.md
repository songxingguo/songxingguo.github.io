title: 用户、组织以及项目页面
author: songxingguo
tags:
  - github
categories:
  - 博客搭建
date: 2018-08-12 08:44:00
---
## 写在前面

该文章用于介绍 GitHub Pages 的相关内容，帮助了解 GitHub Pages ，也可以查看 GitHub [帮助文档](https://help.github.com/articles/user-organization-and-project-pages/) 。

<!-- more -->

## 用户，组织和项目页面

GitHub Pages 有两种基本类型：**项目页面** ，**用户和组织页面** 。它们几乎相同，但有一些重要的区别。

项目页面关联到特定的项目，页面文件位于项目仓库中的分支上。用户和组织页面不依赖于特定项目，页面文件位于专门用于存储 GitHub 页面文件的 **特殊仓库** 中。

> **提示：** GitHub Pages 网站在发布时始终可公开访问，即使其存储库是私有的，也不应该用于发送密码或信用卡号等敏感事务。更多有关信息，请参阅 “ [什么是GitHub页面？](https://help.github.com/articles/what-is-github-pages/) ”。


> **警告：** 如果您的 GitHub 页面站点的 URL 包含以短划线开头或结尾的用户名或组织名称，或包含连续破折号，那么使用 Linux 浏览的用户在访问该站点时将收到服务器错误。要解决此问题，请更改您的 GitHub 用户名以删除非字母数字字符。有关如何执行此操作的说明，请参阅 “ [更改GitHub用户名](https://help.github.com/articles/changing-your-github-username/) ”。

### 项目页面网站

Project Pages站点的源文件与其项目位于同一个存储库中，并从以下位置之一发布：

- master分支。

- gh-pages分支。

- 位于master分支上的名为“docs”的文件夹。

有关更多信息，请参阅“ [为GitHub页面配置发布源](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/) ”。

#### 构建项目页面站点

个人和组织帐户都可以创建项目页面站点，并且创建项目页面站点的步骤对于两者都是相同的。

项目页面网站在构建后可在以下位置使用：

- 个人帐户的项目页面站点位于 `http(s)://<username>.github.io/<projectname>` 。
  
- 组织帐户的项目页面站点位于 `http(s)://<orgname>.github.io/<projectname>` 。
  
- 如果您使用的是自定义域，请参阅“ [GitHub页面站点的自定义域重定向](https://help.github.com/articles/custom-domain-redirects-for-github-pages-sites/) ”。
  
#### 在Project Pages站点中使用自定义域

如果您使用具有用户或组织页面网站的自定义域，则自定义域将替换username.github.io该帐户下托管的所有项目页面网站的URL。使用自定义域的项目页面站点也可username.github.io/projectname用于个人帐户和orgname.github.io/projectname组织。有关详细信息，请参阅“ [将自定义域与GitHub页面一起使用](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) ”。

自定义404错误页面仅在使用自定义域时才有效。否则，使用默认用户页面404。有关详细信息，请参阅“ [为GitHub页面站点创建自定义404](https://help.github.com/articles/creating-a-custom-404-page-for-your-github-pages-site/) ”。
  
 
### 用户和组织页面站点

用户和组织页面站点的源文件master位于以GitHub帐户名称命名的专用存储库中的分支上：

- 要创建用户页面站点，请使用命名方案命名存储库 `<username>.github.io` 。
  
- 要创建Organization Pages站点，请使用命名方案命名存储库 `<orgname>.github.io` 。
  
#### 构建用户和组织页面站点

用户和组织页面网站在构建后可在以下位置使用：

- 用户页面站点位于 `http(s)://<username>.github.io` 。

- 组织页面站点位于 `http(s)://<orgname>.github.io` 。

- 如果您使用的是自定义域，请参阅“ [GitHub页面站点的自定义域重定向](https://help.github.com/articles/custom-domain-redirects-for-github-pages-sites/) ”。

用户页面网站可以由具有经过验证的电子邮件地址的任何用户帐户构建。他们还可以使用部署密钥自动执行该过程。有关更多信息，请参阅GitHub Developer文档中的“ [管理部署密钥](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys) ”。

组织页面站点可以由具有对存储库的推送访问权限和经过验证的电子邮件地址的任何成员构建。要自动化构建，您可以将计算机用户设置为组织的成员。有关更多信息，请参阅“  [管理部署密钥](https://developer.github.com/v3/guides/managing-deploy-keys/#machine-users) ”。组织页面站点不支持部署密钥。
