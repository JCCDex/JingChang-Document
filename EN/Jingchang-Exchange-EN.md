<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# Jingchang Exchange Server

## Features of Jingchang Exchange Server

1. Transaction signature

2. Commission order

3. Cancel order

4. Transfer

5. Set brokerage

## Install and Import

```javascript
npm install jcc_exchange
import JCCExchange from "jcc_exchange"
// const JCCExchange = require('jcc_exchange').JCCExchange
```

## Exchange Server interface detail description

### 1. Transaction signature

* sign function

  [function](https://github.com/JCCDex/jcc_exchange/blob/master/src/index.ts#L245)

* parameters description

  [specific signature parameters](https://github.com/JCCDex/jcc_exchange/blob/master/src/tx/index.ts)

   Parameter|Type|Required|Default|Description
   :--|:--:|:--:|:--:|:--
   secret|String|true|-|transaction initiator wallet secret
   token|String|false|swt|native token of different chains,support jingtum,bizain,seaaps chain,[ChainConfig](https://github.com/JCCDex/jcc_exchange/blob/master/src/util/config.ts#L3)
   tx|Object|true|-|signature object
   &emsp;Account|String|true|-|transaction initiator wallet address
   &emsp;Sequence|Number|true|-|transaction sequence
   &emsp;Flags|Number|true|-|transaction mark
   &emsp;Fee|Number|true|-|transaction gas,unit is `SWTC`,minimum 0.00001
   &emsp;TransactionType|String|true|-|[Transaction Type](#Transaction-Type)

   1. when TransactionType=`OfferCreate`，`tx` includes

         Parameter|Type|Required|Default|Description
         :--|:--:|:--:|:--:|:--
         TakerGets|[Amount](#Amount-Object)|true|-|token and amount of paying
         TakerPays|[Amount](#Amount-Object)|true|-|token and amount of getting
         Platform|String|false|jDXCeSHSpZ9LiX6ihckWaYDeDt5hFrdTto|transaction platform wallet address

   2. when TransactionType=`OfferCancel`，`tx` includes

         Parameter|Type|Required|Default|Description
         :--|:--:|:--:|:--:|:--
         OfferSequence|Number|true|-|sequence of canceled transaction

   3. when TransactionType=`Payment`，`tx` includes

         Parameter|Type|Required|Default|Description
         :--|:--:|:--:|:--:|:--
         Amount|[Amount](#Amount-Object)|true|-|token and amount of transfer
         Destination|String|true|-|transfer counterparty address
         Memos|Array|true|-|memos

   4. when TransactionType=`Brokerage`，`tx` includes

         Parameter|Type|Required|Default|Description
         :--|:--:|:--:|:--:|:--
         Amount|[Amount](#Amount-Object)|true|-|token and amount of fees
         FeeAccountID|String|true|-|fee wallet address
         OfferFeeRateDen|Number|true|-|fee rate denominator
         OfferFeeRateNum|Number|true|-|fee rate numerator

* response

   Key|Type|Description
   :--|:--:|:--
   -|String|signed transaction data

### 2. Commission order

#### 2.1 Instance init

```javascript
const hosts = ["localhost"];
const port = 80;
const https = false;
const urls = ["http://localhost:8080"];
const retry = 3;
JCCExchange.init(hosts, port, https, retry);
// JCCExchange.init(urls, retry);
```
**cancel orders, transfer, and set fees all need to be initialized before continuing.no repeat at below**

* parameters description
  
   Parameter|Type|Required|Default|Description
   :--|:--:|:--:|:--:|:--
   hosts|Array|true|-|domain name of transaction server
   port|Number or String|true|-|port of transaction server
   https|Boolean|true|-|true is https,false is http
   urls|Array|true|-|full url of transaction server(protocol + domain name + port)
   retry|Number|false|3|transaction sequence expired,number of retries

#### 2.2 Create order

* usage example

```javascript
const address = "jxxxx";
const secret = "sxxxxx";
const amount = "1";
const base = "jjcc";
const counter = "swt";
const sum = "1";
const type = "buy";
const platform = ""; 
const issuer;
JCCExchange.init(hosts, port, https, retry);
// JCCExchange.init(urls, retry);
const hash = await JCCExchange.createOrder(address, secret, amount, base, counter, sum, type, platform, issuer);
```

* parameters description

   Parameter|Type|Required|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|true|-|transaction initiator wallet address
   secret|String|true|-|transaction initiator wallet secret
   amount|String|true|-|transaction amount
   base|String|true|-|transaction token(not case sensitive)
   counter|String|true|-|differentiate according to the type of transaction (if it's buy order, counter is the token paid; if it's sell order, counter is the token got, not case sensitive)
   sum|String|true|-|transaction total price
   type|String|true|-|transaction type(buy：buy,sell:sell)
   platform|String|false|jDXCeSHSpZ9LiX6ihckWaYDeDt5hFrdTto|transaction platform wallet address
   issuer|String|false|jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or|issuer address

* response

   Key|Type|Description
   :--|:--:|:--
   hash|String|transaction hash

### 3. Cancel order

* usage example

```javascript
const address = "jxxx";
const secret = "sxxx";
const orderSequence = 0;
JCCExchange.init(hosts, port, https, retry);
// JCCExchange.init(urls, retry);
const hash = await JCCExchange.cancelOrder(address, secret, orderSequence);
```

* parameters description

   Parameter|Type|Required|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|true|-|cancellation wallet address
   secret|String|true|-|Cancellation wallet secret
   orderSequence|Number|true|-|sequence of canceled transaction

* response

   Key|Type|Description
   :--|:--:|:--
   hash|String|transaction hash

### 4. Transfer

* usage example

```javascript
const address = "jxxx";
const secret = "sxxx";
const amount = "1";
const memo = "test";
const to = "jxxx";
const token = "jjcc";
const issuer;
JCCExchange.init(hosts, port, https, retry);
// JCCExchange.init(urls, retry);
const hash = await JCCExchange.transfer(address, secret, amount, memo, to, token, issuer);
```

* parameters description

   Parameter|Type|Required|Default|Description
   --|:--:|:--:|:--:|:--
   address|String|true|-|transferor's wallet address
   secret|String|true|-|transferor's wallet secret
   amount|String|true|-|transfer amount
   meno|String|false|-|memos
   to|String|true|-|transfer counterparty address
   token|String|true|-|transfer token(not case sensitive)
   issuer|String|false|jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or|issuer address

* response

   Key|Type|Description
   :--|:--:|:--
   hash|String|transaction hash

### 5. Set brokerage

* usage example

```javascript
const platformAccount = "jxxx";
const platformSecret = "sxxx";
const feeAccount = "jxxx";
const rateNum = "2";
const rateDen = "1000";
const token = "jjcc";
const issuer;
JCCExchange.init(hosts, port, https, retry);
// JCCExchange.init(urls, retry);
const hash = await JCCExchange.setBrokerage(platformAccount, platformSecret,feeAccount, rateNum, rateDen, token, issuer);
```

* parameters description

   Parameter|Type|Required|Default|Description
   --|:--:|:--:|:--:|:--
   platformAccount|String|true|-|platform wallet address
   platformSecret|String|true|-|platform wallet secret
   feeAccount|String|true|-|fee wallet address
   rateNum|Number|true|-|rate numerator (0 and positive integer)
   rateDen|Number|true|-|rate denominator (positive integer)
   token|String|true|-|fee token(not case sensitive)
   issuer|String|false|jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or|币种发行方地址

* response

   Key|Type|Description
   :--|:--:|:--
   hash|String|transaction hash

### Amount Object

   Key|Type|Description
   :--:|:--:|:--
   currency|String|token name
   issuer|String|issuer address
   value|String|amount

### Transaction Type

Type|Description|Optinal
|:--:|:--|:--
String|transaction type|OfferCreate/OfferCancel/Payment/Brokerage

