---
title: "Teamviewer VPN 连公司内网"
date: "2019-06-05"
categories: 
  - "get-something"
  - "walking-cloud"
tags: 
  - "teamviewer"
  - "vpn"
  - "windows"
---

Teamviewer 远程桌面的确不错，但如果网速慢的时候，就远程不上，这时候teamviewer 的 VPN 拨号就起作用了，网上有很多通过 ccproxy 代理来实现连公司内网的，这种 ccproxy 实现方法实在是最多的实现方法。

公司网络 10.10.10.X

本机IP   3G 网络(公网 IP)

**原理：**

1. teamviewer VPN 拨号后，得到虚拟 IP（主端为虚拟 IPA，拨号端为IPB），一般是 7.X.X.X, 以7开头的IP段
2. 再通过 windows 自带的 VPN 服务，第二次拨号，对 IPA 进行 VPN 拨虚，分配公司 IP 段（10.10.10.X），这样，通过第二次拨号，拨号端就有了内网IP。

![](images/sb1f59gtly.png)

**步骤：**

1. 在公司电脑上与拨号电脑上都安装 teamviewer，并选择 VPN 模块

![](images/k5ngun7u0u.png)

2.在公司电脑上设置 windows 的 VPN 服务

a.开启服务 Routing and Remote Access.

![](images/8f2xqvowlz.png)

b.开启 VPN 服务

![](images/z286vn8hqz.png)

![](images/4nyit5i107.png)

3.第三步开始是在拨号电脑上操作

a.Teamviewer 拨号时，选择 VPN

![](images/ntgr2l7blw.png)

b.拨上后，得到虚拟 IP，如下面的 7.236.34.213

![](images/4yj9g9sbgw.png)

4.第四步，第二次拨号，在拨号机上新建 VPN 连接，配置如下：

![](images/j2i7i55i1t.png)

注意：TCP/IP 高级设置时：“在远程网络上使用默认网关不要点选”

5.连上 window 自带 VPN，连上内网

> 原文链接： [https://cloud.tencent.com/developer/article/1143175](https://cloud.tencent.com/developer/article/1143175)
> 
> 可以试试咯。
