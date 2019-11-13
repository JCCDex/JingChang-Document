
# JingChang-Document

井畅接口文档

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [JingChang-Document](#jingchang-document)
  - [1 井畅服务端接口文档](#1-%E4%BA%95%E7%95%85%E6%9C%8D%E5%8A%A1%E7%AB%AF%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3)
  - [2 井畅钱包接口文档](#2-%E4%BA%95%E7%95%85%E9%92%B1%E5%8C%85%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3)
    - [2.1 安装与引入](#21-%E5%AE%89%E8%A3%85%E4%B8%8E%E5%BC%95%E5%85%A5)
    - [2.2 创建井通钱包或联盟链钱包](#22-%E5%88%9B%E5%BB%BA%E4%BA%95%E9%80%9A%E9%92%B1%E5%8C%85%E6%88%96%E8%81%94%E7%9B%9F%E9%93%BE%E9%92%B1%E5%8C%85)
    - [2.3 验证井通钱包或联盟链钱包地址的合法性](#23-%E9%AA%8C%E8%AF%81%E4%BA%95%E9%80%9A%E9%92%B1%E5%8C%85%E6%88%96%E8%81%94%E7%9B%9F%E9%93%BE%E9%92%B1%E5%8C%85%E5%9C%B0%E5%9D%80%E7%9A%84%E5%90%88%E6%B3%95%E6%80%A7)
    - [2.4 验证井通钱包或联盟链钱包私钥的合法性](#24-%E9%AA%8C%E8%AF%81%E4%BA%95%E9%80%9A%E9%92%B1%E5%8C%85%E6%88%96%E8%81%94%E7%9B%9F%E9%93%BE%E9%92%B1%E5%8C%85%E7%A7%81%E9%92%A5%E7%9A%84%E5%90%88%E6%B3%95%E6%80%A7)
    - [2.5 通过井通钱包私钥获得地址](#25-%E9%80%9A%E8%BF%87%E4%BA%95%E9%80%9A%E9%92%B1%E5%8C%85%E7%A7%81%E9%92%A5%E8%8E%B7%E5%BE%97%E5%9C%B0%E5%9D%80)
  - [3 井畅RPC接口文档](#3-%E4%BA%95%E7%95%85rpc%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3)
  - [4 井畅交易接口文档](#4-%E4%BA%95%E7%95%85%E4%BA%A4%E6%98%93%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3)
    - [4.1 安装与引入](#41-%E5%AE%89%E8%A3%85%E4%B8%8E%E5%BC%95%E5%85%A5)
    - [4.2 签名](#42-%E7%AD%BE%E5%90%8D)
    - [4.3 初始化](#43-%E5%88%9D%E5%A7%8B%E5%8C%96)
    - [4.4 创建挂单](#44-%E5%88%9B%E5%BB%BA%E6%8C%82%E5%8D%95)
    - [4.5 取消挂单](#45-%E5%8F%96%E6%B6%88%E6%8C%82%E5%8D%95)
    - [4.6 转账](#46-%E8%BD%AC%E8%B4%A6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 1 井畅服务端接口文档

[文档](https://github.com/JCCDex/jcc_server_doc)

## 2 井畅钱包接口文档

[文档](https://github.com/JCCDex/jcc_wallet/blob/master/docs/jingtum.md)

### 2.1 安装与引入

```javascript
npm install jcc_wallet
import { jtWallet } from 'jcc_wallet'
// const jtWallet = require('jcc_wallet').jtWallet
```

### 2.2 创建井通钱包或联盟链钱包

```javascript
jtWallet.createWallet(chain)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
chain|String|否|井通链或联盟链的名字,可传swt(创建井通钱包)或bwt(创建bizain钱包)，缺省swt

返回结果:

参数名|参数类型|说明
--|:--:|--:
wallet|Object|井通钱包或bizain钱包
secret|String|井通钱包或bizain钱包私钥
address|String|井通钱包或bizain钱包地址

返回示例:

```json
{"secret":"ssfd6zheHp6y8SEv5imhqvMoiQLiw", "address":"jaR7yuA6TA3aAh1d4gSUKgchh7KYyrUL98"}
```

### 2.3 验证井通钱包或联盟链钱包地址的合法性

```javascript
jtWallet.isValidAddress(address,chain)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
address|String|是|井通钱包或联盟链钱包地址
chain|String|否|井通链或联盟链的名字,可传swt(验证井通钱包)或bwt(验证bizain钱包)，缺省swt

返回结果:

参数名|参数类型|说明
--|:--:|--:
isValid|Boolean|验证结果，合法为true,不合法为false

返回示例:

```javascript
true || false
```

### 2.4 验证井通钱包或联盟链钱包私钥的合法性

```javascript
jtWallet.isValidSecret(secret,chain)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
secret|String|是|井通钱包或联盟链钱包私钥
chain|String|否|井通链或联盟链的名字,可传swt(验证井通钱包)或bwt(验证bizain钱包)，缺省swt

返回结果:

参数名|参数类型|说明
--|:--:|--:
isValid|Boolean|验证结果，合法为true,不合法为false

返回示例:

```javascript
true || false
```
### 2.5 通过井通钱包私钥获得地址

```javascript
jtWallet.getAddress(secret,chain)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
secret|String|是|井通钱包或联盟链钱包私钥
chain|String|否|井通链或联盟链的名字,可传swt(井通钱包)或bwt(bizain钱包)，缺省swt

返回结果:

参数名|参数类型|说明
--|:--:|--:
address|String|井通钱包或联盟链钱包地址,若私钥不合法则返回null

返回示例:

```javascript
address:"jaR7yuA6TA3aAh1d4gSUKgchh7KYyrUL98" || null
```

## 3 井畅RPC接口文档

[Node](https://github.com/JCCDex/jcc_rpc)

[Java](https://github.com/JCCDex/jcc_rpc_java)

## 4 井畅交易接口文档

[文档](https://github.com/JCCDex/jcc_exchange)

### 4.1 安装与引入

```javascript
npm install jcc_exchange
import JCCExchange from "jcc_exchange"
// const JCCExchange = require('jcc_exchange').JCCExchange
```
### 4.2 签名

```javascript
const copyTx = Object.assign({}, tx)
jingtumSignTx(copyTx, { seed: secret })
```
签名函数可参考:
[jingtumSignTx](https://github.com/JCCDex/jcc_jingtum_lib/blob/master/src/local_sign.js)

参数:
以下参数封装成tx对象,传给签名函数

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
Account|String|是|井通钱包地址
Fee|Number|是|交易费用(gas费)
Flags|Number|是|交易标记(挂卖单时是0x00080000,其余情况是0)
Platform|String|否|挂单平台方钱包地址，缺省为jDXCeSHSpZ9LiX6ihckWaYDeDt5hFrdTto(只有创建挂单时才有这个字段)
TakerGets|Object或String|是|挂单付出的币种和数量(只有创建挂单时才有这个字段)
TakerPays|Object或String|是|挂单得到的币种和数量(只有创建挂单时才有这个字段)
OfferSequence|Number|是|交易序列号(只有取消挂单时才有这个字段)
Amount|Object或String|是|转账的数量(只有转账时才有这个字段)
Destination|String|是|转账目标钱包地址(只有转账时才有这个字段)
Memos|Array或String|否|转账备注(只有转账时才有这个字段),当转账备注的数据类型是String时，Memos是数组
TransactionType|String|是|交易类型 OfferCreate(创建挂单) OfferCancel(取消挂单) Payment(转账)

有关签名参数具体可参考:
[签名参数](https://github.com/JCCDex/jcc_exchange/blob/master/src/tx/index.ts)

### 4.3 初始化

```javascript
const hosts = ["localhost"];
const port = 80;
const https = false;
const retry = 3;
JCCExchange.init(hosts, port, https, retry);
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
hosts|String|是|exchange服务器
port|Number|是|exchange服务器端口号
https|Boolean|是|true为https,false为http
retry|Number|否|交易发生错误时重试次数，缺省3次

### 4.4 创建挂单

```javascript
JCCExchange.createOrder(address, secret, amount, base, counter, sum, type, platform, issuer)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
address|String|是|井通钱包地址
secret|String|是|井通钱包私钥
amount|String|是|挂单的数量
base|String|是|交易的币种(不区分大小写)
counter|String|是|交易的交易区(不区分大小写),根据挂单的类型区分(若是买单,counter是付出的币种;若是卖单,counter则是得到的币种)
sum|String|是|挂单的总价
type|String|是|挂单的类型(买：buy,卖:sell)
platform|String|否|挂单的平台方钱包地址,缺省jDXCeSHSpZ9LiX6ihckWaYDeDt5hFrdTto(威链平台)
issuer|String|否|交易币种的发行发，缺省jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or

返回结果:

参数名|参数类型|说明
--|:--:|--:
hash|String|交易哈希

返回示例:

```javascript
hash:"9F1D72707EFBA863B8DAD487857E4D7B4E54E90CF6348BBEA5F32509C4390DE4"
```

### 4.5 取消挂单

```javascript
JCCExchange.cancelOrder(address, secret, orderSequence)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
address|String|是|井通钱包地址
secret|String|是|井通钱包私钥
orderSequence|Number|是|订单的序列号

返回结果:

参数名|参数类型|说明
--|:--:|--:
hash|String|交易哈希

返回示例:

```javascript
hash:"6E3D45E77EB20D73439D10958AF28A648DC49302FEE1212BB4D5E010624F554D"
```

### 4.6 转账

```javascript
JCCExchange.transfer(address, secret, amount, memo, to, token, issuer)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
address|String|是|转出井通钱包地址
secret|String|是|转出井通钱包私钥
amount|String|是|转账的数量
memo|String|否|转账的备注
to|String|是|转账的目标(接收方)井通钱包地址
token|String|是|转账的币种(不区分大小写)
issuer|String|否|转账币种的发行方,缺省jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or

返回结果:

参数名|参数类型|说明
--|:--:|--:
hash|String|交易哈希

返回示例:

```javascript
hash:"445821D60F9294E4ACF911D95166320D2F8EFFC6B4C6C133701B53272684D166"
```
