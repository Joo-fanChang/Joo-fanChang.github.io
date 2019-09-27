---
title: 实践出真知：MVC和MVVM
date: 2018-04-13 08:40:02
tags: mvc mvvm 数据驱动
categories: web
---

> 理论来自于实践，并指导于实践 - 佚名

我相信在MVC等理论形成之前，就已经有人在付诸MVC的实践。只是后来有人总结下来，并指导人们做软件开发。这种设计典范并不是只有后台的代码才有，前端也可以有很好的框架，像react和vue。

为什么这么强调实践。因为在我学习MVC理论的时候，我是蒙蔽的。虽然看了很多优秀的文档，比如，阮大神的[MVC，MVP 和 MVVM 的图示](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)

看了之后挺有感觉，兴冲冲写代码的时候，又感觉不知所措。不知从何下手，不知如何划分。
最近在用react，又帮别人写了一个jquery代码，现在将这种套路试着总结一下。

## 需求
从后台获取数据，渲染一个简单的列表，实现列表的增删改查
类似于这种 
{% asset_img demo1.png %}

## 之前的套路 jquery

这是之前写代码的套路：
1. 页面初始渲染时，从后台获取数据（一个数组）
2. 将数据使用 字符串拼接 或者 前端模板 将数据渲染到页面中
3. 给渲染的列表绑定增删改查的事件
4. 拿一个删除举例，点击删除按钮，获取当前行数据的id值，发送ajax请求，根据id在后台数据库中删除，返回成功码，前端将当前行使用DOM方法删除
5. 其他的事件也是类似，在ajax的成功回调中进行dom的操作，毕竟jquery是DOM操作神器，操作起来都感觉6的飞起

## 使用react实现

1. 在组件中定义`state.arr`将即将要渲染的数据保存在其中
2. 写html模板
3. 请求数据，`setState`数据值，数据改变，页面自动刷新
4. 接着拿删除举例，点击删除按钮，获取当前行数据的id值，发送ajax请求，在成功的回调中，使用
``` js
let a = this.state.arr.filter(item => item.id !== id);
this.setState({
    arr: a
})
```
数据改变，视图又自动刷新
5. 其他事件也是类似，从始至终都没有再操作过dom，只是各种使用`setState`方法，来维护要展示的数据

这种方法肯定就是著名的MVC模式了。和第一种比起来，就发现这种好处是什么。我们只专心的操作数据，维护状态，从没有去操作过dom。
让我们精力更集中。

适时放一张图来感受一下。

{% asset_img mvc1.png %}

看着上图来念一遍：数据改变 -> 视图刷新 -> 事件修改数据（controller）-> 数据改变再次渲染视图。

抄一段阮一峰大神博客的原文
> View 传送指令到 Controller
> Controller 完成业务逻辑后，要求 Model 改变状态
> Model 将新的数据发送到 View，用户得到反馈

TIPS:因为jquery是操作dom神器。使用react和vue都不建议直接操作dom，所以用框架的话还是早是日脱离jquery

如果感觉不强烈，可以画一张示意图来看一下，第一种方法的流程是什么样的。

{% asset_img mvc2.png %}

获取数据后 -> 渲染了一次视图 -> 监听事件完成后台交互 -> 又双叕粗暴的改变了视图

## 根据这种思想改造一下第一种方法 jquery

我们要维护一个状态数组，数组改变后，就去刷新视图，监听事件，改变状态

准备工作：
因为jquery不会自动刷新视图，还是需要写一个`render`方法来渲染视图，这个方法在初次渲染数据和修改数据后改变都要执行的。

``` js
var arr = []; // 要维护的数据

function render(arr) {
    // 渲染 arr
    
    bindEvent();
}

function bindEvent() {
    // 绑定各种事件 
}

```
这样重构完，再来结合图看一次。

{% asset_img mvc3.png %}

[点击查看代码示例](https://changzhn.github.io/2018/04/13/%E6%9D%A5%E8%87%AA%E5%AE%9E%E8%B7%B5%E7%9A%84MVC%E5%92%8CMVVM/form.html)

[点击查看源码](https://github.com/changzhn/changzhn.github.io/blob/master/2018/04/13/%E6%9D%A5%E8%87%AA%E5%AE%9E%E8%B7%B5%E7%9A%84MVC%E5%92%8CMVVM/form.html)


## 接下来对比一下 mvc 和 mvvm

上面实现了简陋的mvc。会发现，不管`arr`状态有个风吹草动，整个列表都会重新调用渲染方法。react和vue内置了各种diff算法，避免了这个尴尬。





未完待续。。。
