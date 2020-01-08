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

### 1. Config API

```java
   JccdexUrl jccUrl = new JccdexUrl("weidex.vip", true);
   JccConfig config = JccConfig.getInstance();
   config.setmBaseUrl(jccUrl);
```

#### 1.1 Query config of jccdex

```java
  config.requestConfig(jCallBack);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#11-get-config-data)

### 2. Exchange API

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

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#21-get-balances)

#### 2.2 Query transaction history

```java
   exchange.requestHistoricTransactions(address,page,ledger,seq,jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#22-get-transaction-history)

#### 2.3 Query the current orders

```java
   exchange.requestOrders(address, page, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#24-get-the-current-pending-order)

#### 2.4 Create order

```java
   exchange.createOrder(signature, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#25-create-ordera)

#### 2.5 Cancel order

```java
   exchange.cancelOrder(signature, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#26-cancel-order)

#### 2.6 Get sequence

```java
   exchange.requestSequence(address);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#27-get-sequence)

#### 2.7 Payment

```java
   exchange.transferToken(signature, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#28-transfer)

#### 2.8 Query the order details

```java
   exchange.requestOrderDetail(hash, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#29-get-order-details)

### 3. Info API

```java
   // JccdexUrl jccUrl = new JccdexUrl("xxx", true);// https://xxx:443
   // JccdexUrl jccUrl = new JccdexUrl("xxx", false);// http://xxx:80
   JccdexUrl jccUrl = new JccdexUrl("xxx", true, 8081);
   JccdexInfo info = JccdexInfo.getInstance();
info.setmBaseUrl(jccUrl);
```

#### 3.1 Query 24-hour market data for the specified currency

```java
   info.requestTicker(base, counter, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#31-get-24-hour-market-data-for-the-specified-currency)

#### 3.2 Query 24-hour market data of all currencies

```java
   info.requestAllTickers(jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#32-get-24-hour-market-data-for-all-currencies)

#### 3.3 Query the market depth

```java
   info.requestDepth(base, counter, type, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#33-get-market-depth)
  
#### 3.4 Query K-line data

```java
   info.requestKline(base, counter, type, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#34-get-k-line-data)

#### 3.5 Query time-sharing data

```java
   info.requestHistory(base, counter, type, time, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#35-get-time-sharing-data)

#### 3.6 Query exchange rate

```java
   info.requestTickerFromCMC(token, currency, jCallback)
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-RPC-Server-EN.md#36-get-inter-currency-exchange-rates)

### 4. Explorer API

```java
   JccdexUrl jccUrl = new JccdexUrl("expjia1b6719b6e6.jccdex.cn", true);
   JccExplore explore = JccExplore.getInstance();
   explore.setmBaseUrl(jccUrl);
```

#### 5.1 Query the balance

```java
   explore.requestBalance(uuid, address, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-Explorer-Server-EN.md#51-query-the-balance-of-the-specified-wallet-including-the-balance-of-all-tokens-and-the-frozen-quantity-of-all-tokens
)

#### 5.2 Query transaction details by hash

```java
   explore.requestTransDetails(uuid, hash, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-Explorer-Server-EN.md#3-query-transaction-details-by-hash)

#### 5.3 Query historical transaction of specified wallet

```java
   explore.requestHistoricTransWithAddr(uuid, page, size, begin, end, type, currency, address, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-Explorer-Server-EN.md#53-query-historical-transaction-of-specified-wallet)

#### 5.4 Query the daily/month/ year payment or receipt of a certain wallet for a certain wallet

```java
   explore.requestPaymentSummary(uuid, address, dateTpye, begin, end, type, token, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-Explorer-Server-EN.md#11-query-the-dailymonth-year-payment-or-receipt-of-a-certain-wallet-for-a-certain-wallet)

#### 5.5 Query the transfer hash information of a certain transaction within a certain period of time

```java
   explore.requestHistoricTransWithCur(uuid, page, size, begin, end, type, currency, jCallback);
```

* [Interface doc](https://github.com/JCCDex/JingChang-Document/blob/master/EN/Jingchang-Explorer-Server-EN.md#10-query-the-transfer-hash-information-of-a-certain-transaction-within-a-certain-period-of-time)
