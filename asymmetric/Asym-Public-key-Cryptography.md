# Public-key cryptography

URL: https://en.wikipedia.org/wiki/Public-key_cryptography

**Public-key cryptography**, or **asymmetric cryptography**, is any cryptographic system that uses **pairs of keys**: **public keys** which may be disseminated(传播；散播；扩散) widely, and **private keys** which are known only to the owner. 

> 这段理解出2个意思：  
> （1）Public-key cryptography 与 asymmetric cryptography 是同一个意思  
> （2）Public-key cryptography是一个广义的概念，任何使用pairs of keys的加密算法，都可以称之为Public-key cryptography。

This accomplishes **two functions**: **authentication**, where the public key verifies that a holder of the paired private key sent the message, and **encryption**, where only the paired private key holder can decrypt the message encrypted with the public key.

> Public-key cryptography完成两个主要功能：  
> （1）authentication： 使用public key来验证消息确实来自于private key的持有者  
> （2）encryption： 经过public key加密的消息，只能由private key的持有者进行解密  
> （3）我有一个问题： 它为什么不提“由private key加密，由public key解密”的问题呢？

In a **public key encryption system**, any person can encrypt a message using the receiver's **public key**. That encrypted message can only be decrypted with the receiver's **private key**. 

> 问题：  
> 还是会提“由public key加密，由private key解密“，为什么不提”由private key加密，由public key解密“呢？

## 安全性：这种加密到底有多安全呢？

To be practical, **the generation of a public and private key-pair** must be computationally economical(经济的；实惠的；节俭的；节约的). **The strength of a public key cryptography system** relies on **the computational effort** (work factor in cryptography) required to find **the private key** from its paired **public key**. 

> 这里要理解两个意思：  
> （1）生成。生成public key和private key的计算要简单（computationally economical）。  
> （2）破解。由public key来计算private key的难度，则代表了public key cryptography system的强度。  

Effective security only requires keeping the private key private; the public key can be openly distributed without compromising security.

> 私钥要保密，公钥可分发。



## key的分发

Public key algorithms, unlike symmetric key algorithms, do not require a secure channel for the initial exchange of one or more secret keys between the parties.

## 实际应用

Because of the computational complexity of asymmetric encryption, it is usually used only for **small blocks of data**, typically the transfer of **a symmetric encryption key** (e.g. a session key). This symmetric key is then used to encrypt the rest of the potentially long message sequence. **The symmetric encryption/decryption** is based on simpler algorithms and is much faster.

## 基于的原理：数学算法

**Public key cryptography systems** often rely on cryptographic algorithms based on **mathematical problems** that currently admit **no efficient solution**, particularly those inherent in certain i**nteger factorization**, **discrete logarithm**, and **elliptic curve relationships**. 

> Public key cryptography systems 常常是基于 mathematical problems。




