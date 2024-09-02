---
title: "HDMI 中 CEC 是什么？"
date: "2014-12-11"
categories: 
  - "硬件接口"
tags: 
  - "cec"
  - "hdmi"
  - "what-is-it"
---

### HDMI 传输原理

HDMI (High-Definition Multimedia Interface)，又被称为高清晰度多媒体接口，是首个支持在单线缆上传输，不经过压缩的全数字高清晰度、多声道音频和智能格式与控制命令的数字接口。

HDMI 采用 TMDS(Time Minimized Differential Signal) 最小化传输差分信号传输技术，TMDS是一种微分信号机制，采用的是差分传动方式，是一种利用2个引脚间电压差来传送信号的技术。

HDMI 包括了4条差分线缆以及 DDC，CEC 等线缆，其中4条差分线缆传输的是3组TMDS数据信息以及1组TMDS时钟信息，每个时钟周期内，每条数据通道都能传输10bit的数据。

HDMI 把视频信号分为 R、G、B、H、V 五种信号，采用 TMDS 差分信号传输技术进行编码，其中： TMDS：这三个通道传输R、G、B三原色，HV编码在B信号通道里面传输，R、G通道的多余位置用来传输音频信号。 DDC：Display Data Channel，即显示数据通道，用来向视频接收装置发送配置信息和数据格式信息，接收装置读取这些 E-EDID（Enhanced Extended Display Identification Data，即增强扩展显示识别数据）的信息；其中，EDID 信息中包含着 HDMI Network 中每个设备的唯一物理地址，EDID由 source 或 repeater 设备从 sink 或 repeater 设备处读取。 CEC: Consumer Electronics Control，即消费电子控制通道，通过这条通道可以控制 HDMI CEC Network 上的设备之间的相互交互和控制。 HPD:Hot Plug Detect，即热拔插查询控制，用于控制是否让下端设备读取DDC数据。

### CEC 的控制原理

CEC 是一个基于总线系统的协议，它是通过 Physical Address Discovery Process 机制来分配物理地址，用DDC来分配物理地址到设备。

当一个带CEC功能的设备得到一个新的物理地址（非F.F.F.F）时，它将做一下两步： 1.主动申请分配与之设备类型相应的逻辑地址； 2.通过广播（Report Physical Address）来报告它的物理地址与逻辑地址的绑定。

这个处理允许任何一个设备来创造一个物理地址到逻辑地址的映射。一个设备通过一个逻辑地址来表示一个功能，如果一个物理设备包含有超过一个设备类型的功能，它将给每个功能分配一个逻辑地址。

**设备功能的逻辑地址的分配有数量和特定地址的限制**

<table border="1" cellspacing="0" cellpadding="0"><tbody><tr><td width="277"><strong>地址</strong></td><td width="277"><strong>设备</strong></td></tr><tr><td>0</td><td>TV</td></tr><tr><td>1</td><td>Recording Device 1</td></tr><tr><td>2</td><td>Recording Device 2</td></tr><tr><td>3</td><td>Tunner 1</td></tr><tr><td>4</td><td>Playback Device 1</td></tr><tr><td>5</td><td>Audio System</td></tr><tr><td>6</td><td>Tunner 2</td></tr><tr><td>7</td><td>Tunner 3</td></tr><tr><td>8</td><td>Playback Device 2</td></tr><tr><td>9</td><td>Recording Device 3</td></tr><tr><td>10</td><td>Tunner 4</td></tr><tr><td>11</td><td>Playback Device 3</td></tr><tr><td>12</td><td>Reserved</td></tr><tr><td>13</td><td>Reserved</td></tr><tr><td>14</td><td>Specific Use</td></tr><tr><td>15</td><td>Unregistered (as Initiator address) Broadcast (as Destination address)</td></tr></tbody></table>

 

### 目前 CEC 包含的功能

**1.One Touch Play** 它通过三条命令< Active Source >、< Text View On >、< Image View On >得以实现，是 CEC 认证中，强制要求的功能。此功能说的简单点，就是用于向 TV 请求显示设备自己的输出会发送 one touch play 的命令，用于要求在TV上显示输出。

**2.Routing Control** 它主要通过< Active Source >、< Inactive Source >、< Request Active Source >、< Set Stream Path >、< Routing Change >和< Routing Information >这几条命令实现，用于控制HTS和HDMI INPUT设备在TV上的显示。

