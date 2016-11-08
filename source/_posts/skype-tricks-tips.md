---
title: Skype 使用小技巧
date: 2016-09-01 20:39:06
tags: Skype
categories: Tools
---
<img src="/imgs/skype.png">

Skype 作为一个即时聊天工具，相信大家都不陌生。我最早接触 Skype 是为了满足短期在国外给国内的家人朋友打电话报平安的需求。给 Skype 充点合适的点卡，然后就可以以非常划算的资费（反正感觉比开通国际漫游要划算的多）打电话，无论手机还是座机，详情请参考：[http://skype.gmw.cn/product/productlist.html](http://skype.gmw.cn/product/productlist.html)。

经过一段时间的使用，本着程序员固有的一股子好奇心，发现了一些 Skype 中的隐藏功能，在这里与大家分享。

<!--more-->

## 发送隐藏表情

平常我们发送表情符号的时候都是直接在表情选择器中选择一个表情然后按 Enter 键发送，如下图：
<img src="/imgs/skype-hidden-emoticon.png" width="400px" height="auto">

这里请注意图中的一个小细节，一个扣篮的英文单词`(slamdunk)`引起了我的注意，心想这个和我们常用的`:)`、`:(`、`:P` 是否一样呢？作为程序员的本能反应，我在聊天输入框中尝试输入 `(slamdunk)`（注意括号为英文半角的括号）然后按 Enter 键发送，果不其然，效果一样，那么有哪些表情可以通过这种方式输入呢？问过狗哥之后，找到了一堆文章，并且还有意外发现——Skype 中居然还有一些隐藏表情。比如：`(wtf)`、`(poop)`、`(mooning)`、`(ie)`、`(win10)`、`(smoking)`、`(finger)`。
<img src="/imgs/skype-hidden-emoticon-1.png" width="400px" height="auto"><img src="/imgs/skype-hidden-emoticon-2.png" width="400px" height="auto">

## 发送国旗符号

与前面的发送隐藏表情类似，我们还可以在聊天中发送国旗符号，以`(flag:XY)`的格式，其中 XY 为国家的代码，比如 CN-中国，US-美国，BR-巴西，可以在这里查询[https://countrycode.org/](https://countrycode.org/)。
<img src="/imgs/skype-country-flag.png">

## 设置文字格式

在 Skype 的聊天过程中，时常会有需要强调的东西，这个时候你是否会想起粗体、斜体、中划线？和 Markdown 类似，Skype 也提供了一些简单的标签。更多请参考[https://www.nesono.com/node/450](https://www.nesono.com/node/450)。

样式	| 例子 | 效果
:---:|:---:|:---:
粗体 | 这是 \*粗体\* | 这是 **粗体**
斜体 | 这是 \_斜体\_ | 这是 _斜体_
中划线 |	这是 \~中划线\~ | 这是 ~~中划线~~

## 高级命令

Skype 的聊天框不仅仅只是一个聊天框，就像很多游戏的聊天窗口一样（据我所知，很多游戏的聊天输入框都支持一些特殊命令，只是你不知道而已），Skype 也一样。
你可以尝试输入`/debug` 或者`/help`然后按 Enter 键，会有意想不到的结果（私聊和群聊支持的命令有一些不同）。这里举几个常用的命令，更多命令的解释请参考：[https://support.skype.com/en/faq/FA10042/what-are-chat-commands-and-roles](https://support.skype.com/en/faq/FA10042/what-are-chat-commands-and-roles)。

命令 | 作用
:---|:---
/get admins | 找出管理员
/get role | 查看自己的角色
/add [skype name] | 加人，比如`/add pengisgood`；如果是以 live 打头的账号，需要加上 live，比如 `/add live:pengisgood`
/kick [skype name] | 踢人
/remotelogout | 停止推送提醒到其他设备
/setrole [Skype Name] SPEAKER &#124; ADMIN | 设置角色

## 参考资料

>http://skype.gmw.cn/product/productlist.html
http://skypesmileyscodes.com/hidden-skype-smileys/
http://skypesmileyscodes.com/skype-flags/
https://countrycode.org/
https://www.nesono.com/node/450
https://support.skype.com/en/faq/FA10042/what-are-chat-commands-and-roles
https://www.reddit.com/r/skype/comments/37ay58/new_wikimarkup_onoff_command/
