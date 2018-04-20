---
title: webpack4配置全记录
date: 2018-04-17 21:31:03
tags: js webpack
categories: web
---

持续更新...
最后更新时间2018.4.17

# webpack(4)

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
|文件|大小|
| :--: | :--: |
|未压缩|337k|
|压缩|86.4k|


## 4 resolve
### 4.1 alias
提供了一些引入模板的简写名称，比如：
想引入的文件不想使用`../` 和 `../../`之类的，都可以从`src`为起始目录往下寻找
``` js
resolve: {
    alias: {
        '@': path.resolve(__dirname, '../src')
    }
}
```
在引入模块的时候
``` js
import '@/assets/common.less'
```

### 4.2 extensions
设置模块可省略的后缀名称，比如：
上面引入模块可以改成这样
``` js
import '@/assets/common'
```

如果两个模板重名的话，应该就需要加上后缀了
``` js
import '@/assets/common.css';
import '@/assets/common.less';
```

## 5 module
webpack默认只能解析`js`模块的，所以前端资源的`css` `less` `jsx` `png`等等都需要`loader`实现的 

### 5.1 rules css
``` js
module: {
    rules: [
        {
            test: /\.css$/,
            use: [
                'style-loader',
                {
                    loader: 'css-loader'
                }
            ]
        }
    ]
}
```

增加`postcss-loader`配置`autoprefixer`

其中有几点注意的地方：
- 解析出来的css会内嵌到页面中，如果想提取出来，需要使用loader做不了，但是plugin可以完成的
- use是一个数组，里面可以直接写`style-loader`这样，或者以对象形式，具体配置一些`options`一些选项
- 比如要处理的`less`文件，只需要`less-loader`是不可以的。需要同时使用`style-loader` 和 `css-loader`，就是需要一级级处理，一个`loader`只做自己的事情，做完后交给下一个`loader`来处理。 TIPS：使用`less-loader`也需要安装`less`模块

`use`的格式
- 字符串
- 数组
- 对象

### 5.3 rules url-loader
关于`url-loader`和`file-loader`：
- `url-loader`是基于`file-loader`的一层封装，可以做一些图片大小限制，小于多少`kb`时，可以将图片转成`base64`内嵌到页面中，而减少一次`http`请求
- `url-loader`是依赖于`file-loader`的，所以使用`url-loader`的话，就必须安装`file-loader`的，否则会报错

同样地，可以使用`url-loader`处理图标字体