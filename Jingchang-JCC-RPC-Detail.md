<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# JCC RPC

## 接口详细设计

### 1. 配置接口

#### 1.1 获取配置数据

* route

   `/static/config/jc_config.json`

* method

   `get`

* 请求参数

   --

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   exHosts|Object|交易服务器列表|-|-
   infoHosts|Object|信息服务器列表|-|-
   cfgHosts|Object|配置服务器列表|-|-

以下所有API请求对应的host都是基于exHosts或者infoHosts

* 交易接口请调用exHosts列表中服务器
* 信息接口请调用infoHosts列表中服务器
* 配置接口请调用cfgHosts列表服务器

### 2. 交易接口

#### 2.1 获取钱包余额

* route

   `/exchange/balances/:address`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Array|查询结果|-|-
     &emsp;&emsp;value|String|余额，包含冻结金额|-|-
     &emsp;&emsp;currency|String|token|-|-
     &emsp;&emsp;issuer|String|发行方|-|-
     &emsp;&emsp;freezed|String|冻结金额|-|-
     &emsp;&emsp;title|String|币种名称|-|-
     &emsp;&emsp;reserve|String|激活钱包所需币种数量|-|-

#### 2.2 获取交易历史记录

* route

   `/exchange/tx/:address?ledger=&seq=`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址
   ledger|Number|-|-|-|账本号，首次查询可以省略，之后值为上次查询返回的ledger值
   seq|Number|-|-|-|上次查询的返回seq值，首次查询可以省略

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Object|查询结果|-|-
     &emsp;marker|Object|记录标记|-|-
     &emsp;&emsp;ledger|Number|账本号|-|-
     &emsp;&emsp;seq|Number|上一次查询返回的最后一条交易号|-|-
     &emsp;transactions|Array|历史交易记录数据|-|-
     &emsp;&emsp;hash|String|交易hash|-|-
     &emsp;&emsp;time|Timestamp|交易时间|-|-
     &emsp;&emsp;type|String|交易类型|-|`sell`<br>`buy`
     &emsp;&emsp;pairs|String|交易对|-|-
     &emsp;&emsp;amount|String|交易数量|-|-
     &emsp;&emsp;sum|String|总价|-|-
     &emsp;&emsp;price|String|单价|-|-
     &emsp;&emsp;status|String|交易状态(详情见下)|-|-
     &emsp;counterparty|Object|交易对家|-|-
     &emsp;&emsp;account|String|交易对家账号|-|-
     &emsp;&emsp;seq|Number|交易对家交易号|-|-
     &emsp;&emsp;hash|String|交易hash|-|-

   交易状态详细说明：

  * offer_funded，交易实际成交
  * offer_partially_funded，交易部分成交
  * offer_cancelled，被关联交易取消单子，交易单子被取消
  * offer_created，交易单子创建
  * offer_bought，挂单买到/卖出，成交单子的信息

#### 2.3 获取转账历史记录

* route

   `/exchange/payments/:address?ledger=&seq=`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址
   ledger|Number|-|-|-|账本号，首次查询可以省略，之后值为上次查询返回的ledger值
   seq|Number|-|-|-|上次查询的返回seq值，首次查询可以省略

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Object|查询结果|-|-
     &emsp;marker|Object|记录标记|-|-
     &emsp;&emsp;ledger|Number|账本号|-|-
     &emsp;&emsp;seq|Number|交易号|-|-
     &emsp;transactions|Array|转账记录数据|-|-
     &emsp;&emsp;hash|String|转账hash|-|-
     &emsp;&emsp;time|Timestamp|转账时间|-|-
     &emsp;&emsp;sender|String|发送方钱包地址|-|-
     &emsp;&emsp;receiver|String|接收方钱包地址|-|-
     &emsp;&emsp;currency|String|币种|-|-
     &emsp;&emsp;amount|String|数量|-|-
     &emsp;&emsp;result|Boolean|-|-|-

