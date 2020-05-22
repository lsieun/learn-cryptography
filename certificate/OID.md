# OID

| Name                   | Hex Notation                 | Dot Notation            |
| ---------------------- | ---------------------------- | ----------------------- |
| RSA                    | `2A 86 48 86 F7 0D 01 01 01` | `1.2.840.113549.1.1.1`  |
| MD5WithRSA             | `2A 86 48 86 F7 0D 01 01 04` | `1.2.840.113549.1.1.4`  |
| SHA1WithRSA            | `2A 86 48 86 F7 0D 01 01 05` | `1.2.840.113549.1.1.5`  |
| SHA256WithRSA          | `2A 86 48 86 F7 0D 01 01 0B` | `1.2.840.113549.1.1.11` |
| DSA                    | `2A 86 48 CE 38 04 01`       | `1.2.840.10040.4.1`     |
| SHA1WithDSA            | `2A 86 48 CE 38 04 03`       | `1.2.840.10040.4.3`     |
| Diffie-Hellman         | `2A 86 48 CE 3E 02 01`       | `1.2.840.10046.2.1`     |
| subjectKeyIdentifier   | `55 1D 0E`                   | `2.5.29.14`             |
| keyUsage               | `55 1D 0F`                   | `2.5.29.15`             |
| subjectAltName         | `55 1D 11`                   | `2.5.29.17`             |
| basicConstraints       | `55 1D 13`                   | `2.5.29.19`             |
| cRLDistributionPoints  | `55 1D 1F`                   | `2.5.29.31`             |
| certificatePolicies    | `55 1D 20`                   | `2.5.29.32`             |
| authorityKeyIdentifier | `55 1D 23`                   | `2.5.29.35`             |
| extKeyUsage            | `55 1D 25`                   | `2.5.29.37`             |

Diffie-Hellman OID解析: `2A 86 48 CE 3E 02 01`, Ephemeral-Static Diffie-Hellman (DH) key agreement algorithm

- `2A`: `1.2`, `iso/memberBody`
- `86 48`: `840`, `usa`
- `CE 3E`: `10046`, `ansi-x942`
- `02 01`: `2.1`, `number-types/dhpublicnumber`

MD5WithRSA OID解析: `2A 86 48 86 F7 0D 01 01 04`, Rivest, Shamir and Adleman (RSA) encryption with Message Digest 5 (MD5) signature

- `2A`: `1.2`, `iso/memberBody`
- `86 48`: `840`, `usa`
- `86 F7 0D`: `113549`, `rsadsi`
- `01 01 04`: `1.1.4`, `pkcs/pkcs1/MD5`

DSA OID解析: `2A 86 48 CE 38 04 01`, Digital Signature Algorithm (DSA) subject public key

- `2A`: `1.2`, `iso/memberBody`
- `86 48`: `840`, `usa`
- `CE 38`: `10040`, `x9-57`
- `04 01`: `4.1`, `x9algorithm/dsa`

## Reference

查询OID

- [OID Repository](http://www.oid-info.com/)
- [Global OID reference database](http://oidref.com/) 这是查询OID的主页
- [Reference record for OID 1.2.840.113549](http://oidref.com/1.2.840.113549)，路径是`/ISO/Member-Body/US/113549`。
- [Reference record for OID 1.2.840.113549.1.1](http://oidref.com/1.2.840.113549.1.1)，路径是`/ISO/Member-Body/US/113549/1/1`。
