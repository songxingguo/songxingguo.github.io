title: Hexo问题解决
author: songxingguo
tags:
  - Hexo
  - 博客搭建
categories:
  - 开发者手册
date: 2018-06-02 17:06:00
---
### 服务busy

结束服务，使用命令清除
```
 hexo clean
```
重新启动服务
```
 hexo s
```

### hexo-admin后台管理博客

#### Hexo Deploy 

- 添加 deployCommand 

  [What is admin.deployCommand?](https://github.com/jaredly/hexo-admin/issues/70)
  
  [hexo-admin安装使用](https://albenw.github.io/posts/4ffa5bc6/)

- deploy Error: spawn UNKNOWN 

  [deploy Error: spawn UNKNOWN ](https://github.com/jaredly/hexo-admin/issues/94)
  
  [Hexo Admin Deploy扩展教程](http://www.inmyai.com/2018/05/01/Hexo-Admin-Deploy%E6%AD%A3%E7%A1%AE%E6%89%93%E5%BC%80%E6%96%B9%E5%BC%8F/)

- EEE TypeError: Cannot read property 'on' of undefined

  [When I click 'deploy' botton, show 'EEE TypeError: Cannot read property 'on' of undefined'](https://github.com/jaredly/hexo-admin/issues/179)

  [hexo 通过hexo-admin进行全自动发布文章，能在线拷贝图片，实时查看效果，更加优雅！！！完成hexo g -d ，彻底脱离命令行操作！！！！](https://blog.csdn.net/dataiyangu/article/details/83066586)

  修改授权：

  ```bash
  chmod +x hexo-generate.sh
  ```

#### 参考

[hexo-admin后台管理博客](https://segmentfault.com/a/1190000010434546)

### 搭建博客样式加载不出来

[hexo + github pages搭建博客样式加载不出来](https://blog.csdn.net/banjw_129/article/details/82261165)

需要修改_config.yml文件中的url地址和根目录:

```yml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://banjingwei.github.io/ban.github.io
root: /ban.github.io/
permalink: :year/:month/:day/:title/
permalink_defaults:
```
