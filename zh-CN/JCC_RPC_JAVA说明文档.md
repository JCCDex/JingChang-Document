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

#### 1.1 获取配置数据

```java
  config.requestConfig(jCallBack);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#1.1-获取配置数据)

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

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|-|-|钱包地址
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.1-获取钱包余额)

#### 2.2 获取交易历史记录

```java
   exchange.requestHistoricTransactions(address,page,ledger,seq,jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|-|-|钱包地址
   page|int|-|-|当前页
   ledger|int|-|-|账本号，首次查询可以省略，之后值为上次查询返回的ledger值
   seq|int|-|-|上次查询的返回seq值，首次查询可以省略
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.2-获取交易历史记录)

#### 2.3 获取当前挂单

```java
   exchange.requestOrders(address, page, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|-|-|钱包地址
   page|int|-|-|当前页
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.4-获取当前挂单)

#### 2.4 创建挂单

```java
   exchange.createOrder(signature, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   sign|String|-|-|签名
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.5-创建挂单)

#### 2.5 取消挂单

```java
   exchange.cancelOrder(signature, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   sign|String|-|-|签名
   jCallback|JCallback|-|-|回调

* 返回结果

  * 同[2.4](#2.4-创建挂单)

#### 2.6 获取序列号

```java
   exchange.requestSequence(address);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   address|String|-|-|钱包地址

* 返回结果
  参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   sequence|int|-|-|最新序列号

#### 2.7 转账

```java
   exchange.transferToken(signature, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名
   jCallback|JCallback|-|-|回调

* 返回结果

  * 同[2.4](#2.4-创建挂单)

#### 2.8 获取订单详情

```java
   exchange.requestOrderDetail(hash, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   hash|String|是|-|-|交易哈希
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#2.9-获取订单详情)

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

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   base|String|-|-|基准币种
   counter|String|-|-|目标币种
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.1-获取指定币种24小时的行情数据)

#### 3.2 获取所有币种24小时的行情数据

```java
   info.requestAllTickers(jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.2-获取所有币种24小时的行情数据)

#### 3.3 获取市场深度

```java
   info.requestDepth(base, counter, type, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   base|String|-|-|基准币种
   counter|String|-|-|目标币种
   type|String|normal : 数据长度为5<br>more : 数据长度为50|-|类型
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.3-获取市场深度)
  
#### 3.4 获取K线数据

```java
   info.requestKline(base, counter, type, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   base|String|-|-|基准币种
   counter|String|-|-|目标币种
   type|String|minute、5minute、15minute、30minute、<br>hour、4hour、day、week、mouth|-|类型
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.4-获取K线数据)

#### 3.5 获取分时数据

```java
   info.requestHistory(base, counter, type, time, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   base|String|-|-|基准币种
   counter|String|-|-|目标币种
   type|String|all、more、newest|-|类型
   time|String|type=`newest`时|-|-|时间戳
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.5-获取分时数据)

#### 3.6 获取币种间汇率

```java
   info.requestTickerFromCMC(token, currency, jCallback)
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   token|String|-|-|支持eth、btc
   currency|String|-|-|支持cny、rub
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-JCC-RPC-Detail.md#3.6-获取币种间汇率)

### 4. 浏览器接口

```java
   JccdexUrl jccUrl = new JccdexUrl("expjia1b6719b6e6.jccdex.cn", true);
   JccExplore explore = JccExplore.getInstance();
   explore.setmBaseUrl(jccUrl);
```

#### 5.1 获取钱包余额

```java
   explore.requestBalance(uuid, address, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|唯一id
   address|String|-|-|钱包地址
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-Explorer-Server.md#5.1-指定钱包的余额查询（包括所有Token的余额、所有Token的冻结数量）)

#### 5.2 根据hash获取详细交易信息

```java
   explore.requestTransDetails(uuid, hash, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|唯一id
   hash|String|-|-|交易哈希
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-Explorer-Server.md#3.-根据哈希查询交易详细)

#### 5.3 获取指定钱包的历史交易

```java
   explore.requestHistoricTransWithAddr(uuid, page, size, begin, end, type, currency, address, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|唯一id
   page|int|-|-|页数，从0开始
   size|int|10/20/50/100|20|每页条数
   begin|String|-|-|开始日期，形如YYYY-MM-DD
   end|String|-|-|结束日期，形如YYYY-MM-DD
   type|String|OfferCreate/OfferAffect/OfferCancel/Send/Receive|-|交易类型（多个类型以逗号分隔，可以不传值，不传值表示查询所有类型）
   currency|String|-|-|交易对或币种（可以不传值，不传值表示币种不作为查询条件。在t=OfferCreate或OfferAffect或OfferCancel时，传值必须形如：SWTC-CNY或swtc-cny，必须是交易对本来的顺序base-counter的形式，比如这里不能是CNY-SWTC，否则买卖关系可能就乱了，另外交易对可以只指定base或counter，如swtc-或-cny，所有的交易对请参见附录；在t=Send或Receive时，传值必须长度<8，如JJCC）
   address|String|-|-|钱包地址
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-Explorer-Server.md#5.3-指定钱包的历史交易查询)

#### 5.4 获取钱包每天/月/年支付或收到的币种笔数和数量

```java
   explore.requestPaymentSummary(uuid, address, dateTpye, begin, end, type, token, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|唯一id
   address|String|-|-|钱包地址
   dateTpye|int|2: 日<br>3:月<br>4:年|-|日期类型
   begin|String|-|-|具体某一天/月/年的日期，若dt=2，则必须形如YYYY-MM-DD；若dt=3，则必须形如YYYY-MM；若dt=4，则必须形如YYYY
   end|String|-|-|格式要求同b参数。若指定e的值，则累加统计b～e时间段范围内相同token的值，另外e的值必须大于b的值。对于dt=2或3时，b和e之间天数之差不能超过一年的时间；dt=4时，b和e之间的间隔没有限制
   type|String|Send/Receive|-|交易类型
   token|String|-|-|token名称（SWT无需发行方，值为SWTC_，其他token需要带发行方，币种大写, 如c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-Explorer-Server.md#11.-查询某个钱包每天/月/年的支付或收到某个token的笔数和数量)

#### 5.5 查询某个token在某个时间段内的交易hash信息

```java
   explore.requestHistoricTransWithCur(uuid, page, size, begin, end, type, currency, jCallback);
```

* 请求参数

   参数|类型|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--
   uuid|String|-|-|唯一id
   page|int|-|-|页数，从0开始
   size|int|10/20/50/100|20|每页条数
   begin|String|-|-|开始日期，形如YYYY-MM-DD
   end|String|-|-|结束日期，形如YYYY-MM-DD
   type|String|Payment|-|交易类型
   currency|String|-|-|token名称
   jCallback|JCallback|-|-|回调

* [返回结果](../Jingchang-Explorer-Server.md#10.-查询某个token在某个时间段内的转账交易hash信息)
