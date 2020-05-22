# Hash

The goal of any secure hashing algorithm, is to reduce **an arbitrarily sized** input into **an n-bit hash** in such a way that it is very unlikely that two messages, regardless of length or content, produce identical hashes — that is, collide — and that it is impossible to specifically reverse engineer such a collision.

不同的Hash算法的对比

| Algorithm | Block Size(byte) | Output(byte) |
| --------- | ---------------- | ------------ |
| `MD5`     | 64               | 16           |
| `SHA1`    | 64               | 20           |
| `SHA256`  | 64               | 32           |


