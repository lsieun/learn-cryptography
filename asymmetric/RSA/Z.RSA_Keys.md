# Keys

<!-- TOC -->

- [1. PROCEDURE FOR GENERATING RSA KEYPAIRS](#1-procedure-for-generating-rsa-keypairs)
- [2. Example](#2-example)
  - [2.1. mini: 430 bit](#21-mini-430-bit)
  - [2.2. middle: 1024 bit](#22-middle-1024-bit)
- [3. Common Public Key](#3-common-public-key)
- [Performance](#performance)

<!-- /TOC -->

## 1. PROCEDURE FOR GENERATING RSA KEYPAIRS

The procedure to generate RSA keypairs is as follows:

1. Select two random prime numbers `p` and `q`.
2. Compute the modulus `n = pq`.
3. Compute the totient function `(p-1)(q-1)`
4. Select a random public exponent `e < Ƶ(n)` (as previously mentioned, `65,537` is a popular choice).
5. Perform a modular inversion to compute the private exponent `d`: `de % n = 1`.

## 2. Example

### 2.1. mini: 430 bit

- public key `e`: 79
- private key `d`: 1019, 2629
- modulus `n`: 3337

示例：

- `m`: 688
- `c`: 1570

计算：

- 688<sup>79</sup> % 3337 = 1570
- 1570<sup>2281</sup> % 3337 = 688

### 2.2. middle: 1024 bit

- public key `e`: 65537
- private key `d`: 89489425009274444368228545921773093919669586065884257445497854456487674839629818390934941973262879616797970608917283679875499331574161113854088813275488110588247193077582527278437906504015680623423550067240042466665654232383502922215493623289472138866445818789127946123407807725702626644091036502372545139713
- modulus `n`: 145906768007583323230186939349070635292401872375357164399581871019873438799005358938369571402670149802121818086292467422828157022922076746906543401224889672472407926969987100581290103199317858753663710862357656510507883714297115637342788911463535102712032765166518411726859837988672111837205085526346618740053

## 3. Common Public Key

A more common `e` value is `65537`.

Note that everybody can, and generally does, use the same `e` value as long as the `n` — and by extension, the `d` — are different.

A **32-bit integer** raised to the power of `65,537` takes up an unrealistic amount of memory.

You can reduce 65,537 operations down to log<sub>2</sub> 65,537 = 17 operations.

Cycle through the bits in the exponent, starting with the least-significant bit. If the bit position is `1`, perform a multiplication. At each stage, square the running exponent, and that’s what you multiply by at the 1 bits. Incidentally, if you look at the binary representation of `65,537 = 10000000000000001`, you can see why it’s so appealing for public-key operations; it’s big enough to be useful, but with just two 1 bits, it’s also quick to operate on. You square `m` 17 times, but only multiply the first and 17th results.

Why `65,537`? Actually, it’s the smallest prime number (which `e` must be) that can feasibly be used as a secure RSA public-key exponent. There are only four other prime numbers smaller than `65,537` that can be represented in just two `1` bits: `3`, `5`, `17`, and `257`, all of which are far too small for the RSA algorithm. `65,537` is also the largest such number that can be represented in 32 bits. You could, if you were so inclined, take advantage of this and speed up computations by using native arithmetic operations.

## Performance

You also likely noticed that your computer slowed to a crawl while decrypting; encrypting isn’t too bad, because of the choice of public exponent (`65,537`). Decrypting is slow — not unusably slow, but also not something you want to try to do more than a few times a minute. It should come as no surprise to you that reams of research have been done into methods of speeding up the RSA decryption operation. None of them are examined here; they mostly involve keeping track of the interim steps in the original computation of the private key and taking advantage of some useful mathematical properties thereof.
