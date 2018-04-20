---
title: ubuntu下安装node(tar.xz)
date: 2018-04-20 08:29:31
tags: node ubuntu tar.xz
categories: web
---

- 去官网下载源码，下载后是`tar.xz`格式的
- 将文件放到`/usr/node/`中
- 使用`sudo xz -d node-v8.11.1-linux-x64.tar.xz`进行解压会得到一个`node-v8.11.1-linux-x64.tar`文件
- 再次进行解压`sudo tar -xvf node-v8.11.1-linux-x64.tar`得到解压后的文件夹
- 可以进入该目录然后使用`./bin/node -v`来查看是否安装成功
- 进行全局配置
``` bash
sudo ln -s /usr/node/node-v8.11.1-linux-x64/bin/node /usr/local/bin/node
```
同样使用上面的命令将`npm`和`npx`也进行全局配置

这样配置完后有个特别坑的地方，就是所有全局安装的包命令全不可用

需要在`/etc/profile`的最后加上两行代码
需要注意的是这个文件是个只读文件，需要修改一下只读属性
``` bash
export NODE_HOME=/usr/node/node-v8.11.1-linux-x64/bin
export PATH=$NODE_HOME:$PATH
```

添加完成后重启，就可以愉快地使用全局命令了