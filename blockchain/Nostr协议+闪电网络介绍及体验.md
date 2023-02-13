文章不是保姆级的，需要带入自己的思考。文末有体验流程的总结，可以前后对比着看。最后的参考文章都非常棒，本文参考了它们，推荐阅读。

特别提醒：`助记词`、`私钥`、`lndhub wallet backup` 都是敏感数据，一定要保存好，泄漏或遗失都会导致你的资产损失。

>我的nostr：https://nostrplebs.com/s/gaohongxiang 欢迎关注

# Nostr
Nostr是一个基于公私钥的消息传输协议。协议本身非常简洁，只解决一个问题：如何让用户直接以公钥为身份向外发送消息。

它提供了一个我们重新构建互联网的机会，从头重建一遍互联网。

## Nostr的账户系统

Nostr的账户系统就是公私钥
- 公钥（npub1nv7...mqmmx）-> 对应账号。通过公钥关注用户
- 私钥（nsec17c6...rswm9）-> 对应密码。私钥控制账户

`账户去中心化`

公私钥基于数学生成，无需许可，不依赖任何其他第三方服务（不需要手机号，不需要邮箱等），完全由自己掌握。也就是说任何第三方机构都无法封禁你的账号，天然抗审查。对比我们常用的twitter、微信等应用，他们有能力封禁账户。典型例子是美国前总统特朗普的twitter账号被封。被封代表着你的账户及数据不属于你了，而nostr账户及数据是可以带走的，与平台无关。

## Nostr运行原理

Nostr 预设了两种角色去运行这个协议
- 客户端
- 转发器（relay）

>它的结构很简单，客户端接收（拉取）消息、递送消息，relay 转发消息。

客户端把用户使用公钥签名的消息发给 relay，relay把此消息转发给关注了这个公钥的其他客户端。同时，每一个客户端也接收（拉取）relay发来的其他公钥的消息。

它有一些关键的属性是什么？

- relay 跟 relay 之间不通信
- 客户端 跟 客户端 之间不通信
- relay 跟 客户端 之间通信

这是一个很重要但是一直被大家忽视的属性。很多人误解了 Nostr 的特性都在于它没有理解到这一点。协议的作者 fiatjaf 一开始说得很清楚，Nostr 不是一个点对点的协议，大家的消息都是要经过 relay 的，而 relay 之间互相是不通信的，所以它也不像我们认识到的区块链的网络一样所有节点都相互通信。

在 nostr 中，有一个很重要的问题需要解决，由于 relay 之间不同步数据，那么数据要怎么在整个网络中同步呢？nostr 的设计很巧妙，每个 relay 之间不同步消息，同步消息的机制由 client 来实现。client把自己知道的用户及相关的 relay 地址传播到其他的 relay 中。这样其他的用户就可以通过这些信息找到目标用户的 relay，拉取到目标用户的消息，从而完成信息的传播。
这样做的好处在于每个 relay 不需要像区块链节点那样存储全量的数据。nostr 的这种数据存储方式类似于数据库分片，让每个 relay 的存储压力不那么大。当然这样做也有坏处，如果用户的消息只在某一个 relay 上，如果这个 relay 出现问题，就有可能导致数据永久丢失。


### 客户端

`客户端去中心化`

nostr是底层协议，直接与协议交互显然不方便，门槛高，就需要客户端来封装nostr协议。也就是说，客户端是用户与nostr协议交互的载体。客户端有很多，下面是目前比较常见的，使用任意一个都可以，他们都实现了nostr协议。

浏览器客户端
- https://snort.social/
- https://iris.to/
- https://astral.ninja/
- https://nostr.band/

移动客户端
- Damus（iOS）
- Amethyst（Android）

官方维护的客户端列表
- https://github.com/vishalxl/Nostr-Clients-Features-List#nostr-client-feature-list

这里对习惯了传统社交应用的人来说比较新奇的点在于：你可以使用任意一个你喜欢的客户端，你在A客户端上做的交互也同时会在B客户端存在。也就是说，这些交互都是绑定你的账户的，并不会困在某一个客户端（应用）里，因为他们底层都是在跟nostr协议交互。

举个例子：你在客户端iris里设置了头像或者发了篇帖子，当你在客户端Damus里使用相同的公私钥时，这个头像或帖子还在！这对于没接触过的人来说非常神奇，因为你不可能在微博里发了篇帖子在twitter上还能看见它，他们是不互通的。请记住：nostr是底层协议，客户端是对nostr协议的实现，是协议上的应用。

### relay

`服务端（relay）去中心化`

