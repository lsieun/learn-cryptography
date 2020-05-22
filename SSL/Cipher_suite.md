# Cipher suite

<!-- TOC -->

- [1. Intro](#1-intro)
  - [1.1. 从数量的角度来说](#11-%e4%bb%8e%e6%95%b0%e9%87%8f%e7%9a%84%e8%a7%92%e5%ba%a6%e6%9d%a5%e8%af%b4)
  - [1.2. 从版本的角度来说](#12-%e4%bb%8e%e7%89%88%e6%9c%ac%e7%9a%84%e8%a7%92%e5%ba%a6%e6%9d%a5%e8%af%b4)
- [2. Naming scheme](#2-naming-scheme)
- [3. Reference](#3-reference)

<!-- /TOC -->

## 1. Intro

A **cipher suite** is **a set of algorithms** that help secure a network connection that uses **Transport Layer Security** (**TLS**) or its now-deprecated predecessor **Secure Socket Layer** (**SSL**). The set of algorithms that cipher suites usually contain include: **a key exchange algorithm**, **a bulk encryption algorithm**, and **a message authentication code (MAC) algorithm**.

- The **key exchange algorithm** is used to exchange a key between two devices. This key is used to encrypt and decrypt the messages being sent between two machines.
- The **bulk encryption algorithm** is used to encrypt the data being sent.
- The **MAC algorithm** provides **data integrity checks** to ensure that the data sent does not change in transit.

In addition, **cipher suites** can include **signatures** and **an authentication algorithm** to help authenticate the server and or client.

### 1.1. 从数量的角度来说

Overall, there are **hundreds of different cipher suites** that contain different combinations of these algorithms. **Some cipher suites offer better security than others**.

### 1.2. 从版本的角度来说

The structure and use of the cipher suite concept are defined in the TLS standard document.

TLS 1.2 is the most prevalent version of TLS.

The next version of TLS (TLS 1.3) includes additional requirements to cipher suites. TLS 1.3 was only recently standardised and is not yet widely used.

Cipher suites defined for TLS 1.2 cannot be used in TLS 1.3, and vice versa, unless otherwise stated in their definition.<sub>笔记：互操作性不强</sub>

## 2. Naming scheme

Each **cipher suite** has **a unique name** that is used to identify it and to describe the algorithmic contents of it. Each segment in a cipher suite name stands for a different algorithm or protocol. An example of a cipher suite name: **TLS_RSA_WITH_3DES_EDE_CBC_SHA**

The meaning of this name is:

- **TLS** defines the protocol that this cipher suite is for; it will usually be TLS.
- **RSA** indicates **the key exchange algorithm** being used. The key exchange algorithm is used to determine if and how the client and server will authenticate during the handshake.
- **3DES_EDE_CBC** indicates **the block cipher** being used to encrypt the message stream, together with the block cipher mode of operation.
- **SHA** indicates the **message authentication algorithm** which is used to authenticate a message.

## 3. Reference

- [Cipher suite](https://en.wikipedia.org/wiki/Cipher_suite)
