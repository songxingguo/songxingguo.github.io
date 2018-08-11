title: ztopic 主题添加导航 “作品”
author: songxingguo
tags:
  - Hexo
categories:
  - 博客搭建
date: 2018-08-11 09:46:00
---
## 写在前面

  ztopic 是 Hexo 的主题之一，该主题的 github 地址为：https://github.com/wa-ri/hexo-theme-ztopic 。
  
## 添加导航

  添加导航一共分为 4 个步骤，它们分别是：
  
  - 修改配置文件
   
  - 修改标题和图标
  
  - 修改布局文件
  
  - 新建 Markdown 文件
 
<!-- more -->
  
### 修改配置文件
 
   > Hexo 博客的配置文件分为 **博客配置** 和 **主题配置**，而添加导航 **两个配置文件都需要修改** 。
   
   - #### 修改博客配置 
   
     > 打开文件：/_config.yml。
   
     只需要在文件的 `Directory` 下添加 `works_dir:works` ，具体修改如下图所示：
   
     ![修改博客配置](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E5%8D%9A%E5%AE%A2%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E4%BF%AE%E6%94%B9.png)
   
   - #### 修改主题配置 
      
     > 打开文件： /themes/ztopic/_config.yml
   
     只需要在文件的 `menu` 下添加一个 `works: /works` ，具体修改如下图所示：
   
     ![修改主题配置](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E4%BF%AE%E6%94%B9.png)
     
### 修改标题和图标

  > 上面的修改之后导航中已经多出了一栏，但是这并不是我们想要的，因为它显示的是 `menu.works` ，而且 **没有图标** ，就像下面一样，所以我们需要 **修改标题和图标** 。
   
   ![没有标题和图标的导航](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E5%AF%BC%E8%88%AA%E6%A0%B7%E5%BC%8F.png)
   
   - #### 修改标题
   
     > 打开文件：/themes/ztopic/languages/zh-CN.yml
     
     需要在文件的 `menu` 下添加一个 `works:作品`，具体修改如下图所示：
     
     ![修改标题](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E4%BF%AE%E6%94%B9%E6%A0%87%E9%A2%98.png)
     
   - #### 修改图标
   
     > 打开文件：/themes/ztopic/source/css/partials/_icon.scss
     
     修改图标是相对来说比较复杂的，因为它需要借助 **阿里云图标库** 生成 **图标代码** ，然后 **将代码拷贝到文件中** 。使用阿里图标库生成图标代码可以查看 [使用帮助](http://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d8cf4382a&helptype=code)。具体修改如下图所示：   
     
     ![添加图标代码](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E5%9B%BE%E6%A0%87%E4%BB%A3%E7%A0%81.png)
     ![添加图标类](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E6%B7%BB%E5%8A%A0%E5%9B%BE%E6%A0%87%E7%B1%BB.png)
     
     添加的代码：
     
     ```css
      @font-face {
        font-family: 'iconfont';  /* project id 788043 */
        src: url('//at.alicdn.com/t/font_788043_i0wkv787ma.eot');
        src: url('//at.alicdn.com/t/font_788043_i0wkv787ma.eot?#iefix') format('embedded-opentype'),
        url('//at.alicdn.com/t/font_788043_i0wkv787ma.woff') format('woff'),
        url('//at.alicdn.com/t/font_788043_i0wkv787ma.ttf') format('truetype'),
        url('//at.alicdn.com/t/font_788043_i0wkv787ma.svg#iconfont') format('svg');
      }
     ```
     ```css
     .icon-works:before { content: "\e693"; }
     ```
     
     
下面就是导航修改后的效果：

![导航修改后的效果](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E5%AF%BC%E8%88%AA%E4%BF%AE%E6%94%B9%E5%90%8E%E7%9A%84%E6%95%88%E6%9E%9C.png)

### 修改布局文件

> 当以为一切都万事大吉的时候，点击了一下导航才发现，导航只是虚有其表，还没有实现导航与之对应的布局，所以下面的任务就是 **修改布局文件** 。

布局文件分为 2 个部分，分别是 **标题** 和 **内容** ，所以我们要做的就是找到这两个部分的文件并修改它们。

 - #### 修改标题布局文件

  > 打开文件：/themes/ztopic/layout/_partials/banner.ejs
  
  ![修改标题布局文件](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E4%BF%AE%E6%94%B9%E5%B8%83%E5%B1%80%E6%A0%87%E9%A2%98%E6%96%87%E4%BB%B6.png)
  
  添加的代码：
  
  ```ejs
  <%}else if(page.type && page.type === 'works'){%>
  <div class="about-title">
    <%= "作品展示" %>
  </div>
  ```

- #### 修改内容布局文件
 
  > 打开文件：/themes/ztopic/layout/page.ejs

  ![修改内容布局文件](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E4%BF%AE%E6%94%B9%E5%B8%83%E5%B1%80%E5%86%85%E5%AE%B9%E6%96%87%E4%BB%B6.png)
  
  添加的代码：
  
  ```ejs
  <%if(page.type && page.type==="works"){%>
      <div id="works">
        <article class="post">
          <header>
            <div class="post-title mobile-post-title">
              <h1><%= page.title %></h1>
            </div>
          </header>
          <div class="post-content">
            <%- page.content %>
          </div>
        </article>
      </div>
  <%}%>
  ```

### 新建 Markdown 文件

> 进入 `/source` 目录， 新建一个目录 `works` ,并在 `/source/works` 目录下， 新建 一个 `index.md` 文件。 

**注意：** 新建的 `index.md ` 文件的 `type` 必须是 `works` ，上面定义的布局文件才才能生效。

下面是 `/source/works/index.md` 文件的内容：

![index.md文件](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/indexMarkdown%E6%96%87%E4%BB%B6.png)

代码：

```md
title: 作品
type: works
<!-- comments: false -->
date: 2018-06-02 10:23:12
---
### 我的作品

- [微信小程序](https://works.songxingguo.com/)

- [静态网页](https://works.songxingguo.com/)
```

## 写在最后

最后导航的效果就是这样了：

![导航的效果](http://p9myzkds7.bkt.clouddn.com/ztopic-add-nav/%E6%B7%BB%E5%8A%A0%E5%AF%BC%E8%88%AA%E5%A4%A7%E5%8A%9F%E5%91%8A%E6%88%90.png)