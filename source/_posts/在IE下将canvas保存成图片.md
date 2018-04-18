---
title: 在IE下将canvas保存成图片
date: 2018-04-13 16:28:02
tags: ie兼容
categories: web
---

最近在做项目的时候，需要在页面生成二维码，然后下载下来。
不管用哪个轮子，生成二维码最终都是canvas的。

在标准浏览器下可以使用
``` js
let imgUrl = canvas.toDataURL('image/png'); // 转成base64
```
然后将结果给a标签，下载

但是在IE下是不支持的

``` js
var blob = canvas.msToBlob();
navigator.msSaveBlob(blob, '文件名称.png');
```
这样就可以直接愉快的下载了
