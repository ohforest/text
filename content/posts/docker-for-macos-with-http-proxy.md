---
title: "Docker for Mac With Http Proxy"
date: 2021-07-27T13:18:20+08:00
draft: false
summary: "Docker for Mac With Http Proxy"
categories:
  - "Docker"
tags:
  - "proxy"
---

# 起因

Docker for Mac 代理设置成 ```127.0.0.1:8118```, 出现如下错误：

```Error response from daemon: Get https://index.docker.io/v1/search?q=redis&n=25: proxyconnect tcp: dial tcp 127.0.0.1:8118: connect: connection refused```

# 原因
Docker for Mac 是运行在docker machine虚拟机中的，127.0.0.1指向本机地址自然会报connection refused。

# 解决
Docker for Mac 代理设置成 ```http://宿主机ip:8118```。

# 延伸
宿主机ip在更换网络或dhcp过期释放等情况下会改变，这样每次都要重新设置proxy并重启docker，为网卡设置固定alias ip，proxy设置成固定ip解决。

```sudo ifconfig en0 alias 192.16.8.8 255.255.255.0```
