---
title: "Force Download From Oss"
date: 2021-07-27T14:02:11+08:00
draft: false
summary: "Force Download From Oss"
categories:
  - "OSS"
tags:
  - "content disposition"
---

# 起因

前端通过OSS提供的链接地址下载资源，结果浏览器并没有弹出保存窗口而是把图片、视频等直接打开了。

# 解决

OSS资源链接地址加```response-content-disposition=attachment```强制下载。

```
https://oss.host/some/resource?response-content-disposition=attachment
```