---
title: "L2TP+IPSec一键安装脚本"
date: "2015-08-29"
categories: 
  - "硬件接口"
  - "网络"
tags: 
  - "g-f-w"
  - "ipsec"
  - "l2tp"
  - "tcpip"
  - "vpn"
---

用VPS在墙上打洞还有一种叫L2TP，也是常见的一种方式。本脚本结合了L2TP（Layer 2 Tunneling Protocol）和[IPSec](http://zh.wikipedia.org/wiki/IPsec)（Internet Protocol Security）,安装的软件包版本分别是openswan-2.6.38、xl2tpd-1.2.4，和PPTP的不同之处请戳[这里](http://baike.baidu.com/view/32692.htm)查看。 同样要保证你的VPS是在外面的自由世界中，且VPS是基于Xen或KVM的。 注意：基于 OpenVZ 虚拟化技术的 VPS 需要开启TUN/TAP才能正常使用，购买 VPS 时请先咨询服务商是否支持开启 TUN/TAP。 检测是否支持TUN模块 执行命令：

```
cat /dev/net/tun
```

如果返回信息为：cat: /dev/net/tun: File descriptor in bad state 说明正常 检测是否支持ppp模块 执行命令：

```
cat /dev/ppp
```

如果返回信息为：cat: /dev/ppp: No such device or address 说明正常

终端里运行以下命令（CentOS）：

```
cd /root
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/across/master/l2tp.sh
chmod +x l2tp.sh
./l2tp.sh

```

终端里运行以下命令（Ubuntu）：

```
cd /root
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/across/master/l2tp_ubuntu.sh
chmod +x l2tp_ubuntu.sh
./l2tp_ubuntu.sh

```

执行后会要求输入一些信息，如「Please input IP-Range:」意为输入本地IP段范围（本地电脑连接到VPS后给分配的一个本地IP地址），直接回车意味着输入默认值10.1.2；「Please input PSK:」PSK意为预共享密钥，即指定一个密钥将来在连接时需要用到，直接回车意味着输入默认值vpn。 输入了IP段范围和PSK之后，程序会显示你的VPS当前的IP（IPV4）、L2TP的本地IP（默认的话是10.1.2.1）、分配给客户端的IP段（默认的话是10.1.2.2-10.1.2.254）以及你所设置的PSK（默认的话是vpn），确认无误后，按任意键，程序便会开始自动配置。 安装完之后，会显示VPS当前的IP「ServerIP:VPS当前公网IP」、默认用户名「username:vpn」、默认用户名的密码「password:随机生成的6位字符串」、预共享密钥「PSK:你所设置的PSK，如果你之前没有设置则为默认值vpn」。

出现如下图所示（下列图片来源于[vpsyou.com](http://www.vpsyou.com/2010/10/04/l2tp-vpn.html)）则说明OK了。 ![l2tp_1](/assets/images/l2tp_1.jpg) 要想增加用户怎么办呢？很简单，用任一文本编辑器打开/etc/ppp/chap-secrets，按照其中既有的用户格式添加即可。 客户端设置（以 Windows XP 为例）： 创建一个VPN新建连接向导：

![l2tp_2](/assets/images/l2tp_2.jpg)

![l2tp_3](/assets/images/l2tp_3.jpg)

公司名可以任意指定 ![l2tp_4](/assets/images/l2tp_4.jpg)

![l2tp_5](/assets/images/l2tp_5.jpg)

![l2tp_6](/assets/images/l2tp_6.jpg)

![l2tp_7](/assets/images/l2tp_7.jpg)

![l2tp_8](/assets/images/l2tp_8.jpg)

![l2tp_9](/assets/images/l2tp_9.jpg)

设置完成后一般即可顺利连接。注意：XP 用户：请确保“控制面板->管理工具->服务”中的 IPSEC 服务被启动。

 

本文转自: [秋水逸冰](http://teddysun.com/ "秋水逸冰") 的 [L2TP+IPSec一键安装脚本](https://teddysun.com/135.html "L2TP+IPSec一键安装脚本")
