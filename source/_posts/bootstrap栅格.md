---
title: bootstrap栅格
date: 2018-04-24 16:26:11
tags: css bootstrap
categories: web
---

## 栅格
bootstrap的栅格使用了12份栅格系统。现在其他UI框架都在使用24栅格系统。

## bootstrap.css源码

### .container
``` css
.container {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}

@media (min-width: 576px) {
  .container {
    max-width: 540px;
  }
}

@media (min-width: 768px) {
  .container {
    max-width: 720px;
  }
}

@media (min-width: 992px) {
  .container {
    max-width: 960px;
  }
}

@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
  }
}

.container-fluid {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
```

栅格系统是包裹在类名为`container`或者`container-fluid`的容器内，上面的代码是对容器的样式。

- `container` 根据屏幕的不同有固定的宽度 两端永远是有`margin` 但是宽度小于`576px`时就是`100%` 页面宽度的变化使用了媒体查询
    将屏幕分为了 0 - 576px - 768px - 992px - 1200px
- `container-fluid` 宽度永远是屏幕的`100%`

**TIPS**
两种类名不允许互相嵌套

## .row

``` css
.row {
  display: -ms-flexbox;
  display: flex;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
  margin-right: -15px;
  margin-left: -15px;
}

.no-gutters {
  margin-right: 0;
  margin-left: 0;
}

.no-gutters > .col,
.no-gutters > [class*="col-"] {
  padding-right: 0;
  padding-left: 0;
}
```

1. 使用了`flex`布局
2. 使用了左右`margin`为负值，这样就使得`row`和`container`的宽度一样
3. 子项在必要时换行
4. 同时加上`no-gutters`类名时，就使`margin`值正常，此时`row`就比`container`宽度短`30px` 这样做的原因是不让`col`靠近屏幕边缘避裸奔

### .col

``` css
.col-1, .col-2, .col-3, .col-4, .col-5, .col-6, .col-7, .col-8, .col-9, .col-10, .col-11, .col-12, .col,
.col-auto, .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12, .col-sm,
.col-sm-auto, .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12, .col-md,
.col-md-auto, .col-lg-1, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-lg-10, .col-lg-11, .col-lg-12, .col-lg,
.col-lg-auto, .col-xl-1, .col-xl-2, .col-xl-3, .col-xl-4, .col-xl-5, .col-xl-6, .col-xl-7, .col-xl-8, .col-xl-9, .col-xl-10, .col-xl-11, .col-xl-12, .col-xl,
.col-xl-auto {
  position: relative;
  width: 100%;
  min-height: 1px;
  padding-right: 15px;
  padding-left: 15px;
}
```

1. 给所有的`col`类名增加了左右`padding`为15px，也就是两个相邻的`col`会有`30px`的`padding`
    如果给`row`加上了`no-gutters`类名，就取消了`col`的`padding`

``` css
.col {
  -ms-flex-preferred-size: 0;
  flex-basis: 0;
  -ms-flex-positive: 1;
  flex-grow: 1;
  max-width: 100%;
}

.col-1 {
  -ms-flex: 0 0 8.333333%;
  flex: 0 0 8.333333%;
  max-width: 8.333333%;
}
```

2. flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。

待续。。