---
title: flex布局及兼容
date: 2018-05-04 08:40:05
tags: css flex
categories: web
---
元素设置为flex模型后，其float clear vertical-align 等属性都会失效

## 用法

#### 作用在父元素上的属性
- `display: flex`
设置这个属性后就会成为flex盒模型

- `flex-direction`
设置flex主轴的方向，属性值有：`row/row-reverse/colum/colum-reverse`

- `flex-wrap`
设置换行的方式，属性值有：`nowrap/wrap/wrap-reverse`

- `flex-flow` 是`flex-direction`和`flex-wrap`的组合属性
默认值为`row nowrap`

- `justify-content`
设置项目在主轴上的排列方式：属性值有
1. flex-start
2. flex-end
3. center
4. space-between 两端对齐，中间均分
5. space-around  两侧有间隙，这个间隙是项目之间间隙的一半

- `align-items`
项目在交叉轴上的对齐方式：属性值有
1. flex-start
2. flex-end
3. center
4. baseline
5. stretch **默认值，占满空间**


- `align-content`
多根轴的对齐方式，如果只有一根轴就不生效
这个的意思是：如果设置为`row`排列，一行又显示不下，设置了`wrap`那么会有两行或者三行，这个属性是设置了这个整体在垂直方向的位置
属性值有：
1. flex-start
2. flex-end
3. center
4. space-between 两端对齐，中间均分
5. space-around  两侧有间隙，这个间隙是项目之间间隙的一半
6. stretch **默认值**


#### 作用在项目上的属性
- `order` 
数值越小越靠前，默认为0


- `flex-grow` 放大比例，如何去分配默认空间，0不放大，1均分，如果按其他比例，可以单独给项目设置
- `flex-shrink` 缩小比例，0不缩小，1等比例缩小，默认值是1
- `flex-basis` 是设置元素的宽高值，默认为auto

- `flex`为上面三个属性的组件属性 默认值为`0 1 auto`
这个属性是有两个快捷属性
- `auto` 1 1 auto
- `none` 0 0 auto

- `align-self` 单独设置对齐方式
属性
0. auto **默认值，从父元素中继承**
1. flex-start
2. flex-end
3. center
4. baseline
5. stretch 

## 兼容写法
下面这套兼容写法是由`autoprefixer`提供

```css
.container {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;

  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -ms-flex-direction: row;
  flex-direction: row;

  -ms-flex-wrap: wrap;
  flex-wrap: wrap;

  -webkit-box-pack: start;
  -ms-flex-pack: start;
  justify-content: flex-start;

  -webkit-box-align: baseline;
  -ms-flex-align: baseline;
  align-items: baseline;

  -ms-flex-line-pack: center;
  align-content: center;
}

.item {
  -webkit-box-ordinal-group: 3;
  -ms-flex-order: 2;
  order: 2;

  -webkit-box-flex: 1;
  -ms-flex-positive: 1;
  flex-grow: 1;

  -ms-flex-negative: 0;
  flex-shrink: 0;

  -ms-flex-preferred-size: 30px;
  flex-basis: 30px;

  -ms-flex: auto;
  flex: auto;
  -ms-flex-item-align: center;
  align-self: center;
}
```

flex的兼容
- 安卓4+
- IOS6+
- IE 10 
