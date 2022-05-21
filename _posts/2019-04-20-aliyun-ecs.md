---
layout: post
title: 云服务器 ECS 的使用方法
categories: [ECS]
description: 云服务器 ECS 的使用方法
keywords: [ECS]
---

### 阿里云云服务器 ECS

1. 购买 ECS 实例，进入控制台，进入该实例，本实例安全组，添加安全组规则。这一步就是给实例 IP 开放一些端口（如 http: 80 / https: 443）。因为下面要用到的宝塔面板默认端口是 8888，所以这一步必须开放 8888 端口，否则无法访问面板。

2. 修改系统盘镜像，详见[如何修改](https://jingyan.baidu.com/article/64d05a021f1ba6de55f73ba1.html)，我用的是宝塔 Linux 面板。

3. 通过 ip:8888 进入面板（端口默认 8888，可修改），安装必要的软件，包括 Nginx，MySQL，PHP，phpMyAdmin 等。

4. 在面板中添加网站，数据库，FTP 等。

5. 已备案的域名添加 DNS 解析。解析到当前 ECS 的公网 ip。