- 消息都经过签名，relay无法篡改，因此不必信任relay
- 如果一个relay把你封禁了，你也可以转发到其他的relay或者创建自己的relay

relay是离用户比较远的一个角色，用户可以根据需要切换relay。目前的运营基本是用爱发电。随着nostr协议的流行，预计未来会出现付费reply，必须付费才能连接。比如说有的人愿意去做图片的 relay，有的人愿意做视频的 relay，有的人可能会做一些像类似于直播这样的 relay。这些都不是协议层的内容，市场会有选择，期待新的商业模式出现。

如果你想自己搭建一个relay，下面是一些教程
- https://github.com/BlockChainCaffe/Nostr-Relay-Setup-Guide
- https://andreneves.xyz/p/set-up-a-nostr-relay-server-in-under

## NIP

NIP是nostr的一些标准，类似比特币的BIP，以太坊的EIP。具体标准查看官方github：https://github.com/nostr-protocol/nips

### `NIP-05 ID`
由于公钥是一串难以记忆的字符串，对人类非常不友好。当我想关注一个人时，看到的都是不同的字符串，很难看出来是不是要关注的人。而NIP-05就是解决这个问题的，将公钥替换为人类可读ID。类似于构建在Nostr上面的一个域名，它是唯一的。下面是一些服务商。

#### 付费
- https://nostrplebs.com/
- https://nip05.nostr.band/

这里付款会用到闪电网络，看下文。nostrplebs普通会员费用大概12500sat，nostr.band费用1000sat。

购买成功后就会分配`NIP-05 ID`。去任意客户端在设置的NIP-05那一项里填进去保存即可。
- nostrplebs：`username@nostrplebs.com`
- nostr.band：`username@nostr.band`

nostrplebs
- 提供的功能：https://nostrplebs.com/features
- 帮助中心：https://help.nostrplebs.com/

其中有一条是闪电地址转发和管理。Nostr Plebs 成员可以将发送到他们注册的 NIP-05 ID 的闪电支付转发到他们现有的闪电地址，为他们提供一个身份和支付地址。

#### 免费
- https://nostr.directory/
- https://nostrverified.com/

这两个是免费的，但是需要用twitter账户验证一下。流程大概就是证明你控制这两个账户。
- twitter发一个包含你公钥的推文，格式根据网站的来。
- nostr验证（nostr.directory是发一个带你twitter用户名的post，nostrverified是填表）

验证通过就会分配`NIP-05 ID`。
- nostr.directory：`yourtwitterhandle@nostr.directory`
- nostrverified：`yourtwitterhandle@NostrVerified.com`

## 小应用

国际象棋游戏
- https://jesterui.github.io/

进入网站，你生成一个公钥，就可以在里面跟人家下棋了，你可以选择机器人跟你下棋，也可以选择跟一个对手下棋。你们的每一步棋其实都是通过 Nostr 协议的 relay 来转发的。象棋的步数的解析，输赢的判定全部放在你本地的客户端，而怎么寻找对手放在 relay 上，就这么简单。

私密聊天室‌
- https://sendstr.com/

你在里面你就可以生成一个一次性的公钥，你把公钥发给别人，然后别人就可以用你的公钥，他自己也生成一个一次性的公钥，然后就跟你进入聊天。这些公钥都是一次性的，用完就丢掉。

## 挑战

### 私钥泄漏遗忘问题

遗忘/丢失私钥即表示丢失账户控制权，任何人也无法帮你找回账户。另外，私钥被盗/泄漏即表示不止你一个人能控制此账户。

目前通过客户端生成的私钥本身存在泄漏的风险。可能的解决方案
- 硬件钱包生成（助记词，按m/44'/1237'/0'/0/0派生路径生成？）
- 离线生成（openssl？）

使用上，每个客户端初次使用都需要导入私钥，存在安全隐患

### 内容存储

社交必定会产生大量的内容。这些内容的存储如果是中心化的，那么nostr的抗审查是不是就打折扣了。也许会有结合区块链的存储方式，比如用AR或者IPFS来做存储。

数据同步问题，假设别人没有使用你使用的reply，就看不到你的内容？是不是未来会发展成公共和私人两个方向？公共relay就像twitter一样，存储足够多的信息。而私人relay就像discord的频道一样，加入了才看得到。

### 垃圾信息

在 nip-13 中， 定义了 pow 机制来增加发送垃圾消息的成本，但这样应该还不足以拦截垃圾信息。

