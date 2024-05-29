# Bitcoin 钱包离线地址生成和签名实战

# 一. Bitcoin 概览

## 1. Bitcoin 简介

比特币（Bitcoin）是由一个或一群化名为中本聪（Satoshi Nakamoto）的人在 2008 年提出，并于 2009 年开始发布的去中心化数字货币。它的出现标志着一种新型金融体系的诞生，具有以下主要特点：

**去中心化** 

💡💡比特币网络没有中央管理机构或中介机构，它通过一个称为区块链的分布式账本来记录所有交易。 

💡💡区块链技术确保了所有参与者都可以查看和验证交易，提高了透明度和安全性。



**有限供应** 

💡💡比特币的总供应量被固定在2100万枚，这通过代码内的机制控制。 

💡💡这一机制使比特币具备了抗通胀的特点。



**点对点交易** 

💡💡比特币允许用户之间直接进行交易，不需要通过银行或支付处理机构。

 💡💡这种点对点交易减少了交易成本和处理时间。



**安全性** 

💡💡比特币使用加密技术确保交易的安全性和隐私性。 

💡💡每一笔交易都需要通过复杂的数学算法进行验证，从而确保系统的完整性。



**不可逆交易** 

💡💡一旦比特币交易被确认，它就无法被撤销。这减少了欺诈的风险，但也意味着用户需要谨慎操作。



**挖矿机制** 

💡💡新的比特币通过一个称为挖矿的过程产生。矿工们使用计算能力来解决复杂的数学问题，成功解决问题的矿工将获得比特币奖励。 

💡💡挖矿不仅生成新币，还维护和保护比特币网络的安全。



**全球流通** 

💡💡比特币可以在全球范围内进行交易，不受地域和国界的限制。 

💡💡它为跨国支付提供了一种高效、低成本的替代方案。



**隐私和匿名性** 

💡💡虽然比特币交易是公开记录的，但用户身份是匿名的。交易是通过地址（类似于账号）进行的，而不是通过真实身份。



## 2.Bitcoin 的升级次数介绍

比特币自 2009 年发布以来，经历了多次重要升级。这些升级旨在提高比特币网络的安全性、效率和功能性。

并且比特币的 Taproot 升级给比特币带来更多的可能性，Taproot 升级带动了 BRC20 和 Bitcoin-Layer2 的发展，这里我们不在做过多的介绍，在未来的 Layer2. 的课程中我们会深入讲解这部分的内容

**P2SH（Pay-to-Script-Hash）** 

💡💡时间：2012年 

💡💡BIP（Bitcoin Improvement Proposal）：BIP-0016 

💡💡内容：允许更复杂的交易脚本，支持多重签名地址。这种升级使得比特币交易更加灵活和安全



**比特币改进提案 BIP 66** 

💡💡时间：2015年 

💡💡BIP：BIP-0066 

💡💡内容：规范了交易中DER格式的签名，解决了交易签名的一致性问题，增强了网络的安全性。



**CheckSequenceVerify（CSV）** 

💡💡时间：2016年 

💡💡BIP：BIP-0112 

💡💡内容：增加了相对时间锁功能，使得交易可以在指定的时间或块高度之后才生效，这为更复杂的支付通道铺平了道路。



**Segregated Witness（SegWit）** 

💡💡时间：2017年 

💡💡BIP：BIP-0141, BIP-0143, BIP-0144 

💡💡内容：分离交易签名数据，提高了区块的有效容量，减小了交易体积，降低了交易费用。还解决了交易的可塑性问题，使得闪电网络等侧链解决方案成为可能。



**隔离见证2x（SegWit2x）** 

💡💡时间：2017年 

💡💡内容：这次升级是对 SegWit 的后续提案，旨在进一步增加区块大小。然而，由于社区共识未达成，这次升级最终未能实现。



**Taproot** 

💡💡时间：2021年 

💡💡BIP：BIP-0340, BIP-0341, BIP-0342 

💡💡内容：引入了 Schnorr 签名和默克尔化抽象语法树（MAST），增强了隐私性和执行脚本验证的功能，进一步提高了交易的灵活性和效率。这是自2017年 SegWit 以来最重要的一次升级。



 💡💡💡💡**MAST（Merkelized Abstract Syntax Trees）** 

