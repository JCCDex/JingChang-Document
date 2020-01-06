<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD046 -->
<!-- markdownlint-disable MD029 -->

# 井畅钱包服务

## 安装与引入

```javascript
npm install jcc_wallet
import { jtWallet } from 'jcc_wallet'
// const jtWallet = require('jcc_wallet').jtWallet
```

## 钱包接口详细说明

### 1. 创建井通钱包或联盟链钱包

* 使用示例
 
```javascript
jtWallet.createWallet(chain)
```

* 参数说明

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   chain|String|否|swt/bwt|swt|井通链或联盟链的名字

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   -|Object|井通钱包或bizain钱包对象
    &emsp;secret|String|井通钱包或联盟链钱包私钥
    &emsp;address|String|井通钱包或联盟链钱包地址

### 2. 验证井通钱包或联盟链钱包地址的合法性

* 使用示例
 
```javascript
jtWallet.isValidAddress(address,chain)
```

* 参数说明

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   address|String|是|-|-|井通钱包或联盟链钱包地址
   chain|String|否|swt/bwt|swt|井通链或联盟链的名字

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   -|Boolean|验证结果，合法为true,不合法为false

### 3. 验证井通钱包或联盟链钱包私钥的合法性

* 使用示例
 
```javascript
jtWallet.isValidSecret(secret,chain)
```

* 参数说明

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   secret|String|是|-|-|井通钱包或联盟链钱包私钥
   chain|String|否|swt/bwt|swt|井通链或联盟链的名字

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   -|Boolean|验证结果，合法为true,不合法为false

### 4. 通过钱包私钥获得地址

* 使用示例
 
```javascript
jtWallet.getAddress(secret,chain)
```

* 参数说明

   参数|类型|必填|可选值 |默认值|描述
   --|:--:|:--:|:--:|:--:|:--
   secret|String|是|-|-|井通钱包或联盟链钱包私钥
   chain|String|否|swt/bwt|swt|井通链或联盟链的名字

* 返回结果

   字段|类型|描述
   :--|:--:|:--
   -|String|井通钱包或联盟链钱包地址,若私钥不合法则返回null
