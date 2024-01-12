zkSync——以太坊第二层零知识汇总扩展终极解决方案。采用zk-rollups技术。

目前zksync1.0主网于2020年6月15日在以太坊主网上上线。

- 官网：https://zksync.io/

zksync2.0主网分三个阶段，目前处于第一阶段。

- 第一阶段Baby Alpha。2022年10月28号开启，预计持续一个月的时间，该阶段内网络将在没有任何外部应用程序开放使用的情况下运行，任何外部参与者无法使用，初始阶段仅用于压力测试和安全工作。
- 第二阶段Fair Onboarding Alpha。第一阶段测试完成后，在11月下旬会进入到第二阶段，在该阶段团队将会开放生态开发者的项目部署权限，以便开发者能够测试并升级其合约，目前已有超过100个项目表示有兴趣在zkSync 2.0上部署其应用程序，该阶段将会持续几个月的时间。
- 第三阶段Full Launch Alpha。预计在明年上半年进入第三阶段，该阶段将是网络对所有人完全开放的时候，届时普通用户也可以真正在zkSync2.0上体验其产品。

交互

- zksync1.0主网
- zksync2.0测试网

# zksync1.0主网交互

官方交互文档：https://docs.zksync.io/userdocs/tutorials/#add-funds-to-zksync-with-metamask

Discord 中给出的zksync 任务单：

