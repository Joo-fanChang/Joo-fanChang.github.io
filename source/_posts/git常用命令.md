---
layout: post
title: git常用命令
date: 2018-04-04 08:47:34
tags: git 版本管理工具
categories: git
---
## 4 代码管理工具

### 4.1 GIT
常用命令（结合自己常用）

#### 4.1.1 提交4部曲
- `git add`
- `git commit`
- `git pull`
- `git push`在多人协作开发的时候，如果不`pull`是绝对`push`不上去的

#### 4.1.2 查看提交记录
- `git log`
如果想查看简单点就是
- `git log --pretty=oneline`
想看是某个人的提交历史
- `git log --author=changzhn`
想查看某个文件的提交历史
- `git log -- filename`
想在芒芒提交中搜索自己印象关键字
- `git log --grep=xxx`
终于找到那个提交的记录了，想看看提交了啥子内容，提交的ID截取前几位就可以了
- `git show hash`

#### 4.1.3 分支管理
查看当前的分支
- `git branch`
新建个分支并且切换过去
- `git checkout -b develop`
切换分支
- `git checkout develop/release/master`
注意：切换分支的时候，在当前分支修改的内容也是带过去，这一点很恶心，所以切换分支前，先使用下面这个命令，来看当前分支的状态
- `git status`
如果有修改（红色）或者添加（绿色）的内容，就要使用提交4部曲的`add`和`commit`来产生版本号后，才能切换分支
或者，使用下面这个命令，将这个分支上的所有修改放入一个异次元...
- `git stash`
再切换回来想恢复修改的时候可以使用
- `git stash list`
想看，往异次元中放入了几次，如果只想恢复最近的一次的话
- `git stash pop stash@{0}`

比如切到`release`分支上想合并`develop`分支
- `git merge develop`
在合并的时候，很不幸运的是冲突了
这个时候就需要解决冲突，在当前分支上，使用提交4部曲，为这次解决冲突产生一个版本号


#### 4.1.4 版本管理
找到版本号的时候想回退代码
- `git reset --HARD hash`
比如现在将代码推到过程，后来发现这真是一次垃圾的提交，在本地使用代码回退上一个版本的时候，想再`push`上去，这个时候发现是推不上去的。
- `git push origin master -f`
可以使用`-f`来暴力提交

所以提交的操作还是需要很谨慎的