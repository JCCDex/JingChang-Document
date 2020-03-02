<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# 井畅交易服务

## 井畅交易服务的功能

1. 交易签名

2. 挂单

3. 取消挂单

4. 转账

5. 设置挂单手续费

## 安装与引入

```javascript
npm install jcc_exchange
import JCCExchange from "jcc_exchange"
// const JCCExchange = require('jcc_exchange').JCCExchange
```

## 交易接口详细说明

### 1. 交易签名

* 签名函数

  [函数](https://github.com/JCCDex/jcc_exchange/blob/master/src/util/sign.ts#L19)

* 参数说明

  [具体签名参数](https://github.com/JCCDex/jcc_exchange/blob/master/src/tx/index.ts)

   参数|类型|必填|默认值|描述
   :--|:--:|:--:|:--:|:--
   secret|String|是|-|交易发起方井通钱包私钥
   chain|String|否|jingtum|[Support Chain](#Support-Chain)
   tx|Object|是|-|需要签名的交易对象
   &emsp;Account|String|是|-|交易发起方井通钱包地址
   &emsp;Sequence|Number|是|-|交易序列号
   &emsp;Flags|Number|是|-|交易标记
   &emsp;Fee|Number|是|-|交易gas费,单位`SWTC`,最小0.00001
   &emsp;TransactionType|String|是|-|[Transaction Type](#Transaction-Type)

   1. 当TransactionType=`OfferCreate`时，`tx`包含

         参数|类型|必填|默认值|描述
         :--|:--:|:--:|:--:|:--
         TakerGets|[Amount](#Amount-Object)|是|-|挂单付出的币种和数量
         TakerPays|[Amount](#Amount-Object)|是|-|挂单得到的币种和数量
         Platform|String|否|jDXCeSHSpZ9LiX6ihckWaYDeDt5hFrdTto|挂单平台方钱包地址

   2. 当TransactionType=`OfferCancel`时，`tx`包含

         参数|类型|必填|默认值|描述
         :--|:--:|:--:|:--:|:--
         OfferSequence|Number|是|-|取消挂单的序列号

   3. 当TransactionType=`Payment`时，`tx`包含

         参数|类型|必填|默认值|描述
         :--|:--:|:--:|:--:|:--
         Amount|[Amount](#Amount-Object)|是|-|转账的币种和数量
         Destination|String|是|-|对方钱包地址
         Memos|Array|是|-|转账备注

   4. 当TransactionType=`Brokerage`时，`tx`包含

         参数|类型|必填|默认值|描述
         :--|:--:|:--:|:--:|:--
         Amount|[Amount](#Amount-Object)|是|-|收取手续费的币种和数量
         FeeAccountID|String|是|-|收取手续费钱包地址
         OfferFeeRateDen|Number|是|-|手续费费率分母
         OfferFeeRateNum|Number|是|-|手续费费率分子

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   -|String|签名后的交易数据

### 2. 设置默认链与初始化

#### 2.1 设置默认链

```javascript
JCCExchange.setDefaultChain(chain);
```
**默认支持井通链,如果要支持其他链,请先设置默认链为其他链**

* 参数说明
  
   参数|类型|必填|默认值|描述
   :--|:--:|:--:|:--:|:--
   chain|String|是|-|[Support Chain](#Support-Chain)

#### 2.2 实例初始化

```javascript
const hosts = ["localhost"];
const port = 80;
const https = false;
const urls = ["http://localhost:8080"];
const retry = 3;
JCCExchange.init(urls, retry);
// JCCExchange.init(hosts, port, https, retry);
```
**上面两种init方式,选择其中一种**
**挂单,取消挂单,转账,设置挂单手续费都需要初始化后才能继续,下面不在赘述**

* 参数说明
  
   参数|类型|必填|默认值|描述
   :--|:--:|:--:|:--:|:--
   hosts|Array|是|-|某个或一组交易服务器域名
   port|Number或String|是|-|某个交易服务器端口号
   https|Boolean|是|-|true为https,false为http
   urls|Array|是|-|某个或一组交易服务器完整的url(协议+域名+端口)
   retry|Number|否|3|交易序列号失效时,尝试重新获取的次数

### 3. 创建挂单

* 使用示例

```javascript
const address = "jxxx";
const secret = "sxxx";
const amount = "1";
const base = "jjcc";
const counter = "swt";
const sum = "1";
const type = "buy";
const platform = ""; 
const issuer;
JCCExchange.init(urls, retry);
const hash = await JCCExchange.createOrder(address, secret, amount, base, counter, sum, type, platform, issuer);
```

* 参数说明

   参数|类型|必填|默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|是|-|挂单方钱包地址
   secret|String|是|-|挂单方钱包私钥
   amount|String|是|-|挂单的数量
   base|String|是|-|挂单的币种(不区分大小写)
   counter|String|是|-|根据挂单的类型区分(若是买单,counter是付出的币种;若是卖单,counter则是得到的币种,不区分大小写)
   sum|String|是|-|挂单的总价
   type|String|是|-|挂单类型(买：buy,卖:sell)
   platform|String|否|jDXCeSHSpZ9LiX6ihckWaYDeDt5hFrdTto|挂单的平台方钱包地址
   issuer|String|否|jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or|币种发行方地址

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   hash|String|交易哈希

### 4. 取消挂单

* 使用示例

```javascript
const address = "jxxx";
const secret = "sxxx";
const orderSequence = 0;
JCCExchange.init(urls, retry);
const hash = await JCCExchange.cancelOrder(address, secret, orderSequence);
```

* 参数说明

   参数|类型|必填|默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|是|-|取消挂单方钱包地址
   secret|String|是|-|取消挂单方钱包私钥
   orderSequence|Number|是|-|取消挂单的序列号

* 返回结果
  
   字段|类型|描述
   :--|:--:|:--
   hash|String|交易哈希

### 5. 转账

* 使用示例

```javascript
const address = "jxxx";
const secret = "sxxx";
const amount = "1";
const memo = "test";
const to = "jxxx";
const token = "jjcc";
const issuer;
JJCCExchange.init(urls, retry);
const hash = await JCCExchange.transfer(address, secret, amount, memo, to, token, issuer);
```

* 参数说明

   参数|类型|必填|默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|是|-|转出方钱包地址
   secret|String|是|-|转出方钱包私钥
   amount|String|是|-|转账数量
   meno|String|否|-|转账备注
   to|String|是|-|转入钱包地址
   token|String|是|-|转账币种(不区分大小写)
   issuer|String|否|jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or|币种发行方地址

* 返回结果
  
   字段|类型|描述
   :--|:--:|:--
   hash|String|交易哈希

### 6. 设置挂单手续费

* 使用示例

```javascript
const platformAccount = "jxxx";
const platformSecret = "sxxx";
const feeAccount = "jxxx";
const rateNum = "2";
const rateDen = "1000";
const token = "jjcc";
const issuer;
JCCExchange.init(urls, retry);
const hash = await JCCExchange.setBrokerage(platformAccount, platformSecret,feeAccount, rateNum, rateDen, token, issuer);
```

* 参数说明

  参数|类型|必填|默认值|描述
   --|:--:|:--:|:--:|:--
   platformAccount|String|是|-|平台方钱包地址
   platformSecret|String|是|-|平台方钱包私钥
   feeAccount|String|是|-|收费钱包地址
   rateNum|Number|是|-|费率分子(0和正整数)
   rateDen|Number|是|-|费率分母(正整数)
   token|String|是|-|收取手续费的币种(不区分大小写)
   issuer|String|否|jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or|币种发行方地址

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   hash|String|交易哈希

### Amount Object

   字段|类型|描述
   :--:|:--:|:--
   currency|String|币种名称
   issuer|String|发行方地址
   value|String|数量

### Transaction Type

类型|描述|可能值
|:--:|:--|:--
String|交易类型|OfferCreate/OfferCancel/Payment/Brokerage

### Support Chain

类型|描述|可能值
|:--:|:--|:--
String|支持链的名称|jingtum/bizain/seaaps

