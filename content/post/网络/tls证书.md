---
title: 'Tls证书'
tags: 
- 网络
categories: 
- 网络
date: "2020-03-13T01:19:56+08:00"
comment:  true  
---

###  基本概念

​	[参考HTTPS精读TLS证书校验](https://zhuanlan.zhihu.com/p/30655259)

​	https比http多了一层安全层（Security Layer）--TLS/SSl

SSL是最早的安全层协议，TLS是SSL发展而来的。

​	证书有四种格式

- Certificate (.cer)
- Privacy Enhanced Mail (.pem)
- Certificate Bundle (.p7b)
- Personal Information Exchange (.p12)

  非对称加密：RSA ，DSA,ECC

  摘要算法：CRC,MD5,SHA256

  数字签名：非对称加密+摘要算法

#### 通过一个.pem证书文件来举例子

```she
Certificate:
	Data:
		version:
		Serial Number:
		Signature Algorithm:sha256fafafsdfsd
		issuer://证书签发者
		...
	Signature Algorthm:
		...
	
```

其中Data包含的主要信息有：

- 证书版本号：X.509v3
- 序列号：一个 CA 机构内是唯一的，但不是全局唯一
- **签名（摘要）算法**：签名的计算公式为`RSA(sha256(Data), IssuerPrivateKey)`
- 签发者：DN（Distinguished Name）
- 有效期：证书的有效期间 [`Not Before`, `Not After`]
- 证书拥有者：也是一个 DN公钥长度一般是 2048bit，1024bit已经被证明不安全
- 扩展字段：证书所携带的域名信息会配置在 SAN 中（X509v3 Subject Alternative Name）
- 和**公钥**

### TLS握手过程

[参考SSL/TLS证书概述](https://segmentfault.com/a/1190000009002353)

1、Client  ------------------->  Server：ClientHello(客户端首先沟通，内容包括自己支持的TLS版本，非对称加密算法，对称加密算法，数据摘要算法，session id)

2、Server -------------------> Client: ServerHello(服务端选择配套的证书，并选择一个自己也支持的对称加密算法，告知客户端选用的tls版本，非对称加密算法，session id 等)

3、Server -------------------> Clietn: Certificate(发送证书（证书包括非对称加密算法和摘要算法）)

4、Server -------------------> Clietn: ServerHelloDone(告知客户端，我发送完了)

5、Client  ------------------->  Server：ClientKeyExchange(客户端验证完服务端发来的证书后，就会生成一个premaster(准密码)，这是用证书中的公钥加密的，要用私钥解密服务器才能看到)

6、finished  结束了

这样双方都有了premaster（准密码），这样之后就可以用premaster计算得到的对称密码，来进行对称密码通讯。

### 验证证书

1. 客户端收到了来自网站的证书
2. 打开证书，查看证书是那个ca签名
3. 在自己信任的ca库中，找到ca的证书
4. **用ca证书里的公钥，解密网站证书上的签名，取出网站证书的校验码A（这个校验码A是保存在网站证书的签名中的，需要用CA的公钥解出来才能获取到校验码A），然后用同样的方法（比如sha256）算出网站证书的校验码,两者比较，相同则是没有被篡改过**还是有问题得的:grey_question:
5. 读出CN，对比cn中的域名，和自己访问的域名是否相同。
6. 结束

### 证书生成（密钥.key->申请文件.csr->证书.crt）

1. 生成CA的私钥和证书
   1. 目的：生成一个私钥，并输出到文件ca.key.生成证书到ca.cert
2. 生成私钥和证书签名申请文件
   1. 生成一个证书签名申请文件，domain.key和domain.csr
3. 使用CA的私钥对申请文件进行签名
   1. 利用ca.crt和ca.key对domain.csr签名生成domain.crt
4. 查看证书内容
   1. 最后生成ca.crt和domain.crt

