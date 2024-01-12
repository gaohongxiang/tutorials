# 全链互操作性协议——LayerZero教程
layerzero是一种原生跨链协议，无需信任的全链互操作性协议，是用于智能合约在链之间进行通信的消息传输层，它可以实现具有低级通信原语的跨链应用程序。它是加密桥接的一个新维度，旨在连接所有EVM链并统一流动性池。

进一步了解
- [LayerZero：无需信任的全链互操作性协议](https://mirror.xyz/searchblock.eth/EoxnJ2lXtK1yYdHhtXP9Ny_TM0cOGg8nmgOY9hv6wZE)
- [LayerZero-对用户极其友好的超轻节点跨链桥【Vic TALK 第64期】](https://www.youtube.com/watch?v=pu-GOA9hSIs)

当前LayerZero正以30亿美元估值筹集新资金，FTX Ventures承诺领投，融资内容包括 LayerZero代币、STG代币及LayerZero股权。3 月 31 日，LayerZero Labs 宣布完成 1.35 亿美元 A+ 轮融资，该轮融资由 FTX Ventures、红杉资本与 a16z 共同领投，Coinbase Ventures、PayPal Ventures、Tiger Global 和 Uniswap Labs 等参投，投后估值为 10 亿美元。此前 LayerZero Labs 曾于 2021 年 9 月完成 600 万美元 A 轮融资，该轮融资由 Multicoin 和 Binance Labs 领投。

layerzero官方github提到了他们的代币名称 $ZRO
![coin](https://s2.loli.net/2022/10/28/ijoXMDPfIOmKbps.png)

这是一个很有潜力的项目，并没有在社区透露太多信息。如果做成了可能会改变跨链生态的格局。这样的项目如果有空投的话，一定是很不错的，值得挑一些重点项目来交互。

## 一、跨链

### 使用bungee和stargate在eth系之间跨链

Stargate FinanceStargate 是一个完全可组合的流动性传输协议，是全链DeFi的核心。它是使用layerzero协议的第一个项目，官方背景，必交互。.bungee.exchange是一个跨链聚合器，除了常规跨链，还有一个`Refuel`功能，专门跨gas用的，新地址一般都没有gas，这个功能非常实用。

>方案：使用bungee跨链聚合器，跨链时选择stargate跨链桥，stargate建立在layerzero之上。这样既交互了bungee，又交互了layerzero（stargate已经发币），一鱼两吃。

- bungee网址：https://www.bungee.exchange/

![](https://s2.loli.net/2022/11/16/zsGB2VFHjwTYgRd.png)

注：2022/09/21 02:00 - 2023/01/18 02:00 GMT+09:00有个Optimism Quests活动，stargate也在其中，只要在optimism跨链满100u即可。两个项目可以同时参与。

## 使用theaptosbridge和liquidswap在以太坊和Aptos之间跨链

- [LayerZero 的 Aptos 桥](https://medium.com/layerzero-official/the-aptos-bridge-by-layerzero-5117030afd4f)

近期aptos大热，layerzero也已经在aptos上部署。theaptosbridge是LayerZero推出的Aptos Bridge。liquidswap是layerzero的合作伙伴（当然，还有其他的项目，我用的liquidswap）。这两个都可以在以太坊和Aptos之间桥接。你可以通过theaptosbridge将 USDC、USDT 和 ETH 从 Etheruem、Arbitrum、Optimism、Avalanche、Polygon 和 BNB 链转移到 Aptos，然后用liquidswap反向操作回来，这样两个桥都交互了。

这两个桥是需要连接两个钱包的，一个eth系的钱包，可以用metamask，一个aptos系的钱包，可以用 [Pontem Aptos Wallet](https://chrome.google.com/webstore/detail/pontem-aptos-wallet/phkbamefinggmakgklpkljjmgibohnba)

>注意时间，从aptos往外桥接需要3天时间才会到账。桥接到aptos等待十几分钟就会到账。

我的方案是使用 theaptosbridge 将资产从eth系的链桥接到aptos链，然后使用 liquidswap 将资产再从aptos链桥接回来。

### 资产通过theaptosbridge从eth系链桥接到aptos链
- theaptosbridge网址：https://theaptosbridge.com/bridge

![theaptosbridge](https://s2.loli.net/2022/10/30/F3ioM94ClVhRpwW.jpg)
如上图所示，上面连接metamask钱包，下面连接aptos的Pontem Aptos Wallet钱包。选择要转移的币种（会自动显示你在哪条链上有资产），比如图中我的是在op上的usdc。下面选择aptos网络。选好之后transfer即可，需要等待较长时间。如果是第一次在aptos上与资产交互的话，需要手动claim资产，见上图。

### 资产通过liquidswap从aptos链桥接到eth系
- liquidswap网址：https://bridge.liquidswap.com/

![liquidswap从aptos桥接回eth系](https://s2.loli.net/2022/10/30/KyYCm3f6kdwRape.png)
跟上一步类似，反向操作即可。需要注意的是，从aptos往外桥接需要3天时间才会到账！

## 二、stargate

### 成为 Stargate Finance 的 DAO 投票者

首先购买Stargate Financ的代币STG（5个左右即可）。STG合约去 [coinmarketcap](https://coinmarketcap.com/currencies/stargate-finance/) 复制。目前六条链上都有，想在哪条链上买就选择对应链上的合约，拿到合约后去对应链上的dex去swap即可
![STG](https://s2.loli.net/2022/10/29/gILb72YRrQ86ZyS.png)

然后到 [Stargate Finance Staking](https://stargate.finance/stake) 选项卡质押你的STG代币。数量和期限自己定。
![stake STG](https://s2.loli.net/2022/10/29/7Zx1gGNu3mj9KLE.png)
质押了代币后，获得veSTG，就获得了投票权，成为Stargate Finance 的 DAO 投票者。定期对 [Stargate Finance 治理提案](https://snapshot.org/#/stgdao.eth) 进行投票（目前没有投票提案，时刻关注，有提案了就来投票，增加参与度）

### farming

你也可以去 [农场](https://stargate.finance/farm) 体验一下，挖矿获得STG收益，目前稳定币收益年化在5-6%左右
![farm](https://s2.loli.net/2022/10/29/gwQCvbeUW3VKLEx.png)

![Gh0stly Gh0sts](https://s2.loli.net/2022/10/29/Isa9ro6CLph3W5t.png)

## 三、USDC 零层桥接
这是官方一个测试demo，它是第 0 层的桥梁，我们可以使用它在 EVM 链之间发送 USDC。
网址：[usdcdemo](https://usdcdemo.layerzero.network/bridge)

- USDC 歌尔力合约地址： 0x07865c6E87B9F70255377e024ace6630C1Eaa37F
- USDC Avax 合约地址： 0x5425890298aed601595a70AB815c96711a31Bc65

领goerli测试网的usdc可以从网站左下角的`Faucet USDC`处领，也可以通过uniswap去兑换。领不到usdc的情况下就用兑换的方式。

`NOT ENOUGH NATIVE FAO GAS` 这个提示是因为gas不够，要再去领点。不知为啥，这个gas消耗巨高，大概0.15个左右
![usdcdemo](https://s2.loli.net/2022/10/29/qZ4KuNVwDycJSBd.png)

## 四、Radiant
Radiant 是一个部署在 Arbitrum 上基于 LayerZero/Stargate 的多链借贷项目。由于arbitrun和layerzero都未发币，所以我们交互radiant可以做到一鱼多吃。

- 教程：https://mirror.xyz/0x891dDE39445f54bc6f1b73e89398f96e7851B4ba/LQ6H3D9-Ta-q6V2zmfFCt8zFwxQsKQGCNQZPITrCenQ

## 五、Gh0stly Gh0sts

Gh0stly Gh0sts（小幽灵）是LayerZero官方发布的第一个真正意义上的“全链NFT”。总量7710个。如果想通过它博空投，只能去二级市场购买。目前地板价0.12ETH（2022/10/29）。根据推特的信息展示，小幽灵将会发布一款小游戏。不清楚是否会对NFT持有者进行空投。
- [Gh0stly Gh0sts 推特](https://twitter.com/gh0stlygh0sts)
- [Gh0stly Gh0sts opensea](https://opensea.io/zh-CN/collection/gh0stlygh0sts)
- [那个可以跨链的Gh0stlyGh0sts会成为下一个Azuki吗？](https://jason.mirror.xyz/3dINEPmJ0UqpvUtvwgFYZ33xkHdmxTQRdhDppexsWLg)

## 其他

上面几个项目权重比较大，建议交互。其他项目自行研究吧。