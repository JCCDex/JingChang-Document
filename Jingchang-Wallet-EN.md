<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# Jingchang Wallet Server

## Install and Import

```javascript
npm install jcc_wallet
import { jtWallet } from 'jcc_wallet'
// const jtWallet = require('jcc_wallet').jtWallet
```

## Wallet Server interface detail description

### 1. Create Jingtum wallet or alliance chain wallet

* usage example
 
```javascript
jtWallet.createWallet(chain)
```

* parameters description

   Parameter|Type|Required|Optional |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   chain|String|false|swt/bwt|swt|Jingtum chain or alliance chain name

* response

   Key|Type|Description
   :--|:--:|:--
   -|Object|Jingtum or alliance chain wallet object
    &emsp;secret|String|Jingtum or alliance chain wallet secret
    &emsp;address|String|Jingtum or alliance chain wallet address

### 2. Valid Jingtum wallet or alliance chain wallet address

* usage example
 
```javascript
jtWallet.isValidAddress(address,chain)
```

* parameters description

   Parameter|Type|Required|Optional |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   address|String|true|-|-|Jingtum or alliance chain wallet address
   chain|String|false|swt/bwt|swt|Jingtum chain or alliance chain name

* response

   Key|Type|Description
   :--|:--:|:--
   -|Boolean|validation result, legal is true, illegal is false

### 3. Valid Jingtum wallet or alliance chain wallet secret

* usage example
 
```javascript
jtWallet.isValidSecret(secret,chain)
```

* parameters description

   Parameter|Type|Required|Optional |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   secret|String|true|-|-|Jingtum or alliance chain wallet secret
   chain|String|false|swt/bwt|swt|Jingtum chain or alliance chain name

* response

   Key|Type|Description
   :--|:--:|:--
   -|Boolean|validation result, legal is true, illegal is false

### 4. Get address via wallet secret

* usage example
 
```javascript
jtWallet.getAddress(secret,chain)
```

* parameters description

   Parameter|Type|Required|Optional |Default|Description
   --|:--:|:--:|:--:|:--:|:--
   secret|String|true|-|-|Jingtum or alliance chain wallet secret
   chain|String|false|swt/bwt|swt|Jingtum chain or alliance chain name

* response

   Key|Type|Description
   :--|:--:|:--
   -|String|address of Jingtum wallet or alliance chain wallet, or null if the secret is invalid
