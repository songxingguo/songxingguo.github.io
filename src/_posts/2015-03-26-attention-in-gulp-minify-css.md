---
title: gulp打包压缩css遇到的坑
categories:
  - 工具
  - 前端杂烩
tags:
  - gulp
  - minifycss
  - hack
date: 2015-03-26 00:00:00
---


很久没过来记录了，今天遇到了个坑，很坑的坑。

之前看过一些 gulp 和 grunt 相关的东西，但是没怎么用，有一次做项目用到，不过那次做的是一个监控系统，内部用，不要考虑兼容性问题，今天在处理业务的时候，用到了 gulp。

业务的老代码，不算很陈旧，2013年的（尚好是吧~）,那个时候也没啥太好用的压缩工具，翻出来的代码也没有一个默认的脚本可以自动打包，于是就用上了 gulp。

css 打包代码：

```css
gulp.task('css', function(){
  gulp.src('src/**/*.css')
    .pipe(gulp.dest('build'))
    .pipe(minicss())
    .pipe(rename({
      suffix:"-min"
    }))
    .pipe(gulp.dest('build'));
});
```

坑在哪里呢，没仔细看文档，`gulp-minify-css` 这个 gulp 插件就直接用上了，没想到他竟然把所有的低版本IE hack代码给干掉了，哭...

同学们要引起注意，记得在 minicss() 中送入参数：

```json
{
  "compatibility": "ie7"
}
```

具体其他配置请移步[官方文档](//github.com/jakubpawlowicz/clean-css#how-to-set-compatibility-mode)。
