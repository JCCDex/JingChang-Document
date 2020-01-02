<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# JCC RPC

## Install

```nodejs
   npm install jcc_rpc
```

## Interface design

### 1. Config interface

```nodejs
   const JcConfig = require("jcc_rpc").JcConfig
   let hosts = ["jccdex.cn", "weidex.vip"]
   let port = 443
   let https = true
   let instance = new JcConfig(hosts, port, https)
```

#### 1.1 Query config of jccdex

```nodejs
   let res = await instance.getConfig()
```

* Parameters

   --

* [Response](../Jingchang-JCC-RPC-Detail.md#1.1-获取配置数据)

### 2. Transaction interface

#### 2.1 Query balances

```nodejs
   let res = await instance.getBalances(address)
```

* Parameters

   Parameter|Type|Required|Optinal|Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address

* [Response](../Jingchang-JCC-RPC-Detail.md#2.1-获取钱包余额)

#### 2.2 Query transaction history

```nodejs
   let res = await instance.getHistoricTransactions(address, ledger, seq)
```

* Parameter

   Parameter|Type|Required|Optinal|Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address
   ledger|Number|-|-|-|ledger No. can be omitted in the first query, and the next value is returned in the last query
   seq|Number|-|-|-|the SEQ of the last query, which can be omitted in the first query

* [Response](../Jingchang-JCC-RPC-Detail.md#2.2-获取交易历史记录)

#### 2.3 Query peyment history

```nodejs
   let res = await instance.getHistoricPayments(address, ledger, seq)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address
   ledger|Number|-|-|-|ledger No. can be omitted in the first query, and the next value is returned in the last query
   seq|Number|-|-|-|the SEQ of the last query, which can be omitted in the first query

* [Response](../Jingchang-JCC-RPC-Detail.md#2.3-获取转账历史记录)

#### 2.4 Query the current orders

```nodejs
   let res = await instance.getOrders(address, page)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address
   page|Number|true|-|-|current page

* [Response](../Jingchang-JCC-RPC-Detail.md#2.4-获取当前挂单)

#### 2.5 Create a order

```nodejs
   let res = await instance.createOrder(sign)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   sign|String|true|-|-|signature

* [Response](../Jingchang-JCC-RPC-Detail.md#2.5-创建挂单)

#### 2.6 Cancel a order

```nodejs
   let res = await instance.deleteOrder(sign)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   sign|String|ture|-|-|signature

* Response

  * see [2.5](#2.5-创建挂单)

#### 2.7 Query the sequence

```nodejs
   let res = await instance.getSequence(address)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address

* [Response](../Jingchang-JCC-RPC-Detail.md#2.7-获取序列号)

#### 2.8 Payment

```nodejs
   let res = await instance.transferAccount(sign)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   sign|String|true|-|-|signature

* Response

  * see [2.5](#2.5-创建挂单)

### 3. Data interface

```nodejs
   const JcInfo = require("jcc_rpc").JcInfo
   let hosts = ["ijiijhg293cabc.jccdex.cn"]
   let port = 443
   let https = true
   let instance = new JcInfo(hosts, port, https)
```

#### 3.1 Query 24-hour market data of a currency

```nodejs
   let res = await instance.getTicker(base, counter)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   base|String|true|-|-|base currency
   counter|String|true|-|-|target currency

* [Response](../Jingchang-JCC-RPC-Detail.md#3.1-获取指定币种24小时的行情数据)

#### 3.2 Query 24-hour market data of all currencies

```nodejs
   let res = await instance.getAllTickers()
```

* Parameter

   --

* [Response](../Jingchang-JCC-RPC-Detail.md#3.2-获取所有币种24小时的行情数据)

#### 3.3 Query the market depth

```nodejs
   let res = await instance.getDepth(base, counter, type)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   base|String|true|-|-|base currency
   counter|String|true|-|-|target currency
   type|String|true|normal : data length is 5<br>more : data length is 50|-|Type

* [Response](../Jingchang-JCC-RPC-Detail.md#3.3-获取市场深度)
  
#### 3.4 Query K-line data

```nodejs
   let res = await instance.getKline(base, counter, type)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   base|String|true|-|-|base currency
   counter|String|true|-|-|target currency
   type|String|true|minute、5minute、15minute、30minute、<br>hour、4hour、day、week、mouth|-|Type

* [Response](../Jingchang-JCC-RPC-Detail.md#3.4-获取K线数据)

#### 3.5 Query time-sharing data

```nodejs
   let res = await instance.getHistory(base, counter, type, time)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   base|String|true|-|-|base currency
   counter|String|true|-|-|target currency
   type|String|true|all、more、newest|-|Type
   time|String|when type=`newest`|-|-|time stamp

* [Response](../Jingchang-JCC-RPC-Detail.md#3.5-获取分时数据)

#### 3.6 Query exchange rate

```nodejs
   let res = await instance.getTickerFromCMC(token, currency)
```

* Parameter

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   token|String|true|-|-|for eth、btc
   currency|String|true|-|-|for cny、rub

* [Response](../Jingchang-JCC-RPC-Detail.md#3.6-获取币种间汇率)
