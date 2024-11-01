---
{"dg-publish":true,"permalink":"/003 学习笔记/PdaiTech/数据库/NoSQL-Redis/Redis进阶 - 消息传递：发布订阅模式详解/","dgPassFrontmatter":true,"created":"2024-05-30T09:58:18.435+08:00","updated":"2024-06-01T10:48:41.378+08:00"}
---

# Redis发布订阅简介

> Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息

Redis 的 SUBSCRIBE 命令可以让客户端订阅任意数量的频道， 每当有新信息发送到被订阅的频道时， 信息就会被发送给所有订阅指定频道的客户端。

作为例子，下图展示了频道 channel1 ，以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![Pasted image 20240530100400.png|340](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530100400.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时，这个消息就会被发送给订阅它的三个客户端：

![Pasted image 20240530100433.png|330](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530100433.png)
# 发布/订阅使用

>[! tip]
>Redis 有两种发布/订阅模式：
>- 基于频道 (Channel) 的发布/订阅
>- 基于模式 (pattern) 的发布/订阅
{ #5fa962}


## 基于频道(Channel)的发布/订阅

"发布/订阅"模式包含两种角色，分别是发布者和订阅者。发布者可以向指定的频道(channel)发送消息; 订阅者可以订阅一个或者多个频道(channel),所有订阅此频道的订阅者都会收到此消息

![Pasted image 20240530101040.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530101040.png)

1. **发布者发布消息**
发布者发布消息的命令是 publish,用法是 publish channel message，如向 channel1.1说一声hi

```zsh
127.0.0.1:6379> publish channel:1 hi
(integer) 1
```

这样消息就发出去了。返回值表示接收这条消息的订阅者数量。发出去的消息不会被持久化，也就是有客户端订阅channel:1后只能接收到后续发布到该频道的消息，之前的就接收不到了。

2. **订阅者订阅频道**

订阅频道的命令是 subscribe，可以同时订阅多个频道，用法是 subscribe channel1 [channel2 ...],例如新开一个客户端订阅上面频道:(不会收到消息，因为不会收到订阅之前就发布到该频道的消息)

```
127.0.0.1:6379> subscribe channel:1
Reading messages... (press Ctrl-C to quit)
1) "subscribe" // 消息类型
2) "channel:1" // 频道
3) "hi" // 消息内容
```

执行上面命令客户端会进入订阅状态，处于此状态下客户端不能使用除`subscribe`、`unsubscribe`、`psubscribe`和`punsubscribe`这四个属于"发布/订阅"之外的命令，否则会报错。

进入订阅状态后客户端可能收到3种类型的回复。每种类型的回复都包含3个值，第一个值是消息的类型，根据消类型的不同，第二个和第三个参数的含义可能不同。

消息类型的取值可能是以下3个:

- subscribe。表示订阅成功的反馈信息。第二个值是订阅成功的频道名称，第三个是当前客户端订阅的频道数量。
- message。表示接收到的消息，第二个值表示产生消息的频道名称，第三个值是消息的内容。
- unsubscribe。表示成功取消订阅某个频道。第二个值是对应的频道名称，第三个值是当前客户端订阅的频道数量，当此值为0时客户端会退出订阅状态，之后就可以执行其他非"发布/订阅"模式的命令了。
## 基于模式(pattern)的发布/订阅

如果有某个/某些模式和这个频道匹配的话，那么所有订阅这个/这些频道的客户端也同样会收到信息。

下图展示了一个带有频道和模式的例子， 其中 tweet.shop.* 模式匹配了 tweet.shop.kindle 频道和 tweet.shop.ipad 频道， 并且有不同的客户端分别订阅它们三个：

![Pasted image 20240530101950.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530101950.png)


当有信息发送到 tweet.shop.kindle 频道时， 信息除了发送给 clientX 和 clientY 之外， 还会发送给订阅 tweet.shop.* 模式的 client123 和 client256 ：

![Pasted image 20240530102346.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530102346.png)

另一方面，如果接收到信息的是频道 tweet.shop.ipad ，那么 client123 和 client256 同样会收到信息：

![Pasted image 20240530102419.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530102419.png)
- **基于模式的例子**

通配符中?表示1个占位符，*表示任意个占位符(包括0)，?*表示1个以上占位符。

publish发布

```zsh
127.0.0.1:6379> publish c m1
(integer) 0
127.0.0.1:6379> publish c1 m1
(integer) 1
127.0.0.1:6379> publish c11 m1
(integer) 0
127.0.0.1:6379> publish b m1
(integer) 1
127.0.0.1:6379> publish b1 m1
(integer) 1
127.0.0.1:6379> publish b11 m1
(integer) 1
127.0.0.1:6379> publish d m1
(integer) 0
127.0.0.1:6379> publish d1 m1
(integer) 1
127.0.0.1:6379> publish d11 m1
(integer) 1
```

psubscribe订阅

```zsh
127.0.0.1:6379> psubscribe c? b* d?*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "c?"
3) (integer) 1
1) "psubscribe"
2) "b*"
3) (integer) 2
1) "psubscribe"
2) "d?*"
3) (integer) 3
1) "pmessage"
2) "c?"
3) "c1"
4) "m1"
1) "pmessage"
2) "b*"
3) "b"
4) "m1"
1) "pmessage"
2) "b*"
3) "b1"
4) "m1"
1) "pmessage"
2) "b*"
3) "b11"
4) "m1"
1) "pmessage"
2) "d?*"
3) "d1"
4) "m1"
1) "pmessage"
2) "d?*"
3) "d11"
4) "m1"
```

- **注意点**

(1)使用psubscribe命令可以重复订阅同一个频道，如客户端执行了`psubscribe c? c?*`。这时向c1发布消息客户端会接受到两条消息，而同时publish命令的返回值是2而不是1。同样的，如果有另一个客户端执行了`subscribe c1`和`psubscribe c?*`的话，向c1发送一条消息该客户顿也会受到两条消息(但是是两种类型:message和pmessage)，同时publish命令也返回2.

(2)punsubscribe命令可以退订指定的规则，用法是: `punsubscribe [pattern [pattern ...]]`,如果没有参数则会退订所有规则。

(3)使用punsubscribe只能退订通过psubscribe命令订阅的规则，不会影响直接通过subscribe命令订阅的频道；同样unsubscribe命令也不会影响通过psubscribe命令订阅的规则。另外需要注意punsubscribe命令退订某个规则时不会将其中的通配符展开，而是进行严格的字符串匹配，所以 `punsubscribe *` 无法退订 `c*` 规则，而是必须使用 `punsubscribe c*` 才可以退订。（它们是相互独立的，后文可以看到数据结构上看也是两种实现）
# 深入理解

>我们通过几个问题，来深入理解Redis的订阅发布机制
## 基于频道(Channel)的发布/订阅如何实现的？

底层是通过字典（图中的pubsub_channels）实现的，这个字典就用于保存订阅频道的信息：字典的键为正在被订阅的频道， 而字典的值则是一个链表， 链表中保存了所有订阅这个频道的客户端。

- **数据结构**

比如说，在下图展示的这个 pubsub_channels 示例中， client2 、 client5 和 client1 就订阅了 channel1 ，而其他频道也分别被别的客户端所订阅：

![Pasted image 20240530111214.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530111214.png)

- **订阅**

当客户端调用 SUBSCRIBE 命令时， 程序就将客户端和要订阅的频道在 pubsub_channels 字典中关联起来。

举个例子，如果客户端 client10086 执行命令 `SUBSCRIBE channel1 channel2 channel3` ，那么前面展示的 pubsub_channels 将变成下面这个样子：

![Pasted image 20240530111327.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240530111327.png)

- **发布**

当调用 `PUBLISH channel message` 命令， 程序首先根据 channel 定位到字典的键， 然后将信息发送给字典值链表中的所有客户端。

比如说，对于以下这个 pubsub_channels 实例，如果某个客户端执行命令 `PUBLISH channel1 "hello moto"` ，那么 client2 、 client5 和 client1 三个客户端都将接收到 "hello moto" 信息：

- **退订**

使用 UNSUBSCRIBE 命令可以退订指定的频道，这个命令执行的是订阅的反操作： 它从 pubsub_channels 字典的给定频道（键）中，删除关于当前客户端的信息，这样被退订频道的信息就不会再发送给这个客户端。
## 基于模式(Pattern)的发布/订阅如何实现的？

