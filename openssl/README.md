# OpenSSL

<!-- TOC -->

- [1. Key and Certificate Management](#1-key-and-certificate-management)
- [2. Key Generation](#2-key-generation)
  - [2.1. Several Decisions](#21-several-decisions)
  - [2.2. RSA](#22-rsa)
  - [2.3. DSA](#23-dsa)
  - [2.4. ECDSA](#24-ecdsa)
- [3. Creating Certificate Signing Requests](#3-creating-certificate-signing-requests)
- [4. Signing Your Own Certificates](#4-signing-your-own-certificates)
- [5. Examining Certificates](#5-examining-certificates)

<!-- /TOC -->

Determine OpenSSL Version and Configuration

```bash
$ openssl version
OpenSSL 1.0.1 14 Mar 2012
```

## 1. Key and Certificate Management

Most users turn to OpenSSL because they wish to configure and run a web server that supports SSL. That process consists of three steps:

- (1) generate a strong **private key**,
- (2) create a **Certificate Signing Request** (**CSR**) and send it to a **CA**, and
- (3) install the **CA-provided certificate** in your web server.

## 2. Key Generation

### 2.1. Several Decisions

The first step in preparing for the use of **public encryption** is to generate **a private key**. Before you begin, you must make several decisions:

- **Key algorithm**: OpenSSL supports RSA, DSA, and ECDSA keys, but not all types are practical for use in all scenarios. For example, for web server keys everyone uses RSA, because DSA keys are effectively limited to 1,024 bits (Internet Explorer doesn’t support anything stronger) and ECDSA keys are yet to be widely supported by CAs. For SSH, DSA and RSA are widely used, whereas ECDSA might not be supported by all clients.
- **Key size**: The default key sizes might not be secure, which is why you should always explicitly configure key size. For example, the default for **RSA keys** is only **512 bits**, which is simply insecure. If you used a 512-bit key on your server today, an intruder could take your certificate and use brute force to recover your private key, after which he or she could impersonate(冒充；扮演) your web site. Today, **2,048-bit RSA keys** are considered secure, and that’s what you should use. Aim also to use **2,048 bits** for **DSA keys** and at least **256 bits** for **ECDSA**.
- **Passphrase**: Using a passphrase with a key is optional, but strongly recommended. Protected keys can be safely stored, transported, and backed up. On the other hand, such keys are inconvenient, because they can’t be used without their passphrases. For example, you might be asked to enter the passphrase every time you wish to restart your web server. For most, this is either too inconvenient or has unacceptable availability implications. In addition, using protected keys in production does not actually increase the security much, if at all. This is because, once activated, private keys are kept unprotected in program memory; an attacker who can get to the server can get the keys from there with just a little more effort. Thus, passphrases should be viewed only as a mechanism for protecting private keys when they are not installed on production systems. In other words, it’s all right to keep passphrases on production systems, next to the keys. If you need better security in production, you should invest in a hardware solution.

### 2.2. RSA

To generate an RSA key, use the `genrsa` command:

```bash
$ openssl genrsa -aes128 -out fd.key 2048
Generating RSA private key, 2048 bit long modulus
....+++
...................................................................................↩
+++
e is 65537 (0x10001)
Enter pass phrase for fd.key: ****************
Verifying - Enter pass phrase for fd.key: ****************
```

Here, I specified that the key be protected with **AES-128**. You can also use AES-192 or AES-256 (switches `-aes192` and `-aes256`, respectively), but it’s best to stay away from the other algorithms (DES, 3DES, and SEED).

The `e` value that you see in the output refers to the **public exponent**, which is set to `65,537` by default. This is what’s known as a **short public exponent**, and it significantly improves the performance of RSA verification. Using the `-3` switch, you can choose `3` as your public exponent and make verification even faster. However, there are some unpleasant historical weaknesses associated with the use of 3 as a public exponent, which is why generally everyone recommends that you stick with `65,537`. The latter choice provides a safety margin that’s been proven effective in the past.

Private keys are stored in the so-called **PEM format**, which is just text:

```bash
$ cat fd.key
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,01EC21976A463CE36E9DB59FF6AF689A
vERmFJzsLeAEDqWdXX4rNwogJp+y95uTnw+bOjWRw1+O1qgGqxQXPtH3LWDUz1Ym
mkpxmIwlSidVSUuUrrUzIL+V21EJ1W9iQ71SJoPOyzX7dYX5GCAwQm9Tsb40FhV/
[21 lines removed...]
4phGTprEnEwrffRnYrt7khQwrJhNsw6TTtthMhx/UCJdpQdaLW/TuylaJMWL1JRW
i321s5me5ej6Pr4fGccNOe7lZK+563d7v5znAx+Wo1C+F7YgF+g8LOQ8emC+6AVV
-----END RSA PRIVATE KEY-----
```

A private key isn’t just a blob of random data, even though that’s what it looks like at a glance. You can see a key’s structure using the following `rsa` command:

```bash
$ openssl rsa -text -in fd.key
Enter pass phrase for fd.key: ****************
Private-Key: (2048 bit)
modulus:
    00:9e:57:1c:c1:0f:45:47:22:58:1c:cf:2c:14:db:
    [...]
publicExponent: 65537 (0x10001)
privateExponent:
    1a:12:ee:41:3c:6a:84:14:3b:be:42:bf:57:8f:dc:
    [...]
prime1:
    00:c9:7e:82:e4:74:69:20:ab:80:15:99:7d:5e:49:
    [...]
prime2:
    00:c9:2c:30:95:3e:cc:a4:07:88:33:32:a5:b1:d7:
    [...]
exponent1:
    68:f4:5e:07:d3:df:42:a6:32:84:8d:bb:f0:d6:36:
    [...]
exponent2:
    5e:b8:00:b3:f4:9a:93:cc:bc:13:27:10:9e:f8:7e:
    [...]
coefficient:
    34:28:cf:72:e5:3f:52:b2:dd:44:56:84:ac:19:00:
    [...]
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
    [...]
-----END RSA PRIVATE KEY-----
```

If you need to have just the **public part** of a key separately, you can do that with the following `rsa` command:

```bash
$ openssl rsa -in fd.key -pubout -out fd-public.key
Enter pass phrase for fd.key: ****************
```

It’s good practice to verify that the output contains what you’re expecting. For example, if you forget to include the `-pubout` switch on the command line, the output will contain your **private key** instead of the **public key**.

```bash
$ openssl rsa -in fd.key -out fd-private.key
Enter pass phrase for fd.key: ****************
```

### 2.3. DSA

**DSA key** generation is a two-step process: **DSA parameters** are created in the first step and **the key** in the second. Rather than execute the steps one at a time, I tend to use the following two commands as one:

```bash
$ openssl dsaparam -genkey 2048 | openssl dsa -out dsa.key -aes128
Generating DSA parameters, 2048 bit long prime
This could take some time
[...]
read DSA key
writing DSA key
Enter PEM pass phrase: ****************
Verifying - Enter PEM pass phrase: ****************
```

This approach allows me to generate a password-protected key without leaving any temporary files (**DSA parameters**) and/or temporary keys on disk.

### 2.4. ECDSA

The process is similar for ECDSA keys, except that it isn’t possible to create keys of arbitrary sizes. Instead, for each key you select a **named curve**, which controls key size, but it controls other EC parameters as well. The following example creates a 256-bit ECDSA key using the secp256r1 named curve:

```bash
$ openssl ecparam -genkey -name secp256r1 | openssl ec -out ec.key -aes128
using curve name prime256v1 instead of secp256r1
read EC key
writing EC key
Enter PEM pass phrase: ****************
Verifying - Enter PEM pass phrase: ****************
```

OpenSSL supports many named curves (you can get a full list with the `-list_curves` switch), but, for web server keys, you’re limited to only two curves that are supported by all major browsers: `secp256r1` (OpenSSL uses the name `prime256v1`) and `secp384r1`.

## 3. Creating Certificate Signing Requests

Once you have a **private key**, you can proceed to create a **Certificate Signing Request** (**CSR**). This is a formal request asking a CA to sign a certificate, and it contains the **public key** of the entity requesting the certificate and **some information** about the entity. This data will all be part of the certificate. A **CSR** is always signed with the **private key** corresponding to the **public key** it carries.

**CSR creation** is usually **an interactive process**, during which you’ll be providing the elements of the certificate distinguished name. Read the instructions given by the openssl tool carefully; if you want **a field** to be **empty**, you **must enter a single dot (`.`) on the line, rather than just hit Return**. If you do the latter, OpenSSL will populate the corresponding CSR field with the default value. (This behavior doesn’t make any sense when used with the default OpenSSL configuration, which is what virtually everyone does. It does make sense once you
realize you can actually change the defaults, either by modifying the OpenSSL configuration or by providing your own configuration files.)

```bash
$ openssl req -new -key fd.key -out fd.csr
Enter pass phrase for fd.key: ****************
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:GB
State or Province Name (full name) [Some-State]:.
Locality Name (eg, city) []:London
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Feisty Duck Ltd
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:www.feistyduck.com
Email Address []:webmaster@feistyduck.com
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

After a CSR is generated, use it to sign your own certificate and/or send it to a public CA and ask him or her to sign the certificate. Both approaches are described in the following sections. But before you do that, it’s a good idea to double-check that the CSR is correct. Here’s how:

```bash
$ openssl req -text -in fd.csr -noout
Certificate Request:
Data:
Version: 0 (0x0)
Subject: C=GB, L=London, O=Feisty Duck Ltd, CN=www.feistyduck.com↩
    /emailAddress=webmaster@feistyduck.com
Subject Public Key Info:
Public Key Algorithm: rsaEncryption
Public-Key: (2048 bit)
Modulus:
    00:b7:fc:ca:1c:a6:c8:56:bb:a3:26:d1:df:e4:e3:
    [16 more lines...]
    d1:57
Exponent: 65537 (0x10001)
Attributes:
    a0:00
Signature Algorithm: sha1WithRSAEncryption
    a7:43:56:b2:cf:ed:c7:24:3e:36:0f:6b:88:e9:49:03:a6:91:
    [13 more lines...]
    47:8b:e3:28
```

## 4. Signing Your Own Certificates

If you’re installing a TLS server for your own use, you probably don’t want to go to a CA to get a publicly trusted certificate. It’s much easier to sign your own. The fastest way to do this is to generate a self-signed certificate. If you’re a Firefox user, on your first visit to the web site you can create a certificate exception, after which the site will be as secure as if it were protected with a publicly trusted certificate.

If you already have a CSR, create a certificate using the following command:

```bash
$ openssl x509 -req -days 365 -in fd.csr -signkey fd.key -out fd.crt
Signature ok
subject=/CN=www.feistyduck.com/emailAddress=webmaster@feistyduck.com/O=Feisty Duck ↩
Ltd/L=London/C=GB
Getting Private key
Enter pass phrase for fd.key: ****************
```

## 5. Examining Certificates

Certificates might look a lot like random data at first glance, but they contain a great deal of information; you just need to know how to unpack it. The `x509` command does just that, so use it to look at the self-signed certificates you generated.

In the following example, I use the `-text` switch to print certificate contents and `-noout` to reduce clutter by not printing the encoded certificate itself (which is the default behavior):

```bash
$ openssl x509 -text -in fd.crt -noout
Certificate:
    Data:
    Version: 1 (0x0)
    Serial Number: 13073330765974645413 (0xb56dcd10f11aaaa5)
    Signature Algorithm: sha1WithRSAEncryption
    Issuer: CN=www.feistyduck.com/emailAddress=webmaster@feistyduck.com, ↩
        O=Feisty Duck Ltd, L=London, C=GB
    Validity
        Not Before: Jun 4 17:57:34 2012 GMT
        Not After : Jun 4 17:57:34 2013 GMT
    Subject: CN=www.feistyduck.com/emailAddress=webmaster@feistyduck.com, ↩
        O=Feisty Duck Ltd, L=London, C=GB
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            Public-Key: (2048 bit)
            Modulus:
                00:b7:fc:ca:1c:a6:c8:56:bb:a3:26:d1:df:e4:e3:
                [16 more lines...]
                d1:57
            Exponent: 65537 (0x10001)
    Signature Algorithm: sha1WithRSAEncryption
        49:70:70:41:6a:03:0f:88:1a:14:69:24:03:6a:49:10:83:20:
        [13 more lines...]
        74:a1:11:86
```

Self-signed certificates usually contain only the most basic certificate data, as seen in the previous example. By comparison, certificates issued by public CAs are much more interesting, as they contain a number of additional fields (via the X.509 extension mechanism).