💡💡💡💡💡💡时间：2021年

 💡💡💡💡💡💡BIP：作为 Taproot 的一部分 

💡💡💡💡💡💡内容：允许将多种条件的智能合约合并为一个，使得只有满足条件的部分才被公开，提高了隐私性和效率。 



💡💡💡💡**Schnorr Signatures** 

💡💡💡💡💡💡时间：2021年 

💡💡💡💡💡💡BIP：作为 Taproot 的一部分 

💡💡💡💡💡💡内容：提供了一种更高效和安全的签名算法，允许多重签名聚合，提高了交易的隐私性和可扩展性。



## 3.UTXO 介绍

UTXO（Unspent Transaction Output，未花费交易输出）模型是比特币及其他一些加密货币使用的一种记账方法。它与账户模型不同，通过追踪未花费的交易输出来记录每个地址的余额。以下是UTXO模型的详细介绍：



**模型解说**

在解说 UTXO 模型之前，我想先说说一个名词，叫做 Transaction，即交易，Transaction 和 UTXO 是相辅相成的，下面先来举个例子：

张三：通过挖矿得到了120个比特币，现在张三饿了，想要花费2个比特币去购买一个面包。这是只是举例而已，以现在比特币的价格，1个比特币就可以买到一大堆面包了。

现在我们假设，李四是面包店营业主

这样的话，在比特币中，张三付给李四将一次性付给李四 120 块，李四给张三找零 118 块，这里其实产生了两笔交易，咱们可以这样理解，张三的钱被分成了两份，其中 118 打给了自己，剩下的 2 块打给了李四。

交易与交易之间组成了网状关系，1 个交易的输出，成为了下 1 个交易的输入；下 1 个交易的输出，又成了下下 1 个交易的输入。所有的钱，在这个网络中流动，每 1 笔钱的去向、来源，都是可追溯的，而这也是区块链网络的一个重要特点。

上面的讲解可能大多数人都比较懵逼，接下来咱们用比较通俗的方式来说明 UTXO 和 Transaction 到底是什么

在现实生活中，一笔转账对应的事一个付款人和一个收款人，而在比特币种，一笔转账对应的事多个转账人和多个收款人。

现在咱们仔细分析一下上面的这个例子，对于张三买面包这个案列

- 付款人：张三 120块
- 收款人：张三 118块，李四 2块

张三的120，转118块给自己，转2块给李四，对应到交易里面，就是这笔交易有1个输入，2个输出！

下面来一个多输入，多输出的案例

考虑如下场景：用户A和用户B之间发生了一个交易T3，A 向 B 转 100 元。 A的100元，来自T1：C向A转的80元 + T2：D向A转的30元（共110元，但A只转了100元给B，10元找零返回给A的账号）。 同理，C向A转的这80元，来自用户E、F的某次交易...... D向A转的这30元，来自用户E的某次交易......

这个交易就有2个输入，2个输出： 2个输入（也就是2个UTXO）： T1: C向A转的80元 T2：D向A转的30元

2个输出： B：100元 A：10元（找零）

当你理解上面的例子时，我们再来说一下UTXO，理解上面的例子对你理解UTXO会特别有帮助。

- 比特币的交易中不是通过账户的增减来实现的，而是一笔笔关联的输入/输出交易事务。

- 每一笔的交易都要花费“输入”，然后产生“输出”，这个产生的“输出”就是所谓的“未花费过的交易输出”，也就是UTXO。每一笔交易事务都有一个唯一的编号，称为交易事务ID，这是通过哈希算法计算而来的，当需要引用某一笔交易事务中的“输出”时，主要提供交易事务ID和所处“输出”列表中的序号就可以了。

- 由于没有账户的概念，因此当“输入”部分的金额大于所需的“输出”时，必须给自己找零，这个找零也是作为交易的一部分包含在“输出”中。

- 旧的 UTXO不断消亡，新的UTXO不断产生。所有的UTXO，组成了UTXO Set 的数据库，存在于每个节点

- 任何1笔 UTXO，有且仅可能被1个交易花费1次

- 1 个 UTXO，具有如下的表达形式： 1 个 UTXO = 1个Transaction ID + Output Index

  

**2. UTXO模型的区块链钱包余额形式**