**3.System Standby** 它通过< Standby >命令实现，会以广播的方式或者特别的地址的方式发送，一般情况当按 TV 遥控器上的 power 键关机时，TV 发送< Standby >命令，其他设备接收命令后响应关机。

**4.One touch Record**

**5.Timer Programming**

**6.System Information** 这个功能包含< Get CEC Version >和< CEC Version >、< Get Menu Language >和< Set Menu Language > 、< Give Physical Address >和< Report Physical Address >、< Polling Message >这7条命令。 < Polling Message >命令用于检测 HMDI 网络中其他设备和分配每个设备的逻辑地址的作用。 < Get CEC Version >和< CEC Version >用于说明 HDMI CEC 的版本，前者通常要求得到版本，后者是对前者的回复，需要通过CEC的测试设备测试。 < Get Menu Language >和< Set Menu Language > 用于要求得到和回复关于 menu 语言的设置功能。 < Give Physical Address >和< Report Physical Address >将被用于要求得到和回复关于设备的物理地址,通常是以广播的形式向 HDMI 网络播放。

**7.Deck Control**

**8.Tuner Control**

**9.Vendor Specific Commands** 由< Device Vendor ID >、< Give Device Vendor ID >、< Vendor Command >、< Vendor Command With ID >、< Vendor Remote Button Down >、< Vendor Remote Button Up >这几条命令实现。 < Give Device Vendor ID >和< Device Vendor ID >命令将被用于显示 vendor 的 ID，是一组显示设备厂商的标准的命令。 < Vendor Command >、< Vendor Command With ID >、< Vendor Remote Button Down >、< Vendor Remote Button Up >则被用于和厂商的其他产品交互使用，由厂商定义了一些特殊的命令用于交互。

**10.OSD Display**

**11.Device OSD Name Transfer** 主要用于显示设备的名称，它包含< Give OSD Name >和< Set OSD Name >这两条命令，提出要求和回复要求，可以在 TV 上看到 HTS 设备的名称。

**12.Device Menu Control** 主要由< Menu Request >和< Menu Status >两条命令构成这个功能， < Menu Request >有\[Actived\]、\[Deactived\]、\[Query\]3个参数，而它的答复命令 < Menu Status >则带有\[Actived\]、\[Deactived\]2个参数回复。TV 和 HTS 通过这2个命令可以切换 menu 的显示状态，需要通过 CEC 的测试设备或者工具才可以测试。

**13.Remote Control Pass Through** 通过< User Control Pressed >、< User Control released >这条命令携带的不同参数，利用 TV 遥控器像 BD-HTS 遥控器一样控制 HTS 和用户交互的功能，可以通过 TV 的遥控器进行测试。

**14.Give Device Power Status** < Give Device Power Status >和< Report Power Status >两条命令用于这个功能的实现，前者提出请求，后者答复，一般情况下< Report Power Status >将有\[Power on\]和\[Standby\]两种状态，后者一般是在 Standby CEC 中实现。

**15.System Audio Control** 以下的命令除了最后一对，其他前者都是TV（sink device）发送，后者都是HTS(source device)发送答复。 < Give System Audio Mode Status>和< System Audio Mode Status>，< Request Short Audio Descriptor>和< Report Short Audio Descriptor>，< System Audio Mode Request>和< Set System Audio Mode>，< Give Audio Status>和< Reprot Audio Status> 其中，前2对需要用 HDMI CEC 的测试设备进行测试，以判断命令执行的情况，倒数第二对用于控制TV的声音输出可以通过TV的控制声音输出的 UI 进行验证，最后一对则是用于通过 TV 的遥控器控制音量和 mute 的命令，可以通过 TV 遥控器上的 volume 和 mute 键测试判断，需要注意的是这时候 volume 和 mute 的 UI 只显示 TV 的 UI，HTS 不显示自己的 UI。

**16.Audio Rate Control**

**17.Audio Return Channel Control** Audio Return Channe Control(ARC)是HDMI 1.4规格中新增加的内容简单说来就是在原有的 HDMI 端口中一个预留脚上回传 S-PDIF 信号。一个带 ARC OUT 的电视再加上一个支持 ARC IN 的功放产品完美的视听体验就齐了。

**18.Capability Discovery and Control for HEC**
