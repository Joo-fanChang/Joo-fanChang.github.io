---
title: item2与服务器操作
date: 2018-04-16 10:15:09
tags: item2 服务端
categories: 服务端
---
要实现的内容：
- mac下使用item2工具与服务器连接
- 免密登录服务器
- 使用item2与服务器互传文件


## mac下使用item2工具与服务器连接
mac下可以使用命令行直接与服务器连接。基本使用是
``` bash
ssh root@ip
```
如果地址没有写错的话，会显示输入密码，正确输入密码，然后愉快地连接上了服务器

如果是第一次连接的话会出现
``` bash
　　The authenticity of host 'host (12.18.429.21)' can't be established.

　　RSA key fingerprint is 98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d.

　　Are you sure you want to continue connecting (yes/no)?

```
> 这段话的意思是，无法确认host主机的真实性，只知道它的公钥指纹，问你还想继续连接吗？
> 所谓"公钥指纹"，是指公钥长度较长（这里采用RSA算法，长达1024位），很难比对，所以对其进行MD5计算，将它变成一个128位的指纹。上例中是98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d，再进行比较，就容易多了。
> 很自然的一个问题就是，用户怎么知道远程主机的公钥指纹应该是多少？回答是没有好办法，远程主机必须在自己的网站上贴出公钥指纹，以便用户自行核对。
> 假定经过风险衡量以后，用户决定接受这个远程主机的公钥。

然后打出`yes`，才会到输入密码的步骤

来自[阮一峰的博客](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)

如果经常登录服务器的话，可以在 item2 -> 偏好设置 -> profiles 设置服务器信息

{% asset_img item2-01.png %}

下次使用的话就可以直接快捷键打开一个item2窗口来连接服务器

## 免密登录服务器
每次登录都是很麻烦的。可以利用的ssh的公钥和私钥系统，将本地的公钥上传服务器。下次登录的时候就可以免密了。

因为我本地有配置git的时候公钥，直接将这个id_rsa.pub上传到服务器
``` bash
ssh-copy-id -i ~/.ssh/id_rsa.pub root@id
```
过程有一个输入密码的过程，输入密码后，上传成功，之后就可以愉快的免密连接服务器了。

## 使用item2与服务器互传文件
需要借助 `scp` 命令
主要有4个

1. 上传文件至服务器
``` bash
scp 文件名 root@ip:目标路径
```

2. 上传文件夹至服务器
``` bash
scp -r 文件夹名称 root@ip:目标路径
```

3. 从服务器下载文件
``` bash
scp root@ip:文件路径 本地路径
```

4. 从服务器上下载文件夹
``` bash
scp root@ip:文件夹路径 本地路径
```

放一张下载的图感受一下

{% asset_img item2-02.png %}