---
title: "Nginx 配置上更安全的 SSL & ECC 证书"
date: "2016-03-03"
categories: 
  - "sometime-programmer"
tags: 
  - "linux"
  - "nginx"
  - "security"
  - "ssltls"
  - "漫步云端"
---

关于 ECC 证书，好处就不多说了，256 位的证书加密强度比 2048 位的 RSA 都高，还避免了性能消耗 吗？。

谷歌一搜 ECC 证书，首页已经被沃通承包了，沃通本身就是个流氓，他不断夸大自己的数据，说 ECC 如何如何好，对移动设备友好，支持 XP Android 2.1.x 等等等等。

沃通颁发的 ECC 证书是经过特殊处理的，猜测是采用了传说中的双证书。

![ECC证书](/assets/images/ecccert.png) 

ECC证书

所以支持上述系统。实测 ECC 的性能消耗跟加密程度一样强悍，384 位的 ECC 给 CPU 造成的压力和 3000 位左右的 RSA 一样，那要是 4096 位的 ECC，那效果，简直爆炸。

但是 ECC 证书的部署不大一样，加密套件和 RSA 不一样，用错之后会影响向前安全性（Forward secrecy）。

加密套件应该这么改：

```

ssl_ciphers "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+aRSA+RC4 EECDH EDH+aRSA RC4 !aNULL !eNULL !LOW !3DES !MD5 !EXP !PSK !SRP !DSS !RC4";
```

```

// Generate RSA key
openssl genrsa -out private.key 4096

// Generate ECC key
openssl ecparam -genkey -name secp384r1 -out private-ecc.key

// Generate CSR
openssl req -new -sha384 -key private-ecc.key -out www_pupboss_com.csr
```

`secp384r1` 是一个曲线名，`openssl ecparam -list_curves` 可以看所有的曲线名。

经过测试，

CSR 的 SHA1 加密不影响生成的证书用 SHA256 加密！ CSR 的 SHA1 加密不影响生成的证书用 SHA256 加密！ CSR 的 SHA1 加密不影响生成的证书用 SHA256 加密！

申请完 SSL 证书，应该有以下几样东西

- `private.key` 私钥
- `cert.csr` CSR
- `www_pupboss.crt` 公钥
- `xxxx.crt` 证书商的中级证书和根证书

首先把证书商提供的根证书中级证书添加到自己的证书后面，不然有的浏览器可能报错，我的习惯是域名证书 + 中级证书 + 根证书，别的顺序不知道行不行。 当然这些是最初级的，下面来点高级的东西，先上臭美图两张：

现在服务器采用了 SNI 技术，安装了多个 SSL 证书，所以 chrome 下会有黄色警告

![已使用证书 Chrome 浏览器显示结果](/assets/images/safessl01.png) 

已使用证书 Chrome 浏览器显示结果

![SSL Labs 测试结果](/assets/images/safessl02.png) SSL Labs 测试结果

