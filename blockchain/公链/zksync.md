zkSync——以太坊第二层零知识汇总扩展终极解决方案。采用zk-rollups技术。

目前zksync1.0主网于2020年6月15日在以太坊主网上上线。

zksync2.0主网分三个阶段，目前处于第一阶段。

- 第一阶段Baby Alpha。2022年10月28号开启，预计持续一个月的时间，该阶段内网络将在没有任何外部应用程序开放使用的情况下运行，任何外部参与者无法使用，初始阶段仅用于压力测试和安全工作。
- 第二阶段Fair Onboarding Alpha。第一阶段测试完成后，在11月下旬会进入到第二阶段，在该阶段团队将会开放生态开发者的项目部署权限，以便开发者能够测试并升级其合约，目前已有超过100个项目表示有兴趣在zkSync 2.0上部署其应用程序，该阶段将会持续几个月的时间。
- 第三阶段Full Launch Alpha。预计在明年上半年进入第三阶段，该阶段将是网络对所有人完全开放的时候，届时普通用户也可以真正在zkSync2.0上体验其产品。

交互

- zksync1.0主网
- zksync2.0测试网

# zksync1.0主网交互

下图基本涵盖了zksync1.0主网的主要交互。

![zksync1.0](https://s2.loli.net/2022/11/04/hcfP56NBdkMD1SC.png)

- 资产从以太坊主网桥接到zksync1.0主网
- 转账功能
- zigzag兑换功能
- 铸造nft
- gitcoin捐赠通过zksync1.0付款

## 资产在以太坊主网与zksync1.0主网之间桥接

建议走官方桥或者zigzag桥。二选一即可

### 走官方桥

- https://wallet.zksync.io/account

eth桥接到zksync1.0

![eth桥接到zksync1.0](https://s2.loli.net/2022/11/04/3TACDb9tZJ7oQiW.jpg)

zksync1.0桥接到eth

![zksync1.0桥接到eth](https://s2.loli.net/2022/11/04/CFnON5QyPBRIXSE.jpg)

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