title: ztopic 主题更改
author: songxingguo
tags:
  - Hexo
categories:
  - 博客搭建
date: 2018-08-11 12:42:00
---
## 添加文章总条数显示

 **效果展示**
 
 ![效果展示](http://p9myzkds7.bkt.clouddn.com/ztopic/%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E6%80%BB%E6%9D%A1%E6%95%B0.png)
 
 <!-- more-->
 **代码展示**
 
 ```ejs
 <div class="archives-container tags-container"> <!-- 我是新增的 -->
    
    <!-- 我是新增的 -->
    <div class="tags-title">
      <%- _p('counter.archive', site.posts.length) %>
    </div>
    <!-- 我是新增的 -->
    
    <div class="archive-body"> <!-- 我是新增的 -->
      <% var last; %>
      <% page.posts.each(function(post, i){ %>
        <% var year = post.date.year(); %>
        <% if (last != year){ %>
          <% if (last != null){ %>
          </div></section>
          <% } %>
          <% last = year; %>
          <section class="archive-wrap">
            <div class="archive-year">
              <!-- <a href="<%- url_for(config.archive_dir + '/' + year) %>" class="archive-year"><%= year %></a> -->
              <%= year %>
            </div>
            <div class="archives">
        <% } %>
        <div class="archive-content">
        <a class="archive-article" href="<%= url_for(post.path) %>">
          <span class="archive-meta"><%= date(post.date, "MM-DD") %></span>
          <span class="archive-title"><%= post.title %></span>
        </a>
      </article>
      <% }) %>
      <% if (page.posts.length){ %>
        </div></section>
      <% } %>
    </div>
  </div>
 ```
 上面 `ejs` 代码中添加了 `tags-title` 中的代码, 其中 `site.posts` 为博客变量，想了解更多可以查看 Hexo 文档中的 [变量](https://hexo.io/zh-cn/docs/variables) 。
 
 ```css
 .archives-container{
  min-height: 400px;
  padding: 30px 40px;
  margin-bottom: 10px;
  background: #fff;
  box-shadow: 0 0 3px rgba(0,0,0,.15);
  .archive-year{
    line-height: 1;
    font-size: 30px;
    margin: 10px 10px 15px;
    color: $main-font-color;
  }
  .archive-body { // 我是新增的
    margin-top: 25px;
  }
  .archive-article{
    display: block;
    text-decoration: none;
    padding: 10px;
    transition: all .3s;
    border-right: 3px solid transparent;
    border-left: 3px solid transparent;
    cursor: pointer;
    .archive-meta{
      color: #8a8a8a;
    }
    .archive-title{
      color: $main-font-color;
      font-size: 20px;
      padding-left: 20px;
    }
    &:hover{
      transform: translateY(-5px);
      box-shadow: 0 5px 10px #eaeaea;
      border-color: $main-red;
      .archive-title{
        color: $main-red;
      }
    }
  }
}
 ```
 `archive-body ` 为新增的用于设置与标题部分的距离。
 
 ```yml
 counter:
  category:
    zero: "暂无分类"
    one: 共计一个分类
    other: "共计 %d 个分类"
  tagcloud:
    zero: "暂无标签"
    one: 共计1个标签
    other: "共计 %d 个标签"
  archive: # 我是新增的
    zero: "暂无文章"
    one: 共计一篇文章
    other: "共计 %d 篇文章"
 ```
 `archive ` 为新增的国际化标识。