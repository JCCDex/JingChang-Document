<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# Jingchang Explorer Server

## Server interface design

### 1. Query transaction list

#### 1.1 Query the latest 6 transaction list

* route

   `/trans/new/:uuid`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optional |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
     &emsp;list|Array|transaction list|-|-
     &emsp;&emsp;_id|String|transaction hash|-|-
     &emsp;&emsp;type|String|transaction type|-|[Transaction Type](#Transaction-Type)
     &emsp;&emsp;time|Number|transaction time|-|-
     &emsp;&emsp;past|Number|the number of seconds since the transaction passed|-|-
     &emsp;&emsp;account|String|transaction initiator wallet address|-|-
     &emsp;&emsp;succ|String|whether the transaction result is successful|"tesSUCCESS" means success|-

   1. when type=`Payment`，`data.list` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         dest|String|transfer counterparty address|-|-
         amount|[Amount](#Amount-Object)|token and amount|-|-

   2. when type=`OfferCreate`，`data.list` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         takerGets|[Amount](#Amount-Object)|token and amount of paying|-|-
         takerPays|[Amount](#Amount-Object)|token and amount of getting|-|-
         realGets|[Amount](#Amount-Object)|actual paid token and amount(excluding immediate deal)|-|-
         realPays|[Amount](#Amount-Object)|actual got token and amount(excluding immediate deal)|-|-
         flag|Number|buy or sell|-|[Flag Type](#Flag-Type)

   3. when type=`OfferCancel`，`data.list` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         takerGets|[Amount](#Amount-Object)|paid token and amount of canceled transaction|may not exist|-
         takerPays|[Amount](#Amount-Object)|got token and amount of canceled transaction|may not exist|-
         flag|Number|buy or sell|Only be exist when `takerGets` and `takerPays` is exist|[Flag Type](#Flag-Type)

#### 1.2 Query all transaction list

* route

   `/trans/all/:uuid?p=&s=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|true|10/20/50/100|20|size per page

* response

  * see [1.1](#11-Query-the-latest-6-transaction-list)

  * Can only query up to 1 million transaction records (calculated based on 100 transactions per block, about one day of transaction data)

### 2. Query block list

#### 2.1 Query the latest 6 blocks

* route

   `/block/new/:uuid`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
     &emsp;list|Array|-|-|-
     &emsp;&emsp;_id|Number|block height|-|-
     &emsp;&emsp;time|Number|block closed time|-|-
     &emsp;&emsp;past|Number|the number of seconds the block has elapsed from now|-|-
     &emsp;&emsp;transNum|Number|number of transactions in the block|-|-
     &emsp;&emsp;hash|String|block hash|-|-
     &emsp;&emsp;parentHash|String|hash of the previous block|-|-

#### 2.2 Query all blocks

* route

   `/block/all/:uuid?p=&s=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|true|10/20/50/100|20|size per page

* response

  * see [2.1](#21-Query-the-latest-6-blocks)

  * Can only query up to 3 million block records (about one year of block data)

### 3. Query transaction details by hash

* route

   `/hash/detail/:uuid?h=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   h|String|true|-|-|hash

* response

   1. If is transaction hash

      Key|Type|Description|Remark|Possible
      :--|:--:|:--|:--|:--
      code|String|-|"0" means success|-
      msg|String|message description|-|-
      data|Object|-|-|-
      &emsp;_id|String|transaction hash|-|-
      &emsp;hashType|Number|hash type|1:block hash<br>2:transaction hash|2
      &emsp;upperHash|String|block hash|-|-
      &emsp;block|Number|block height|-|-
      &emsp;time|Number|occurred time|-|-
      &emsp;past|Number|the number of seconds since the transaction passed|-|-
      &emsp;type|String|transaction type|-|[Transaction Type](#Transaction-Type)
      &emsp;account|String|transaction initiator wallet address|-|-
      &emsp;seq|Number|sequence|-|-
      &emsp;fee|Number|transaction gas|unit is `SWTC`, decimal, minimum 0.00001|-
      &emsp;succ|String|if success|"tesSUCCESS" means success|-

      1. when type=`Payment`，`data` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         dest|String|destination address|-|-
         amount|[Amount](#Amount-Object)|token and amount|-|-
         memos|Array|memo|-|-
         &emsp;Memo|[Memo](#Memo-Object)|memo data|-|-

      2. when type=`OfferCreate`，`data` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         flag|Number|buy/sell|-|[Flag Type](#Flag-Type)
         takerGets|[Amount](#Amount-Object)|token and amount of paying|-|-
         takerPays|[Amount](#Amount-Object)|token and amount of getting|-|-
         realGets|[Amount](#Amount-Object)|the token and amount paid for the actual transaction(deducting the immediate transaction)|is not exist if the transaction is filled immediately|-
         realPays|[Amount](#Amount-Object)|the token and amount got from the actual transaction(deducting the immediate transaction)|is not exist if the transaction is filled immediately|-
         matchGets|[Amount](#Amount-Object)|token and amount actual paid|is not exist if there is no actual deal|-
         matchPays|[Amount](#Amount-Object)|token and amount actual got|is not exist if there is no actual deal|-
         matchFlag|Number|match flag|is not exist if there is no match, indicates a multi-party match, such as 3 for a three-party match|-
         affectedNodes|Array|part of the deal for immediate transaction (from the perspective of a passive transaction wallet)|analyze `matchGets`, `matchPays`, and `matchFlag` based on the array|-
         &emsp;&emsp;account|String|wallet address of passive transaction|-|-
         &emsp;&emsp;seq|Number|sequence of passive deal|-|-
         &emsp;&emsp;flag|Number|transaction type of passive deal|-|[Flag Type](#Flag-Type)
         &emsp;&emsp;previous|Object|token and amount of transaction pair before passive transaction|may not exist. Without this field, it means that this passive transaction record is to cancel your own reverse pending order. This situation will happen when your new pending order will eat your previous reverse pending order, which means you are not allowed to eat it yourself. Drop your own pending order. When this happens, you will first cancel your previous reverse pending order and then place the new order|-
         &emsp;&emsp;final|Object|amount after passive deal|-|-

      3. when type=`OfferCancel`，`data` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         offerSeq|Number|sequence of canceled transaction|-|-
         takerGets|[Amount](#Amount-Object)|paid token and amount of canceled transaction|-|-
         takerPays|[Amount](#Amount-Object)|got token and amount of canceled transaction|-|-

   2. If is block hash

      Key|Type|Description|Remark|Possible
      :--|:--:|:--|:--|:--
      code|String|-|"0" means success|-
      msg|String|message description|-|-
      data|Object|-|-|-
      &emsp;list|Array|-|details see previous|-
      &emsp;count|Number|total number of transactions in the block|-|-
      &emsp;info|Object|block info|-|-
      &emsp;&emsp;_id|String|block hash|-|-
      &emsp;&emsp;hashType|Number|hash type|1:block hash；2:transaction hash|1
      &emsp;&emsp;upperHash|String|block hash|-|""
      &emsp;&emsp;block|Number|block height|-|-
      &emsp;&emsp;time|Number|block closed time|-|-
      &emsp;&emsp;past|Number|number of seconds since block elapsed|-|-
      &emsp;&emsp;transNum|Number|number of transactions in the block|-|-
      &emsp;&emsp;parentHash|String|hash of the previous block|-|-
      &emsp;&emsp;totalCoins|String|total amount of SWTC|divided by 1000000 to get the actual SWTC amount|-

### 4. Query the list of transactions contained by block hash

* route

   `/hash/trans/:uuid?p=&s=&n=&h=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   h|String|true|-|-|transaction hash
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|true|10/20/50/100|20|size per page
   n|Number|true|-|-|number of transactions in the block

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;list|Array|-|For the description of most fields, see response of transaction hash query|-
   &emsp;&emsp;index|Number|sequence of the transaction in the block|sort by ascending|-

### 5. Specify wallet query

#### 5.1 Query the balance of the specified wallet (including the balance of all tokens and the frozen quantity of all tokens)

* route

   `/wallet/balance/:uuid?w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   w|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;_id|String|wallet address|-|-
   &emsp;(`token name`_`issuer address`)|Object|-|-|-
   &emsp;&emsp;value|String|balance|-|-
   &emsp;&emsp;frozen|String|frozen quantity|-|-

### 5.2 Query the current order of the specified wallet

* route

   `/wallet/offer/:uuid?p=&s=&c=&bs=&w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|false|10/20/50/100|20|size per page
   w|String|true|-|-|wallet address
   c|String|false|-|-|transaction pair (You can pass no value. If you do n’t pass a value, it means to query the order of all types of transaction pairs. For example: SWTC-CNY or swtc-cny, it must be in the form of the original sequential base-counter of the transaction pair. For example, it cannot be CNY -SWTC, otherwise the buying and selling relationship may be disordered. In addition, the transaction pair can only specify base or counter, such as swtc- or -cny. For all transaction pairs, please refer to the [appendix](#appendix)）
   bs|Number|false|0:buy and sell<br>1:buy<br>2:sell|0|delegate type, if passed by value must be used with `c` parameter

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;count|Number|number of order|-|-
   &emsp;list|Array|-|-|-
   &emsp;&emsp;time|Number|time of order|-|-
   &emsp;&emsp;past|Number|number of seconds since create|-|-
   &emsp;&emsp;hash|String|hash|-|-
   &emsp;&emsp;block|Number|block height|-|-
   &emsp;&emsp;flag|Number|buy/sell|-|[Flag Type](#Flag-Type)
   &emsp;&emsp;takerGets|[Amount](#Amount-Object)|pay token and amount|-|-
   &emsp;&emsp;takerPays|[Amount](#Amount-Object)|get token and amount|-|-

#### 5.3 Query historical transaction of specified wallet

* route

   `/wallet/trans/:uuid?p=&s=&b=&e=&t=&bs=&c=&w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|true|10/20/50/100|20|size per page
   b|String|false|-|-|start date，YYYY-MM-DD
   e|String|false|-|-|end date，YYYY-MM-DD
   t|String|false|OfferCreate/OfferAffect/OfferCancel/Send/Receive|-|transaction type (multiple types are separated by commas, and no value can be passed which means that all types are queried)
   c|String|false|-|-|交易对或币种（可以不传值，不传值表示币种不作为查询条件。在t=OfferCreate或OfferAffect或OfferCancel时，传值必须形如：SWTC-CNY或swtc-cny，必须是交易对本来的顺序base-counter的形式，比如这里不能是CNY-SWTC，否则买卖关系可能就乱了，另外交易对可以只指定base或counter，如swtc-或-cny，所有的交易对请参见附录；在t=Send或Receive时，传值必须长度<8，如JJCC）
   bs|Number|false|0:买或卖<br>1:买<br>2:卖|-|只有在t=OfferCreate或OfferAffect或OfferCancel时有效果
   w|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;list|Array|区块中包含的交易列表|-|-
   &emsp;type|String|交易类型|-|[Transaction Type](#Transaction-Type)
   &emsp;time|Number|交易发生的时间|-|-
   &emsp;past|Number|交易距现在过去的秒数|-|-
   &emsp;hash|String|交易哈希|-|-
   &emsp;block|Number|区块高度|-|-
   &emsp;fee|String|交易gas费用|OfferAffect和Receive时，fee=""|-
   &emsp;success|String|交易是否成功|"tesSUCCESS"表示成功|-
   &emsp;seq|Number|交易序号|对于OfferAffect和Receive，该字符无意义|-

   1. 当type=`Send`或`Receive`时，`data.list`包含

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         account|String|对方钱包地址|-|-
         amount|[Amount](#Amount-Object)|支付或收到的币种和数量|-|-

   2. 当type=`OfferCreate`时，`data.list`包含

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         flag|Number|买/卖|-|[Flag Type](#Flag-Type)
         matchFlag|Number|撮合标志|若没有撮合，则该字段不存在；数字: 表示多方撮合，比如3表示三方撮合|-
         takerGets|[Amount](#Amount-Object)|创建挂单时付出的币种和数量|-|-
         takerPays|[Amount](#Amount-Object)|创建挂单时得到的币种和数量|-|-
         takerGetsFact|[Amount](#Amount-Object)|立即成交剩余的实际挂单部分的付出币种和数量|如果挂单全部成交，则没有该字段|-
         takerPaysFact|[Amount](#Amount-Object)|立即成交剩余的实际挂单部分的得到币种和数量|如果挂单全部成交，则没有该字段|-
         takerGetsMatch|[Amount](#Amount-Object)|立即成交部分的付出币种和数量|如果没有立即成交，则没有该字段|-
         takerPaysMatch|[Amount](#Amount-Object)|立即成交部分的得到币种和数量|如果没有立即成交，则没有该字段|-

   3. 当type=`OfferAffect`时，`data.list`包含

        Key|Type|Description|Remark|Possible
        :--|:--:|:--|:--|:--
        flag|Number|买/卖|-|[Flag Type](#Flag-Type)
        matchFlag|Number|撮合标志|若没有撮合，则该字段不存在；数字: 表示多方撮合，比如3表示三方撮合|-
        takerGets|[Amount](#Amount-Object)|被动成交前挂单的付出币种和数量|-|-
        takerPays|[Amount](#Amount-Object)|被动成交前挂单的得到币种和数量|-|-
        takerGetsFact|[Amount](#Amount-Object)|被动成交剩余部分的付出币种和数量|如果全部被动成交，则没有该字段|-
        takerPaysFact|[Amount](#Amount-Object)|被动成交剩余部分的得到币种和数量|如果全部被动成交，则没有该字段|-
        takerGetsMatch|[Amount](#Amount-Object)|被动成交部分的付出币种和数量|-|-
        takerPaysMatch|[Amount](#Amount-Object)|被动成交部分的得到币种和数量|-|-

   4. 当type=`OfferCancel`时，`data.list`包含

        Key|Type|Description|Remark|Possible
        :--|:--:|:--|:--|:--
        offerSeq|Number|被撤消挂单的序号|-|-
        flag|Number|被撤消挂单的交易性质|-|[Flag Type](#Flag-Type)
        takerGets|[Amount](#Amount-Object)|被撤消挂单的付出币种和数量|账本中经常出现一个挂单被多次撤消的情况，所以该字段可能没有|-
        takerPays|[Amount](#Amount-Object)|被撤消挂单的得到币种和数量|账本中经常出现一个挂单被多次撤消的情况，所以该字段可能没有|-

#### 5.4 指定钱包收益分析

* route

   `/wallet/profit/:uuid?t=&v=&w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|Number|true|1:日<br>2:周<br>3:月<br>4:年|-|Type
   w|String|true|-|-|wallet address

### 6. 持仓排行

* 设计说明：后台设计一个数据库 skywell_sum目前存放两个表
   1. tokens: 存放所有tokens列表
   2. tokensSort: 存放所有tokens的前100数据，不够100显示所有，每天生成 一次数据，目前160多种tokens，每天增长7000多条，数据量不大，放在一个表

#### 6.1 获取所有tokens列表

* route

   `/sum/all/:uuid?t=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|String|false|-|-|token名称

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;(key为token首字母或`num`)|Array|-|-|-
   &emsp;&emsp;-|String|token名称和发行方地址，下划线连接|-|-

### 7. 获取某种token的100排名

* route

   `/sum/list/:uuid?t=&p=&s=&b=&e=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|String|false|-|-|token名称（SWT无需发行方，值为SWTC_，其他token需要带发行方，币种大写, 如c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|false|10/20/50/100|20|size per page
   b|String|false|-|-|开始日期，形如YYYY-MM-DD
   e|String|false|-|-|结束日期，形如YYYY-MM-DD

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
   &emsp;tokens|String|token名称和发行方地址，下划线连接|-|-
   &emsp;totalsupply|Number|总发行量|-|-
   &emsp;holders|Number|对应钱包持有数量|-|-
   &emsp;circulation|Number|流通量|-|-
   &emsp;issueDate|Number|token发行日期|-|-
   &emsp;flag |Number|是否跨链标志|-|0没有跨链，1表示跨链token

### 8. 查看自己排名

* route

   `/sum/self/:uuid?t=&w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|String|true|-|-|token名称（SWT无需发行方，值为SWTC_，其他token需要带发行方，币种大写, 如c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）
   w|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Number|个人排名| 如果没有持有，则data返回空|-

### 9. 查看银关地址发行过tokens

* route

   `/wallet/fingate_tokenlist/:uuid?w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   w|String|true|-|-|银关地址

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;(key为银关地址)|Array|-|-|-
   &emsp;&emsp;currency|String|token名称|-|-
   &emsp;&emsp;issuer|String|发行方地址|-|-

### 10. 查询某个token在某个时间段内的转账交易hash信息

* route

   `/hash/payment_trans/:uuid?p=&s=&b=&e=&t=&c=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|false|10/20/50/100|20|size per page
   b|String|false|-|-|开始日期，形如YYYY-MM-DD
   e|String|false|-|-|结束日期，形如YYYY-MM-DD
   t|String|true|Payment|-|交易类型
   c|String|true|-|-|token名称

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;count|Number|查询结果的个数|-|-
   &emsp;list|Array|-|-|-
   &emsp;&emsp;_id|String|交易哈希|-|-
   &emsp;&emsp;hashType|Number|哈希类型|1:区块哈希<br>2:交易哈希|2
   &emsp;&emsp;time|Number|时间戳|与当前时间换算关系new Date((time+946684800)*1000)|-
   &emsp;&emsp;index|Number|区块内的交易编号|-|-
   &emsp;&emsp;type|String|交易类型|-|[Transaction Type](#Transaction-Type)
   &emsp;&emsp;account|String|交易发起方钱包地址|-|-
   &emsp;&emsp;seq|Number|交易序列号|-|-
   &emsp;&emsp;fee|Number|交易gas费用|单位是SWTC，小数，最小0.00001|-
   &emsp;&emsp;succ|String|交易是否成功|"tesSUCCESS" means success|-
   &emsp;&emsp;dest|String|对方钱包地址|-|-
   &emsp;&emsp;amount|[Amount](#Amount-Object)|支付币种和数量|-|-

### 11. 查询某个钱包每天/月/年的支付或收到某个token的笔数和数量

* route

   `/sum/payment_summary/:uuid?w=&b=&dt=&t=&c=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   w|String|true|-|-|wallet address
   dt|Number|true|2: 日<br>3:月<br>4:年|-|日期类型
   b|String|true|-|-|具体某一天/月/年的日期，若dt=2，则必须形如YYYY-MM-DD；若dt=3，则必须形如YYYY-MM；若dt=4，则必须形如YYYY
   e|String|false|-|-|格式要求同bParameter。若指定e的值，则累加统计b～e时间段范围内相同token的值，另外e的值必须大于b的值。对于dt=2或3时，b和e之间天数之差不能超过一年的时间；dt=4时，b和e之间的间隔没有限制
   t|String|false|Send/Receive|-|交易类型
   c|String|false|-|-|token名称（SWT无需发行方，值为SWTC_，其他token需要带发行方，币种大写, 如c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
   &emsp;wallet|String|wallet address|-|-
   &emsp;token|String|token名称和发行方地址，下划线连接|-|-
   &emsp;type|String|支付/收到|-|Send:支付；Receive:收到
   &emsp;num|Number|总笔数|-|-
   &emsp;amount|Number|总数量|-|-

   1. 如果某个钱包有转出或收到某个token，这时若没有指定type类型，则会返回2条记录信息如果某个钱包有转出多个token，若没有指定token名称，则会返回多条记录。

### 12. 查询收费平台对应token的收费详情接口

* route

   `/wallet/fee_config/:uuid?p=&w=&k=&s=&t=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|false|10/20/50/100|20|size per page
   w|String|true|-|-|平台地址
   k|Number|false|1:gas费设置<br>2:手续费设置|2|收费类型
   t|String|false|-|-|token名称（SWT无需发行方，值为SWTC，其他token需要带发行方，币种大写, 如t=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;data[0]|Array|各币种收费列表|-|-
   &emsp;&emsp;platform|String|平台方钱包地址|-|-
   &emsp;&emsp;token|String|token名称和发行方地址，下划线连接|-|-
   &emsp;&emsp;den|Number|费率分母|-|-
   &emsp;&emsp;feeAccount|String|收费钱包地址|-|-
   &emsp;&emsp;num|Number|费率分子|-|-
   &emsp;&emsp;time|Number|设置时间|-|-
   &emsp;data[1]|Number|count|-|-

## Appendix

### Transaction Pair

```json
{
    "BASE_COUNTER": {
        "CNY": [
            { "base": "SWTC", "counter": "CNY" },
            { "base": "JJCC", "counter": "CNY" },
            { "base": "JMOAC", "counter": "CNY" },
            { "base": "JETH", "counter": "CNY" },
            { "base": "JSTM", "counter": "CNY" },
            { "base": "VCC", "counter": "CNY" },
            { "base": "JCALL", "counter": "CNY" },
            { "base": "JSLASH", "counter": "CNY" },
            { "base": "CSP", "counter": "CNY" }
        ],
        "JETH": [
            { "base": "SWTC", "counter": "JETH" },
            { "base": "JMOAC", "counter": "JETH" }
        ],
        "SWTC": [
            { "base": "JJCC", "counter": "SWTC" },
            { "base": "JMOAC", "counter": "SWTC" },
            { "base": "JBIZ", "counter": "SWTC" },
            { "base": "JSLASH", "counter": "SWTC" },
            { "base": "JDABT", "counter": "SWTC" },
            { "base": "HJT", "counter": "SWTC" },
            { "base": "JSTM", "counter": "SWTC" },
            { "base": "JSNRC", "counter": "SWTC" },
            { "base": "MYT", "counter": "SWTC" },
            { "base": "JCALL", "counter": "SWTC" },
            { "base": "JEKT", "counter": "SWTC" },
            { "base": "BIC", "counter": "SWTC" },
            { "base": "YUT", "counter": "SWTC" },
            { "base": "VCC", "counter": "SWTC" },
            { "base": "JCKM", "counter": "SWTC" }
        ]
    }
}
```

### Amount Object

   Key|Type|Description
   :--:|:--:|:--
   currency|String|token name
   issuer|String|issuer address
   value|String|amount

### Transaction Type

Type|Description|Optinal
|:--:|:--|:--
String|transaction type|Payment/OfferCreate/OfferCancel/TrustSet/RelationSet<br>RelationDel/SetBlackList/RemoveBlackList/ManageIssuer/SetRegularKey

### Memo Object

Key|Type|Description|Remark
:--:|:--:|:--|:--
MemoData|String|Memo data|hex, [How to parse](https://github.com/swtcca/swtclib/blob/master/packages/swtc-serializer/src/types/STMemo.ts#L80)
MemoType|String|Memo type|hex, [How to parse](https://github.com/swtcca/swtclib/blob/master/packages/swtc-serializer/src/types/STMemo.ts#L80)

### Flag Type

Type|Description|Optinal
|:--:|:--|:--
Number|buy or sell|1:buy<br>2:sell<br>0:unknown
