<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# JCC RPC JAVA

## 接口详细设计

### JCallback

```java
  /**
   * @param code  code为0，表示响应结果正确
   * @param response
   */
   void onResponse(String code, String response);
   void onFail(Exception e);
```

### 1. 配置接口

```java
   JccdexUrl jccUrl = new JccdexUrl("weidex.vip", true);
   JccConfig config = JccConfig.getInstance();
   config.setmBaseUrl(jccUrl);
```

#### 1.1 [获取配置数据](../Jingchang-RPC-Server.md#1.1-获取配置数据)

```java
  config.requestConfig(jCallBack);
```

### 2. 交易接口

```java
   // JccdexUrl jccUrl = new JccdexUrl("xxx", true);
   // JccdexUrl jccUrl = new JccdexUrl("xxx", false);
   JccdexUrl jccUrl = new JccdexUrl("xxx", true, 8081);
   JccdexExchange exchange = JccdexExchange.getInstance();
   exchange.setmBaseUrl(jccUrl);
```

#### 2.1 [获取钱包余额](../Jingchang-RPC-Server.md#2.1-获取钱包余额)

```java
   exchange.requestBalance(address, jCallback);
```

#### 2.2 [获取交易历史记录](../Jingchang-RPC-Server.md#2.2-获取交易历史记录)

```java
   exchange.requestHistoricTransactions(address,page,ledger,seq,jCallback);
```

#### 2.3 [获取当前挂单](../Jingchang-RPC-Server.md#2.4-获取当前挂单)

```java
   exchange.requestOrders(address, page, jCallback);
```

#### 2.4 [创建挂单](../Jingchang-RPC-Server.md#2.5-创建挂单)

```java
   exchange.createOrder(signature, jCallback);
```

#### 2.5 [取消挂单](../Jingchang-RPC-Server.md#2.6-取消挂单)

```java
   exchange.cancelOrder(signature, jCallback);
```

#### 2.6 [获取序列号](../Jingchang-RPC-Server.md#2.7-获取序列号)

```java
   exchange.requestSequence(address);
```

#### 2.7 [转账](../Jingchang-RPC-Server.md#2.8-转账)

```java
   exchange.transferToken(signature, jCallback);
```

#### 2.8 [获取订单详情](../Jingchang-RPC-Server.md#2.9-获取订单详情)

```java
   exchange.requestOrderDetail(hash, jCallback);
```

### 3. 数据接口

```java
   // JccdexUrl jccUrl = new JccdexUrl("xxx", true);// https://xxx:443
   // JccdexUrl jccUrl = new JccdexUrl("xxx", false);// http://xxx:80
   JccdexUrl jccUrl = new JccdexUrl("xxx", true, 8081);
   JccdexInfo info = JccdexInfo.getInstance();
info.setmBaseUrl(jccUrl);
```

#### 3.1 [获取指定币种24小时的行情数据](../Jingchang-RPC-Server.md#3.1-获取指定币种24小时的行情数据)

```java
   info.requestTicker(base, counter, jCallback);
```

#### 3.2 [获取所有币种24小时的行情数据](../Jingchang-RPC-Server.md#3.2-获取所有币种24小时的行情数据)

```java
   info.requestAllTickers(jCallback);
```

#### 3.3 [获取市场深度](../Jingchang-RPC-Server.md#3.3-获取市场深度)

```java
   info.requestDepth(base, counter, type, jCallback);
```
  
#### 3.4 [获取K线数据](../Jingchang-RPC-Server.md#3.4-获取K线数据)

```java
   info.requestKline(base, counter, type, jCallback);
```

#### 3.5 [获取分时数据](../Jingchang-RPC-Server.md#3.5-获取分时数据)

```java
   info.requestHistory(base, counter, type, time, jCallback);
```

#### 3.6 [获取币种间汇率](../Jingchang-RPC-Server.md#3.6-获取币种间汇率)

```java
   info.requestTickerFromCMC(token, currency, jCallback)
```

### 4. 浏览器接口

```java
   JccdexUrl jccUrl = new JccdexUrl("expjia1b6719b6e6.jccdex.cn", true);
   JccExplore explore = JccExplore.getInstance();
   explore.setmBaseUrl(jccUrl);
```

#### 5.1 [获取钱包余额](../Jingchang-Explorer-Server.md#5.1-指定钱包的余额查询（包括所有Token的余额、所有Token的冻结数量）)

```java
   explore.requestBalance(uuid, address, jCallback);
```

#### 5.2 [根据hash获取详细交易信息](../Jingchang-Explorer-Server.md#3.-根据哈希查询交易详细)

```java
   explore.requestTransDetails(uuid, hash, jCallback);
```

#### 5.3 [获取指定钱包的历史交易](../Jingchang-Explorer-Server.md#5.3-指定钱包的历史交易查询)

```java
   explore.requestHistoricTransWithAddr(uuid, page, size, begin, end, type, currency, address, jCallback);
```

#### 5.4 [获取钱包每天/月/年支付或收到的币种笔数和数量](../Jingchang-Explorer-Server.md#11.-查询某个钱包每天/月/年的支付或收到某个token的笔数和数量)

```java
   explore.requestPaymentSummary(uuid, address, dateTpye, begin, end, type, token, jCallback);
```

#### 5.5 [查询某个token在某个时间段内的交易hash信息](../Jingchang-Explorer-Server.md#10.-查询某个token在某个时间段内的转账交易hash信息)

```java
   explore.requestHistoricTransWithCur(uuid, page, size, begin, end, type, currency, jCallback);
```
