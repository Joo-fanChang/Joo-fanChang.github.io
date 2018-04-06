---
layout: post
title: Web前端实用知识点
date: 2018-03-19 15:02:14
tags: html css javascript web前端
categories: web
---

## HTML
#### `<form>`
当`method`属性值为`POST`时，`enctype`是提交给服务器的内容的`MIME`类型，可能的取值有：
- `application/x-www-form-urlencoded`：如果属性未指定的默认值
- `nultipart/form-data`：这个值用于一个type属性设置为file的`<input>`元素
- `text/plain`(HTML5)
- `application/json`

#### 判断IE
```html
<!--[if IE 6]><html class="ie lt-ie8"><![endif]-->
<!--[if IE 7]><html class="ie lt-ie8"><![endif]-->
<!--[if IE 8]><html class="ie ie8"><![endif]-->
<!--[if IE 9]><html class="ie ie9"><![endif]-->
<!--[if !IE]><!--> <html> <!--<![endif]-->
```



## CSS
#### 省略号：
```css
max-width: ;
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```

#### 渐变
```css
background: -ms-linear-gradient(top,#463D89,  #362D7B); /* IE 10 */
background: -moz-linear-gradient(top,#463D89,#362D7B); /*火狐*/
background: -webkit-gradient(linear, 0% 0%, 0% 100%,from(#463D89), to(#362D7B)); /*谷歌*/
background: -webkit-gradient(linear, 0% 0%, 0% 100%, from(#463D89), to(#362D7B)); /* Safari 4-5, Chrome 1-9*/
background: -webkit-linear-gradient(top, #463D89, #362D7B); /*Safari5.1 Chrome 10+*/
background: -o-linear-gradient(top, #463D89, #362D7B); /*Opera 11.10+*/
filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#463D89', endColorstr='#362D7B',GradientType=0); /*IE9*/
```

### Font
#### familyfont
- 小程序的
```css
 font-family: -apple-system-font,Helvetica Neue,Helvetica,sans-serif;
```


#### icon-font
```css
@font-face {font-family: "iconfont";
  src: url('iconfont.eot?t=1500862206137'); /* IE9*/
  src: url('iconfont.eot?t=1500862206137#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('iconfont.woff?t=1500862206137') format('woff'), /* chrome, firefox */
  url('iconfont.ttf?t=1500862206137') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
  url('iconfont.svg?t=1500862206137#iconfont') format('svg'); /* iOS 4.1- */
}

.iconfont {
  font-family:"iconfont" !important;
  font-size:16px;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-usericon:before { content: "\e676"; }
```

#### 媒体查询
```css
@media screen and (min-width: 800px) and (max-width: 1000px) {
    #box {
        background-color: #00A5FF;
        width: 100px;
        height: 100px;
    }
}
```

#### flexbox
```css
.flex-box {
	display: -webkit-box; /* Chrome 4+, Safari 3.1, iOS Safari 3.2+ */
	display: -moz-box; /* Firefox 17- */
	display: -webkit-flex; /* Chrome 21+, Safari 6.1+, iOS Safari 7+, Opera 15/16 */
	display: -moz-flex; /* Firefox 18+ */
	display: -ms-flexbox; /* IE 10 */
	display: flex; /* Chrome 29+, Firefox 22+, IE 11+, Opera 12.1/17/18, Android 4.4+ */
}
```
#### IOS上光标不显示
```css
// 因为加了这个属性
user-select: none;
```




## JS
#### encodeURIComponent
使用ajax向台发get请求，将中文字符使用这个进行编码处理。后台再进行相应处理
注：
jquery ajax和axios都已对中文字符进行了编码处理

