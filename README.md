# JingChang-Document

井畅接口文档

## 井畅服务端接口文档

[文档](https://github.com/JCCDex/jcc_server_doc)

## 井畅钱包接口文档

[文档](https://github.com/JCCDex/jcc_wallet/blob/master/docs/jingtum.md)

### 使用

```javascript
npm install jcc_wallet
import { jtWallet } from 'jcc_wallet'
// const jtWallet = require('jcc_wallet').jtWallet
```

#### 1 创建井通钱包或联盟链钱包

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

#### 2 验证井通钱包或联盟链钱包地址的合法性

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

#### 3 验证井通钱包或联盟链钱包私钥的合法性

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
#### 4 通过井通钱包私钥获得地址

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

```json
{"address":"jaR7yuA6TA3aAh1d4gSUKgchh7KYyrUL98"}
```

## 井畅RPC接口文档

[Node](https://github.com/JCCDex/jcc_rpc)

[Java](https://github.com/JCCDex/jcc_rpc_java)

## 井畅交易接口文档

[文档](https://github.com/JCCDex/jcc_exchange)

### 使用

```javascript
npm install jcc_exchange
import JCCExchange from "jcc_exchange"
// const JCCExchange = require('jcc_exchange').JCCExchange
```
### 1 初始化

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
https|Boolean|是|开发环境为false,生产环境为true
retry|Number|否|交易发生错误时重试次数，缺省3次

#### 2 创建挂单

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

```json
{"hash":"9F1D72707EFBA863B8DAD487857E4D7B4E54E90CF6348BBEA5F32509C4390DE4"}
```

#### 3 取消挂单

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

```json
{"hash":"6E3D45E77EB20D73439D10958AF28A648DC49302FEE1212BB4D5E010624F554D"}
```

#### 4 转账

```javascript
JCCExchange.transfer(address, secret, amount, memo, to, token, issuer)
```
参数:

参数名|参数类型|是否必须|描述
--|:--:|--:|--:
address|String|是|用户自己的井通钱包地址
secret|String|是|用户自己的井通钱包私钥
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

```json
{"hash":"445821D60F9294E4ACF911D95166320D2F8EFFC6B4C6C133701B53272684D166"}
```
