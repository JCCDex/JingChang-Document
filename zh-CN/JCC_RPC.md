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

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#11-%E8%8E%B7%E5%8F%96%E9%85%8D%E7%BD%AE%E6%95%B0%E6%8D%AE)

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

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#21-%E8%8E%B7%E5%8F%96%E9%92%B1%E5%8C%85%E4%BD%99%E9%A2%9D)

#### 2.2 获取交易历史记录

```nodejs
   let res = await instance.getHistoricTransactions(address, ledger, seq)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#22-%E8%8E%B7%E5%8F%96%E4%BA%A4%E6%98%93%E5%8E%86%E5%8F%B2%E8%AE%B0%E5%BD%95)

#### 2.3 获取转账历史记录

```nodejs
   let res = await instance.getHistoricPayments(address, ledger, seq)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#23-%E8%8E%B7%E5%8F%96%E8%BD%AC%E8%B4%A6%E5%8E%86%E5%8F%B2%E8%AE%B0%E5%BD%95)

#### 2.4 获取当前挂单

```nodejs
   let res = await instance.getOrders(address, page)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#24-%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E6%8C%82%E5%8D%95)

#### 2.5 创建挂单

```nodejs
   let res = await instance.createOrder(sign)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#25-%E5%88%9B%E5%BB%BA%E6%8C%82%E5%8D%95)

#### 2.6 取消挂单

```nodejs
   let res = await instance.deleteOrder(sign)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#26-%E5%8F%96%E6%B6%88%E6%8C%82%E5%8D%95)

#### 2.7 获取序列号

```nodejs
   let res = await instance.getSequence(address)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#27-%E8%8E%B7%E5%8F%96%E5%BA%8F%E5%88%97%E5%8F%B7)

#### 2.8 转账

```nodejs
   let res = await instance.transferAccount(sign)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#28-%E8%BD%AC%E8%B4%A6)

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

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#31-%E8%8E%B7%E5%8F%96%E6%8C%87%E5%AE%9A%E5%B8%81%E7%A7%8D24%E5%B0%8F%E6%97%B6%E7%9A%84%E8%A1%8C%E6%83%85%E6%95%B0%E6%8D%AE)

#### 3.2 获取所有币种24小时的行情数据

```nodejs
   let res = await instance.getAllTickers()
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#32-%E8%8E%B7%E5%8F%96%E6%89%80%E6%9C%89%E5%B8%81%E7%A7%8D24%E5%B0%8F%E6%97%B6%E7%9A%84%E8%A1%8C%E6%83%85%E6%95%B0%E6%8D%AE)

#### 3.3 获取市场深度

```nodejs
   let res = await instance.getDepth(base, counter, type)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#33-%E8%8E%B7%E5%8F%96%E5%B8%82%E5%9C%BA%E6%B7%B1%E5%BA%A6)
  
#### 3.4 获取K线数据

```nodejs
   let res = await instance.getKline(base, counter, type)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#34-%E8%8E%B7%E5%8F%96k%E7%BA%BF%E6%95%B0%E6%8D%AE)

#### 3.5 获取分时数据

```nodejs
   let res = await instance.getHistory(base, counter, type, time)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#35-%E8%8E%B7%E5%8F%96%E5%88%86%E6%97%B6%E6%95%B0%E6%8D%AE)

#### 3.6 获取币种间汇率

```nodejs
   let res = await instance.getTickerFromCMC(token, currency)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#36-%E8%8E%B7%E5%8F%96%E5%B8%81%E7%A7%8D%E9%97%B4%E6%B1%87%E7%8E%87)