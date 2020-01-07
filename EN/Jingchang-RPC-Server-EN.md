<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# JCC RPC

## Details

### 1. Config API

#### 1.1 Get config data

* route

   `/static/config/jc_config.json`

* method

   `get`

* request parameters

   --

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   exHosts|Object|exchange server list|-|-
   infoHosts|Object|info server list|-|-
   cfgHosts|Object|config server list|-|-

### 2. Exchange API

#### 2.1 Get balances

* route

   `/exchange/balances/:address`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Array|-|-|-
     &emsp;&emsp;value|String|balance, including frozen amount|-|-
     &emsp;&emsp;currency|String|token name|-|-
     &emsp;&emsp;issuer|String|issuer address|-|-
     &emsp;&emsp;freezed|String|frozen amount|-|-
     &emsp;&emsp;title|String|mirror name|-|-
     &emsp;&emsp;reserve|String|Number of token required to activate the address|-|-

#### 2.2 Get transaction history

* route

   `/exchange/tx/:address?ledger=&seq=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address
   ledger|Number|-|-|-|ledger number, which can be omitted for the first query, after which the value is the ledger value returned from the last query
   seq|Number|-|-|-|the last query returned the seq value, which can be omitted for the first query

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Object|-|-|-
     &emsp;marker|Object|record mark|-|-
     &emsp;&emsp;ledger|Number|ledger|-|-
     &emsp;&emsp;seq|Number|the last transaction number returned by the previous query|-|-
     &emsp;transactions|Array|-|-|-
     &emsp;&emsp;hash|String|hash|-|-
     &emsp;&emsp;time|Timestamp|transaction time|-|-
     &emsp;&emsp;type|String|transaction type|-|`sell`<br>`buy`
     &emsp;&emsp;pairs|String|transaction pair|-|-
     &emsp;&emsp;amount|String|amount|-|-
     &emsp;&emsp;sum|String|total price|-|-
     &emsp;&emsp;price|String|unit price|-|-
     &emsp;&emsp;status|String|transaction status (see below for details)|-|-
     &emsp;counterparty|Object|counterparty|-|-
     &emsp;&emsp;account|String|counterparty address|-|-
     &emsp;&emsp;seq|Number|sequence|-|-
     &emsp;&emsp;hash|String|hash|-|-

   Detailed description of the transaction status:

  * offer_funded: Actual transaction
  * offer_partially_funded: Partial transaction
  * offer_cancelled: Cancelled orders for related transactions, transaction orders cancelled
  * offer_created: Transaction order creation
  * offer_bought: Information on pending order buy / sell, deal order

#### 2.3 Get transfer history

* route

   `/exchange/payments/:address?ledger=&seq=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address
   ledger|Number|-|-|-|ledger number, which can be omitted for the first query, after which the value is the ledger value returned from the last query
   seq|Number|-|-|-|the last query returned the seq value, which can be omitted for the first query

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Object|-|-|-
     &emsp;marker|Object|record mark|-|-
     &emsp;&emsp;ledger|Number|ledger|-|-
     &emsp;&emsp;seq|Number|sequence|-|-
     &emsp;transactions|Array|-|-|-
     &emsp;&emsp;hash|String|hash|-|-
     &emsp;&emsp;time|Timestamp|time|-|-
     &emsp;&emsp;sender|String|sender address|-|-
     &emsp;&emsp;receiver|String|receiver address|-|-
     &emsp;&emsp;currency|String|token name|-|-
     &emsp;&emsp;amount|String|amount|-|-
     &emsp;&emsp;result|Boolean|-|-|-

#### 2.4 Get the current pending order

* route

   `/exchange/orders/:address/:page`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address
   page|Number|true|-|-|-

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Array|-|-|-
     &emsp;&emsp;type|String|type|-|`sell`<br>`buy`
     &emsp;&emsp;pair|String|pair|token name + issuer address|-
     &emsp;&emsp;hash|String|hash|-|-
     &emsp;&emsp;price|String|price|-|-
     &emsp;&emsp;amoount|String|amount|-|-
     &emsp;&emsp;sequence|String|sequence|-|-
     &emsp;&emsp;passive|Boolean|-|-|-

#### 2.5 Create order

* route

   `/exchange/sign_order`

* method

   `post`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   sign|String|true|-|-|signature

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Object|-|-|-
     &emsp;&emsp;hash|String|hash|-|-

#### 2.6 Cancel order

* route

   `/exchange/sign_cancel_order`

* method

   `post`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   sign|String|true|-|-|signature

