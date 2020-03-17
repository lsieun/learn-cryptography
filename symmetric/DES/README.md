# DES

The **Data Encryption Standard** (**DES**) algorithm, implemented and specifi ed by IBM at the behest<sub>命令；紧急指示</sub> of the NSA<sub>美国国家安全局(National Security Agency)</sub> in 1974, was the first publicly available computer-ready
encryption algorithm.

> 笔记：主要介绍DES出现的背景

Although for reasons you see later, DES is not considered particularly secure any more, it’s still in widespread use and serves as a good starting point for the study of symmetric cryptography algorithms in general. Most of the concepts that made DES work when it was first introduced appear in other cryptographic algorithms.

> 在现在这个世代（硬件运算能力提升），从安全性方面来说，DES的加密算法已经不再那么安全。但是，它是学习symmetric cryptography algorithms过程中一个很好的起点。在DES算法中，它引入的一些概念，在其他的算法中都有使用到。

DES breaks its **input** up into **eight-byte blocks** and scrambles them using **an eight-byte key**. This scrambling process involves a series of **fixed permutations** — swapping bit 34 with bit 28, bit 28 with bit 17, and so on — **rotations**, and **XOR**s. **The core of DES**, though, and where it gets its security, is from what the standard calls **S boxes** where **six bits** of **input** become **four bits** of **output** in a fixed, but non-reversible (except with the **key**) way.

> 笔记：这段我提取出了3个要点

- （1） DES每一次处理的数据量为8 byte，也就是64 bit
- （2） 在每一次处理当中，基本操作包括：permutation、rotation和XOR这三种
- （3） DES的安全性来自于S boxes，它能接收一个6 bit数据，然后输出一个4 bit数据。在以前不了解S boxes的时候，我觉得很神奇，但是了解之后，就觉得一般般的啦。

Like any modern **symmetric cryptographic algorithm**, **DES** relies heavily on the **XOR** operation.

> 笔记：DES很大程度上依赖XOR操作。同时，其他的symmetric cryptographic algorithm也很大程度上依赖XOR操作。
