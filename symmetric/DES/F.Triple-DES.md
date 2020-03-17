# Triple-DES

## Using the Triple-DES Encryption Algorithm to Increase Key Length

DES is secure. After forty years of cryptanalysis, no feasible attack has been demonstrated; if anybody has cracked it, they’ve kept it a secret. Unfortunately, the 56-bit key length is built into the algorithm. Increasing the key length requires redesigning the algorithm completely because the s-boxes and the permutations are specific to a 64-bit input. 56 bits is not very many, these days. 2<sup>56</sup> possible keys means that the most naive brute-force attack would need to try, on the average, 2<sup>55</sup> (2<sup>56</sup> / 2), or 36,028,797,018,963,968 (about 36,000 trillion operations) before it hit the right combination.

> 笔记：key所占用空间越多，加密的数据越安全。

This is not infeasible; my modern laptop can repeat the non-optimized decrypt routine shown roughly 7,500 times per second. This means it would take me about 5 trillion seconds, or about 150,000 years for me to search the entire keyspace. This is a long time, but the brute-forcing process is infinitely parallelizable. If I had two computers to dedicate to the task, I could have the first search keys from 0–2<sup>55</sup> and the second search keys from 2<sup>55</sup> –2<sup>56</sup>. They would crack the key in about 79,000 years. If, with significant optimizations, I could increase the decryption time to 75,000 operations per second (which is feasible), I’d only need about 7,500 years with two computers. With about 7,500 computers, I’d only need a little less than two years to search through half the keyspace.

> 这本书的内容是在2012年写的，在这几年，硬件的运算能力提升了不少，因此这里的运算次数和运算时间相关的数据可能是不对的。但是，它表明了一个趋势：计算能力越来越高，需要的时间越来越少。

In fact, optimized, parallelized hardware has been shown to be capable of cracking a DES key by brute force in just a few days. The hardware is not cheap, but if the value of the data is greater than the cost of the specialized hardware, alternative encryption should be considered.

> 笔记：当数据的价值，超过了硬件的价值，那就有必要进行破解。

**The proposed solution** to increase the keyspace beyond what can feasibly be brute-forced is called **triple DES** or **3DES**. **3DES** has a **168-bit** (56 * 3) key. It’s called **triple-DES** because it splits the key into **three 56-bit keys** and repeats the DES rounds described earlier three times, once with each key. The clearest and most secure way to generate the three keys that 3DES requires is to just generate 168 bits, split them cleanly into three 56-bit chunks, and use each independently. The 3DES specification actually allows the same 56-bit key to be used three times, or to use a 112-bit key, split it into two, and reuse one of the two keys for one of the three rounds. Presumably this is allowed for backward-compatibility reasons (for example, if you have an existing DES key that you would like to or need to reuse as is), but you can just assume **the simplest case** where you have **168 bits** to play with — this is what SSL expects when it uses 3DES as well.

> 笔记：可选的解决方案有triple DES，它使用168 bit的key。

One important wrinkle in the 3DES implementation is that you don’t encrypt three times with the three keys. Instead you **encrypt with one key**, **decrypt that with a different key** — remember that decrypting with a mismatched key produces garbage, but reversible garbage, which is exactly what you want when doing cryptographic work — and **finally encrypt that with yet a third key**. **Decryption**, of course, is the opposite — **decrypt with the third key**, **encrypt that with the second**, and **finally decrypt that with the first**. Notice that you reverse the order of the keys when decrypting; this is important! The **Encrypt/Decrypt/Encrypt** procedure makes cryptanalysis more difficult. Note that the “use the same key three times” option mentioned earlier is essentially useless. You encrypt with a key, decrypt with the same key, and re-encrypt again with the same key to produce the exact same results as encrypting one time.

> 笔记：加密顺序是“Encrypt/Decrypt/Encrypt”，而解密顺序是“Decrypt/Encrypt/Decrypt”

**Padding** and **cipher-block-chaining** do not change at all. 3DES works with eight-byte blocks, and you need to take care with **initialization vectors** to ensure that the same eight-byte block encrypted twice with the same key appears different in the output.

> 笔记：Padding和CBC并没有发生变化。






