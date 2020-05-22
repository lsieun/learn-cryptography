# certificate

Fundamentally, the **certificate** is a holder for **a public key**. Although it contains a lot more information about the subject of the public key, the primary purpose of the certificate is to present the user agent with a public key that should then be used to encrypt a **symmetric key** that is subsequently used to protect the remainder of the connection’s traffic.

> 笔记： 最本质上来说，certificate是存储public key的容器。

At this point, you may have at least a hazy idea of how most of the concepts of the past three chapters can be put together to establish a secure communications link: First, a **symmetric algorithm** and **key** is chosen, and then the **key** is exchanged using **public-key techniques**<sub>笔记：交换key，要使用非对称加密算法</sub>. Finally, everything is **encrypted** using the **secret symmetric key**<sub>笔记：对data进行加密的时候，要使用对称加密</sub> and **authenticated** using an **HMAC** with another **secret key**<sub>笔记：HMAC算法，是为了data传输是完整的，没有被人“剪掉一部分”</sub>.

> 笔记：对于HMAC保证完整性，我当时整理了一个很好的例子，如果忘记的时候，可以去看看。

However, the **digital signatures** haven’t come into play yet. How are these used and why are they important? **Digital signatures** are how **certificates** are authenticated and how you can determine whether or not to trust a certificate. This is examined in much greater detail later in this chapter.

> 笔记： digital signatures的作用就是验证certifacate的有效性，换句话说，就是验证certificate当中的public key是有效的。

```text
第一步： public key - digital signature(验证certificate的技术) --- certificate(是public key的容器) --- private key
第二步： key exchange
第三步： data(传输的数据) + HMAC(先保证其完整性) + sysmetric cryptography(再保证其安全性)
```


