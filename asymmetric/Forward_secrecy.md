# Forward secrecy

目前我对这个概念，并不是特别的清楚，不知道它是如何实现的。

In cryptography, **forward secrecy** (**FS**), also known as **perfect forward secrecy** (**PFS**), is a feature of specific **key agreement protocols** that gives assurances that **session keys** will not be compromised even if the **private key** of the server is compromised.<sub>笔记：这里的引出的两个概念是session key和private key。</sub>

**Forward secrecy** protects **past sessions** against future compromises of secret keys or passwords. By generating **a unique session key** for every session a user initiates, the compromise of a single session key will not affect any data other than that exchanged in the specific session protected by that particular key.<sub>笔记：保护过去的session。</sub>

**Forward secrecy** further protects data on the **transport layer**<sub>笔记：是发生在网络中的传输层</sub> of a network that uses common SSL/TLS protocols, including OpenSSL, which had previously been affected by the Heartbleed security bug<sub>笔记：这里提到Heartbleed，可能是说private key会被泄漏出去。</sub>. If forward secrecy is used, encrypted communications and sessions recorded in the past cannot be retrieved and decrypted should long-term secret keys or passwords be compromised in the future, even if the adversary actively interfered, for example via a man-in-the-middle attack.

## Example

The following is a hypothetical example of a simple instant messaging protocol that employs forward secrecy:

- (1) Alice and Bob each generate a pair of long-term, **asymmetric public and private keys**, then verify public-key fingerprints in person or over an already-authenticated channel. Verification establishes with confidence that the claimed owner of a public key is the actual owner.
- (2) Alice and Bob use a **key exchange algorithm** such as Diffie–Hellman, to securely agree on an **ephemeral session key**. They use the keys from step 1 only to authenticate one another during this process.
- (3) Alice sends Bob a message, encrypting it with **a symmetric cipher** using the **session key** negotiated in step 2.
- (4) Bob decrypts Alice's message using the key negotiated in step 2.
- (5) The process repeats for each new message sent, starting from step 2 (and switching Alice and Bob's roles as sender/receiver as appropriate). Step 1 is never repeated.

**Forward secrecy** (achieved by generating **new session keys for each message**) ensures that **past communications cannot be decrypted** if **one of the keys** generated in an iteration of step 2 is compromised, since such a key is only used to encrypt a single message.

**Forward secrecy** also ensures that **past communications cannot be decrypted** if the **long-term private keys** from step 1 are compromised. However, masquerading<sub>伪装</sub> as Alice or Bob would be possible going forward if this occurred, possibly compromising all future messages.

## Attacks

**Forward secrecy** is designed to prevent the compromise of a **long-term secret key** from affecting the confidentiality of past conversations.

However, forward secrecy cannot defend against a successful cryptanalysis of the underlying ciphers being used, since a cryptanalysis consists of **finding a way to decrypt an encrypted message without the key**, and **forward secrecy only protects keys, not the ciphers themselves**.

A patient attacker can capture a conversation whose confidentiality is protected through the use of public-key cryptography and wait until the underlying cipher is broken (e.g. large quantum computers could be created which allow the discrete logarithm problem to be computed quickly). This would allow the recovery of old plaintexts even in a system employing forward secrecy.

## Reference

- [Forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy)
