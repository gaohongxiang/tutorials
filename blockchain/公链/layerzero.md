# 全链互操作性协议——LayerZero教程
layerzero是一种原生跨链协议，无需信任的全链互操作性协议，是用于智能合约在链之间进行通信的消息传输层，它可以实现具有低级通信原语的跨链应用程序。它是加密桥接的一个新维度，旨在连接所有EVM链并统一流动性池。

进一步了解
- [LayerZero：无需信任的全链互操作性协议](https://mirror.xyz/searchblock.eth/EoxnJ2lXtK1yYdHhtXP9Ny_TM0cOGg8nmgOY9hv6wZE)
- [LayerZero-对用户极其友好的超轻节点跨链桥【Vic TALK 第64期】](https://www.youtube.com/watch?v=pu-GOA9hSIs)

当前LayerZero正以30亿美元估值筹集新资金，FTX Ventures承诺领投，融资内容包括 LayerZero代币、STG代币及LayerZero股权。3 月 31 日，LayerZero Labs 宣布完成 1.35 亿美元 A+ 轮融资，该轮融资由 FTX Ventures、红杉资本与 a16z 共同领投，Coinbase Ventures、PayPal Ventures、Tiger Global 和 Uniswap Labs 等参投，投后估值为 10 亿美元。此前 LayerZero Labs 曾于 2021 年 9 月完成 600 万美元 A 轮融资，该轮融资由 Multicoin 和 Binance Labs 领投。

layerzero官方github提到了他们的代币名称 $ZRO
![coin](https://s2.loli.net/2022/10/28/ijoXMDPfIOmKbps.png)

这是一个很有潜力的项目，并没有在社区透露太多信息。如果做成了可能会改变跨链生态的格局。这样的项目如果有空投的话，一定是很不错的，值得挑一些重点项目来交互。

## Stargate Finance

Stargate FinanceStargate 是一个完全可组合的流动性传输协议，是全链DeFi的核心。它是使用layerzero协议的第一个项目，官方背景，必交互。

### 1、成为 Stargate Finance 的 DAO 投票者

首先购买Stargate Financ的代币STG（5个左右即可）。STG合约去 [coinmarketcap](https://coinmarketcap.com/currencies/stargate-finance/) 复制。目前六条链上都有，想在哪条链上买就选择对应链上的合约，拿到合约后去对应链上的dex去swap即可
![STG](https://s2.loli.net/2022/10/29/gILb72YRrQ86ZyS.png)

然后到 [Stargate Finance Staking](https://stargate.finance/stake) 选项卡质押你的STG代币。数量和期限自己定。
![stake STG](https://s2.loli.net/2022/10/29/7Zx1gGNu3mj9KLE.png)
质押了代币后，获得veSTG，就获得了投票权，成为Stargate Finance 的 DAO 投票者。定期对 [Stargate Finance 治理提案](https://snapshot.org/#/stgdao.eth) 进行投票（目前没有投票提案，时刻关注，有提案了就来投票，增加参与度）

### 2、跨链转移资产

跨过去用stargate的Transfer功能，跨回来可以交互其他使用layerzero的产品，比如：Sushiswap、Hashflow等。这样即交互了stargate又交互了其他应用。同时，现在有个OP奥德赛活动，只要`通过其他链向op链转最少100usdc`，即可奖励一个nft，一举三得。

#### 资产通过stargate从其他链跨到OP
打开 [stargate Transfer](https://stargate.finance/transfer)，From部分选你有资产同时又便宜的链，token目前支持USDT、USDC。比如我选的bsc链，转USDT。To的部分链必须选OP链，token必须选USDC。然后等待转过去即可。
![transfer](https://s2.loli.net/2022/10/29/Ofr89PTWumoe4E5.png)
#### 领取OP活动nft
过一个小时左右去 [galxe](https://galxe.com/Optimism/campaign/GC6HiUtSAs) 领nft
![op_stargate](https://s2.loli.net/2022/10/29/ZQFciKkyeMpLgn6.png)

比较可惜的是，stargate官方自己还出了一个nft活动，做完上述任务可以领。但是10.3号已经结束了。就是下面这个nft，领了nft可以使用转移功能，做交互，不知道以后还会不会有。
网址也放在这了：[galxestargatoronft](https://www.galxestargatoronft.com/mint)
![nft](https://s2.loli.net/2022/10/29/kJmjOQlNw3bHs2a.png)

#### 资产通过Sushiswap从OP跨回来
>如果想交互下面提到的farming的话，就先不要跨回来啦。

进入 [sushi官网](https://www.sushi.com/xswap)，选择`xswap`，走跨链。这个底层也是用的layerzero协议。
![sushi](https://s2.loli.net/2022/10/29/s1wKInhS2uZWRGf.png)

### 3、farming交互
你也可以去 [农场](https://stargate.finance/farm) 体验一下，挖矿获得STG收益，目前稳定币收益年化在5-6%左右
![farm](https://s2.loli.net/2022/10/29/gwQCvbeUW3VKLEx.png)

## USDC 零层桥接
这是官方一个测试demo，它是第 0 层的桥梁，我们可以使用它在 EVM 链之间发送 USDC。
网址：[usdcdemo](https://usdcdemo.layerzero.network/bridge)

- USDC 歌尔力合约地址： 0x07865c6E87B9F70255377e024ace6630C1Eaa37F
- USDC Avax 合约地址： 0x5425890298aed601595a70AB815c96711a31Bc65

领goerli测试网的usdc可以从网站左下角的`Faucet USDC`处领，也可以通过uniswap去兑换。领不到usdc的情况下就用兑换的方式。

`NOT ENOUGH NATIVE FAO GAS` 这个提示是因为gas不够，要再去领点。不知为啥，这个gas消耗巨高，大概0.15个左右
![usdcdemo](https://s2.loli.net/2022/10/29/qZ4KuNVwDycJSBd.png)

## liquidswap交互

它是第零层上的一座桥梁，我们可以使用它在以太坊和 Aptos 之间发送代币。近期aptos大热，很有必要交互一下。网址：[liquidswap](https://bridge.liquidswap.com/)

上面连接metamask钱包，下面连接aptos的petra钱包（其他的aptos钱包也可以）。选择要转移的币种（会自动显示你在哪条链上有资产），比如图中我的是在op上的usdc。选好之后transfer即可，需要等待较长时间。
![liquidswap](https://s2.loli.net/2022/10/29/HWEY9N8vhcRpbLk.png)

## Gh0stly Gh0sts

Gh0stly Gh0sts（小幽灵）是LayerZero官方发布的第一个真正意义上的“全链NFT”。总量7710个。如果想通过它博空投，只能去二级市场购买。目前地板价0.12ETH（2022/10/29）。根据推特的信息展示，小幽灵将会发布一款小游戏。不清楚是否会对NFT持有者进行空投。
- [Gh0stly Gh0sts 推特](https://twitter.com/gh0stlygh0sts)
- [Gh0stly Gh0sts opensea](https://opensea.io/zh-CN/collection/gh0stlygh0sts)
- [那个可以跨链的Gh0stlyGh0sts会成为下一个Azuki吗？](https://jason.mirror.xyz/3dINEPmJ0UqpvUtvwgFYZ33xkHdmxTQRdhDppexsWLg)

![Gh0stly Gh0sts](https://s2.loli.net/2022/10/29/Isa9ro6CLph3W5t.png)

## 其他

上面几个项目权重比较大，建议交互。其他项目自行研究吧。