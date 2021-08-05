---
title: "Nginx Replace Proxy Error"
date: 2021-07-30T13:31:56+08:00
draft: false
summary: "Nginx Replace Proxy Error"
categories:
  - "Nginx"
tags:
  - "proxy"
  - "intercept error"
---

# 起因

Nginx反向代理的时候，默认不使用Nginx的错误页面，即上游返回什么错误数据，Nginx就展示什么数据

# 解决

```
...
server {
    ...
    location / {
        ...
        proxy_intercept_errors on;
        ...
    }
    ...

    error_page   404  /404.html;
    location = /404.html {
       root /var/www/html;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
       root /var/www/html;
    }
}

...
```

fastcgi也有类似设置

```
fastcgi_intercept_errors on;
```