#### 正则
- IP`/^((25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))\.){3}(25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))$/`
- 端口`/^([0-9]|[1-9]\d{1,3}|[1-5]\d{4}|6[0-5]{2}[0-3][0-5])$/`
- IP + 端口(可以不带端口)`/^((25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))\.){3}(25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))(:([0-9]|[1-9]\d{1,3}|[1-5]\d{4}|6[0-5]{2}[0-3][0-5]))?$/`
- IP + 端口(必须带端口)`/^((25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))\.){3}(25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))(:([0-9]|[1-9]\d{1,3}|[1-5]\d{4}|6[0-5]{2}[0-3][0-5]))$/`
- 域名`/((https|http|ftp|rtsp|mms):\/\/)?(([0-9a-z_!~*'().&=+$%-]+:)?[0-9a-z_!~*'().&=+$%-]+@)?(([0-9]{1,3}\.){3}[0-9]{1,3}|([0-9a-z_!~*'()-]+\.)*([0-9a-z][0-9a-z-]{0,61})?[0-9a-z]\.[a-z]{2,6})(:[0-9]{1,4})?((\/?)|(\/[0-9a-z_!~*'().;?:@&=+$,%#-]+)+\/?)/`
- 中文正则`/[\u4e00-\u9fa5]/`


#### web注销后浏览器回退
第一种方式：
```javascript
window.onpopstate = function() {
 window.location.reload()
};

history.pushState(null, null)
```
利用浏览器后退按钮，让该页面再刷新，就回不到那个页面

第二种方式：
replace会替换history对象中的记录
```javascript
window.location.replace('https://www.baidu.com');
```



#### src的绝对路径转base64
```javascript
var img = "https://ncc-ys-prod-oss.oss-cn-beijing.aliyuncs.com//a2e6a30c-eb66-4d4b-bded-65b109d74f2e";
function getBase64Image(img) {
    var canvas = document.createElement("canvas");
    canvas.width = img.width;
    canvas.height = img.height;

    var ctx = canvas.getContext("2d");
    ctx.drawImage(img, 0, 0, img.width, img.height);
    var ext = img.src.substring(img.src.lastIndexOf(".")+1).toLowerCase();
    var dataURL = canvas.toDataURL("image/"+ext);
    return dataURL;
}
var image = new Image();
image.crossOrigin = '';
image.src = img;
image.onload = function(){
    var base64 = getBase64Image(image);
    console.log(base64);
}
```



#### getQueryString
```javascript
function getQueryString(name) {
	var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
	var r = window.location.search.substr(1).match(reg);
	if(r!=null)return decodeURIComponent(r[2]); return "";
}
```

### AJAX
#### 上传图片预览
```javascript
  function getObjectURL(file) {
    var url = null;
    if (window.createObjectURL != undefined) { // basic
        url = window.createObjectURL(file);
    } else if (window.URL != undefined) { // mozilla(firefox)
        url = window.URL.createObjectURL(file);
    } else if (window.webkitURL != undefined) { // webkit or chrome
        url = window.webkitURL.createObjectURL(file);
    }
    return url;
}
```

#### 文件上传
```javascript
var formData = new FormData();
var file = $('#file')[0].files[0];
formData.append('file', file);

$.ajax({
    url: url,
    type: 'POST',
    data: formData,
    contentType: false,
    cache: false,
    processData: false,
	...
})
```

