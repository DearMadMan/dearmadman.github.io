title: '在报错时，让gulp监视仍继续的方法'
date: 2015-08-19 09:58:55
categories: javascript
tags: [javascript, gulp]
---

![gulp](/images/gulp.jpg)

在运行glup任务时，如果使用watch进行task,如果task没有做错误处理，gulp进程会直接崩溃掉,如果我们想出错扔继续监听怎么办？
有时候我们需求以下场景:
实时的编译jade模板，编写jade模板时实时的进行编译输出，偶尔语法错误，这时gulp就崩溃了，又要重新运行watch? 是不是很烦？


原理：gulp官方推荐在编写的插件中在发生错误时抛出一个gulp-util的错误，这个错误会出现在文件处理的stream中，想要捕获这个错误就需要在stream添加`on('error',handler)`处理函数.
但是这个方法在jade出错时无效，wacher是在继续但是却不会继续执行任务，这应该是不同插件的实现原理不同导致的，那么就需要一个通用的可以处理所有错误的方法，那就是`gulp-plumber`插件了！

```javascript
var gulp=require('gulp');
var jade=require('jade');
var gulpJade=require('gulp-jade');
var util=require('gulp-util');
var plumber=require('gulp-plumber');

function errorHandle(e){
	util.beep(); //控制台发声,错误时beep一下
	util.log(e);
}

gulp.task('jade',function(){
		return gulp.src('views/*.jade')
			.pipe( plumber({errorHandler:errorHandle}) )  //在处理前注册plumber
			.pipe(
				gulpJade({
					jade:jade,
					pretty:true
				})
				)
			.pipe(gulp.dest('bin/html/'));
})

/*  jade watch */

gulp.task('default',function(){
	gulp.watch("views/*.jade",['jade']);
	console.log('start task...');
});

```


ok，这样即使jade编译出错也会继续监视并运行task了！
