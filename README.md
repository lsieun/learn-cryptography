# Learn Cryptography

:bug:

<!-- TOC -->

- [1. Intro](#1-intro)
- [2. Symmetric Encryption Algorithm](#2-symmetric-encryption-algorithm)
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

## 3. Reference

- [nist: National Institute of Standards and Technology](https://csrc.nist.gov/) 可以查询DES和AES的文档
- [Cryptography Tutorials - Herong's Tutorial Examples](http://www.herongyang.com/Cryptography/index.html) 这个网站包含的内容很多，我学习过程中的一个好助手
