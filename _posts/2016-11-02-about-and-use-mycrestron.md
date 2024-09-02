---
title: "关于 mycrestron 的注册及使用"
date: "2016-11-02"
categories: 
  - "教程"
tags: 
  - "crestron"
  - "demo"
  - "interface"
  - "mycrestron"
  - "simpl-windows"
  - "smart-graphics"
  - "教程"
  - "about-program"
---

> 网站已经改版，另需经销商/员工编号方能注册。

**mycrestron 是什么？**

简单说吧，mycrestron 就是 快思聪 提供的 DDNS 服务，类似花生壳之类的，这样你就不需要记住 IP 地址了，动态 IP 想记也记不住啊，当然，这项服务目前并不收费，而且也并不限制申请数量，不过限制一天最多申请 20 个域名。

**怎么申请域名呢？**

首先，你得有 [Crestron 官网](https://crestron.com) 的帐户，目前我也并不清楚不具备经销商编号及员工编号的帐户能不能申请，如果你没有，那么可以试一下。

1，登录帐户，然后点开右上角的 PRO PANEL: \[caption id="attachment\_641" align="aligncenter" width="1348"\]![PRO PANEL](/assets/images/myCrestron-ProPanel.png) PRO PANEL\[/caption\]

2，找到 [MyCrestron.com Dynamic DNS](https://crestron.com/resources/design-install-tools/my-crestron-dynamic-dns-ddns) ，并打开，页面往下： \[caption id="attachment\_642" align="aligncenter" width="1012"\]![myCrestron Search/Manage Domain](/assets/images/myCrestron-RegBotton.png) myCrestron Search/Manage Domain\[/caption\]

3，域名注册页面： \[caption id="attachment\_643" align="aligncenter" width="760"\]![myCrestron Add a new Subdomain](/assets/images/myCrestron-RegForm.png) myCrestron Add a new Subdomain\[/caption\]

填入相关信息，提交，就完成注册了，可以在 Registered Dynamic DNS 列表上看到你刚刚注册的域名了，正常就是列表的第一个。

**怎么使用呢？**

1，下载 DEMO 程序，（我没有，我要[下载](https://dailyuploads.net/z4f0yn571v4u)），并打开。 2，打开 S-1.1:Crestron Name Resolution Interface v1\_0\_2(cm) ，修改 Domain 及 Password 为你注册时填的信息。 \[caption id="attachment\_644" align="aligncenter" width="570"\]![Crestron Name Resolution Interface](/assets/images/Crestron-Name-Resolution-Interface.png) Crestron Name Resolution Interface\[/caption\]

3，主机请自行更换为你当前使用的主机型号，怎么更换就不提了。 4，按 F12 编译程序，并上传到主机就完成了。 5，可以用 toolbox 的 Simpl Debugger 看看有没有注册上，也可以到 myCrestron 页面查看，如果注册上了会在你申请的域名下面列出主机注册的 IP 地址。

**测试连接** 在 [myCrestron](https://crestron.com/resources/design-install-tools/my-crestron-dynamic-dns-ddns/) 页面的 Registered Dynamic DNS 列表中点击你域名，然后找到 Utilities 进行测试。 \[caption id="attachment\_645" align="aligncenter" width="549"\]![myCrestron Utilities](/assets/images/myCrestron-Utilities.png) myCrestron Utilities\[/caption\]

选择端口，点击 Test Connection 进行测试。

**我换了一台主机怎么办？** 如果域名使用过了，会绑定主机的 mac 地址，传到另一台主机是注册不了的，这时你需要到 测试连接 的那部分（就是上图）提到的页面，点击 Clear Mac Address 按钮，清除记录，然后就能在新的主机上用了。

**其它需要注意的** 1，端口映射要做好，要不然连不上，不知道怎么映射？参考 [快思聪 远程控制 要怎么做呢？](https://www.apcube.com/question/%e5%bf%ab%e6%80%9d%e8%81%aa-%e8%bf%9c%e7%a8%8b%e6%8e%a7%e5%88%b6-%e8%a6%81%e6%80%8e%e4%b9%88%e5%81%9a%e5%91%a2%ef%bc%9f/) 2，主机需要连接外网（就是主机网络设置里面 网关 及 DNS 需要设置，并能正常工作 3，主机连接外网必然就存在安全问题，所以建议关闭普通的 http 连接，采用 ssl ，并打开主机认证功能，确保别人不能随意连上主机。

暂时就这些...
