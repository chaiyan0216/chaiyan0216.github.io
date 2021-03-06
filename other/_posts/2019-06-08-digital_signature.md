---
title: 数字签名
tags: rsa
typora-root-url: ../..
---

[编码、摘要与加密](https://zju-cy.github.io/2019/06/02/编码-摘要与加密.html)一文的最后提到了非对称加密算法。

非对称加密算法的正向应用是：RSA。

非对称加密算法的反向应用就是：数字签名。

**数字签名**：私钥持有者发出数据时，对**数据的摘要使用私钥加密**，这个加密后的信息就是数字签名。

发送者将数据+数字签名发出，接收者收到数据及签名后用公钥解密数字签名得到摘要信息1，并对数据进行摘要算法得到摘要信息2，然后对比两个摘要信息是否一致，就可以验证数据是否从发送者处发出。

![数字签名](/images/数字签名.png)

**风险情况**：当接收者本地的公钥被坏人替换为它自己的公钥后，坏人使用自己的私钥对数据生成数字签名，并假冒发送者给接收者发送数据，接收者使用了坏人公钥，所以依据能够验证成功，这是接收者使用的就是坏人提供的数据了。

**证书中心(certificate authority，简称CA)**：针对风险情况，证书中心出现了。证书中心用自己的私钥，对发送者的公钥和一些相关信息一起加密，生成**数字证书(Digital Certificate)**。有了数字证书，发送者在发送数据时就使用数据+数字证书，接收者收到后用证书中心CA的公钥去解开数字证书，拿到发送者的公钥，这时就不必担心公钥被替换了。



ps：两篇数字签名文章，一篇原文、一篇翻译，通透！

[What is a Digital Signature?](http://www.youdzone.com/signature.html)

[数字签名是什么？](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)