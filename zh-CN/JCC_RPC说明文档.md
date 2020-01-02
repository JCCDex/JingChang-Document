<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# JCC RPC

## 安装

```nodejs
   npm install jcc_rpc
```

## 接口详细设计

### 1. 配置接口

```nodejs
   const JcConfig = require("jcc_rpc").JcConfig
   let hosts = ["jccdex.cn", "weidex.vip"]
   let port = 443
   let https = true
   let instance = new JcConfig(hosts, port, https)
```

#### 1.1 获取配置数据

```nodejs
   let res = await instance.getConfig()
```

* 请求参数

   --

* [返回结果](../Jingchang-JCC-RPC-Detail.md#1.1-获取配置数据)

### 2. 交易接口

```nodejs
   const JcExchange = require("jcc_rpc").JcExchange
   let hosts = ["ejia348ffbda04.jccdex.cn"]
   let port = 443
   let https = true
   let instance = new JcExchange(hosts, port, https)
```

#### 2.1 获取钱包余额

```nodejs
   let res = await instance.getBalances(address)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.1-获取钱包余额)

#### 2.2 获取交易历史记录

```nodejs
   let res = await instance.getHistoricTransactions(address, ledger, seq)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址
   ledger|Number|-|-|-|账本号，首次查询可以省略，之后值为上次查询返回的ledger值
   seq|Number|-|-|-|上次查询的返回seq值，首次查询可以省略

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.2-获取交易历史记录)

#### 2.3 获取转账历史记录

```nodejs
   let res = await instance.getHistoricPayments(address, ledger, seq)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址
   ledger|Number|-|-|-|账本号，首次查询可以省略，之后值为上次查询返回的ledger值
   seq|Number|-|-|-|上次查询的返回seq值，首次查询可以省略

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.3-获取转账历史记录)

#### 2.4 获取当前挂单

```nodejs
   let res = await instance.getOrders(address, page)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址
   page|Number|是|-|-|当前页

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.4-获取当前挂单)

#### 2.5 创建挂单

```nodejs
   let res = await instance.createOrder(sign)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.5-创建挂单)

#### 2.6 取消挂单

```nodejs
   let res = await instance.deleteOrder(sign)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名

* 返回结果

  * 同[2.5](#2.5-创建挂单)

#### 2.7 获取序列号

```nodejs
   let res = await instance.getSequence(address)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.7-获取序列号)

#### 2.8 转账

```nodejs
   let res = await instance.transferAccount(sign)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名

* 返回结果

  * 同[2.5](#2.5-创建挂单)

### 3. 数据接口

```nodejs
   const JcInfo = require("jcc_rpc").JcInfo
   let hosts = ["ijiijhg293cabc.jccdex.cn"]
   let port = 443
   let https = true
   let instance = new JcInfo(hosts, port, https)
```

#### 3.1 获取指定币种24小时的行情数据

```nodejs
   let res = await instance.getTicker(base, counter)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   base|String|是|-|-|基准币种
   counter|String|是|-|-|目标币种

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.1-获取指定币种24小时的行情数据)

#### 3.2 获取所有币种24小时的行情数据

```nodejs
   let res = await instance.getAllTickers()
```

* 请求参数

   --

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.2-获取所有币种24小时的行情数据)

#### 3.3 获取市场深度

```nodejs
   let res = await instance.getDepth(base, counter, type)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   base|String|是|-|-|基准币种
   counter|String|是|-|-|目标币种
   type|String|是|normal : 数据长度为5<br>more : 数据长度为50|-|类型

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.3-获取市场深度)
  
#### 3.4 获取K线数据

```nodejs
   let res = await instance.getKline(base, counter, type)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   base|String|是|-|-|基准币种
   counter|String|是|-|-|目标币种
   type|String|是|minute、5minute、15minute、30minute、<br>hour、4hour、day、week、mouth|-|类型

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.4-获取K线数据)

#### 3.5 获取分时数据

```nodejs
   let res = await instance.getHistory(base, counter, type, time)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   base|String|是|-|-|基准币种
   counter|String|是|-|-|目标币种
   type|String|是|all、more、newest|-|类型
   time|String|type=`newest`时|-|-|时间戳

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.5-获取分时数据)

#### 3.6 获取币种间汇率

```nodejs
   let res = await instance.getTickerFromCMC(token, currency)
```

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   token|String|是|-|-|支持eth、btc
   currency|String|是|-|-|支持cny、rub

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.6-获取币种间汇率)
