---
layout: mindmap
title: HTTP
cate1: Skills
cate2:
description: HTTP 协议
keywords: HTTP 协议, 思维导图, mindmap
---

```mindmap
# HTTP 协议
## HTTP 请求
### 请求行
#### 请求方法
##### GET
##### POST
##### OPTIONS
##### TRACE
##### Others
#### 请求路径(即请求的相对路径)
#### 请求协议
##### HTTP/1.1
##### HTTP/1.0
##### Others
### 请求头信息
#### Host (必须)
#### Cookie （模拟登陆）
#### Content-Type (POST必须)
#### Content-Length (POST必须)
#### Referer (HTTP访问的来源)
#### Others
### 请求主体信息 (POST必须)
#### JSON
#### XML
## HTTP 响应
### 响应行
#### 协议版本
#### 状态码
##### 1XX：接收到请求，继续处理
##### 2XX：请求成功
##### 3XX：请求重定向
###### 302： 请求被临时重定向到另一个地址
###### 304：Not Modified （资源未修改，从本地取缓存）
###### 301： 请求被永久重定向到另一个地址
###### 307： 重定向中保持原有的 post 数据，防止数据丢失
##### 4XX：客户端请求错误
###### 404：没有找到请求文件
###### 403：客户端访问错误
##### 5XX：服务端出现问题
#### 状态文字
##### 对于状态码的文字解释
### 响应头信息
### 响应主体信息
#### 文本
#### 二进制文件
```
