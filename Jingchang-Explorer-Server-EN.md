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
   &emsp;&emsp;past|Number|number of seconds since the transaction passed|-|-
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
   c|String|false|-|-|transaction pair or token（You can not pass a value, which means that the currency is not used as the query condition. When `t` is `OfferCreate`, `OfferAffect` or `OfferCancel`, the value passed must be in the form of: `SWTC-CNY` or `swtc-cny`, and must be in the form of the original sequence base-counter of the transaction pair. For example, it cannot be `CNY-SWTC`, otherwise the sales relationship may mess, in addition, the transaction pair can only specify base or counter, such as `swtc-` or `-cny`. For all transaction pairs, please refer to the [appendix](#appendix); when t is `Send` or `Receive`, the value must be less than 8 in length, such as `JJCC`）
   bs|Number|false|0:buy and sell<br>1:buy<br>2:sell|-|Only effective when `t` is `OfferCreate`, `OfferAffect` or `OfferCancel`
   w|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;list|Array|-|-|-
   &emsp;type|String|transaction type|-|[Transaction Type](#Transaction-Type)
   &emsp;time|Number|occurred time|-|-
   &emsp;past|Number|number of seconds since the transaction passed|-|-
   &emsp;hash|String|transaction hash|-|-
   &emsp;block|Number|block height|-|-
   &emsp;fee|String|transaction gas|when type is `OfferAffect` or `Receive`，fee is ""|-
   &emsp;success|String|if success|"tesSUCCESS" means success|-
   &emsp;seq|Number|transaction sequence|for type is `OfferAffect` or `Receive`, is meaningless|-

   1. when type is `Send` or `Receive` ，`data.list` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         account|String|counterparty wallet address|-|-
         amount|[Amount](#Amount-Object)|sent or received token and amount|-|-

   2. when type is `OfferCreate`，`data.list` includes

         Key|Type|Description|Remark|Possible
         :--|:--:|:--|:--|:--
         flag|Number|buy/sell|-|[Flag Type](#Flag-Type)
         matchFlag|Number|match flag|is not exist if there is no match, indicates a multi-party match, such as 3 for a three-party match|-
         takerGets|[Amount](#Amount-Object)|token and amount paid when creating a order|-|-
         takerPays|[Amount](#Amount-Object)|token and amount got when creating a order|-|-
         takerGetsFact|[Amount](#Amount-Object)|token and amount actual paid (deducting the immediate transaction)|is not exist if the transaction is filled immediately|-
         takerPaysFact|[Amount](#Amount-Object)|token and amount actual got (deducting the immediate transaction)|is not exist if the transaction is filled immediately|-
         takerGetsMatch|[Amount](#Amount-Object)|token and amount paid immediately|is not exist if there is no immediate deal|-
         takerPaysMatch|[Amount](#Amount-Object)|token and amount got immediately|is not exist if there is no immediate deal|-

   3. when type=`OfferAffect`，`data.list` includes

        Key|Type|Description|Remark|Possible
        :--|:--:|:--|:--|:--
        flag|Number|buy/sell|-|[Flag Type](#Flag-Type)
        matchFlag|Number|match flag|is not exist if there is no match, indicates a multi-party match, such as 3 for a three-party match|-
        takerGets|[Amount](#Amount-Object)|token and amount paid of order before passive transaction|-|-
        takerPays|[Amount](#Amount-Object)|token and amount got of order before passive transaction|-|-
        takerGetsFact|[Amount](#Amount-Object)|token and amount paid for the remainder of the passive transaction|is not exist if all transactions are passive|-
        takerPaysFact|[Amount](#Amount-Object)|token and amount got for the remainder of the passive transaction|is not exist if all transactions are passive|-
        takerGetsMatch|[Amount](#Amount-Object)|token and amount paid for passive transaction|-|-
        takerPaysMatch|[Amount](#Amount-Object)|token and amount got for passive transaction|-|-

   4. when type=`OfferCancel`，`data.list` includes

        Key|Type|Description|Remark|Possible
        :--|:--:|:--|:--|:--
        offerSeq|Number|sequence of canceled order|-|-
        flag|Number|type of canceled order|-|[Flag Type](#Flag-Type)
        takerGets|[Amount](#Amount-Object)|token and amount paid of canceled order|there is often a case where a pending order is cancelled in the ledger, so this field may not have|-
        takerPays|[Amount](#Amount-Object)|token and amount got of canceled order|there is often a case where a pending order is cancelled in the ledger, so this field may not have|-

#### 5.4 analysis of specified wallet

* route

   `/wallet/profit/:uuid?t=&v=&w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|Number|true|1:day<br>2:week<br>3:month<br>4:year|-|Type
   w|String|true|-|-|wallet address

### 6. Position ranking

#### 6.1 Get list of all tokens

* route

   `/sum/all/:uuid?t=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|String|false|-|-|token name

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;(key is first letter of token or `num`)|Array|-|-|-
   &emsp;&emsp;-|String|token name and issuer address, format: `name`_`issuer`|-|-

### 7. Get 100 ranks by token

* route

   `/sum/list/:uuid?t=&p=&s=&b=&e=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|String|false|-|-|token name（SWT does not require an issuer. The value is `SWTC_`. Other tokens need to be issued with the issuer in uppercase, like c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）
   p|Number|true|-|-|number of pages, starting from 0
   s|Number|false|10/20/50/100|20|size per page
   b|String|false|-|-|start date, YYYY-MM-DD
   e|String|false|-|-|end date，YYYY-MM-DD

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
   &emsp;tokens|String|token name and issuer address, format: `name`_`issuer`|-|-
   &emsp;totalsupply|Number|total circulation|-|-
   &emsp;holders|Number|wallet holdings|-|-
   &emsp;circulation|Number|liquidity|-|-
   &emsp;issueDate|Number|token issue date|-|-
   &emsp;flag |Number|whether cross-chain flag|-|0: no，1: yes

### 8. View your ranking

* route

   `/sum/self/:uuid?t=&w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   t|String|true|-|-|token name（SWT does not require an issuer. The value is `SWTC_`. Other tokens need to be issued with the issuer in uppercase, like c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）
   w|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Number|personal ranking| is empty if not hold|-

### 9. Query the tokens issued by the issuer address

* route

   `/wallet/fingate_tokenlist/:uuid?w=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   w|String|true|-|-|issuer address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;(key is issuer address)|Array|-|-|-
   &emsp;&emsp;currency|String|token name|-|-
   &emsp;&emsp;issuer|String|issuer address|-|-

### 10. Query the transfer hash information of a certain transaction within a certain period of time

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
   b|String|false|-|-|start date，YYYY-MM-DD
   e|String|false|-|-|end date，YYYY-MM-DD
   t|String|true|Payment|-|transaction type
   c|String|true|-|-|token name

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;count|Number|number of query results|-|-
   &emsp;list|Array|-|-|-
   &emsp;&emsp;_id|String|transaction|-|-
   &emsp;&emsp;hashType|Number|hash type|1:block hash<br>2:transaction hash|2
   &emsp;&emsp;time|Number|timestamp|unit: second, new Date((time+946684800)*1000)|-
   &emsp;&emsp;index|Number|transaction number within the block|-|-
   &emsp;&emsp;type|String|transaction type|-|[Transaction Type](#Transaction-Type)
   &emsp;&emsp;account|String|transaction initiator wallet address|-|-
   &emsp;&emsp;seq|Number|sequence|-|-
   &emsp;&emsp;fee|Number|transaction gas|unit is `SWTC`, decimal, minimum 0.00001|-
   &emsp;&emsp;succ|String|whether success|"tesSUCCESS" means success|-
   &emsp;&emsp;dest|String|counterparty wallet address|-|-
   &emsp;&emsp;amount|[Amount](#Amount-Object)|sent token and amount|-|-

### 11. Query the daily/month/ year payment or receipt of a certain wallet for a certain wallet

* route

   `/sum/payment_summary/:uuid?w=&b=&dt=&t=&c=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   uuid|String|true|-|-|unique id
   w|String|true|-|-|wallet address
   dt|Number|true|2: day<br>3:month<br>4:year|-|date type
   b|String|true|-|-|for a specific day/month/year date, if dt = 2, it must be like YYYY-MM-DD; if dt = 3, it must be like YYYY-MM; if dt = 4, it must be like YYYY
   e|String|false|-|-|like `b` parameter。If the value of `e` is specified, the value of the same token in the time range of `b` to `e` is accumulated, and the value of `e` must be greater than the value of `b`. when `dt` is 2 or 3, the difference in days between `b` and `e` cannot exceed one year; when `dt` is 4, there is no limit on the interval between `b` and `e`
   t|String|false|Send/Receive|-|transaction type
   c|String|false|-|-|token name（SWT does not require an issuer. The value is `SWTC_`. Other tokens need to be issued with the issuer in uppercase, like c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
   &emsp;wallet|String|wallet address|-|-
   &emsp;token|String|token name and issuer address, format: `name`_`issuer`|-|-
   &emsp;type|String|send/receive|-|Send；Receive
   &emsp;num|Number|total count|-|-
   &emsp;amount|Number|total amount|-|-

   1. If a wallet transfers or receives a token, if no type is specified, then 2 records will be returned. If a wallet has transferred multiple tokens, if no token name is specified, multiple returns records.

### 12. Query the charging details

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
   w|String|true|-|-|platform address
   k|Number|false|1:gas<br>2:charging|2|charge type
   t|String|false|-|-|token name（SWT does not require an issuer. The value is `SWTC_`. Other tokens need to be issued with the issuer in uppercase, like c=CNY_jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or）

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
   &emsp;data[0]|Array|-|-|-
   &emsp;&emsp;platform|String|platform address|-|-
   &emsp;&emsp;token|String|token name and issuer address, format: `name`_`issuer`|-|-
   &emsp;&emsp;den|Number|rate denominator|-|-
   &emsp;&emsp;feeAccount|String|received wallet address|-|-
   &emsp;&emsp;num|Number|rate numerator|-|-
   &emsp;&emsp;time|Number|sett time|-|-
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
