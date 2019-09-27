---
layout: post
title: gulp基本配置
date: 2018-03-15 09:17:13
tags: gulp 前端自动化构建工具
categories: web
---

# 简单的gulp配置
## 基本功能
- 起本地服务
    + 启用热更新
    + 启用代理
- 编译静态文件
    + 包括html文件 将其打包到dist文件夹
    + 包括less文件 将其打包到dist文件夹 并进行压缩
    + 包括js文件 将其打包到dist文件夹 并进行压缩 配置了sourcemap
- 设置了default任务

## package.json
```json
{
  "name": "g-ex",
  "version": "1.0.0",
  "description": "a gulp template",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "autoprefixer": "^7.1.4",
    "babel-core": "^6.26.0",
    "babel-preset-env": "^1.6.0",
    "gulp": "^3.9.1",
    "gulp-babel": "^7.0.0",
    "gulp-clean-css": "^3.9.0",
    "gulp-concat": "^2.6.1",
    "gulp-connect": "^5.0.0",
    "gulp-less": "^3.3.2",
    "gulp-postcss": "^7.0.0",
    "gulp-rename": "^1.2.2",
    "gulp-sourcemaps": "^2.6.1",
    "gulp-uglify": "^3.0.0",
    "http-proxy-middleware": "^0.17.4",
    "uglify-es": "^3.1.3"
  }
}
```

## gulpfile.js
```javascript
var gulp = require('gulp');
var connect = require('gulp-connect');
var babel = require('gulp-babel');
var sourcemaps = require('gulp-sourcemaps');
var rename = require('gulp-rename');
var uglify = require('gulp-uglify');
var less = require('gulp-less');
var cleanCss = require('gulp-clean-css');
var autoprefixer = require('autoprefixer');
var postCss = require('gulp-postcss');
var proxy = require('http-proxy-middleware');

// 起服务
gulp.task('connect', function() {
	connect.server({
		root: 'dist',
		livereload: true,
		port: 9000,
    middleware: function(connect, opt) {
		   return [
			    proxy('/api',  {
				    target: '127.0.0.1:3000',
				    changeOrigin:true
			   })
		   ]
	   }
	});
});

// html文件
gulp.task('html', function() {
	gulp.src('./src/*.html')
		.pipe(connect.reload())
		.pipe(gulp.dest('./dist'));
});

// css文件
gulp.task('less', ['html'], function() {
	gulp.src('./src/css/*.less')
	    .pipe(sourcemaps.init())
		.pipe(less())
		.pipe(postCss([autoprefixer({browsers: ['last 2 versions']})]))
		.pipe(gulp.dest('./dist/css'))
		.pipe(cleanCss())
		.pipe(rename({extname: '.min.css'}))
		.pipe(sourcemaps.write('./'))
        .pipe(connect.reload())
		.pipe(gulp.dest('./dist/css/min'))
});

// js文件
gulp.task('js',['html'] , function () {
	gulp.src('./src/js/*.js')
		.pipe(sourcemaps.init())
		.pipe(babel({
			presets: ['env']
		}))
		.pipe(gulp.dest('./dist/js'))
		.pipe(uglify())
		.pipe(rename({extname: '.min.js'}))
        .pipe(sourcemaps.write())
        .pipe(connect.reload())
		.pipe(gulp.dest('./dist/js/min'))
});

// 图片
gulp.task('img', function () {
	gulp.src('./src/img/*.*')
    .pipe(connect.reload())
		.pipe(gulp.dest('./dist'));
})

// 监控
gulp.task('watch', function() {
	gulp.watch(['./src/*.html'], ['html']);

	gulp.watch(['./src/js/*.js'], ['js']);

	gulp.watch(['./src/css/*.less'], ['less']);
});

// 默认任务
gulp.task('default', ['connect', 'watch', 'js', 'less']);

```

[源码](https://github.com/changzhn/gulp-template/tree/normal)

还有针对于打包方案
[源码](https://github.com/changzhn/gulp-template/tree/spa)