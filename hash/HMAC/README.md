# HMAC

![HMac Function](images/hmac_function.png)

```text
 1 function hmac (key, message)
 2     if (length(key) > blocksize) then
 3         // keys longer than blocksize are shortened
 4         key = hash(key)
 5     end if
 6     if (length(key) < blocksize) then
 7         // keys shorter than blocksize are zero-padded
 8         key = key ∥ zeroes(blocksize - length(key))
 9     end if
10
11     // Where blocksize is that of the underlying hash function
12     o_key_pad = [0x5c * blocksize] ⊕ key
13     i_key_pad = [0x36 * blocksize] ⊕ key // Where ⊕ is exclusive or (XOR)
14     // Where ∥ is concatenation
15     return hash(o_key_pad ∥ hash(i_key_pad ∥ message))
16 end function
```

使用场景

- 服务端生成key，传给客户端；
- 客户端使用key将帐号和密码做HMAC，生成一串散列值，传给服务端；
- 服务端使用key和数据库中用户和密码做HMAC计算散列值，比对来自客户端的散列值。

## Where Does All of This Fit into SSL?

You may still be wondering what all this has to do with SSL. After all, you have **secure key exchange** and **symmetric cryptography**. Where does the HMAC function fit in?

SSL requires that **every record** first be **HMAC’ed** before being **encrypted**. This may seem like overkill — after all, **HMAC** guarantees **the integrity of a record**. But because you’re using symmetric cryptography, the odds are infinitesimally small that an attacker could modify a record in such a way that it decrypts meaningfully, at least without access to the **session key**. Consider a secure application that transmits the message, “**Withdraw troops from Bunker Hill and move them to Normandy beach**.” If you run this through the **AES** algorithm with the key “**passwordsecurity**” and the initialization vector “**initializationvc**,” you get:

```text
0xc99a87a32c57b80de43c26f762556a76bfb3040f7fc38e112d3ffddf4a5cb703
989da2a11d253b6ec32e5c45411715006ffa68b20dbc38ba6fa03fce44fd581b
```

So far, so good. An attacker can’t modify the message and move the troops — say, **to Fort Knox** — without the key. If he tries to change even one bit of the message, it decrypts to gibberish and is presumably rejected.

He can, however, cut half of it off. The attacker could modify the encrypted
message to be

```txt
0xc99a87a32c57b80de43c26f762556a76bfb3040f7fc38e112d3ffddf4a5cb703
```

This message is received and decrypted correctly to “**Withdraw troops from Bunker Hill**.” The recipient has no way to detect the modification. For this reason, some hash function must be used to verify the integrity of the message after it’s been decrypted. SSL mandates that every record be protected this way with an HMAC function. You examine this in more detail when the details of the SSL protocol are discussed.

Also, SSL uses the **HMAC function** quite a bit as a pseudo-random number generator. Because the output is not predictably related to the input, the HMAC function is actually used to generate the keying material from a shared secret. In fact, the HMAC function is used to generate the final HMAC secret!


## Reference

- [RFC 2104:HMAC: Keyed-Hashing for Message Authentication](https://tools.ietf.org/html/rfc2104)
