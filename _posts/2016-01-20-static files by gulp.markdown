---
layout: post
title: gulp压缩合并静态文件
categories:
- 前端笔记
---

项目中用了Angular，分成多个模块后js文件也越来越多，访问一次会产生大量的请求，选择使用gulp来完成压缩合并静态文件，在这个过程中遇到了很多问题，在这里记录下来

主要用到了以下插件

* `gulp-sourcemaps` 生成source map，方便跟踪问题代码
* `gulp-concat` 合并文件
* `gulp-uglify` 压缩js
* `gulp-minify-css` 压缩css
* `gulp-rev` 为文件加上版本控制信息
* `gulp-rev-collector` 配合gulp-rev替换文件名
* `gulp-jshint` js代码检查
* `run-sequence` 顺序执行task  

如果需要更多gulp插件的话，可以去[这里](https://github.com/Platform-CUF/use-gulp)逛逛有没有可用的，有人整理好了一份


---


简单说明一下我的目录结构，`third`目录存放第三方的框架和插件，`css`，`js`目录下分别存放自己编写的css，js，`dist`目录用于存放gulp压缩合并后的文件，`templates`目录下存放了首页的模版，里面的css，js路径即硬盘里的绝对路径

[![webProjectDir](../../../../../public/Image/2016/01/webProjectDir.png)](../../../../../public/Image/2016/01/webProjectDir.png)

---

gulpfile代码奉上，仅供参考

```js
var del = require('del'),
	gulp = require('gulp'),
	concat = require('gulp-concat'),
	jshint = require('gulp-jshint'),
	minifycss = require('gulp-minify-css'),
	rev = require('gulp-rev'),
	uglify = require('gulp-uglify'),
	sourcemaps = require('gulp-sourcemaps'),
	revCollector = require('gulp-rev-collector-r'),
	runSequence = require('run-sequence');

var source = {
	js: {
		'third': [  
			'master/third/jquery/dist/jquery.min.js',
			'master/third/angular/angular.min.js',
			//...
		],

		'app': [
			'master/js/app.js',
			//...
		] 
	},
	css: [ 
		'master/third/bootstrap/dist/css/bootstrap.min.css',
		//...
	]
}

gulp.task('clean:app', function() {
	return del.sync('master/dist/*', {
		force: true
	});
});

gulp.task('hint', function(){
	return gulp.src(source.js.app)
		.pipe(jshint())
		.pipe(jshint.reporter('default'));
});   

gulp.task('scripts:app', [], function() {
	return gulp.src([source.js.third, source.js.app].join(",").split(",")) 
		.pipe(sourcemaps.init())
		.pipe(concat('vorder.js'))
		.pipe(uglify({
	        compress: {
	            drop_console: true
	        }
	    })) 
		.pipe(sourcemaps.write()) 
    	.pipe(gulp.dest('master/dist')); 
});

gulp.task('styles:app', function() {
	return gulp.src(source.css) 
		.pipe(minifycss())
		.pipe(concat('vorder.css'))   
        .pipe(gulp.dest('master/dist'));
});     

gulp.task('rev', function() {
	return gulp.src(['master/dist/*.js', 'master/dist/*.css'], {
			base: 'master/dist'
		})
		.pipe(rev())
		.pipe(gulp.dest('master/dist'))
		.pipe(rev.manifest())
		.pipe(gulp.dest('master/dist'));
});

gulp.task('revCollector', function() {
	return gulp.src(['master/dist/*.json', 'master/templates/index.html']) 
		.pipe(revCollector({
            replaceReved: true,
            dirReplacements: {
                '': 'dist/'
            }
        }))
		.pipe(gulp.dest('master'));
}); 

gulp.task('default', function() {
	runSequence('clean:app', 'hint', ['scripts:app', 'styles:app'], 'rev', 'revCollector');
});  
```








 