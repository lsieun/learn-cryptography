# DER

<!-- TOC -->

- [1. Encoded Values](#1-encoded-values)
  - [1.1. integer(2)](#11-integer2)
  - [1.2. OID(6)](#12-oid6)
- [2. Date(23,24)](#2-date2324)
- [3. String(19,22)](#3-string1922)
- [4. Bit Strings(3)](#4-bit-strings3)
- [5. constructed type(48,49)](#5-constructed-type4849)
- [6. ASN.1 Explicit Tags](#6-asn1-explicit-tags)

<!-- /TOC -->

Quite a bit has been said so far about **the abstract structure of a certificate** without discussing **how one is actually represented in byte form**. The translation of **primitive (ASN.1) types** to **byte representation** is described according to a set of rules. Technically, these rules are independent of ASN.1 itself.

> 笔记：“certificate的结构” 和 “如何将certificate转换成byte的形式” 是两回事。

I mentioned earlier that a certificate is the sort of thing that would probably be represented in XML these days — there is, in fact, a set of rules to encode ASN.1 in XML format! However, by far the most common encoding, and the one that SSL relies on, is called the **Distinguished Encoding Rules** (**DER**).

> 笔记：DER是最常用的将certificate转换成byte的方式。

The **distinguished** differentiates the rules from another set called the **basic** encoding rules. Fundamentally, the distinguished rules are more restrictive than the basic rules. For example, the basic rules allow the encoder to use more bytes than necessary to specify lengths (if the encoder wants all lengths to be encoded in a fixed set of bytes, for example). For the most part, the differences are superficial, and the **basic encoding rules** (**BER**) won’t be specifically covered here.

> 笔记：这里讲述了distinguished这个单词

The **DER** describes how to format **integers**, **strings**, **dates**, **object identifiers**, **bit strings**, **sequences** and **sets** — as well as several others, but these are the ones that are pertinent to the present discussion about `X.509` certificates. See `X.690` for a complete listing of DER encoding rules.

## 1. Encoded Values

Every encoded value is represented as a `type`, followed by the **value’s length**, followed by **the actual contents of the value itself**; the representation of the value depends on the `type`.

> 笔记： 基本结构是 type + length + content

### 1.1. integer(2)

So, for example, the `type` **integer** is byte `02`. DER allows for multi-byte types as well — and has complex rules on how to encode and recognize them — but X.509 doesn’t need to make use of them and sticks with **single-byte types**. Therefore, the integer value `5` is encoded, according to DER, as

```text
02 01 05
```

That’s type `2` (integer), **one byte in length**, value 5. The integer value 65535 is encoded as

```text
02 02 FF FF
```

That’s type `2`, **two bytes**, value `0xFFFF` equals 65535. The `length` byte tells you when to stop reading the value and start looking for another tag.

### 1.2. OID(6)

So far, so good. It’s pretty simple. OID’s are just as simple to encode. They’re stored just like integers, but they have a type of `6` instead of `2`. Otherwise, they’re encoded the same way: `type`, `length`, `value`. The OID **common name** (in the **subject** and **issuer** distinguished name fields) of `55 04 03` is represented as

```text
06 03 55 04 03
```

The `length` byte tells you that there are **three bytes** of OID.

## 2. Date(23,24)

The `type` code for a **date** is either `23` or `24`; `23` is a generalized — **four-digit year** — time. `24` is a UTC — **two-digit year** — time. Although the `type` actually includes enough information to infer the length — you know that **generalized times** are **15 digits**, and **UTC times** are `13` — for consistency’s sake the lengths are included as well. After that, the `year`, `month`, `day`, `hour`, `minute`, `second` and `Z` are included in ASCII format.

## 3. String(19,22)

Strings are also coded this way. However, there are quite **a few different string types** to account for different byte encodings (among other things). The official specification is actually not proscriptive about which type of string should be used, and you actually see different kinds. However, the most common are **IA5Strings** (type `22`) and **printable strings** (type `19`), which you can treat interchangeably.

Given, for example, the country code “US” in a name field, the encoding would be

```text
13 02 55 53
```

which is the ASCII representation of the string “US.”

## 4. Bit Strings(3)

**Bit strings** are just like **strings**, with one minor difference. Their `type` is `3` to distinguish them from **printable strings**, but the encoding is exactly the same: `tag`, `length`, `contents`.

The only difference between **bit strings** and **character strings** is that bit strings don’t necessarily have to end on an eight-bit boundary, so they have **an extra byte** to indicate **how much padding was included**. In practice, this is always 0 because all useful bit patterns are eight-bit aligned anyway.

> 笔记：这里主要说明bit strings还有一个padding

All the examples of DER-encoded values examined so far have been able to get away with representing their **length** with **a single byte**, but a nested ASN.1 structure is bound to be larger than this. So how are lengths greater than 255 represented?

> 笔记：这里提出问题“如果content长度大于255 byte，那该怎么办呢？”

Actually, a single-length byte can only represent 127 byte values. The **highorder bit** is reserved. If it’s `1`, then **the low order seven bits** represent not the length of the value, but **the length of the length** — that is, how many of the bytes following encode the length of the subsequently following value.

Technically, a value doesn’t have to be a **bit string** to have a length greater than 127; integers, strings, and OIDs could, at least in theory. In practice, though, this never happens.

## 5. constructed type(48,49)

**Sets** and **sequences** are what ASN.1 calls a **constructed type** — that is, a type containing other types. Technically, they’re encoded the same way other values are. They start with a `tag`, are followed by **a variable number of length bytes**, and are then followed by **their contents**.

> 笔记一： Sets 和 sequences 两个都是constructed type  
> 笔记二： constructed type = tag + length + content

However, for **constructed types**, the contents themselves are further ASN.1-encoded tags. **Sequences** are identified by tag `0x30`, and **sets** are identifi ed by tag `0x31`. Any `tag` value whose **sixth bit** is `1` is a **constructed tag** and the parser must recognize that it contains additional ASN.1-encoded data.

> 笔记： 这里从bit角度说明了判断类型的“技术标准”

## 6. ASN.1 Explicit Tags

So far, **tags** have been presented as randomly distributed identifiers. Actually, **the first two bits** of a `tag` identify its **tag class**. In X.509 you come across **two types** of **tag classes**: **universal** (`00`) and **context-specific** (`10`). (The other two are **application** and **private** and are not used in X.509 certificates.) **Context-specific tags** are **explicit tags**. So, to create an explicit tag `0`, OR `0` with `1000 0000` (`0x80`). This is also **a constructed tag** — its contents are the actual version number — so the sixth bit is set to `1` (OR `0x20`=`0010 0000`).




