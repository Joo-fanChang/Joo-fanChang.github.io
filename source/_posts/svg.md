---
title: svg
date: 2019-09-12 14:22:12
tags: svg
catetory: svg web
---

# SVG

## SVG整体缩放的2种方式
1. 使用viewbox属性
```jsx
<svg
    width={`${Math.floor(width * ratio)}px`}
    height={`${Math.floor(height * ratio)}px`}
    viewBox={`0 0 ${width} ${height}`}
    version="1.1"
    baseProfile="full"
    xmlns="http://www.w3.org/2000/svg"
>
    内部使用的是设计时的 width 和 height尺寸
</svg>
```

2. 使用g的scale功能
```jsx
<svg
    width={`${Math.floor(pageSize.width * ratio)}px`}
    height={`${Math.floor(pageSize.height * ratio)}px`}
    version="1.1"
    baseProfile="full"
    xmlns="http://www.w3.org/2000/svg"
>
    <g transform={`scale(${ratio})`}>
        内部使用的是设计时的 width 和 height尺寸
    </g>
</svg>
```