在这个问题上，有另外一个思路，由于 relay 和 client 实现上的自由度很高，relay 和 client 可以自行决定可以传播什么样的信息，对哪些信息进行拦截，对一些恶意用户设置黑名单，然后在 relay 之间分享。
这样可以让 nostr 形成各种类型的社区，每个社区都有自己的规则，那些任由垃圾消息传播的社区会成为垃圾场，有着良好规则和优质内容的社区就会越来越庞大。

# 比特币闪电网络

Nostr是一个去中心化的社交协议，没有定义协议层面的经济激励。目前发展趋势是使用比特币闪电网络来完成支付功能。原生去中心化社交+原生去中心化支付，构成了生态闭环。

闪电网络是比特币的二层，是一个链下支付通道，最终结算在比特币网络上。可以为用户提供更便宜，更高效的比特币充提体验。

## 支持闪电网络的钱包

常用的闪电网络钱包
- https://bluewallet.io/ (不支持闪电地址)
- https://walletofsatoshi.com/ (支持闪电地址)

bluewallet比特币主网是助记词，闪电网络是`lndhub wallet backup`,长下面的样子
```
lndhub://5678...ju997@https://lndhub.io
```

walletofsatoshi是直接邮箱注册，全托管，没有助记词，对新手比较友好。

## BOLT

BOLT（Basis of Lightning Technology）协议是闪电网络的基础，它是对各种层面上通信和应用处理的一个全面的协议。具体标准查看官方github：https://github.com/lightning/bolts

### 支付

跟支付相关的bolt是bolt11，以及还没实施的bolt12

#### BOLT11 Lightning Invoice

Lightning Invoice（发票）是闪电网络一次性的收款地址，有具体数额。收款后就失效。

在闪电网络钱包的接收（receive）页面创建，需要输入具体数额。类似下面这样的地址,`lnbc`开头，`ln`是闪电网络，`bc`是比特币主网
```
lnbc1u1p370f5mpp5qkffutcv63z8f8nvg3p95k7938m40s2e24h45rxxp3sd8r0ecpesdqu2askcmr9wssx7e3q2dshgmmndp5scqzpgxqyz5vqsp5hkny0uvacea2sdway60tvvaxuxgypzz0qn5hm6fp7zsw274zu4pq9qyyssqkvwwt24s6y2c9kw0dqkyvcpw6zll367yr80q0rf3wjewa8ndgm2ymeeywrj33eg4pf2l4kkkqf08eu6f8k5dstxfdkjxzlwpwslkgegp00lg5f
```
得到这个地址后在nostr发post，nostr客户端自动将此地址转换成一个求打赏链接，效果如下
```
⚡ Pay with lightning 
```
当有人打赏后，钱包里就会收到比特币。

BOLT11 Lightning Invoice目前的问题是
- 只能使用一次，收款后就失效。
- 需要实时生成
- 只能接收不能发送

#### BOLT12 Offer

bolt12可以看作是bolt11的升级，解决了bolt11的问题
- 可以持久使用
- 能接收也能发送

关于发送，类似提款机，你扫码后会想你支付钱。持久使用比较重要，可以作为收款码使用，不必每次都要创建一个新的。但是目前bolt12还没有实施，只能等了。目前想要持久使用的替代方案是使用闪电地址。

#### lightning Address（闪电地址）

lightning Address（闪电地址）或 LNURL 并非闪电网络规范BOLT的一部分，实际上它是一种网络工程的运用。最终的支付方依然用利用闪电发票，只不过借助了服务端来获取发票。

闪电地址可读性非常高，并且是闪电网络持续的收款地址，可以一直使用。

下面是支持闪电地址的应用，可以挑选一个自己喜欢的后缀。
- https://lightningaddress.com/

我体验过的应用
- lightingTipBot，是一个telegram应用，加入后会发给你一个lightning Address，例如：`gaohongxiang@ln.tips`。收款在tg的频道机器人钱包里，总感觉有点怪。
- walletofsatoshi，是一个移动端钱包，收付款都在一个移动端钱包里。但是它好像自动生成闪电地址，例如：`machoroot85@walletofsatoshi.com`，这个前缀不是我想要的，但是没有找到修改前缀的功能。

得到闪电地址后，在nostr的设置里把这个地址填在`LN Address`里，然后主页会有一个闪电打赏标志
```
⚡
```
不同客户端可能显示不一样。

#### BOLT12与lightning Address的区别

bolt12是闪电网络的规范，是协议层的解决方案，而lightning Address/LNURL不是闪电网络的规范，是应用层的解决方案。这意味着bolt12具有更好的隐私，较小的集中化风险（DNS）和非技术用户的UX更好，并且不需要像lightning Address/LNURL那样需要Web服务器，TLS证书和域名等外部依赖项。

