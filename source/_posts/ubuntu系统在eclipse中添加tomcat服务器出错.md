---
title: ubuntu系统在eclipse中添加tomcat服务器出错
date: 2018-04-23 14:01:27
tags: ubuntu eclipse tomcat
categories: 服务端
---

第一次在ubuntu系统中在eclipse配置tomcat，报了一个
``` bash
Unknown version of Tomcat was specified.
```

明明很正常但是不知道为啥子。

其实是读取权限的原因

使用
```
sudo chmod -R 755 ./apache-tomcat-8.5.30
```
搞定