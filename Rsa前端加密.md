# RSA前端加密

## 1.介绍

RSA公钥加密算法是1977年由Ron Rivest、Adi Shamirh和LenAdleman在（美国麻省理工学院）开发的。RSA取名来自开发他们三者的名字。RSA是目前最有影响力的公钥加密算法，它能够抵抗到目前为止已知的所有密码攻击，已被ISO推荐为公钥数据加密标准。目前该加密方式广泛用于网上银行、数字签名等场合。RSA算法基于一个十分简单的数论事实：将两个大素数相乘十分容易，但那时想要对其乘积进行因式分解却极其困难，因此可以将乘积公开作为加密密钥。

​    JS前端RSA加密需要用到第三方的加密组件JSENCRYPT：<https://github.com/travist/jsencrypt> 具体的大家可以到github上看下。

​    在开始之前，我们先要使用OpenSSL生成一对公钥、私钥，为了方便讲述，飘易还是直接提供一对生成好的公钥、私钥，至于如何生成，网上资料很多了。支付宝那里也有关于RSA密钥生成的文档：<https://doc.open.alipay.com/doc2/detail?treeId=58&articleId=103242&docType=1>，支付宝提供了一键生成工具

网上例子:https://www.cnblogs.com/Leo_wl/p/5763243.html

## 2.JS前端的加密方法

### 1.获取后台发来的公钥

el:

```javascript
var rsaKey= '-----BEGIN PUBLIC KEY-----\n' +
      'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzqT4fqWv/0EhYg9B8FWY\n' +
      'IhU06ySrXQmTgZE0KyVy4lMENzoYh7sMHSDFWeGEvCFYUd4ClwXt0jLNepFnNVVO\n' +
      'i/jLBtIH3tHqbdC6xg2adiuSUJEW2ziYIhMsHYXcIgaCPMwIYY4TzEuZGNxf/dS1\n' +
      '+BKuBD9LJNHRzQUvDrvpZlepgJ30zRHHTpmGfBlhW8KF6HU+ML1ZoQYrMAbkh8gT\n' +
      'K1MZMSEbBCnM/jL2LEnMxsxqmuPgBnJh7J/7i+w9lwG8wwDqOZYAot7+m4UYYKod\n' +
      'U5Avs6ED9dyDDp2UQCrpnmwCmSKlepaGqajQkq/LUzk6dEwL8ESIesuYNlKx+WlA\n' +
      'WwIDAQAB\n' +
      '-----END PUBLIC KEY-----\n';
```

### 2.用公钥来加密

```javascript
// 使用jsencrypt类库加密js方法，需要引入
let encrypt = new JSEncrypt();
encrypt.setPublicKey(rsaKey);
var data = new Object();//需要加密的内容
let c = JSON.stringify(data);//转为json格式
let encryptTxt = encrypt.encrypt(c);//加密后的内容
```

