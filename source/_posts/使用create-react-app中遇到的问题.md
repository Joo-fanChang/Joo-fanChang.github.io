---
title: 使用create-react-app中遇到的问题
date: 2018-04-12 08:52:29
tags: web javascript js react create-react-app
categories: web
---

1. 将配置文件导出
``` bash
npm run eject
```

2. 打包的静态资源路径
``` js
// 在config -> path.js -> 36 line位置
const servedUrl =
envPublicUrl || (publicUrl ? url.parse(publicUrl).pathname : './');
```

3.   在create-react-app中配置了less，然后在引入的时候，会报一个`Inline JavaScript is not enabled. Is it set in your options?`的错，这个时候需要
``` js
{
    loader: require.resolve('less-loader'),
    options: { javascriptEnabled: true }
}
```

持续更新...