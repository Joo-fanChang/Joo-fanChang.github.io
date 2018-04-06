---
layout: post
title: JavaScript中数据类型四种检测方式
date: 2018-03-15 09:13:04
tags: javascript基础 js 数据类型
categories: web
---

## 检测宗旨
对一些不确定的值操作时，为了避免发生错误，最稳妥的方式是在操作之前要加一个数据类型判断，再根据相应类型来进行不同的操作。

比如回调函数
```javascript
function fn(callBack) {
	callBack();
}
```
这个时候就要在`callBack`执行之前加一个是否为函数的判断，如果是再让函数执行
修改如下
```javascript
function fn(callBack) {
	if(typeof callBack === 'function') {
		callBack();
	}
}
```

在检测一些引用数据类型的时候，我们更想知道这个对象是属于哪个类，比如想判断一个对象是否为数组，好去调用数组上的方法，而不是想判断这个对象是不是属于对象。因为数组的方法都是保存在数组的原型上，而不是在Object基类上。

## 检测方法
### typeof
#### 用法
typeof是一个**操作符**
在执行的时候可以`typeof(要检测的对象)`；
也可以这样`typeof 要检测的对象`，因为typeof不是函数，因此`()`不是必需的，可以将检测的对象直接跟在`typeof`后面

#### 检测范围
在JS中，能用typeof能检测出来的数据类型，最好使用typeof来检测
typeof是基本数据类型检测利器(但是不包括null)；也可以检测是否为一个函数，但是在检测对象时，并不能为每个对象明确指出是属于哪一类，因为返回值都是`object`。

示例
```javascript
typeof 1;//'number'
typeof true;//'boolean'
typeof '';//'string'
typeof undefined;//'undefined'
typeof function (){};'function'
```

#### tips
typeof的返回值是一个**字符串**，有时候在浏览器里会把引号省略掉，但是可以用typeof去检测这个返回值，比如
```javascript
typeof typeof 1;//'string'
//这行代码的意思是，用typeof去检测typeof 1的返回值，最终的返回值是string
```
因为这个也以引伸，如果两个及以上的typeof连用的话，那么输出的结果就是`string`

#### typeof null
null在JS中叫做空对象指针，因此可以理解为null为对象

而更专业的解释是
>JS类型值是存在32 BIT 单元里,32位有1-3位表示TYPE TAG,其它位表示真实值   而表示object的标记位正好是低三位都是0    000: object. The data is a reference to an object.   而js  里的Null 是机器码NULL空指针, (0x00 is most platforms).所以空指针引用 加上 对象标记还是0,最终体现的类型还是object..


### instanceof
## 用法
instanceof是一个操作符，返回值是一个布尔值
instanceof是检测引用数据类型，而不能检测基本数据类型
```javascript
var arr = [];
arr instanceof Array;//true
```
但是只要是在原型链上出现过构造函数都会返回true，所以这个检测结果不很准确
```javascript
arr instanceof Object;//true
```

### constructor
constructor这个属性存在于构造函数的原型上，指向构造函数，对象可以通过`__proto__`在其所属类的原型上找到这个属性
```javascript
var arr = [];
arr.constructor === Array;//true
arr.constructor === Object;//false
//因为arr通过原型链查找到的constructor指向了Array，所以跟Object判断就是错误滴
```

constructor是比较准确判断对象的所属类型的，但是如果有继承的话
```javascript
function Fn() {

}
Fn.prototype = new Array();
var fn = new Fn();
console.log(fn.constructor === Array);
//fn是一个对象，但是它的构造函数指向Array竟然是true
```
因此这个方法要慎用

### Object.prototype.toString.call()
在Object基本类定义的这个toString()方法，是用来检测数据类型的；
跟字符串、数字、布尔等原型上定义的toString()方法基本用法都是转换字符串的。

这对对数据检测应该是最准确的
```javascript
console.log(Object.prototype.toString.call(1));//[object Number]
console.log(Object.prototype.toString.call(''));//[object String]
console.log(Object.prototype.toString.call(true));//[object Boolean]
console.log(Object.prototype.toString.call(null));// [object Null]
console.log(Object.prototype.toString.call(undefined));//[object Undefined]
console.log(Object.prototype.toString.call([]));// [object Array]
console.log(Object.prototype.toString.call({}));// [object Object]
console.log(Object.prototype.toString.call(/^$/));//[object RegExp]
console.log(Object.prototype.toString.call((function () {})));//[object Function]
```

## 基本包装类型
JS是一门弱类型的语言，因此在定义变量时，一般都可以使用字面量方式和实例创建方式。
一般两种创建方式没有差别，但是涉及到基本数据类型中的number、string、boolean时就完成不一样。

例如
```javascript
var a = 1;
var b = new Number(1);
typeof a;//'number'
typeof b;//'object'
```
因为字面创建基本数据类型的值时，并不会调用JS的内置构造函数，因此字面量创建的值就是基本数据类型，而不是实例(对象数据类型)。-->所以用instanceof检测基本数据类型时都会返回false


但是一个基本数据类型并不是对象，是不应该有属性的，但是给一个字符串添加一个属性时，也不会报错，在读取属性时，也什么也读取不到。
```javascript
var str = 'chang';
str.name = 'joo';
console.log(str.name);//undefined
```
这是因为，在JS中有三种基本包装类型，Nmber，String，Boolean
例如一个字符串，当作对象去调用其原型上的方法时，JS会临时创建一个与其对应的**基本包装类型的值**，这个值是String的实例，调用方法，其实是这个实例在调用字符串的方法。
```javascript
var str = 'chang';
str.split('');//代码执行到这行时，会临时创建与str等价的对象，去调用字符串的方法
```
这个临时创建的对象的生命周期，只有在调用方法的一瞬间，调用完成后就随即销毁了。因此上例在给字符串添加属性时不会报错，在读取时也读不到销毁的字符串，因此读取时返回undefined。

这样，在用constructor检测数据类型时
```javascript
var str = 'chang';
str.constructor === String;//true
```
检测的其实是临时创建的基本包装类型的对象