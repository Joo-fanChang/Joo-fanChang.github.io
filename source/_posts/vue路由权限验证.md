---
layout: post
title: vue路由权限验证
date: 2018-03-28 14:51:01
tags: javascript vue vue-router 权限验证
categories: web
---
# vue 路由权限验证

> 因为路由是前端配的，本质上是一个页面，此不可能通过后台来做拦截器。

在一个应用中，有用户无限制的访问一些页面，同时，还有一些登录后才有的权限。
比如有这样的路由表
``` javascript
let router = new Router({
    routes: [
        {
            path: '/login',
            name: 'login',
            component: Login
        },
        {
            path: '/desk',
            name: 'desk',
            component: Desk,
            children: [
                {
                    path: 'public',
                    name: 'public',
                    component: Public
                },
                {
                    path: 'admin',
                    name: 'admin',
                    component: Admin
                }
            ]
        },
        {
            path: '/*',
            name: 'index',
            redirect: '/desk/public'
        }
    ]
});
```

这就路由表即是最简单的，有无权限访问页面和需要登录权限才可以访问的页面

可以在路由跳转前进行判断，用户是否有权进行页面的访问，这个是由前端来完成。
除此之外，任何前端的限制都可以被突破。因此在调用登录权限的页面接口时，需要后台做接口验证。

## 前端做的路由拦截
使用`router.beforeEach((to, from, next) => {})`来做验证

首先给需要权限的路由加上
```javascript
meta: {
    requireAuth: true
}
```
然后
```javascript
router.beforeEach((to, from, next) => {
    if (to.meta.requireAuth){
        // 来获取此前设置的requireAuth值
    }
})
```
那么根据什么来判断
一般登录后，后台返回token，前端将此token缓存，如果跳转路由前读取token，有值并且在有效期内，可以进行跳转，如果无效则进行跳转登录页面操作

前端缓存 token的地方，可以是
- vuex
- localStorage
- cookie

## 后台接口的验证

前端在发送所有接口在`request.header`中添加auth字段，携带koken，后台来判断是否可以访问，如果正常则返回数据，否则返回状态码401未登录，前端根据这个状态码来进行登录页面的跳转。

因此做这个操作需要进行对前端接口请求进行拦截，下面是axios[示例代码](https://github.com/changzhn/vue-router-guard/blob/master/src/axios/axios.js)

## 整体示例代码
[git](https://github.com/changzhn/vue-router-guard)

``` bash
# serve with hot reload at localhost:9016
npm run dev
npm run server
```
