---
title: "快思聪主机通过 HTTP 方式控制网络设备的一些想法"
date: "2015-07-27"
categories: 
  - "硬件接口"
  - "程序"
tags: 
  - "80"
  - "get"
  - "http"
  - "tcpip"
---

为什么通过浏览器发送控制命令可以控制设备, 而主机程序里面 TCP/IP Client 发送的命令却控制不了设备, 正好前几天在折腾的时候碰到类似事情.

通过 TCP/IP Client 连接设备状态正常, 发送命令总是返回错误, 接着就断开连接, 一直试验了好几次都是一样的状况. 我用 tcp test tool 连接 www.sha1-online.com 做为服务器测试, 网站运行正常, 因此通过 80 端口连接肯定也是正常的, 发送命令: `GET / HTTP/1.1` 注意: GET, POST 及 HTTP 都必须大写, 并且命令后面需要 2 行空行. 返回的信息: `HTTP/1.1 400 Bad Request Content-Type: text/html Content-Length: 349 Connection: close Date: Mon, 27 Jul 2015 01:55:59 GMT Server: ymag` 最后通过网络抓包及浏览器带的开发工具找到了原因所在, 原来是因为浏览器在每次发送 GET/POST 请求时都会附带发送一些本地设备的信息, 而服务器需要识别这些信息然后发回相应执行结果. 浏览器发送的完整命令: `GET / HTTP/1.1 Host: www.sha1-online.com Connection: keep-alive Cache-Control: max-age=0 Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8 User-Agent: Mozilla/5.0 (Windows NT 6.3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.7 Safari/537.36 HTTPS: 1 Accept-Encoding: gzip, deflate, sdch Accept-Language: zh-CN,zh;q=0.8,en-GB;q=0.6,en;q=0.4` 返回的信息: `HTTP/1.1 200 OK`及其它的代码.

 

发现在请求过程中包含了其它的一些信息, 因些我们在发送的时候也是需要发送附加的一些信息给服务器, 服务器才会做出正确的反应.

希望能帮到你解决问题.
