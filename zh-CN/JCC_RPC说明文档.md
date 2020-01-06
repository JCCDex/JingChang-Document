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

#### 1.1 [获取配置数据](../Jingchang-RPC-Server.md#1.1-获取配置数据)

```nodejs
   let res = await instance.getConfig()
```

### 2. 交易接口

```nodejs
   const JcExchange = require("jcc_rpc").JcExchange
   let hosts = ["ejia348ffbda04.jccdex.cn"]
   let port = 443
   let https = true
   let instance = new JcExchange(hosts, port, https)
```

#### 2.1 [获取钱包余额](../Jingchang-RPC-Server.md#2.1-获取钱包余额)

```nodejs
   let res = await instance.getBalances(address)
```

#### 2.2 [获取交易历史记录](../Jingchang-RPC-Server.md#2.2-获取交易历史记录)

```nodejs
   let res = await instance.getHistoricTransactions(address, ledger, seq)
```

#### 2.3 [获取转账历史记录](../Jingchang-RPC-Server.md#2.3-获取转账历史记录)

```nodejs
   let res = await instance.getHistoricPayments(address, ledger, seq)
```

#### 2.4 [获取当前挂单](../Jingchang-RPC-Server.md#2.4-获取当前挂单)

```nodejs
   let res = await instance.getOrders(address, page)
```

#### 2.5 [创建挂单](../Jingchang-RPC-Server.md#2.5-创建挂单)

```nodejs
   let res = await instance.createOrder(sign)
```

#### 2.6 [取消挂单](../Jingchang-RPC-Server.md#2.6-取消挂单)

```nodejs
   let res = await instance.deleteOrder(sign)
```

#### 2.7 [获取序列号](../Jingchang-RPC-Server.md#2.7-获取序列号)

```nodejs
   let res = await instance.getSequence(address)
```

#### 2.8 [转账](../Jingchang-RPC-Server.md#2.8-转账)

```nodejs
   let res = await instance.transferAccount(sign)
```

### 3. 数据接口

```nodejs
   const JcInfo = require("jcc_rpc").JcInfo
   let hosts = ["ijiijhg293cabc.jccdex.cn"]
   let port = 443
   let https = true
   let instance = new JcInfo(hosts, port, https)
```

#### 3.1 [获取指定币种24小时的行情数据](../Jingchang-RPC-Server.md#3.1-获取指定币种24小时的行情数据)

```nodejs
   let res = await instance.getTicker(base, counter)
```

#### 3.2 [获取所有币种24小时的行情数据](../Jingchang-RPC-Server.md#3.2-获取所有币种24小时的行情数据)

```nodejs
   let res = await instance.getAllTickers()
```

#### 3.3 [获取市场深度](../Jingchang-RPC-Server.md#3.3-获取市场深度)

```nodejs
   let res = await instance.getDepth(base, counter, type)
```
  
#### 3.4 [获取K线数据](../Jingchang-RPC-Server.md#3.4-获取K线数据)

```nodejs
   let res = await instance.getKline(base, counter, type)
```

#### 3.5 [获取分时数据](../Jingchang-RPC-Server.md#3.5-获取分时数据)

```nodejs
   let res = await instance.getHistory(base, counter, type, time)
```

#### 3.6 [获取币种间汇率](../Jingchang-RPC-Server.md#3.6-获取币种间汇率)

```nodejs
   let res = await instance.getTickerFromCMC(token, currency)
```
