<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# JCC RPC JAVA

## Interface design

### JCallback

```java
  /**
   * @param code  code is 0, the response is correct
   * @param response
   */
   void onResponse(String code, String response);
   void onFail(Exception e);
```

### 1. Config interface

```java
   JccdexUrl jccUrl = new JccdexUrl("weidex.vip", true);
   JccConfig config = JccConfig.getInstance();
   config.setmBaseUrl(jccUrl);
```

#### 1.1 Query config of jccdex

```java
  config.requestConfig(jCallBack);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#1.1-获取配置数据)

### 2. Transaction interface

```java
   // JccdexUrl jccUrl = new JccdexUrl("xxx", true);
   // JccdexUrl jccUrl = new JccdexUrl("xxx", false);
   JccdexUrl jccUrl = new JccdexUrl("xxx", true, 8081);
   JccdexExchange exchange = JccdexExchange.getInstance();
   exchange.setmBaseUrl(jccUrl);
```

#### 2.1 Query balances

```java
   exchange.requestBalance(address, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|-|-|wallet address
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#2.1-获取钱包余额)

#### 2.2 Query transaction history

```java
   exchange.requestHistoricTransactions(address,page,ledger,seq,jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|-|-|wallet address
   page|int|-|-|current page
   ledger|int|-|-|ledger No. can be omitted in the first query, and the next value is returned in the last query
   seq|int|-|-|the SEQ of the last query, which can be omitted in the first query
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#2.2-获取交易历史记录)

#### 2.3 Query the current orders

```java
   exchange.requestOrders(address, page, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|-|-|wallet address
   page|int|-|-|current page
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#2.4-获取当前挂单)

#### 2.4 Create a order

```java
   exchange.createOrder(signature, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   sign|String|-|-|signature
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#2.5-创建挂单)

#### 2.5 Cancel a order

```java
   exchange.cancelOrder(signature, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   sign|String|-|-|signature
   jCallback|JCallback|-|-|call back

* Response

  * see [2.4](#2.4-创建挂单)

#### 2.6 Query the sequence

```java
   exchange.requestSequence(address);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|-|-|wallet address

* Response
  Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   sequence|int|-|-|latest sequence

#### 2.7 Payment

```java
   exchange.transferToken(signature, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   sign|String|是|-|-|signature
   jCallback|JCallback|-|-|call back

* Response

  * see [2.4](#2.4-创建挂单)

#### 2.8 Query the order details

```java
   exchange.requestOrderDetail(hash, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   hash|String|-|-|transaction hash
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#2.9-获取订单详情)

### 3. Data interface

```java
   // JccdexUrl jccUrl = new JccdexUrl("xxx", true);// https://xxx:443
   // JccdexUrl jccUrl = new JccdexUrl("xxx", false);// http://xxx:80
   JccdexUrl jccUrl = new JccdexUrl("xxx", true, 8081);
   JccdexInfo info = JccdexInfo.getInstance();
info.setmBaseUrl(jccUrl);
```

#### 3.1 Query 24-hour market data of a currency

```java
   info.requestTicker(base, counter, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   base|String|-|-|base currency
   counter|String|-|-|target currency
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#3.1-获取指定币种24小时的行情数据)

#### 3.2 Query 24-hour market data of all currencies

```java
   info.requestAllTickers(jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#3.2-获取所有币种24小时的行情数据)

#### 3.3 Query the market depth

```java
   info.requestDepth(base, counter, type, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   base|String|-|-|base currency
   counter|String|-|-|target currency
   type|String|normal : data length is 5<br>more : data length is 50|-|Type
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#3.3-获取市场深度)
  
#### 3.4 Query K-line data

```java
   info.requestKline(base, counter, type, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   base|String|-|-|base currency
   counter|String|-|-|target currency
   type|String|minute、5minute、15minute、30minute、<br>hour、4hour、day、week、mouth|-|Type
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#3.4-获取K线数据)

#### 3.5 Query time-sharing data

```java
   info.requestHistory(base, counter, type, time, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   base|String|-|-|base currency
   counter|String|-|-|target currency
   type|String|all、more、newest|-|Type
   time|String|when type=`newest`|-|-|time stamp
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#3.5-获取分时数据)

#### 3.6 Query exchange rate

```java
   info.requestTickerFromCMC(token, currency, jCallback)
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   token|String|-|-|for eth、btc
   currency|String|-|-|for cny、rub
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-JCC-RPC-Detail.md#3.6-获取币种间汇率)

### 4. Explorer interface

```java
   JccdexUrl jccUrl = new JccdexUrl("expjia1b6719b6e6.jccdex.cn", true);
   JccExplore explore = JccExplore.getInstance();
   explore.setmBaseUrl(jccUrl);
```

#### 5.1 Query the balance

```java
   explore.requestBalance(uuid, address, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|unique id
   address|String|-|-|wallet address
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-Explorer-Server.md#5.1-指定钱包的余额查询（包括所有Token的余额、所有Token的冻结数量）)

#### 5.2 Query transaction details by hash

```java
   explore.requestTransDetails(uuid, hash, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|unique id
   hash|String|-|-|transaction hash
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-Explorer-Server.md#3.-根据哈希查询交易详细)

#### 5.3 Query historical transaction of specified wallet

```java
   explore.requestHistoricTransWithAddr(uuid, page, size, begin, end, type, currency, address, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|unique id
   page|int|-|-|number of pages, starting from 0
   size|int|10/20/50/100|20|size per page
   begin|String|-|-|start date，YYYY-MM-DD
   end|String|-|-|end date，YYYY-MM-DD
   type|String|OfferCreate<br>OfferAffect<br>OfferCancel<br>Send<br>Receive|-|transaction type (multiple types are separated by commas, and no value can be passed which means that all types are queried)
   currency|String|-|-|transaction pair or token（You can not pass a value, which means that the currency is not used as the query condition. When `type` is `OfferCreate`, `OfferAffect` or `OfferCancel`, the value passed must be in the form of: `SWTC-CNY` or `swtc-cny`, and must be in the form of the original sequence base-counter of the transaction pair. For example, it cannot be `CNY-SWTC`, otherwise the sales relationship may mess, in addition, the transaction pair can only specify base or counter, such as `swtc-` or `-cny`. For all transaction pairs, please refer to the [appendix](#appendix); when `type` is `Send` or `Receive`, the value must be less than 8 in length, such as `JJCC`）
   address|String|-|-|wallet address
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-Explorer-Server.md#5.3-指定钱包的历史交易查询)

#### 5.4 Query the daily/month/ year payment or receipt of a certain wallet for a certain wallet

```java
   explore.requestPaymentSummary(uuid, address, dateTpye, begin, end, type, token, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|unique id
   address|String|-|-|wallet address
   dateTpye|int|2: day<br>3:month<br>4:year|-|date type
   begin|String|-|-|for a specific day/month/year date, if dateTpye = 2, it must be like YYYY-MM-DD; if dateTpye = 3, it must be like YYYY-MM; if dateTpye = 4, it must be like YYYY
   end|String|-|-|like `begin` parameter。If the value of `end` is specified, the value of the same token in the time range of `begin` to `end` is accumulated, and the value of `end` must be greater than the value of `begin`. when `dateTpye` is 2 or 3, the difference in days between `begin` and `end` cannot exceed one year; when `dateTpye` is 4, there is no limit on the interval between `begin` and `end`
   type|String|Send/Receive|-|transaction type
   token|String|-|-|token name（SWT does not require an issuer. The value is `SWTC_`. Other tokens need to be issued with the issuer in uppercase, like c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-Explorer-Server.md#11.-查询某个钱包每天/月/年的支付或收到某个token的笔数和数量)

#### 5.5 Query the transfer hash information of a certain transaction within a certain period of time

```java
   explore.requestHistoricTransWithCur(uuid, page, size, begin, end, type, currency, jCallback);
```

* Parameters

   Parameter|Type|Optinal|Default|Description
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|unique id
   page|int|-|-|number of pages, starting from 0
   size|int|10/20/50/100|20|size per page
   begin|String|-|-|start date，YYYY-MM-DD
   end|String|-|-|end date，YYYY-MM-DD
   type|String|Payment|-|transaction type
   currency|String|-|-|token name
   jCallback|JCallback|-|-|call back

* [Response](../Jingchang-Explorer-Server.md#10.-查询某个token在某个时间段内的转账交易hash信息)
