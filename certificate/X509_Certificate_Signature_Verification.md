# Signature Verification

```text
Certificate ::= SEQUENCE {
    tbsCertificate TBSCertificate,
    signatureAlgorithm AlgorithmIdentifier,
    signatureValue BIT STRING
}
```

Remember that you also have to be able to **verify this signature**; just ensuring that it’s there isn’t enough. You must also check that it is a proper digital signature of the hash of the `tbsCertificate` bytes. So, after **parsing the entire certificate**, you must **hash it** and store the hash for later inspection.

## Validating PKCS #7-Formatted RSA Signatures

Validating a certificate involves finding **the public key of the issuer**, using it to run the **digital signature algorithm** on the **computed hash**, and then verifying that it matches the signature included in the certificate itself.

When **the RSA algorithm** is used for signing a certificate, **the hash value** itself is concatenated onto **the OID representing the signing algorithm** and stored in an ASN.1 sequence. This is then DER encoded, and the whole thing is encrypted with **the private key**. This is called **PKCS #7**.



