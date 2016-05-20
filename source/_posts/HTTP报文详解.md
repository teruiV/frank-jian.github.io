---
title: HTTP报文详解
date: 2016-05-17 12:15:05
tags: [HTTP]
categories: HTTP
---
######  Http协议通用首部字段：
```
Cache-Control  控制缓存行为
Connection 控制不再转发给代理的首部字段、管理持久连接
Date 创建报文的日期时间
Pragma 报文指令
Trailer 报文末端的首部一览
Transfer-Encoding 指定报文主体的传输编码方式
Upgrade 升级为其他协议
Via 代理服务器相关信息
Warning 错误通知
```
###### Http请求首部字段：
```
Accept 用户代理可处理的媒体类型
Accept-Charset 优先的字符集
Accept-Encoding 优先的内容编码
Accept-Language 优先的语言
Authrization Web认证信息
Expect 期待服务器的特定行为
From 用户的电子邮箱地址
Host 请求资源所在服务器
If-Match 比较实体标记(Etag)
If-Modified-Since 比较资源的更新时间
if-None-Match 比较实体标记(与If-Match相反)
If-Range 资源未更新时发送实体Byte的范围请求
If-Unmodified-Since 比较资源的更新时间
Max-forwards 最大传输逐跳数
Proxy-Authorization 代理服务器要求客户端的认证信息
Range 实体的字节范围请求
Referer 对请求中的URI的原始获取方
TE 传输编码的优先级
User-Agent HTTP客户端程序的信息
Cookie 服务器接收到的Cookie信息
```
###### Http响应首部字段：
```
Accept-Ranges 是否接受字节范围
Age 推算资源创建经过时间
Etag 资源匹配信息
Location 令客户端重定向至指定URI
Proxy-Authenticate 代理服务器对客户端的认证信息
Retry-After 对再次发起请求的时机要求
Server HTTP服务器的安装信息
Vary 代理服务器缓存的管理信息
WWW-Authenticate 服务器对客户端的认证信息
Set-Cookie 开始状态管理所使用的Cookie信息
```
###### 实体首部字段：
```
Allow 资源可支持的HTTP方法
Content-Encoding 实体主体适用的编码方式
Content-Length 实体主体的自然语言
Content-Location 替代对应资源的URI
Content-MD5 实体主体的报文摘要
Content-Range 实体主体的位置范围
Content-Type 实体主体的媒体类型
Expires 实体主体的过期时间
Last-Modified 资源的最后修改时间
```
###### 为Cookie服务的首部字段
```
Set-Cookie
Cookie
```
###### 请求和响应报文的组成
请求报文
```
请求报文由报文首部+空行（CR+LF）+报文主体构成；
报文首部由：请求行+ 请求首部字段+通用首部字段+实体首部字段+其他
```
响应报文
```
响应报文由报文首部+空行（CR+LF）+报文主体构成；
报文首部由：状态行+ 响应首部字段+通用首部字段+实体首部字段+其他
```
