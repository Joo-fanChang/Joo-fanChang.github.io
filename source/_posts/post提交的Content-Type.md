---
layout: post
title: post提交的Content-Type
date: 2018-03-15 08:50:44
tags: ajax axios content-type ajax
categories: web
---
`jQuery.ajax`的`post`提交默认的请求头的`Content-Type:  application/x-www-form-urlencoded`
而`axios.post`提交的请求头是`Content-Type: application/json`。

`application/json`是一个趋势，但是如果改一个旧项目，把`jQuery.ajax`全部换成`axios.post`时，需要对请求做一些配置。

改之前的代码：
```javascript
// 没有指定请求头的content-type
var data = {age: 18};
$.ajax({
	url: '',
	type: 'POST',
	data: data
	dataType: 'json',
	success: function(result) {
		// do something
	}
})
```

使用`axios`的代码
```javascript
import axios from 'axios';
import qs from 'qs';

var data = {age: 18};
var url = '';

axios.post(
	url,
	qs.stringify(data),
	{headers: {'Content-Type': 'application/x-www-form-urlencoded'}}
).then(result => {
	// do something
})
```