---
title: "ocserv(Cisco AnyConnect 兼容服务端) 一键安装脚本 for CentOS 7"
date: "2015-08-30"
categories: 
  - "硬件接口"
  - "网络"
tags: 
  - "anyconnect"
  - "centos7"
  - "g-f-w"
  - "ocserv"
  - "tcpip"
  - "vpn"
---

CentOS 6 的 repository 里的 gnutls、nettle 等版本比 ocserv 要求的版本低，自己编译较为麻烦。 RedHat 最近发布了 RHEL 和 CentOS 的最新版本，repository 的 gnutls 和 nettle 均已符合 ocserv 的版本要求，所以部署 ocserv 方便了很多。

这是 ocserv 在 CentOS 7 和 RHEL 7 的一键安装脚本，可以在最小化安装环境的 CentOS 7 和 RHEL 7 下一键部署 ocserv。

已知部分 64M 内存的 VPS 一次 yum 太多软件包会报错，可以修改脚本分多次安装。 OpenVZ 的 VPS 需要开启 tun 支持。 另外建议 Android 使用 S-hadow-sock-s，比 AnyConnect 方便而且防污染做得更好。 AnyConnect 推荐用于 iOS 和需要系统路由的 PC。

须知：

- 支持自动判断防火墙，请确保 Firewalld 或者 iptables 其中一个是 active 状态；
- 默认采用用户名密码验证，本安装脚本编译的 ocserv 也支持 pam 验证，只需要修改配置文件即可；
- 默认配置文件在 /usr/local/etc/ocserv/ 目录，可自行更改脚本里的参数；
- 安装时会提示你输入端口、用户名、密码等信息，也可直接回车采用默认值，密码是随机生成的；
- 安装脚本会关闭 SELINUX，不知 SELINUX 是否会影响 ocserv；
- 自带路由表，只有路由表里的 IP 才会走 VPN，如果你有需要添加的路由表可自行添加，最多支持 200 条路由；
- 如果你有证书机构颁发的证书，可以把证书放到脚本的同目录下，确保文件名和脚本里的匹配，安装脚本会使用你的证书，客户端连接时不会提示证书错误；
- 配置文件修改为单 IP 允许 10 个连接，全局 1024 个连接，可修改脚本前面的变量。1024 个连接大约需要 2048 个 IP，所以虚拟接口的 IP 配置了 8 个 C 段。

脚本下载：[github](https://github.com/travislee8964/Ocserv-install-script-for-CentOS-RHEL-7)

 

本文来自: [stunnel.info](https://www.stunnel.info/ocserv-cisco-anyconnect-%E5%85%BC%E5%AE%B9%E6%9C%8D%E5%8A%A1%E7%AB%AF-%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC-for-centos-7/)
