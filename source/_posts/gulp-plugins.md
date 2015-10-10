title: gulp plugins
date: 2015-09-04 10:02:25
categories: javascript
tags: gulp
---

![](/images/s12.jpg)

# Glup简介


## 常用插件及介绍


### gulp-live-server

`gulp-live-server`可以配合chrome插件`liveReload`使用，其功能是在写前端功能的时候可以自动刷新的页面，与当前保持的代码同步。

useage:

```javascript
var gulp = require('gulp');

var gls = require('gulp-live-server');

var server = gls.static('.', 3000);

server.start();


gulp.task('default', function() {
    console.log('default ....');

    gulp.watch([
        '*.html',
        './scripts/*.js',
        './styles/*.css'
    ], function(file) {
        server.notify.apply(server, [file]);
        console.log('file is reload');
    });
});
```