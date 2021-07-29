---
title: "Systemd Template Unit Files"
date: 2021-07-20T12:47:40+08:00
draft: false
summary: "Systemd: Template unit files"
categories:
  - "Systemd"
tags:
- "template"
---
# [名称]
Systemd: Template unit files

# [功效]
多个单元复用同一模版。

# [成分]
```
<service_name>@<argument>.service
```

模版文件中饮用argument的两种形式
- %i (```/``` to ```-```, ``` ```to```\x20```)
- I% (```-```to```/```)

例如 /etc/path/to/some-conf
- %i: -etc-path-to-some-conf
- %I: /etc/path/to/some/conf

# [用法]
PPPOE
```
[Unit]
Description=PPP connection for %I
Before=network.target

[Service]
ExecStart=/usr/bin/pon %I
Restart=always
RestartSec=10
StartLimitInterval=0
ExecStop=/usr/bin/poff %I

[Install]
WantedBy=multi-user.target
```
```
#systemctl start ppp@dsl-provider
```