bolt11是一长串字符，而lightning Address是可读性很高的标识符。bolt12也是一长串字符吗？能不能也转成可读性很高的标识符？

# 体验流程

Nostr是自由的消息传递协议，比特币是自由的价值传递协议。两者结合，形成一个生态闭环。

1. 创建一个nostr账户。可以从任意nostr客户端创建（自动生成公私钥，保存好私钥），也可以先用安全方式生成公私钥，然后在客户端导入私钥（切记保护好私钥）
    - 浏览器：https://snort.social/
    - ios：Damus
    - Android：Amethyst
2. nostr账户设置一下用户名、头像等
3. 创建一个支持闪电网络的钱包（切记保护好`lndhub wallet backup`）
    - bluewallet：https://bluewallet.io/
    - walletofsatoshi：https://walletofsatoshi.com/
4. 创建一个闪电地址（3和4可以在一个钱包，也可以不同钱包）
    - lightningaddress集合：https://lightningaddress.com/ （暂时推荐walletofsatoshi，期待bluewallet支持此功能）
    - 我目前策略是主用bluewallet，nostr的闪电地址暂时用walletofsatoshi，然后用nostrplebs的`NIP-05 ID`转发闪电地址。如果未来bluewallet能用闪电地址功能肯定会切换过去。这样收付款、打赏都在一个钱包，方便。或者等bolt12成熟后会替代闪电地址？
5. 从交易所提点比特币到钱包
    - ok支持比特币闪电网络
6. 购买一个 `NIP-05 ID`
    - 付费推荐：https://nostrplebs.com/
    - 免费推荐：https://nostr.directory/
7. nostr账户设置 `NIP-05 ID`（替代公钥）
    - 将刚刚购买的 `NIP-05 ID`填到nostr账户设置的`NIP-05`里，替换公钥。别人通过这个域名就可以找到你
8. nostr账户设置 `LN Address`（这是一个持续性的收款地址，主页里会显示这个长期闪电网络地址）
    - 将闪电地址填到nostr账户设置的`LN Address`里。
    - 如果你`NIP-05`是购买的nostrplebs的服务，它提供一个转发功能，可以用你的`NIP-05 ID`链接到你的闪电地址。对外你的nostr id和闪电地址就统一了。强迫症患者的福音。[这是设置 Lightning 地址转发教程](https://help.nostrplebs.com/hc/nostrplebs/en/setup-use/6)
9. nostr发一个post，内容为`Lightning Invoice`（这是一个一次性的收款地址）。
    - 在闪电网络钱包里创建一个固定数额的`Lightning Invoice`，复制到nostr里，发一个post。有人打赏后你的钱包会收到比特币。
10. 多研究nostr生态应用
    - 国际象棋游戏：https://jesterui.github.io/
    - 私密聊天室‌：https://sendstr.com/

# 参考
- nostr文档：https://github.com/nostr-protocol/nostr
- nips：https://github.com/nostr-protocol/nips
- 聊聊Nostr那些事：社交协议、去代币经济、监管与破圈：https://8btcnews.substack.com/p/nostr?r=14dl19&utm_campaign=post&utm_medium=email
- Nostr 协议详解（科普篇）：https://juejin.cn/post/7196290097548722233
- 科普各地址：https://twitter.com/evilcos/status/1621380824867430400
- Nostr 爆火后，去中心化社交才刚刚开始：https://twitter.com/EvieEvieXia/status/1621824477339455489
- 聊聊 nostr：https://twitter.com/AurtrianAjian/status/1604892353478942721?s=20&t=QaVe_ENpk00K6G8bzpPR5w
- Damus一出，Nostr能「杀」死Twitter吗？：https://mirror.xyz/jeanchen.eth/nkOORLT96GxuuSBJ6pH0HQVovaiuY9kf7Pdi0ADMMO0
- NostrPlebs帮助文档：https://help.nostrplebs.com/hc/nostrplebs/en
- 深入解读闪电网络：探寻比特币支付通道的前世今生：http://www.odaily.site/post/5185000
- 闪电网络历史与现实：http://www.nfgyl.com/gbhq/9184.html
- 闪电网络简史：https://www.qklw.com/baike/20220723/256783.html
- blots：https://github.com/lightning/bolts
- blot11与blot12的对比：https://twitter.com/LN_Capital/status/1589321549894799360
- The Lightning Invoice：https://www.bolt11.org/
- offer：Lightning 的原生体验，无处不在：https://bolt12.org/