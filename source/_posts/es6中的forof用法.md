---
title: es6中的forof用法
date: 2018-04-07 16:44:34
tags: javascript es6
categories: web
---

``` js
let arr = ['a', 'b', 'c'];

let o = {
    name: 'chang',
    age: 18
};

for (const item of arr) {
    // console.log(item); // a b c
}

for (const item of arr.keys()) {
    // console.log(item); // 0 1 2
}

for (const item of arr.entries()) {
    // console.log(item); // [0, 'a'] [1, 'b'] [2, 'c']
}

for (const item of Object.keys(o)) {
    // console.log(item); // name age
}

for (const item of Object.values(o)) {
    // console.log(item); // chang 18
}

for (const item of Object.entries(o)) {
    console.log(item); // [ 'name', 'chang' ] [ 'age', 18 ]
}
```