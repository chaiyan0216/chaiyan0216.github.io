---
title: Spring Boot 配置 Https
tags: https
typora-root-url: ../../..
---

开发 Spring Boot 应用时，如果没做特别的配置，默认会启动在 Http 端口上，这里记下如何配置启动在 Https 端口上。



#### 1. 自签证书及 Spring Boot Https 配置

1. 在 [Spring Initializr](https://start.spring.io/) 上生成新项目，依赖只选 Spring Web 就够了。

2. 导入项目到 Idea 中，并添加一个 Controller 用于直观的访问验证。

   ```java
   package com.cy.tlsDemo;
   
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   public class HelloController {
       @GetMapping("/")
       public String index() {
           return "Greetings from Spring Boot!";
       }
   }
   ```

3. 启动应用，访问：http://localhost:8080/ 确保可以返回正确信息。（注意这里是 **http**）

4. 生成自签证书，如果已经有了认证过的数字证书可以跳到最后生成 p12 文件那一小步。

   - 生成私钥：`openssl genrsa -out private.key 2048`，当前目录下会生成一个名为 private.key 的文件
   - 生成请求：`openssl req -new -key private.key -out request.csr`，当前目录下会生成一个名为 request.csr 的文件，这个文件一般是用来发给 CA 机构进行认证并生成数字证书用的，当然也可以用来生成自签证书
   - 自签名：`openssl x509 -req -days 365 -in request.csr -signkey private.key -out root.crt`，当前目录下会生成一个名为 root.crt 的数字证书
   - 生成 p12：`openssl pkcs12 -export -out cert.p12 -inkey private.key -in root.crt`，这里会要求输入密码，如果不想手动输入也可以用 `-password pass:xxx` 打在命令中

5. 生成自签证书，另一种单命令方式，步骤 4、5 任选一步执行就可以。

   - 生成私钥和自签证书：`openssl req -x509 -newkey rsa:4096 -keyout private.key -out root.crt -days 365 -nodes -subj '/CN=localhost'`，-nodes 表示不用密码保护私钥，-subj '/CN=localhost' 跳过生成证书的一些问题
   - 生成 p12：同上

6. 添加 Spring Boot 配置。

   ```properties
   # p12 文件的路径
   server.ssl.key-store=cert.p12
   # pass 是之前生成 p12 时输入的密码
   server.ssl.key-store-password=pass
   server.ssl.key-store-type=PKCS12
   ```

7. 启动应用，再次访问：http://localhost:8080/ ，发现已经无法正常访问，并提示：This combination of host and port requires TLS，说明配置已经生效。改为访问：https://localhost:8080/ （注意这里是 **https**）可以返回正常信息。



Spring Boot Https 的配置相当简单，让应用启在 Https 的核心还是在数字证书的生成上。



#### 2. 数字证书

数字证书是在非对称加密的基础上发展而来，在回顾数字证书前，再澄清公私钥的两个概念：

- 加密：**公钥加密，私钥解密**的行为称为加密
- 签名：**私钥加密，公钥解密**的行为称为签名

数字证书简而言之就是**经证书认证机构（CA）认证过的公钥**。由证书认证机构（CA）对证书申请者真实身份验证之后，**用 CA 的根证书对申请人的一些基本信息以及申请人的公钥进行签名**（相当于加盖发证书机构的公章）后形成的一个数字文件。CA 完成签发证书后，会将证书发布在 CA 的证书库（目录服务器）中，任何人都可以查询和下载。

数字征书在 Https 中如何发挥作用可以参考[这篇文章（https协议说明和自签证书使用）](https://blog.csdn.net/qq_30062125/article/details/84589988)的第一部分，图片和解释都很清楚。

![image](/images/https.jpg)

> 1、客户端请求 Https 地址，隐藏了 TCP 三次握手逻辑
>
> 2、3、服务端下发 CA 证书（公钥）
>
> 4、客户端通过 CA 认证机构的公钥对服务器传过来的证书验签
>
> - 最初在 CA 机构对证书进行签名时，其会根据证书里的签名算法字段来决定算法。如算法字段为 SHA256RSA，则 CA 机构就会使用 SHA256 对证书进行摘要，再使用 RSA 算法对摘要使用私钥签名。
> - 对于传来的证书，如果是经 CA 机构认证的证书（非自签），则颁发这个证书的 CA 机构的公钥一般已经预置在操作系统中，浏览器会使用该公钥进行验签（解密）。验签之后得到 CA 机构使用 SHA256 算法生成的证书摘要，然后客户端再使用 SHA256 对证书内容进行一次摘要，如果得到的值和验签之后得到的摘要值相同，则表示证书没有被修改过。
> - 如果验证通过，则显示安全图标；如果不通过，则显示警告和不安全图标。
>
> 5、客户端验签成功之后，生成随机的对称秘钥，使用服务器公钥加密对称秘钥发送给服务端
>
> 6、7、服务器通过自己的私钥解密密文，得到对称秘钥，加密相关信息传递给客户端
>
> 8、客户端拿到服务器传递的信息，解析验证 MAC (Message Authentication Code) 等信息，最终建立加密连接

最后是一篇说明各种证书关系及操作的文章：[各种安全证书间的关系及相关操作](https://www.jianshu.com/p/96df7de54375)

> **以文件形式存在的证书一般有这几种格式**：
>
> - 带有私钥的证书（P12）：包含了**公钥**和**私钥**的**二进制格式**的证书形式
> - 二进制编码的证书（DER）：证书中没有私钥，DER 编码二进制格式的证书文件
> - Base64 编码的证书（PEM）：证书中没有私钥，Base64 编码格式的证书文件

> **PEM 格式**
>
> PEM（Privacy Enhanced Mail），是 OpenSSL 默认采用的信息存放方式，是 CA 颁发证书最常用的格式。包含 `-----BEGIN CERTIFICATE-----` 和 `-----END CERTIFICATE-----` 声明。具备诸如 `.pem, .crt, .cer, .key` 的扩展名。
>
> 证书：
>
> ```
> -----BEGIN CERTIFICATE-----
> Base64EncodedContent
> -----END CERTIFICATE-----
> ```
>
> 私钥：
>
> ```
> -----BEGIN RSA PRIVATE KEY-----
> Base64EncodedContent
> -----END RSA PRIVATE KEY-----
> ```
>
> 请求：
>
> ```
> -----BEGIN CERTIFICATE REQUEST-----
> Base64EncodedContent
> -----END CERTIFICATE REQUEST-----
> ```

> **PFX/PKCS#12**
>
> PKCS12（个人数字证书标准）用于存放用户证书、crl、用户私钥以及证书链。PKCS12 中的私钥是加密存放的。扩展名为 `.pfx, .p12` 的二进制文件。