* response

  * see [2.5](#2.5-Create-order)

#### 2.7 Get sequence

* route

   `/exchange/sequence/:address`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|wallet address

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Object|-|-|-
     &emsp;&emsp;sequence|Number|sequence|-|-

#### 2.8 Transfer

* route

   `/exchange/sign_payment`

* method

   `post`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   sign|String|true|-|-|signature

* response

  * see [2.5](#2.5-Create-order)
  
#### 2.9 Get order details

* route

   `/exchange/detail/:hash`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   hash|String|true|-|-|hash

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   isActive|Boolean|whether is active|-|-
   data|Object|-|-|-
     &emsp;&emsp;Account|String|wallet address|-|-
     &emsp;&emsp;[Amount](#Amount-Object)|Object|token and amount|-|-
     &emsp;&emsp;Destination|String|counterparty|-|-
     &emsp;&emsp;Fee|String|gas|-|-
     &emsp;&emsp;Flags|Number|buy or sell|-|[Flag Type](#Flag-Type)
     &emsp;&emsp;Memos|Array|remark|-|-
     &emsp;&emsp;Sequence|Number|sequence|-|-
     &emsp;&emsp;SigningPubKey|String|public key|-|-
     &emsp;&emsp;TransactionType|String|type|-|-
     &emsp;&emsp;date|Number|time|-|-
     &emsp;&emsp;hash|String|hash|-|-
     &emsp;&emsp;inLedger|Numer|ledger|-|-
     &emsp;&emsp;ledger_index|Numer|ledger index|-|-
     &emsp;&emsp;TxnSignature|String|signature|-|-

### 3. Info API

#### 3.1 Get 24-hour market data for the specified currency

* route

   `/info/ticker/:pari`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   pari|String|true|-|-|like `SWT-CNY`

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
     &emsp;&emsp;-|Timestamp|current time|-|-
     &emsp;&emsp;-|String|current price|-|-
     &emsp;&emsp;-|Number|daily change|(%)|-
     &emsp;&emsp;-|String|highest price|-|-
     &emsp;&emsp;-|String|lowest price|-|-
     &emsp;&emsp;-|Number|daily turnover|-|-
     &emsp;&emsp;-|Number|daily volume|-|-
     &emsp;&emsp;-|Number|number of transactions|-|-

#### 3.2 Get 24-hour market data for all currencies

* route

   `/info/alltickers`

* method

   `get`

* request parameters

   --

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
     &emsp;-|Array|-|-|SWT-JUSDT
     &emsp;&emsp;-|Timestamp|current time|-|-
     &emsp;&emsp;-|String|current price|-|-
     &emsp;&emsp;-|Number|daily change|(%)|-
     &emsp;&emsp;-|String|highest price|-|-
     &emsp;&emsp;-|String|lowest price|-|-
     &emsp;&emsp;-|Number|daily turnover|-|-
     &emsp;&emsp;-|Number|daily volume|-|-
     &emsp;&emsp;-|Number|number of transactions|-|-

#### 3.3 Get market depth

* route

   `/info/depth/:pari/:type`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   pari|String|true|-|-|like `SWT-CNY`
   type|String|true|`normal`: data length is 5<br>`more`: data length is 50|-|Type

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Object|-|-|-
     &emsp;base|String|base token name|-|-
     &emsp;counter|String|counter token name|-|-
     &emsp;asks|Array|ask orders|-|-
     &emsp;&emsp;price|String|price|-|-
     &emsp;&emsp;amount|String|amount|-|-
     &emsp;&emsp;total|String|total to current price|-|-
     &emsp;&emsp;type|String|Type|-|`buy`<br>`sell`
     &emsp;bids|Array|bid orders|-|-
     &emsp;&emsp;price|String|price|-|-
     &emsp;&emsp;amount|String|amount|-|-
     &emsp;&emsp;total|String|total to current price|-|-
     &emsp;&emsp;type|String|Type|-|`buy`<br>`sell`

#### 3.4 Get K-line data

* route

   `/info/kline/:pari/:type`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   pari|String|true|-|-|like `SWT-CNY`
   type|String|true|minute、5minute、15minute、30minute、<br>hour、4hour、day、week、mouth|-|Type

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
     &emsp;&emsp;-|Timestamp|order time|-|-
     &emsp;&emsp;-|String|opening price|-|-
     &emsp;&emsp;-|String|closing price|-|-
     &emsp;&emsp;-|String|highest price|-|-
     &emsp;&emsp;-|String|lowest price|-|-
     &emsp;&emsp;-|Number|daily turnover|-|-
     &emsp;&emsp;-|Number|daily volume|-|-
     &emsp;&emsp;-|Number|number of transactions|-|-

#### 3.5 Get time-sharing data

* route

   `/info/history/:pari/:type?time=`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   pari|String|true|-|-|like `SWT-CNY`
   type|String|true|`all`、`more`、`newest`|-|Type
   time|String|false|-|-|timestamp, is required if type is `newest`

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   code|String|-|"0" means success|-
   msg|String|message description|-|-
   data|Array|-|-|-
     &emsp;&emsp;-|String|daily turnover|-|-
     &emsp;&emsp;-|String|daily volume|-|-
     &emsp;&emsp;-|String|price|-|-
     &emsp;&emsp;-|Timestamp|time|-|-
     &emsp;&emsp;-|Number|change|-|0 : rise<br>1 : fall
     &emsp;&emsp;-|Number|match flag|-|1:non-matching<br>3: 3-match

#### 3.6 Get inter-currency exchange rates

* route

   `/:pair.json`

* method

   `get`

* request parameters

   Parameter|Type|Required|Optinal |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   pair|String|true|-|-|support `eth`、`btc`，`cny`、`rub`，like `eth-cny`

* response

   Key|Type|Description|Remark|Possible
   :--|:--:|:--|:--|:--
   data|Object|-|-|-
     &emsp;quotes|Object|daily turnover|-|-
     &emsp;&emsp;-|Object|rate|-|`currency`
     &emsp;&emsp;&emsp;price|Number|price|-|-
     &emsp;last_updated|Number|-|-|-
     &emsp;id|Number|-|-|-
     &emsp;symbol|String|change|-|`token`

### Amount Object

   Key|Type|Description
   :--:|:--:|:--
   currency|String|token name
   issuer|String|issuer address
   value|String|amount

### Flag Type

Type|Description|Possible
|:--:|:--|:--
Number|buy or sell|1:buy<br>2:sell<br>0:unknown