#### 2.4 获取当前挂单

* route

   `/exchange/orders/:address/:page`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址
   page|Number|是|-|-|当前页

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Array|查询结果|-|-
     &emsp;&emsp;type|String|挂单类型|-|`sell`<br>`buy`
     &emsp;&emsp;pair|String|挂单交易对|币种+发行方|-
     &emsp;&emsp;hash|String|转账hash|-|-
     &emsp;&emsp;price|String|挂单价格|-|-
     &emsp;&emsp;amoount|String|挂单数量|-|-
     &emsp;&emsp;sequence|String|挂单序列号|-|-
     &emsp;&emsp;passive|Boolean|-|-|-

#### 2.5 创建挂单

* route

   `/exchange/sign_order`

* method

   `post`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Object|-|-|-
     &emsp;&emsp;hash|String|交易哈希|-|-

#### 2.6 取消挂单

* route

   `/exchange/sign_cancel_order`

* method

   `post`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名

* 返回结果

  * 同[2.5](#2.5-创建挂单)

#### 2.7 获取序列号

* route

   `/exchange/sequence/:address`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|钱包地址

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Object|-|-|-
     &emsp;&emsp;sequence|Number|序列号|-|-

#### 2.8 转账

* route

   `/exchange/sign_payment`

* method

   `post`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   sign|String|是|-|-|签名

* 返回结果

  * 同[2.5](#2.5-创建挂单)
  
#### 2.9 获取订单详情

* route

   `/exchange/detail/:hash`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   hash|String|是|-|-|交易哈希

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   isActive|Boolean|当前钱包是否激活|-|-
   data|Object|-|-|-
     &emsp;&emsp;Account|String|发起交易地址|-|-
     &emsp;&emsp;[Amount](#Amount-Object)|Object|挂单币种和数量|-|-
     &emsp;&emsp;Destination|String|交易对家|-|-
     &emsp;&emsp;Fee|String|gas费|-|-
     &emsp;&emsp;Flags|Number|买卖|-|[Flag Type](#Flag-Type)
     &emsp;&emsp;Memos|Array|备注|-|-
     &emsp;&emsp;Sequence|Number|序列号|-|-
     &emsp;&emsp;SigningPubKey|String|公共签名|-|-
     &emsp;&emsp;TransactionType|String|转账类型|-|-
     &emsp;&emsp;date|Number|时间|-|-
     &emsp;&emsp;hash|String|交易哈希|-|-
     &emsp;&emsp;inLedger|Numer|账本序号|-|-
     &emsp;&emsp;ledger_index|Numer|账本序号|-|-
     &emsp;&emsp;TxnSignature|String|交易签名|-|-

### 3. 数据接口

#### 3.1 获取指定币种24小时的行情数据

* route

   `/info/ticker/:pari`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   pari|String|是|-|-|币种对，如SWT-CNY

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   data|Array|查询结果|-|-
     &emsp;&emsp;-|Timestamp|当前时间|-|-
     &emsp;&emsp;-|String|当前价|-|-
     &emsp;&emsp;-|Number|日涨跌|(%)|-
     &emsp;&emsp;-|String|最高价|-|-
     &emsp;&emsp;-|String|最低价|-|-
     &emsp;&emsp;-|Number|日交易额|-|-
     &emsp;&emsp;-|Number|日成交量|-|-
     &emsp;&emsp;-|Number|成交笔数|-|-

#### 3.2 获取所有币种24小时的行情数据

* route

   `/info/alltickers`

* method

   `get`

* 请求参数

   --

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   data|Object|查询结果|-|-
     &emsp;-|Array|币种对|-|SWT-JUSDT
     &emsp;&emsp;-|Timestamp|当前时间|-|-
     &emsp;&emsp;-|String|当前价|-|-
     &emsp;&emsp;-|Number|日涨跌|(%)|-
     &emsp;&emsp;-|String|最高价|-|-
     &emsp;&emsp;-|String|最低价|-|-
     &emsp;&emsp;-|Number|日交易额|-|-
     &emsp;&emsp;-|Number|日成交量|-|-
     &emsp;&emsp;-|Number|成交笔数|-|-

#### 3.3 获取市场深度

* route

   `/info/depth/:pari/:type`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   pari|String|是|-|-|币种对，如SWT-CNY
   type|String|是|normal : 数据长度为5<br>more : 数据长度为50|-|类型

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   data|Object|查询结果|-|-
     &emsp;base|String|基准币种|-|-
     &emsp;counter|String|目标币种|-|-
     &emsp;asks|Array|卖出挂单|-|-
     &emsp;&emsp;price|String|价格|-|-
     &emsp;&emsp;amount|String|数量|-|-
     &emsp;&emsp;total|String|到当前价的总量|-|-
     &emsp;&emsp;type|String|类型|-|`buy`<br>`sell`
     &emsp;bids|Array|买入挂单|-|-
     &emsp;&emsp;price|String|价格|-|-
     &emsp;&emsp;amount|String|数量|-|-
     &emsp;&emsp;total|String|到当前价的总量|-|-
     &emsp;&emsp;type|String|类型|-|`buy`<br>`sell`

#### 3.4 获取K线数据

* route

   `/info/kline/:pari/:type`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   pari|String|是|-|-|币种对，如SWT-CNY
   type|String|是|minute、5minute、15minute、30minute、<br>hour、4hour、day、week、mouth|-|类型

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   data|Array|查询结果|-|-
     &emsp;&emsp;-|Timestamp|挂单时间|-|-
     &emsp;&emsp;-|String|开盘价|-|-
     &emsp;&emsp;-|String|收盘价|-|-
     &emsp;&emsp;-|String|最高价|-|-
     &emsp;&emsp;-|String|最低价|-|-
     &emsp;&emsp;-|Number|交易额|-|-
     &emsp;&emsp;-|Number|交易数量|-|-
     &emsp;&emsp;-|Number|成交笔数|-|-

#### 3.5 获取分时数据

* route

   `/info/history/:pari/:type?time=`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   pari|String|是|-|-|币种对，如SWT-CNY
   type|String|是|all、more、newest|-|类型
   time|String|type=`newest`时|-|-|时间戳

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   code|String|-|"0"表示成功|-
   msg|String|消息描述|-|-
   data|Array|查询结果|-|-
     &emsp;&emsp;-|String|交易额|-|-
     &emsp;&emsp;-|String|成交量|-|-
     &emsp;&emsp;-|String|价格|-|-
     &emsp;&emsp;-|Timestamp|时间|-|-
     &emsp;&emsp;-|Number|涨跌|-|0 : 涨<br>1 : 跌
     &emsp;&emsp;-|Number|撮合标志|-|1:非撮合<br>3:三方撮合

#### 3.6 获取币种间汇率

* route

   `/:pari.json`

* method

   `get`

* 请求参数

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   pari|String|是|-|-|币种对，如SWT-CNY

* 返回结果

   字段|类型|描述|备注|可能值
   :--|:--:|:--|:--|:--
   data|Object|查询结果|-|-
     &emsp;quotes|Object|交易额|-|-
     &emsp;&emsp;-|Object|汇率|-|`currency`
     &emsp;&emsp;&emsp;price|Number|价格|-|-
     &emsp;last_updated|Number|-|-|-
     &emsp;id|Number|-|-|-
     &emsp;symbol|String|涨跌|-|`token`

### Amount Object

   字段|类型|描述
   :--:|:--:|:--
   currency|String|币种名称
   issuer|String|发行方地址
   value|String|数量

### Flag Type

类型|描述|可能值
|:--:|:--|:--
Number|买或卖|1:买<br>2:卖<br>0:未知
