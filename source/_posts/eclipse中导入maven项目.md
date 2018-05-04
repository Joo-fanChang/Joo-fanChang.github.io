---
title: eclipse中导入maven项目
date: 2018-04-26 10:01:33
tags: eclipse maven
categories: 服务端
---


1. 配置好jdk和maven
2. 导入maven项目
3. 公司内部都有`setting.xml`文件，指引到哪里下载依赖。需要修改
``` xml
<localRepository>本地下载目录</localRepository>
```
4. `maven update`
5. `maven install`

