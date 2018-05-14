---
title: 代理工具whistler的使用
date: 2018-05-14 20:15:40
tags: web js
category: web
---

## 为什么是whistler
在windows系统上常用的工具是`fiddler`，在mac上常用的工具是`charles`。`fiddler`收不收费就不知道了，反正`charles`是收费滴。

反观`whistler`只是一个`node`包，在任何系统上是任意运行的。

## 使用

1. 使用`npm`全局安装`whistler`
``` bash
npm i whistler -g
```

2. 下载后可以在命令行里自带几个命令，常用的
``` bash
w2 start/restart
w2 stop
```

意思是起一个代理服务，关闭代理服务。
其他常用命令直接`w2`可以查看命令列表

3. 可以直接打开本地端口可以查看配置 `127.0.0.1:8899`
之前在网上找的教程界面太旧了，这个截图目前是最新的
{% asset_img image1.png %}

4. map local
可以新建一个规则，比如
``` bash
test
```

也可以在默认的`default`中写
```
http://uretailprint.yonyouup.com/iuap-print/static-build/js/core/iprint.designer.js file:///Users/changzhn/yonyou/iprint/iuap-iprint/iuap-print-service/WebContent/resources/js/core/iprint.designer.js
```
中间只一个空格，没有换行。

这样访问的时候，线上js就可以替换成本地js。这个好处谁用谁知道。

5. whistler配合的chrome插件　SwitchyOmega
本来这个包是有自己的插件的，不知道为啥下架了，官方也是推荐使用这个插件　

{% asset_img image2.png %}

配置就是这个样子

{% asset_img image3.png %}

6. 其他功能下次再say...