深刻理解了UTXO的概念，钱包就很容易理解了，某个人的钱包的余额 = 属于他的UTXO的总和；在这里，你会发现一个不同于现实世界的“银行”里的一个概念，在银行里，会存储每个账号剩余多少钱。但这里，我们存储的并不是每个账号的余额，而存的是1笔笔的交易，也就是1 笔笔的UTXO，每个账户的余额是通过UTXO计算出来的，而不是直接存储余额。



# 二. Bitcoin 钱包地址类型

Bitcoin 钱包地址有几种不同的类型，每种类型都有其特定的用途和特点。主要的几种类型包括：



**P2PKH（Pay-to-PubKeyHash）地址** 

💡💡格式：以1开头，例如，1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa。

 💡💡特点：这是最传统和最常见的地址类型，广泛用于比特币的早期交易。

 💡💡优点：兼容性好，几乎所有钱包和交易所都支持。 

💡💡缺点：随着时间的推移，这种地址类型的使用效率较低，交易费用可能会较高。



**P2SH（Pay-to-Script-Hash）地址** 

💡💡格式：以3开头，例如，3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy。 

💡💡特点：这种地址允许更复杂的交易脚本，例如多重签名地址。

 💡💡优点：支持更复杂的交易和脚本，安全性更高。 

💡💡缺点：创建和管理比P2PKH地址更复杂。



**Bech32（SegWit）地址** 

💡💡格式：以 bc1 开头，例如，bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwfvenl。 

💡💡特点：这是比特币改进提案BIP-0173中引入的新地址格式，旨在提高交易效率和减少费用。 

💡💡优点：交易费用更低，处理速度更快，且有助于减少交易体积。 

💡💡缺点：并非所有的钱包和交易所都支持这种地址类型，尽管支持率在逐步增加。

每种地址类型都有其特定的应用场景和优缺点，用户可以根据自己的需求选择合适的地址类型来存储和交易比特币



# 三. Bitcoin 离线地址生成代码

## NodeJs 代码

```javascript
export function createAddress (params: any): any {
    const {seedHex, receiveOrChange, addressIndex, network, method } = params
    const root = bip32.fromSeed(Buffer.from(seedHex, 'hex'));
    let path = "m/44'/0'/0'/0/" + addressIndex + '';
    if (receiveOrChange === '1') {
        path = "m/44'/0'/0'/1/" + addressIndex + '';
    }
    const child = root.derivePath(path);
    let address: string
    switch(method) {
        case "p2pkh":
            const p2pkhAddress = bitcoin.payments.p2pkh({
                pubkey: child.publicKey,
                network: bitcoin.networks[network]
            });
            address = p2pkhAddress.address
            break
        case "p2wpkh":
            const p2wpkhAddress = bitcoin.payments.p2wpkh({
                pubkey: child.publicKey,
                network: bitcoin.networks[network]
            });
            address = p2wpkhAddress.address
            break
        case "p2sh":
            const p2shAddress = bitcoin.payments.p2sh({
                redeem: bitcoin.payments.p2wpkh({
                    pubkey: child.publicKey,
                    network: bitcoin.networks[network]
                }),
            });
            address = p2shAddress.address
            break
        default:
            console.log("This way can not support")
    }

    return {
        privateKey: Buffer.from(child.privateKey).toString('hex'),
        publicKey: Buffer.from(child.publicKey).toString('hex'),
        address
    };
}
```

代码中通过 method 来控制生成的地址类别



# 四. Bitcoin 离线签名代码

```javascript
export function buildAndSignTx (params: { privateKey: string; signObj: any; network: string; }): string {
    const { privateKey, signObj, network } = params;
    const net = bitcore.Networks[network];
    const inputs = signObj.inputs.map(input => {
        return {
            address: input.address,
            txId: input.txid,
            outputIndex: input.vout,
            script: new bitcore.Script.fromAddress(input.address).toHex(),
            satoshis: input.amount
        }
    });
    const outputs = signObj.outputs.map(output => {
        return {
            address: output.address,
            satoshis: output.amount
        };
    });
    const transaction = new bitcore.Transaction(net).from(inputs).to(outputs);
    transaction.version = 2;
    transaction.sign(privateKey);
    return transaction.toString();
}
```
