# MD5

The goal of MD5, specified in RFC 1321, or any secure hashing algorithm, is to reduce **an arbitrarily sized** input into **an n-bit hash** in such a way that it is very unlikely that two messages, regardless of length or content, produce identical hashes — that is, collide — and that it is impossible to specifically reverse engineer such a collision. For MD5, `n = 128` bits. This means that there are 2<sup>128</sup> possible MD5 hashes. Although the input space is vastly larger than this, 2<sup>128</sup> makes it highly unlikely that two messages will share the same MD5 hash. More importantly, it should be impossible, assuming that MD5 hashes are evenly, randomly distributed, for an attacker to compute a useful message that collides with another by way of brute force.

MD5 operates on **512-bit** (64-byte) blocks of input.<sub>笔记：input是512 bit大小</sub> Each block is reduced to a **128-bit** (16-byte) hash.<sub>笔记：最终的hash值是128 bit。</sub> Obviously, with such a 4:1 ratio of input blocks to output blocks, there will be at least a one in four chance of a collision. The challenge that MD5’s designer faced is making it difficult or impossible to work backward to find one.

If the message to be hashed is greater than 512 bits, each 512-bit block is hashed independently and the hashes are added together, being allowed to overflow, and the result is the final sum. This obviously creates more potential for collisions.

Unlike cryptographic algorithms, though, message digests do not have to be reversible — in fact, this irreversibility is the whole point. Therefore, algorithm designers do not have to be nearly as cautious with the number and type of operations they apply to the input.<sub>笔记：message digest的不可逆性</sub> The more operations, in fact, the better; this is because operations make it more difficult for an attacker to work backward from a hash to a message.<sub>笔记：算法越复杂，越不利于破解</sub>

MD5 applies **64 transformations** to each input block.<sub>笔记：对于512 bit的input进行了64次操作</sub> It first splits the input into 16 32-bit chunks, and the current hash into four 32-bit chunks referred to tersely as A, B, C, and D in the specification. Most of the operations are done on A, B, C, and D, which are subsequently added to the input. The **64 operations** themselves consist of 16 repetitions of the four bit flipping functions `F`, `G`, `H` and `I`.<sub>笔记：这64次操作是由16次重复的调用F、G、H和I四个方法组成的。</sub>

注意：下面的方法，是我第一次写的，并没有经过验证，后续更新的时候，要记得更新为有效的代码

```java
public static int F(int x, int y, int z) {
    return (x & y) | (~x & z);
}

public static int G(int x, int y, int z) {
    return (x & y) | (y & ~z);
}

public static int H(int x, int y, int z) {
    return (x ^ y ^ z);
}

public static int I(int x, int y, int z) {
    return y ^ (x | ~z);
}
```

The purpose of these functions is simply to shuffle bits in an unpredictable way; don’t look for any deep meaning here.

Notice that this is implemented using **unsigned integers**.<sub>笔记：这里说的是unsigned integer，我在使用Java处理的时候要注意一些，因为在Java中的int是包含正负值的，但是它的空间（4 byte）是足够的。</sub> As it turns out, MD5, unlike any of the other cryptographic algorithms, operates on **little-endian numbers**, although MD5 has an odd concept of “**little endian**” in places.

The function `F` is invoked 16 times — once for each input block — and then `G` is invoked 16 times, and then `H`, and then `I`. So, what are the inputs to `F`, `G`, `H`, and `I`? They’re actually permutations of `A`, `B`, `C`, and `D`. The results of `F`, `G`, `H`, and `I` are added to `A`, `B`, `C`, and `D` along with **each of the input blocks**, as well as **a set of constants**, **shifted**, and **added** again. In all cases, **adds** are performed modulo 32 — that is, they’re allowed to silently overflow in a 32-bit register. After all 64 operations, the final values of `A`, `B`, `C`, and `D` are concatenated together to become the hash of a 512-bit input block.

More specifically, each of the 64 transformations on `A`, `B`, `C`, and `D` involve applying one of the four functions `F`, `G`, `H`, or `I` to some permutation of `A`, `B`, `C`, or `D`, adding it to the other, adding the value of input block (i % 4), adding the value of `4294967296 * abs(sin(i))`, rotating by a per-round amount, and adding the whole mess to yet one more of the `A`, `B`, `C`, or `D` hash blocks.


## Reference

- [RFC 1321: The MD5 Message-Digest Algorithm](https://tools.ietf.org/html/rfc1321)
- [Wiki: MD5](https://en.wikipedia.org/wiki/MD5)
- [The MD5 cryptographic hash function](https://www.iusmentis.com/technology/hashfunctions/md5/)

javascript

- [How does MD5 (Message Digest 5) work?](https://paginas.fe.up.pt/~ei10109/ca/md5.html)