1. 从以太坊主链跨链到zksync 上，做几笔转账。
2. Mint zksync NFTs
印几个nft ，教程：[docs.zksync.io/userdocs/tutor…](http://docs.zksync.io/userdocs/tutor%E2%80%A6)
3. 创建付款链接：
[checkout.zksync.io/link](http://checkout.zksync.io/link)
4. 为Gitcoin 捐款，并用zksync 支付:
[gitcoin.co/grants](http://gitcoin.co/grants)
5. 在ZigZag上交易几笔
trade.zigzag.exchange
6. 在Argent上创建zksync 账户
Argent wallet 可以直接使用Yearn, Lido dapp
7. 玩一下这个元宇宙游戏Tevaera
[tevaera.com](http://tevaera.com/)
8. 完成这个rss3任务
[rss3.io](http://rss3.io/)
9. 导出你的zksync 1.0地址交易记录：
httns://kexnort netlifv.ann/

下图基本涵盖了zksync1.0主网的主要交互。

![zksync1.0](https://s2.loli.net/2022/11/04/hcfP56NBdkMD1SC.png)

- 资产从以太坊主网桥接到zksync1.0主网
- 转账功能
- zigzag兑换功能
- 铸造nft
- gitcoin捐赠通过zksync1.0付款

## 资产在以太坊主网与zksync1.0主网之间桥接

官方桥、zigzag桥、orbiter桥、layerswap桥
其中orbiter支持在以太坊、zkSync、Arbitrum、Optimism、Polygon之间转移ETH和USDC、USDT、DAI

### 走官方桥

- https://wallet.zksync.io/account

eth桥接到zksync1.0

![eth桥接到zksync1.0](https://s2.loli.net/2022/11/04/3TACDb9tZJ7oQiW.jpg)

zksync1.0桥接到eth

![zksync1.0桥接到eth](https://s2.loli.net/2022/11/04/CFnON5QyPBRIXSE.jpg)

### 走orbiter桥
- https://www.orbiter.finance/

orbiter支持在以太坊、zkSync、Arbitrum、Optimism、Polygon之间转移ETH和USDC、USDT、DAI。如果在其他二层有资产，用这个桥就非常方便了。

![](https://s2.loli.net/2022/11/17/HJVrRoMw3Ki6lqL.png)

### 走zigzag桥

- https://trade.zigzag.exchange/bridge

eth桥接到zksync1.0

![zigzag桥接](https://s2.loli.net/2022/11/04/AbePs5YimhaB6Fr.png)

zksync1.0桥接到eth只需要将上图中网络互换即可。

## 转账

- https://wallet.zksync.io/account

![zksync1.0转账](https://s2.loli.net/2022/11/04/LUzTFqr7SGXNVxB.jpg)

## zigzag swap

- https://trade.zigzag.exchange/?market=ETH-USDC&network=zksync

swap大同小异，没什么好说的。注意右上角网络选zksync1.0即可。

![zigzag swap](https://s2.loli.net/2022/11/04/F4tn3YvSla5BOrN.png)

## nft

- https://wallet.zksync.io/account

### mint nft

![mint nft](https://s2.loli.net/2022/11/04/ER1jHixWklAsSt2.jpg)

mint nft 时需要自己填写cid，可以用pinata上传图片获得cid

- https://app.pinata.cloud/

![nft cid](https://s2.loli.net/2022/11/04/6CbLcsdZr1KxB2E.png)

### transfer nft on zksync

![transfer nft on zksync](https://s2.loli.net/2022/11/04/XfJj3RzF2Dexy6w.jpg)

### transfer nft to eth

![transfer nft to eth](https://s2.loli.net/2022/11/04/zGxcn3twZmI7OWJ.jpg)

## gitcoin捐赠通过zksync1.0付款

- https://gitcoin.co/grants/

gitcoin需要使用github登陆，没有github先注册：https://github.com/

![](https://s2.loli.net/2022/11/16/8JreQfHh3MK1snA.jpg)
![](https://s2.loli.net/2022/11/16/7iReV5DwJapZzTA.jpg)

## crew3
- https://zksync.crew3.xyz/questboard

所有答案如下

zkSync Roadmap：
1. Baby Alpha
2. 最长选项
3. Fair Onboarding Alpha
4. Full Launch Alpha
5. 最长选项

Open Source is Freedom：
1. freedom to view, change, fork
2. Fair Onboarding Alpha
3. ecosystem
4. Spicy

## argent x钱包

# zksync2.0测试网交互

## 领水

1. 直接在zksync2.0测试网官网水龙头领水 [https://portal.zksync.io/faucet](https://portal.zksync.io/faucet)
2. 如果zk测试网没有eth，可以从以太坊goerli测试网领水（link：[https://faucets.chain.link/](https://faucets.chain.link/)、alchemy：[https://goerlifaucet.com/](https://goerlifaucet.com/)），然后走zksync2.0官方测试网跨链桥跨链到zksync [https://portal.zksync.io/bridge](https://portal.zksync.io/bridge)
3. 如果以太坊goerli测试网也没eth，可以从bsc测试网[https://testnet.binance.org/faucet-smart](https://testnet.binance.org/faucet-smart) 领bnb，然后去zetachain [https://labs.zetachain.com/leaderboard](https://labs.zetachain.com/leaderboard) 跨链成goerli测试网eth ，然后走zksync2.0官方测试网跨链桥跨链 [https://portal.zksync.io/bridge](https://portal.zksync.io/bridge)
4. 都不行的话去推特搜，碰运气

获取Goerli ETH测试币方法汇总：
- https://mirror.xyz/newairdrops.eth/RcXK5YrgCmw30I75kW2kkdKwHVsKHCUuGJ57fOFigdM
- https://mirror.xyz/noblexx.eth/0TUq9OfFZs-ksUGJMqS57_wjIWclzScbIIArczrEXvo

## 应用交互

官方列出的应用
- https://matterlabs.notion.site/zkSync-2-0-Testnet-Applications-e38328bccda7472793024a25e26a1cac
- https://ecosystem.zksync.io/(所有的)

社区整理应用
- https://airtable.com/shrt6eK6LvZvUXvOL/tblkUgJJmbCiqr6Fl/viwbsQt9FsWjU64P1
- https://aramz.super.site/alpha/zksync
- https://m.defidaonews.com/article/6784356

另外官方推特会不断更新最新的dapp，持续关注并交互
- https://twitter.com/zksync/status/1592170192808779776