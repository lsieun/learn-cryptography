# Learn Cryptography

:bug:

<!-- TOC -->

- [1. Intro](#1-intro)
- [2. Symmetric Encryption Algorithm](#2-symmetric-encryption-algorithm)
- [Different Level](#different-level)
- [3. Reference](#3-reference)

<!-- /TOC -->

## 1. Intro

When implementing a cipher, it's useful to have **a test vector** with **the cipher intermediate states** (if you get it right the first time, you don't need it; if you get a detail wrong, it makes finding that detail a lot easier).

## 2. Symmetric Encryption Algorithm

Julius Caesar is credited with perhaps the oldest known **symmetric cipher algorithm**. The so-called **Caesar cipher** — a variant of which you can probably find as a diversion in your local newspaper — assigns each letter, at random, to a number. This mapping of **letters** to **numbers** is the key in this simple algorithm.

> 思路：第一步，从Caesar cipher开始，引入symmetric cipher algorithm这个概念

Modern cipher algorithms must be much more sophisticated than Caesar’s in order to withstand automated attacks by computers. Although the basic premise remains — **substituting one letter or symbol for another**, and **keeping track of that substitution for later** — further elements of confusion and diffusion were added over the centuries to create modern cryptography algorithms. One such hardening technique is to **operate on several characters at a time**, rather than just one.

> 思路：第二步，随着时间的流逝，symmetric cipher algorithm也在不断的进行演变。

By far the most common category of **symmetric encryption algorithm** is the **block cipher algorithm**, which operates on a fixed range of bytes rather than on a single character at a time.

> 思路：第三步，在当代，symmetric cipher algorithm中最常使用的是block cipher algorithm。

## Different Level

```text
Algorithm --> Language --> implementation
```

使用哪一个算法（Algorithm）是一回事，而使用哪一种编程语言（Language）编写这个算法是另一回事。那么，即使说确定了使用Java语言，那么，由谁去实现又是另外一回事，它可能是由JDK自己提供的实现，也可能是由BouncyCastle提供的实现，也可能是由我们自己来编写代码实现。


## 3. Reference

- [nist: National Institute of Standards and Technology](https://csrc.nist.gov/) 可以查询DES和AES的文档
- [Cryptographic Standards and Guidelines](https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines/example-values) 这里是NIST网站提供的算法示例，包括Encryption-Block Ciphers(AES、TDES、Skipjack)、Block Cipher Modes(ECB、CBC、CFB、OFB、CTR)、Digital Signatures(DSA、RSA)、Secure Hashing(SHA1/SHA256)、Key Management(ECC)、Random Number Generation、Message Authentication。
- [Cryptography Tutorials - Herong's Tutorial Examples](http://www.herongyang.com/Cryptography/index.html) 这个网站包含的内容很多，我学习过程中的一个好助手
- [Technology](https://www.iusmentis.com/technology/) 这个网站对加密、数字签名、摘要做一个简要描述，对于整体上把握这些要素还是有帮助的

javascript

- [Crypto Academy](https://paginas.fe.up.pt/~ei10109/ca/index.html): 这是用javascript来实现各种算法
- [MD5/SHA-1 Hash Generator (with steps)](https://cse.unl.edu/~ssamal/crypto/genhash.php) 这个显示详细步骤
