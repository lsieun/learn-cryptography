# ECDSA

Recall that DSA signature generation involved the computation of two numbers `r` and `s` from the parameters `g`, `p`, and `q`. ECDSA signatures are similar. In essence, the **modular multiplications** are replaced by **point multiplications**. The **generator** is a point on an elliptic curve; `r` is that point multiplied by a random integer `k`; and `s` is computed exactly the same way as in DSA. The only elliptic-curve function involved is in the computation of `r`.

## Reference

Source Code

- [Github: bipinkh/ecdsa](https://github.com/bipinkh/ecdsa): Java Implementation of "Elliptic Curve Digital Signature Algorithm" signature generation and verification. 
- [RFC 4754: IKE and IKEv2 Authentication Using ECDSA](https://tools.ietf.org/html/rfc4754)

Sample Data

- [ECDSA-256](https://tools.ietf.org/html/rfc4754#section-8)
