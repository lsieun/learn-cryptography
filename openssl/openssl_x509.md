# openssl x509

<!-- TOC -->

- [1. Display](#1-display)
- [2. Format Convert](#2-format-convert)

<!-- /TOC -->

## 1. Display

To display the contents of a **PEM encoded certificate** using OpenSSL:

```bash
openssl x509 -noout -text -in example.pem
```

To display the contents of a **DER encoded certificate** using OpenSSL:

```bash
openssl x509 -noout -text -inform der -in example.der
```

## 2. Format Convert

To convert from **DER encoded certificate format** to **PEM encoded certificate format**:

```bash
openssl x509 -in example.der -inform der -outform pem -out example.pem
```

To convert from **PEM encoded certificate format** to **DER encoded certificate format**:

```bash
openssl x509 -in example.pem -outform der -out example.der
```