测试地址：[SSL Labs](https://www.ssllabs.com/ssltest/analyze.html?d=pupboss.com)

## SSL 协议

`SSLv2` 是不安全的，绝对不能用，`SSLv3` 能不用则不用，正常情况下用不到的，推荐使用 `TLSv1 TLSv1.1 TLSv1.2`，附配置代码：

```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
```

## 加密算法

RC4 不要用，再就是 OpenSSL 记得更新到最新版，可以避免很多麻烦。别的也没什么了，附代码：

```
ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";

ssl_prefer_server_ciphers on;
```

如果你的 OpenSSL 版本比较旧，不可用的加密算法会被自动丢弃。最好使用完整套件，让 OpenSSL 自动选择。所以套件的顺序就灰常重要

- `ECDHE+AESGCM` 加密是首选的。它们是 TLS 1.2 加密算法，现在还没有广泛支持。当然也没有破解的方案。
- `PFS` 加密套件好一些，首选 `ECDHE`，然后是 `DHE`。
- `AES 128` 要好于 `AES 256`。`AES 256` 会造成更大的性能消耗，但是带来的安全提升是有限的。反之还能承受更大压力。
- 在向后兼容的加密套件里面，`AES` 要优于 `3DES`。在 TLS 1.1及其以上，减轻了针对 `AES` 的野兽攻击（BEAST）的威胁，而在 TLS 1.0上则难以实现该攻击。在非向后兼容的加密套件里面，不支持 `3DES`。
- `RC4` 整个不支持了。`3DES` 用来向后兼容。

**强制丢弃的算法**

- `aNULL` 包含了非验证的 `Diffie-Hellman` 密钥交换，这会受到中间人（MITM）攻击
- `eNULL` 包含了无加密的算法（明文）
- `EXPORT` 是老旧的弱加密算法，是被美国法律标示为可出口的
- `RC4` 包含使用了已弃用的 `ARCFOUR` 的加密算法
- `DES` 包含使用了弃用的数据加密标准（`DES`）的加密算法
- `SSLv2` 包含了定义在旧版本 SSL 标准中的所有算法，现已弃用
- `MD5` 包含了使用已弃用的 `MD5` 的所有算法

## 前向安全性(Forward Secrecy)

首先 Nginx 配置上如下代码：

```
ssl_dhparam /your/path/to/dhparam.pem;
```

pem 文件是这样生成的：

```
openssl dhparam -out dhparam.pem 4096
```

然后会显示

```
This is going to take a long time  
.......+...........................+............................+............................+............++*++*++*
```

当时这个命令，VPS 上单核执行了 100 分钟，我的电脑 i7 4750HQ 处理器，只能跑一个核，用了大概 70 分钟，VPS 上生成的 ..+... 字符数大概在 50000 个，我的电脑产生了 74900 多个，不知道什么原因

以下内容来自 [@justwd](https://blog.justwd.net/) 的邮件：

> dhparam 算法是在 2^4096 个数字中找出两个质数，所以需要的时间挺长。..... 意思是有可能的质数，+ 是正在测试的质数，\* 是已经找到的质数。

前向安全性(Forward Secrecy)的概念很简单：客户端和服务器协商一个永不重用的密钥，并在会话结束时销毁它。服务器上的 RSA 私钥用于客户端和服务器之间的 Diffie-Hellman 密钥交换签名。从 Diffie-Hellman 握手中获取的预主密钥会用于之后的编码。因为预主密钥是特定于客户端和服务器之间建立的某个连接，并且只用在一个限定的时间内，所以称作短暂模式(Ephemeral)。

如果使用前向安全机制，攻击者取得了一个服务器的私钥，他是不能解码之前的通讯信息的。这个私钥仅用于 Diffie Hellman 握手签名，并不会泄露预主密钥。Diffie Hellman 算法会确保预主密钥绝不会离开客户端和服务器，而且不能被中间人攻击所拦截。

所有版本的 nginx 都依赖于 OpenSSL 给 Diffie-Hellman 的输入参数。如果不特别声明，将使用 OpenSSL 的默认设置，1024 位密钥。这绝壁是不安全的，因为我们正在使用 2048 位证书，所以要有一个更强大的 DH

## HTTP 严格传输安全(HSTS)(HTTP Strict Transport Security)

```
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
```

接下来的半年，都会保持直接发送 HTTPS。

## 配置 OCSP 装订

```
ssl_stapling on;  
ssl_stapling_verify on;  
ssl_trusted_certificate /you/path/to/domain.chain.pem;  
resolver 8.8.8.8 8.8.4.4 valid=300s;(国外)  
resolver 223.5.5.5 223.6.6.6 valid=300s;(国内)  
resolver_timeout 10s;  
```

`domain.chain.pem` 里面是域名证书到 ROOT 证书的一个链。

连接到一个服务器时，客户端应该使用证书吊销列表（CRL）或在线证书状态协议（OCSP）记录来校验服务器证书的有效性。CRL 存在一个问题，它已经增长的太快，永远也下载不完。

OCSP 更轻量一些，只需发一个请求。但是副作用是访问一个站点时必须对第三方 OCSP 响应服务器发起 OCSP 请求，这就增加了延迟带来的潜在隐患。事实上，CA 所运营的 OCSP 响应服务器非常不可靠，浏览器如果不能及时收到答复，就会静默失败。攻击者通过 DDoS 攻击一个 OCSP 响应服务器可以禁用其校验功能，这样就降低了安全性。

解决方法是允许服务器在 TLS 握手中发送缓存的 OCSP 记录，以绕开 OCSP 响应服务器。这个机制减少了客户端和 OCSP 响应服务器之间的通讯，称作 OCSP 装订。

客户端会在它的 CLIENT HELLO 中告知其支持 status\_request TLS 扩展，服务器仅在客户端请求它的时候才发送缓存的 OCSP 响应。

大多数服务器最长会缓存 OCSP 48 小时。服务器会按照常规的间隔连接到 CA 的 OCSP 响应服务器来获取刷新的 OCSP 记录。OCSP 响应服务器的位置可以从签名的证书中 Authority Information Access 字段中获得。

我的服务器是这么配置的：

```
server {

    listen 80;
    listen 443 ssl http2;
    server_name www.pupboss.com;
    client_max_body_size 10M;

    ssl_certificate /etc/nginx/ssl/wc_pupboss.crt;
    ssl_certificate_key /etc/nginx/ssl/wildcard.key;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";
    ssl_prefer_server_ciphers on;

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;

    ssl_session_tickets on;

    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_trusted_certificate /etc/nginx/ssl/pupboss_chain.pem;
    resolver 223.5.5.5 223.6.6.6 valid=300s;
    resolver_timeout 10s;

    add_header X-Frame-Options deny;
    add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
}
```

正常情况下，这样配置完之后，测试就会显示 A+ 了。

## 重启免密码

有的时候生成 key 的时候，手贱输了密码，敲入如下指令可以解除：

```
openssl rsa -in pupboss.key -out pupboss_unsecure.key
```

![Start SSL](/assets/images/startssl01.jpg) 

Start SSL

上臭美图一张

![Start SSL](/assets/images/startssl03.png) 

Start SSL

## 强制 HTTPS

加上如下代码

```
server {

    listen 80;
    server_name www.pupboss.com;
    return 301 https://$server_name$request_uri;
}
```

 

> 本文采用 [知识共享署名 - 非商业性使用 - 相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/)进行许可。
>
> 原文来自：[@{HOME: PUPBOSS};](https://www.pupboss.com/)
>
> 原文链接：[Nginx 配置上更安全的 SSL & ECC 证书](https://www.pupboss.com/nginx-add-ssl/)
