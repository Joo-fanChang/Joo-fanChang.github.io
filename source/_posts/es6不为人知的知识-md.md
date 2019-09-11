---
title: es6不为人知的知识.md
date: 2019-09-04 09:16:35
tags: es6
categories: web
---

# ES6

> From [阮一峰的 EcmaScript 6入门](http://es6.ruanyifeng.com)

## 1 let const

### 1.0 循环中的let
```js
let a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

> 上面代码中，变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。

> 另外，for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```js
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

> 上面代码正确运行，输出了 3 次abc。这表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域。

### 1.1 如何将对象与变量绑定（不允许改变）

使用`const`定义的对象还是可以增加属性，因为`const`保证的是变量与内存地址的绑定；

可以使用`Object.freeze`将对象冻结；
```js
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

> 除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。

```js
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

### 1.2 ES6声明变量的6种方式
1. var
2. function
3. let
4. const
5. import
6. class

### 1.3 为什么let const声明的变量不再是window的属性

> 顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。这样的设计带来了几个很大的问题，
> 首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的）；
> 其次，程序员很容易不知不觉地就创建了全局变量（比如打字出错）；
> 最后，顶层对象的属性是到处可以读写的，这非常不利于模块化编程。
> 另一方面，window对象有实体含义，指的是浏览器的窗口对象，顶层对象是一个有实体含义的对象，也是不合适的。

## 2 变量的解构赋值

### 2.1 常见的解构赋值

```js
let str = 'a_b';
let [ , b ] = str.split('_');

let [x = 1] = [null];
x // null 只有严格等于 undefined 才能启用默认值
```
### 2.2 用途

1. 交换变量的值
```js
let a = 1;
let b = 2;
[ a, b ] = [ b, a ];
```

2. 函数参数的默认值

3. map解构
```js
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

## 3 字符串的扩展

### 3.1 添加了遍历接口
```js
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```

### 3.2 扩展的实例的方法
- startWith
- endWith
- includes
- padStart 补充字符串至指定的长度
- padEnd
- trimStart
- trimEnd

## 4 数值的扩展

### 4.1 将一些全局方法放到Number类上静态方法
或者增加了一些

- Number.isNaN
- Number.parseFloat
- Number.parseInt
- Number.isFinite
- Number.isInteger 判断是否为整数  

```js
Number.isInteger(25.0) // true
Number.isInteger(3.0000000000000002) // true
```

常量
- Number.EPSILON js数字的最小精度
- Number.Max_SAFE_INTEGER 最大整数
- Number.MIN_SAFE_INTEGER 最小整数

### 4.2 Math的静态方法
- Math.sign

```js
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN

Math.sign('')  // 0
Math.sign(true)  // +1
Math.sign(false)  // 0
Math.sign(null)  // 0
Math.sign('9')  // +1
Math.sign('foo')  // NaN
Math.sign()  // NaN
Math.sign(undefined)  // NaN
```

## 5 函数

### 5.1 尾递归
```js
function Fibonacci (n) {
  if ( n <= 1 ) {return 1};

  return Fibonacci(n - 1) + Fibonacci(n - 2);
}

Fibonacci(10) // 89
Fibonacci(100) // 超时
Fibonacci(500) // 超时

function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
  if( n <= 1 ) {return ac2};

  return Fibonacci2 (n - 1, ac2, ac1 + ac2);
}

Fibonacci2(100) // 573147844013817200000
Fibonacci2(1000) // 7.0330367711422765e+208
Fibonacci2(10000) // Infinity
```

> 函数式编程有一个概念，叫做柯里化（currying），意思是将多参数的函数转换成单参数的形式。这里也可以使用柯里化。

```js
function currying(fn, n) {
  return function (m) {
    return fn.call(this, m, n);
  };
}

function tailFactorial(n, total) {
  if (n === 1) return total;
  return tailFactorial(n - 1, n * total);
}

const factorial = currying(tailFactorial, 1);

factorial(5) // 120
```

> 总结一下，递归本质上是一种循环操作。纯粹的函数式编程语言没有循环操作命令，所有的循环都用递归实现，这就是为什么尾递归对这些语言极其重要。对于其他支持“尾调用优化”的语言（比如 Lua，ES6），只需要知道循环可以用递归代替，而一旦使用递归，就最好使用尾递归。