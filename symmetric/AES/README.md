# AES

3DES works and is secure — that is, brute-force attacks against it are computationally infeasible, and it has withstood decades of cryptanalysis. However, it’s clearly slower than it needs to be. To triple the key length, you also have to triple the operation time. If DES itself were redesigned from the ground up to accommodate a longer key, processing time could be drastically reduced.

In 2001, the NIST announced that **the Rijndael algorithm** would become the official replacement for DES and renamed it the **Advanced Encryption Standard**. NIST evaluated several competing block-cipher algorithms, looking not just at security but also at ease of implementation, relative efficiency, and existing market penetration.

If you understand the overall workings of DES, AES is easy to follow as well. Like DES, it does a non-linear s-box translation of its input, followed by several permutation- and shift-like operations over a series of rounds, applying a key-schedule to its input at each stage. Just like DES, AES relies heavily on the XOR operation — particularly the reversibility of it. However, it operates on much longer keys; AES isdefined for 128-, 192-, and 256-bit keys. Note that, assuming that a brute-force attack is the most effiient means of attacking a cipher, 128-bit keys are less secure than 3DES, and 192-bit keys are about the same (although 3DES does throw away 24 bits of key security due to the parity check built into DES). 256-bit keys are much more secure. Remember that every extra bit doubles the time that an attacker would have to spend brute-forcing a key.

## Reference

- [AES standard](https://csrc.nist.gov/csrc/media/publications/fips/197/final/documents/fips-197.pdf) 这是NIST的官方PDF文档。我希望自己熟悉AES算法后，能够读一读这份文档。这个文档，我读了一遍，感觉有些地方，还是很难懂。不过，我发现，这个文档最好的地方，就是提供了示例展示过程中结果。
