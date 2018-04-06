---
layout: post
title: a标签的download下载 兼容firefox
date: 2018-03-28 12:17:23
tags: javascript web前端 js下载
categories: web
---

# 使用a标签的download属性下载

download是H5新增的属性，目前只有chrome和firefox支持，该属性表示是一个下载链接，并且属性值会作为文件名称，其中文件名可以带扩展名，也可以不带，如果不带则由浏览器自动识别。

href是要下载文件的链接，除了文件的绝对路径外，还可以使用base64和bolb等格式

可以封装成一个函数
```javascript
function download(filename, href) {
    var a = document.createElement('a');
    a.setAtrribute('href', href);
    a.setAtrribute('download', filename);
    a.click();
    a = null;
}
```
这样只需要传进来文件名称和路径就可以了。在chrome下是没有问题的，但是在firefox下是没有反应的

这个时候只需要把a标签插入到页面中就可以了

```javascript
function download(filename, href) {
    var a = document.createElement('a');
    a.setAtrribute('href', href);
    a.setAtrribute('download', filename);
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    a = null;
}

```


