[ES6入门-async(阮一峰)](https://es6.ruanyifeng.com/#docs/async)

async/await使用同步方式实现异步，类似于promise调用then方法链式回调，但更容易理解阅读

async声明函数里有异步操作，await表示等待（只能在async函数内部使用），相当于promise里的then。一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

async函数可以看作多个异步操作包装成的一个Promise 对象。async函数返回值是 Promise 对象，可以使用then方法添加回调函数。
```
const get_Balance = async (address) => {
    const token_balance = ethers.utils.formatEther(await provider.getBalance(address))
    // console.log(`钱包 ${address} ETH余额: ${token_balance} ETH`);
    return token_balance;
}
get_Balance("0x12E216FE19D38194d51ec7Fb6a37C30bf9D6b026").then(conlose.log);
```
