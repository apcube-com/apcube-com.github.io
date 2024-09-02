---
title: "SOAP - 简介"
date: "2015-11-12"
categories: 
  - "介绍"
  - "硬件接口"
  - "网络"
tags: 
  - "interface"
  - "soap"
  - "upnp"
  - "what-is-it"
---

SOAP（原为Simple Object Access Protocol的首字母缩写，即简单对象访问协议）是交换数据的一种协议规范，使用在计算机网络Web服务（web service）中，交换带结构信息。SOAP為了簡化網頁服务器（Web Server）從XML資料庫中提取資料時，节省去格式化頁面時間，以及不同應用程式之間按照HTTP通信协议，遵从XML格式执行资料互换，使其抽象于语言实现、平台和硬件。此標準由IBM、Microsoft、UserLand和DevelopMentor在1998年共同提出，並得到IBM，蓮花（Lotus），康柏（Compaq）等公司的支持，於2000年提交給全球資訊網聯盟（World Wide Web Consortium；W3C），目前SOAP 1.1版是業界共同的標準，屬於第二代的XML協定（第一代具主要代表性的技術為XML-RPC以及WDDX）。

用一个简单的例子来说明SOAP使用过程，一个SOAP消息可以发送到一个具有Web Service功能的Web站点，例如，一个含有房价信息的数据库，消息的参数中标明这是一个查询消息，此站点将返回一个XML格式的信息，其中包含了查询结果（价格，位置，特点，或者其他信息）。由于数据是用一种标准化的可分析的结构来传递的，所以可以直接被第三方站点所利用。

**相关定义**

* * *

SOAP封装（envelop），它定义了一个框架，描述消息中的内容是什么，是谁发送的，谁应当接受并处理它以及如何处理它们； SOAP编码规则（encoding rules），它定义了一种序列化的机制，用于表示应用程序需要使用的数据类型的实例； SOAP RPC表示（RPC representation），它定义了一个协定，用于表示远程过程调用和应答； SOAP绑定（binding），它定义了SOAP使用哪种协议交换信息。使用HTTP/TCP/UDP协议都可以。 把SOAP绑定到HTTP提供了同时利用SOAP的样式和分散的灵活性的特点以及HTTP的丰富的特征库的优点。在HTTP上传送SOAP并不是说SOAP会覆盖现有的HTTP语义，而是HTTP上的SOAP语义会自然的映射到HTTP语义。在使用HTTP作为协议绑定的场合中，RPC请求映射到HTTP请求上，而RPC应答映射到HTTP应答。然而，在RPC上使用SOAP并不仅限于HTTP协议绑定。

**历史**

* * *

SOAP曾经代表“Simple Object Access Protocol”，但是这种缩写已经在标准的1.2版后被废止了。1.2版在2003年6月24日成为W3C的推荐版本。这种缩写容易与SOA——Service-oriented architecture产生歧义，虽然它们之间存在非常大的差异。

SOAP由Dave Winer, Don Box，Bob Atkinson, Mohsen Al-Ghosein於1998年设计，當時只作为一种对象访问协议。现在，SOAP规范由 万维网联盟的 XML工作组维护。

**传输方式**

* * *

![soap](/assets/images/soap.png)

SOAP使用因特网应用层协议作为其传输协议。SMTP以及HTTP协议都可以用来传输SOAP消息，但是由于HTTP在如今的因特网结构中工作得很好，特别是在网络防火墙下仍然正常工作，所以被广泛采纳。SOAP亦可以在HTTPS上传输。

SOAP的消息格式采用XML。

**语法规则**

* * *

SOAP消息必须用XML来编码 SOAP消息必须使用SOAP Envelope命名空间 SOAP消息必须使用SOAP Encoding命名空间 SOAP消息不能包含DTD引用 SOAP消息不能包含XML处理指令

**SOAP消息实例**

* * *

请求

```
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <soapenv:Body>
    <req:echo xmlns:req="http://localhost:8080/wxyc/login.do">
      <req:category>classifieds</req:category>
    </req:echo>
  </soapenv:Body>
</soapenv:Envelope>
```

回应

```
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:wsa="http://schemas.xmlsoap.org/ws/2004/08/addressing">
  <soapenv:Header>
    <wsa:ReplyTo>
      <wsa:Address>http://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous</wsa:Address>
    </wsa:ReplyTo>
    <wsa:From>
      <wsa:Address>http://localhost:8080/axis2/services/MyService</wsa:Address>
    </wsa:From>
    <wsa:MessageID>ECE5B3F187F29D28BC11433905662036</wsa:MessageID>
  </soapenv:Header>
  <soapenv:Body>
    <req:echo xmlns:req="http://localhost:8080/axis2/services/MyService/">
      <req:category>classifieds</req:category>
    </req:echo>
  </soapenv:Body>
</soapenv:Envelope>
```

本文来自：[维基百科](https://zh.wikipedia.org/wiki/SOAP)
