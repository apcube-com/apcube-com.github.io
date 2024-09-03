---
title: "用Tasker转发Android上收到的短信至Telegram"
date: "2017-12-23"
categories: 
  - "get-something"
tags: 
  - "android"
  - "tasker"
  - "telegram"
  - "telegram-bot"
---

你还在为iPhone不能双卡双待发愁吗？你还在为给P友准备的小号发愁吗？那么你来对地方了！

接下来我们将用Telegram申请一个bot(机器人)，在Android上用Tasker把短信转发到这个bot里。

1\. Telegram上关注botfather，申请一个bot(机器人)。关注之后发送 **_/newbot_** 创建新bot，起个难听的名字比如x6A0A，之后会让你再起个username。输入之后你会得到以下回复,请记住下面那个HTTP API

```
Done! Congratulations on your new bot. You will find it at  t.me/x6AOA . You can now add a description, about section and profile picture for your bot, see /help for a list of commands. By the way, when you've finished creating your cool bot, ping our Bot Support if you want a better username for it. Just make sure the bot is fully operational before you do this.
Use this token to access the HTTP API:

477905544:AAxxxxxxxxxxKzl0fSeb6Z_edLQOOf4KK5w

For a description of the Bot API, see this page: https://core.telegram.org/bots/api
```

2\. 这时候你就可以搜索到自己刚刚创建的bot了，进去先给那个bot打个招呼。

3\. 关注 **_userinfobot_** 获得自己的id，一个九位的数字，记下来。

4\. 接下来小试牛刀，重构以下URL，粘贴到浏览器中回车，不出意外的话是不是收到了 **我吹呀吹** 几个字？？

```
https://api.telegram.org/bot【替换成刚刚的HTTP API】/sendMessage?chat_id=【你步骤3中的ID】&text=我吹呀吹
替换完了长这个样子
https://api.telegram.org/bot508980960:AAExxxxx_kcLaPcqTKWTXAzlGz_vsGmuKhUR4/sendMessage?chat_id=508911160&text=我吹呀吹
```

5\. 好，现在我们需要用Taskber调用这个URL，把收到的短信发过去，首先你得下载一个tasker，tasker教程很多，我在这里写的省略点.......

6\. 创建event：搜索receive text，保持默认设置ok

7\. 创建一个新的task，起个名字如 **发送至telegram**

8\. 在新的task中创建任务，搜索 **HTTP POST** ，按照下面的填

```
Server:Port
https://api.telegram.org
Path: 像4中一样[把这里的东西替换掉]
/bot[替换成刚刚的HTTP API]/sendMessage?chat_id=[你步骤3中的ID]&text=%SMSRN 内容:%SMSRB
```

9\. All done！现在你可以自己给自己发条短信试试。

```
注意: 要给Tasker分配足够的权限
```

ps：我试过许多方案，短信直接转发有很大的延迟，IFTTT又经常大姨妈，telegram又快又稳定！

```
作者：我吹呀吹
链接：https://www.jianshu.com/p/6a5a13db87dd
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

\--------------------------------------------- 好像是分割线 -------------------------------------------

试验了一下，转发中文短信会乱码，试了 tasker 4.7/4.9 的都会，不知道是不是因为手机的原因。

我是把短信 URI 编码后再发送就可以发中文了。

1，加一个 Variable Set

```
Name: %msg
to: %SMSRN: %SMSRB
```

2, 加一个 JavaScriptlet

```
Code:
emsg = encodeURI(msg)
tk.setLocal("umsg", emsg)
```

3, 最后的 text=%umsg

\-= 完 =-
