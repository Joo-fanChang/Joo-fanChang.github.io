---
title: nginx
date: 2019-06-28 15:36:30
tags:
---


## 1.0 mac安装
1. brew install nginx
2. 如果出现错误让手动 brew link nginx，并且输入出提示 /usr/local/share/man/man8 is not writable.则输入以下命令
```
sudo chown -R $(whoami):admin /usr/local/share/man/man8
```
3. conf的本地路径在`/usr/local/etc/nginx/nginx.conf`

## 2.0 命令

### 2.1 没有link成功
1. brew services start nginx
2. brew services stop nginx
3. brew services reload nginx

### 2.2 link成功后
1. nginx -t 检查配置文件是否正确
   nginx -t -c /path/to/nginx.conf
2. nginx 启动nginx （以`/usr/local/etc/nginx/nginx.conf`为配置文件 ）
3. nginx -s stop 停止
4. nginx -s reload 重启

## 3.0 配置文件 

| 变量名 | 功能 | 
| ------ | ------ | 
| $host| 请求信息中的Host，如果请求中没有Host行，则等于设置的服务器名 | 
| $request_method | 客户端请求类型，如GET、POST |
| $remote_addr | 客户端的IP地址 | 
|$args | 请求中的参数 | 
|$content_length| 请求头中的Content-length字段 | 
|$http_user_agent | 客户端agent信息 | 
|$http_cookie | 客户端cookie信息 | 
|$remote_addr | 客户端的IP地址 | 
|$remote_port | 客户端的端口 | 
|$server_protocol | 请求使用的协议，如HTTP/1.0、·HTTP/1.1| 
|$server_addr| 服务器地址 | 
|$server_name| 服务器名称| 
|$server_port`|服务器的端口号|

### 3.1 错误日志
```conf
error_log  /Users/changzhn/repo/nginx/logs/error.log info;
```

级别：常见的错误日志的级别有 debug|info|notice|warn|error|crit|alert|emerg 级别越高，记录的信息越少
生产场景一般是 warn|error|crit三个级别

### 3.2 自定义静态文件目录位置
```conf
location / {
  root   /Users/changzhn/repo/nginx/html;

  # 用于配合 browserHistory使用
  try_files $uri $uri/ /index.html;
}
```
并且匹配其他路由匹配不到时指定返回页面，如果是SPA则返回index.html，如果是其他多页应用，可以返回指定的404页面

### 3.3 根据不同的userAgent跳转到pc页面还是mobile页面
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.131 Safari/537.36

User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1

```conf
 location / {
    if ( $http_user_agent ~ "(MIDP)|(WAP)|(UP.Browser)|(Smartphone)|(Obigo)|(Mobile)|(AU.Browser)|(wxd.Mms)|(WxdB.Browser)|(CLDC)|(UP.Link)|(KM.Browser)|(UCWEB)|(SEMC-Browser)|(Mini)|(Symbian)|(Palm)|(Nokia)|(Panasonic)|(MOT-)|(SonyEricsson)|(NEC-)|(Alcatel)|(Ericsson)|(BENQ)|(BenQ)|(Amoisonic)|(Amoi-)|(Capitel)|(PHILIPS)|(SAMSUNG)|(Lenovo)|(Mitsu)|(Motorola)|(SHARP)|(WAPPER)|(LG-)|(LG/)|(EG900)|(CECT)|(Compal)|(kejian)|(Bird)|(BIRD)|(G900/V1.0)|(Arima)|(CTL)|(TDG)|(Daxian)|(DAXIAN)|(DBTEL)|(Eastcom)|(EASTCOM)|(PANTECH)|(Dopod)|(Haier)|(HAIER)|(KONKA)|(KEJIAN)|(LENOVO)|(Soutec)|(SOUTEC)|(SAGEM)|(SEC-)|(SED-)|(EMOL-)|(INNO55)|(ZTE)|(iPhone)|(Android)|(Windows CE)|(Wget)|(Java)|(curl)|(Opera)" )
        {
            #跳转到 m.duweixin.net
            rewrite ^.+ /mobile last; 
        }

    # 用于配合 browserHistory使用
    try_files $uri $uri/ /index.html;
}

location /mobile {
    # 手机页面
    try_files $uri /mobile.html;
}
```

TIPS：大括号里面最后都要加分号，注释最好写到每行的上面

### 3.4 gzip压缩

```
{
    # gzip config
    gzip on;
    gzip_min_length 1k;
    gzip_comp_level 9;
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    # 根据浏览器是否支持压缩
    gzip_vary on;
    # ie1-6不要压缩
    gzip_disable "MSIE [1-6]\.";
}
```

### 3.5 location
匹配的几个规则
- ~ 表示正则匹配，区分大小写
- ~* 表示正则匹配，不区分大小写
- ^~ 表示普通字符匹配，如果该选项匹配则不匹配其他选项，一般用来匹配目录
- = 精确匹配
- @ 

location匹配的优先级别(与书写的顺序没有关系)
1. = 精确匹配会第一个被处理。如果发现精确匹配，nginx停止搜索其他匹配。
2. 普通字符匹配，正则表达式规则和长的块规则将被优先和查询匹配，也就是说如果该项匹配还需去看有没有正则表达式匹配和更长的匹配。
3. ^~ 则只匹配该规则，nginx停止搜索其他匹配，否则nginx会继续处理其他location指令。
4. 最后匹配理带有"~"和"~*"的指令，如果找到相应的匹配，则nginx停止搜索其他匹配；当没有正则表达式或者没有正则表达式被匹配的情况下，那么匹配程度最高的逐字匹配指令会被使用。

### 3.5.1
location / {
    root /usr/share/nginx/html;
}


### 3.5.2
clientUrl: 
```
/api/get1
```
nginx:
```
location /api {
    proxy_pass http://proxy.com;
}
```
result:
请求的服务器的真实路径为 `http://proxy.com/get1`
tip:
- 如果location后面的路径写成 `/api/` 那么proxy_pass的路径要写成 `http://proxy.com/`，相当于把匹配的路径截取然后拼接到代理地址上


tip:
- root后面的路径加不加`/`都是一样的效果，就等同于 `/usr/share/nignx/html/;`
- 比如：
clientUrl:
```
hello-abc/get1
hello-bcd/get2
```
nginx:
```conf
location /hello {
    proxy_pass http://proxy.com/hello;
}
```

### 3.5.3
clientUrl:
```
/api/get1
/api/get2
```
nginx:
```conf
location /api {
    # 错误的代理地址
    proxy_pass https://easy-mock.com/mock/5b0f43793ba2c72d1aec2dbf/example/xxx;
}

location =/api/get2 {
    proxy_pass https://easy-mock.com/mock/5b0f43793ba2c72d1aec2dbf/example/get2;
}
```
result:
1. get1失败
2. get2成功

说明：
使用了`=`优先级比普通匹配要高，所以`/api/get2`走的是下面的正确代码的匹配。