#### 文件下载
```javascript
function download(href, filename) {
	var a = document.createElement('a');
	a.setAttribute('target', '_blank');
	a.setAttribute('href', href);
	a.setAttribute('download', filename);
	a.click();
}
```
#### jquery ajax 超时
```javascript
$.ajax({
	url: '/rest/v1/checkVersion',
	type: 'POST',
	dataType: 'json',
	timeout: 5000,
	success: function(result) {
		if (result.flag == 1) {
			callback(result.cur_version, result.data);
		} else {
			showMessage('当前已是最新版本，当前版本号为：' + result.cur_version);
		}
	},
	error: function(xhr, textStatus) {
		if (textStatus == 'timeout') {
			showMessage('请求服务器超时，请联系管理员')
		} else {
			showMessage('发生错误，请联系管理员')
		}
	}
})
```
#### request header content-type
[来自小程序开发](https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-request.html#wxrequestobject)
- 对于 GET 方法的数据，会将数据转换成 query string（encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）
- 对于 POST 方法且 header['content-type'] 为 application/json 的数据，会对数据进行 JSON 序列化
- 对于 POST 方法且 header['content-type'] 为 application/x-www-form-urlencoded 的数据，会将数据转换成 query string （encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）

小程序的用法
```javascript
wx.request({
	url: '',
	...
	header: {
		'content-type': 'application/json'//'application/x-www-form-urlencoded'
	}
})
```

原生设置请求头
```javascript
xmlhttp.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
xmlhttp.setRequestHeader("Content-Type","application/json; charset=utf-8");
```

使用axios设置请求头
```javascript
axios.post(url, data, {headers: {'Content-Type': 'application/json'}})...
```

#### axios timeout
```javascript
import axios from 'axios'
axios.defaults.timeout = 5000;
```

#### 使用JS关闭当前窗口
```javascript
function closePageForm(){
  window.opener=null;
  window.open('','_self');
  window.close();
}
```

### Date
#### yyyy-mm-dd
```javascript
function getNowFormatDate() {
	var date = new Date();
	var seperator1 = "-";
	var year = date.getFullYear();
	var month = date.getMonth() + 1;
	var strDate = date.getDate();
	if (month >= 1 && month <= 9) {
		month = "0" + month;
	}
	if (strDate >= 0 && strDate <= 9) {
		strDate = "0" + strDate;
	}
	var currentdate = year + seperator1 + month + seperator1 + strDate;
	return currentdate;
}
```


### Bootstrap
#### Bootstra validator的重置
```javascript
$('#uploadForm').bootstrapValidator('resetForm', true);
```

```javascript
//Modal验证销毁重构
$('#editModal').on('hidden.bs.modal', function() {
	$("#saveadmin_form").data('bootstrapValidator').destroy();
	$('#saveadmin_form').data('bootstrapValidator', null);
	formValidator();
});
```

#### 验证
```javascript
var bootstrapValidator = $('#editPassword').data('bootstrapValidator');
//手动触发验证
bootstrapValidator.validate();
//校验通过
if (bootstrapValidator.isValid()) {

}
```

#### 基本配置
- 匹配不相同 different
- 匹配相同 identical
```javascript
$('#editPassword').bootstrapValidator({
    message: 'This value is not valid',
    fields: {
        oldPassword: {
            message: '验证失败',
            validators: {
                notEmpty: {
                    message: '不能为空'
                },
                stringLength: {
                    min: 6,
                    max: 20,
                    message: '请输入至少6位，最多20位密码'
                }
            }
        },
        newPassword: {
            message: '验证失败',
            validators: {
                notEmpty: {
                    message: '不能为空'
                },
                stringLength: {
                    min: 6,
                    max: 20,
                    message: '请输入至少6位，最多20位密码'
                },
                callback: {
                    message: 'This value is not valid',
                    callback: function(value, validator, field) {
                        if (value != "" && value == $("#oldPassword").val()) {
                            return {
                                valid: false,    // or false
                                message: '新密码不能与旧密码相同'
                            }
                        }
                        return true;
                    }
                },
                identical: {
                    field: 'newPassword',
                    message: '两次输入的密码不一致'
                }
            }
        },
        confirmPassword: {
            message: '验证失败',
            validators: {
                notEmpty: {
                    message: '不能为空'
                },
                stringLength: {
                    min: 6,
                    max: 20,
                    message: '请输入至少6位，最多20位密码'
                },
                identical: {
                    field: 'newPassword',
                    message: '两次输入的密码不一致'
                }
            }
        }
    }
});
```


## VUE
#### 不要使用箭头函数
> 不要在选项属性或回调上使用箭头函数，比如 `created: () => console.log(this.a)`或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，且 this.a 或 this.myMethod 也会是未定义的。