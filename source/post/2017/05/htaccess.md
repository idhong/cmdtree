---
title: .htaccess常用跳转规则
date: 2017-05-19T16:56:53+00:00
category: 使用笔记
---
## 301跳转规则

必须运行的是apache服务器

下文写法是将不带www的跳转到带www的网址

具体的写法如下 大家可以参考

自行修改其中的url即可 如果不行 请检查服务器环境
```
Options +FollowSymLinks
rewriteEngine on
rewriteCond %{http_host} ^fanzhiyang.com [NC]
rewriteRule ^(.*)$ http://www.fanzhiyang.com/$1 [R=301,L]
```


## 404规则

```
ErrorDocument 404 /404.html
```

如果上面的规则不生效，使用
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /404.html [L]
```

## 非HTTPS跳转至HTTPS规则

```
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://www.domain.com/$1 [R,L]
```
