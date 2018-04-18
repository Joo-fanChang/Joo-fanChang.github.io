---
layout: post
title: git ssh key
date: 2018-04-07 09:38:58
tags: git ssh key
categories: git
---
在使用ssh key之前先要确保电脑上有生成的ssh key。没有的话就生成一个，如果有的话就直接将公钥放到`github`或者`gitlab`上

## 查看

如果使用`windows`系统自带的`cmd`
``` bash
type %userprofile%\.ssh\id_rsa.pub
```

如果使用`git bash`工具或者`macOS`或者`powershell`的话
``` bash
cat ~/.ssh/id_rsa.pub
```
如果有的话就是这个样子的

{% asset_img git01.png %}

其中以`ssh-rsa`开头，以创建ssh key的邮箱结尾

如果电脑中已经有的话，可以跳过生成过程，直接到使用，没有的话就看生成的过程

## 生成

windows系统建议使用git bash，macOS就随意
``` bash
ssh-keygen -t rsa -C "changzhn@yonyou.com" -b 4096
```
这个邮箱要替换成自己的邮箱

## 使用
{% asset_img git02.png %}

无论是github还是gitlab都可以在settings的ssh key选项中找到上面这个选项，直接将自己的ssh key复制进去就可以了