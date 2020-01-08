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

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#11-%E8%8E%B7%E5%8F%96%E9%85%8D%E7%BD%AE%E6%95%B0%E6%8D%AE)

#### 1.1 获取配置数据

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

#### 2.1 获取钱包余额

```java
   exchange.requestBalance(address, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#21-%E8%8E%B7%E5%8F%96%E9%92%B1%E5%8C%85%E4%BD%99%E9%A2%9D)

#### 2.2 获取交易历史记录

```java
   exchange.requestHistoricTransactions(address,page,ledger,seq,jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#22-%E8%8E%B7%E5%8F%96%E4%BA%A4%E6%98%93%E5%8E%86%E5%8F%B2%E8%AE%B0%E5%BD%95)

#### 2.3 获取当前挂单

```java
   exchange.requestOrders(address, page, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#24-%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E6%8C%82%E5%8D%95)

#### 2.4 创建挂单

```java
   exchange.createOrder(signature, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#25-%E5%88%9B%E5%BB%BA%E6%8C%82%E5%8D%95)

#### 2.5 取消挂单

```java
   exchange.cancelOrder(signature, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#26-%E5%8F%96%E6%B6%88%E6%8C%82%E5%8D%95)

#### 2.6 获取序列号

```java
   exchange.requestSequence(address);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#27-%E8%8E%B7%E5%8F%96%E5%BA%8F%E5%88%97%E5%8F%B7)

#### 2.7 转账

```java
   exchange.transferToken(signature, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#28-%E8%BD%AC%E8%B4%A6)

#### 2.8 获取订单详情

```java
   exchange.requestOrderDetail(hash, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#29-%E8%8E%B7%E5%8F%96%E8%AE%A2%E5%8D%95%E8%AF%A6%E6%83%85)

### 3. 数据接口

```java
   // JccdexUrl jccUrl = new JccdexUrl("xxx", true);// https://xxx:443
   // JccdexUrl jccUrl = new JccdexUrl("xxx", false);// http://xxx:80
   JccdexUrl jccUrl = new JccdexUrl("xxx", true, 8081);
   JccdexInfo info = JccdexInfo.getInstance();
info.setmBaseUrl(jccUrl);
```

#### 3.1 获取指定币种24小时的行情数据

```java
   info.requestTicker(base, counter, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#31-%E8%8E%B7%E5%8F%96%E6%8C%87%E5%AE%9A%E5%B8%81%E7%A7%8D24%E5%B0%8F%E6%97%B6%E7%9A%84%E8%A1%8C%E6%83%85%E6%95%B0%E6%8D%AE)

#### 3.2 获取所有币种24小时的行情数据

```java
   info.requestAllTickers(jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#32-%E8%8E%B7%E5%8F%96%E6%89%80%E6%9C%89%E5%B8%81%E7%A7%8D24%E5%B0%8F%E6%97%B6%E7%9A%84%E8%A1%8C%E6%83%85%E6%95%B0%E6%8D%AE)

#### 3.3 获取市场深度

```java
   info.requestDepth(base, counter, type, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#33-%E8%8E%B7%E5%8F%96%E5%B8%82%E5%9C%BA%E6%B7%B1%E5%BA%A6)
  
#### 3.4 获取K线数据

```java
   info.requestKline(base, counter, type, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#34-%E8%8E%B7%E5%8F%96k%E7%BA%BF%E6%95%B0%E6%8D%AE)

#### 3.5 获取分时数据

```java
   info.requestHistory(base, counter, type, time, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#35-%E8%8E%B7%E5%8F%96%E5%88%86%E6%97%B6%E6%95%B0%E6%8D%AE)

#### 3.6 获取币种间汇率

```java
   info.requestTickerFromCMC(token, currency, jCallback)
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-RPC-Server.md#36-%E8%8E%B7%E5%8F%96%E5%B8%81%E7%A7%8D%E9%97%B4%E6%B1%87%E7%8E%87)

### 4. 浏览器接口

```java
   JccdexUrl jccUrl = new JccdexUrl("expjia1b6719b6e6.jccdex.cn", true);
   JccExplore explore = JccExplore.getInstance();
   explore.setmBaseUrl(jccUrl);
```

#### 4.1 获取钱包余额

```java
   explore.requestBalance(uuid, address, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-Explorer-Server.md#51-%E6%8C%87%E5%AE%9A%E9%92%B1%E5%8C%85%E7%9A%84%E4%BD%99%E9%A2%9D%E6%9F%A5%E8%AF%A2%E5%8C%85%E6%8B%AC%E6%89%80%E6%9C%89token%E7%9A%84%E4%BD%99%E9%A2%9D%E6%89%80%E6%9C%89token%E7%9A%84%E5%86%BB%E7%BB%93%E6%95%B0%E9%87%8F)

#### 4.2 根据hash获取详细交易信息

```java
   explore.requestTransDetails(uuid, hash, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-Explorer-Server.md#3-%E6%A0%B9%E6%8D%AE%E5%93%88%E5%B8%8C%E6%9F%A5%E8%AF%A2%E4%BA%A4%E6%98%93%E8%AF%A6%E7%BB%86)

#### 4.3 获取指定钱包的历史交易

```java
   explore.requestHistoricTransWithAddr(uuid, page, size, begin, end, type, currency, address, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-Explorer-Server.md#53-%E6%8C%87%E5%AE%9A%E9%92%B1%E5%8C%85%E7%9A%84%E5%8E%86%E5%8F%B2%E4%BA%A4%E6%98%93%E6%9F%A5%E8%AF%A2)

#### 4.4 获取钱包每天/月/年支付或收到的币种笔数和数量

```java
   explore.requestPaymentSummary(uuid, address, dateTpye, begin, end, type, token, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-Explorer-Server.md#11-%E6%9F%A5%E8%AF%A2%E6%9F%90%E4%B8%AA%E9%92%B1%E5%8C%85%E6%AF%8F%E5%A4%A9%E6%9C%88%E5%B9%B4%E7%9A%84%E6%94%AF%E4%BB%98%E6%88%96%E6%94%B6%E5%88%B0%E6%9F%90%E4%B8%AAtoken%E7%9A%84%E7%AC%94%E6%95%B0%E5%92%8C%E6%95%B0%E9%87%8F)

#### 4.5 查询某个token在某个时间段内的交易hash信息

```java
   explore.requestHistoricTransWithCur(uuid, page, size, begin, end, type, currency, jCallback);
```

* [接口详情](https://github.com/JCCDex/JingChang-Document/blob/master/zh-CN/Jingchang-Explorer-Server.md#10-%E6%9F%A5%E8%AF%A2%E6%9F%90%E4%B8%AAtoken%E5%9C%A8%E6%9F%90%E4%B8%AA%E6%97%B6%E9%97%B4%E6%AE%B5%E5%86%85%E7%9A%84%E8%BD%AC%E8%B4%A6%E4%BA%A4%E6%98%93hash%E4%BF%A1%E6%81%AF)
