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

### 1. Config API

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

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#11-get-config-data)

### 2. Exchange API

#### 2.1 Query balances

```nodejs
   let res = await instance.getBalances(address)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#21-get-balances)

#### 2.2 Query transaction history

```nodejs
   let res = await instance.getHistoricTransactions(address, ledger, seq)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#22-get-transaction-history)

#### 2.3 Query payment history

```nodejs
   let res = await instance.getHistoricPayments(address, ledger, seq)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#23-get-transfer-history)

#### 2.4 Query the current orders

```nodejs
   let res = await instance.getOrders(address, page)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#24-get-the-current-pending-order)

#### 2.5 Create order

```nodejs
   let res = await instance.createOrder(sign)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#25-create-ordera)

#### 2.6 Cancel order

```nodejs
   let res = await instance.deleteOrder(sign)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#26-cancel-order)

#### 2.7 Get sequence

```nodejs
   let res = await instance.getSequence(address)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#27-get-sequence)

#### 2.8 Payment

```nodejs
   let res = await instance.transferAccount(sign)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#28-transfer)

### 3. Info API

```nodejs
   const JcInfo = require("jcc_rpc").JcInfo
   let hosts = ["ijiijhg293cabc.jccdex.cn"]
   let port = 443
   let https = true
   let instance = new JcInfo(hosts, port, https)
```

#### 3.1 Query 24-hour market data for the specified currency

```nodejs
   let res = await instance.getTicker(base, counter)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#31-get-24-hour-market-data-for-the-specified-currency)

#### 3.2 Query 24-hour market data of all currencies

```nodejs
   let res = await instance.getAllTickers()
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#32-get-24-hour-market-data-for-all-currencies)

#### 3.3 Query the market depth

```nodejs
   let res = await instance.getDepth(base, counter, type)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#33-get-market-depth)
  
#### 3.4 Query K-line data

```nodejs
   let res = await instance.getKline(base, counter, type)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#34-get-k-line-data)

#### 3.5 Query time-sharing data

```nodejs
   let res = await instance.getHistory(base, counter, type, time)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#35-get-time-sharing-data)

#### 3.6 Query exchange rate

```nodejs
   let res = await instance.getTickerFromCMC(token, currency)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#36-get-inter-currency-exchange-rates)
