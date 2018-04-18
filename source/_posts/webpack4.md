---
title: webpack4配置全记录
date: 2018-04-17 21:31:03
tags: js webpack
categories: web
---

持续更新...
最后更新时间2018.4.17

## 1 配置分离
项目过大的时候，配置文件最好是分离开来。导出`webpack`配置的时候，需要使用`webpack-merge`这个包。
比如
- webpack.base.js
- webpack.dev.js
- webpack.prod.js

其中`base`放一些基本的配置。`dev`中可以配置`devServer`等开发时的参数。`prod`中配置压缩等配置。

## 2 entry output

### 2.1 默认值
在webpack4中，给了这两个`key`了默认值。分别为
``` js
entry: './src/main.js',
output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
}
```
也就是说，如果在一个项目目录中没有任何配置的话，直接运行`webpack`也是可以的。

**TIPS**
1. 在webpack4中，命令行与配置文件已经分成两个包了。如果直接使用命令行工具的话，需要使用`webpack-cli`这个包。
2. 如果想更改`output.path`的时候，也需要加了`entry`参数，也就是说，修改默认参数的时候，两个需要一起修改。否则就会报一个
``` base
ERROR in multi (webpack)-dev-server/client?http://localhost:8080 ./src
Module not found: Error: Can't resolve './src' in '/Users/changzhn/git/webp4'
 @ multi (webpack)-dev-server/client?http://localhost:8080 ./src
```
因此就算和默认值相同也要加了`entry`属性

### 2.2 output.publicPath

> 这是我找工作的时候一道面试题

这个属性决定了，`index.html`文件引入静态资源的路径
比如：
``` js
publicPath: 'build'
```
- 开发时，该属性代表静态资源打包的路径，所以在相应`index.html`中引入`<script src="./build/bundle.js"></script>`
例如`publicPath`设置为 `xx/build/`
那么相应的`index.html`引入的就是 `<script src="./xx/build/bundle.js"></script>`
- 生产时，该属性。。也是这个意思，不过可以配置`cdn`目录
例如`publicPath`设置为`https://bootcdn.com/changzhn/`
那么相应的`index.html`引入就是`<script src="https://bootcdn.com/changzhn/build/bundle.js"></script>`

## 3 devServer

### 3.1 compress
boolean
一切服务都启用`gzip压缩`
测试：
||大小|
|:-|:-|
|未压缩|337k|
|压缩|86.4k|

