# Secure Hash Algorithm (SHA-1)

Secure Hash Algorithm (SHA-1) is similar to MD5. The only principal difference is in **the block operation itself**. The other two superficial differences are that SHA-1 produces **a 160-bit (20-byte) hash** rather than a 128-bit (16-byte) hash, and SHA-1 deals with **big-endian** rather than little-endian numbers. Like MD5, SHA-1 operates on **512-bit blocks**, and the final output is the sum (modulo 32) of the results of all of the blocks.

The operation itself is slightly simpler; you start by breaking the **512-bit input** into 16 4-byte values `x`. You then compute 80 four-byte `W` values from the original input where the following is rotated left once: `W[0<t<16] = x[t]`, and `W[17<7<80] = W[t-3] xor W[t-8] xor W[t-14] xor W[t-16]`.

This `W` array serves the same purpose as the `4294967296 * abs(sin(i))` computation in MD5, but is a bit easier to compute and is also based on the input.

After that, the hash is split up into five four-byte values `a`, `b`, `c`, `d`, and `e`, which are operated on in a series of 80 rounds, similar to the operation in MD5 — although in this case, somewhat easier to implement in a loop. At each stage, **a rotation**, **an addition** of another hash integer, **an addition** of an indexed constant, **an addition** of the `W` array, and **an addition** of a function whose operation depends on the round number is applied to the active hash value, and then the hash values are cycled so that a new one becomes the active one.

## Reference

- [SHA1示例](https://csrc.nist.gov/CSRC/media/Projects/Cryptographic-Standards-and-Guidelines/documents/examples/SHA1.pdf)，我保存在`pdf/SHA1.pdf`目录下。
