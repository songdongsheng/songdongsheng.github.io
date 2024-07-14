---
title: 'Post-Quantum Cryptography in OpenPGP'
excerpt: 'Post-Quantum Cryptography in OpenPGP'
date: 2024-07-13 14:53:36
tags:
  - Linux
  - Windows
  - Security
categories: [Utility, Security]
---

# Post-Quantum Cryptography in OpenPGP

The newly released GnuPG 2.5.0 begins to experimentally support the post-quantum public key algorithm extension of the OpenPGP protocol. The post-quantum public key algorithm extension of the OpenPGP protocol provides the basis for long-term secure OpenPGP signatures and ciphertexts. It defines composite public key encryption based on `ML-KEM` and composite public key signatures based on `ML-DSA` (both of which are used in conjunction with elliptic curve cryptography), as well as `SLH-DSA` as an independent public key signature scheme. GnuPG currently only supports composite public key encryption based on `ML-KEM`, and support for digital signatures is under development.

GnuPG 2.5.0 supports PGC encryption algorithms `ky768_cv25519` and `ky1024_cv448` based on `X25519` and `X448`, and PQC encryption algorithms `ky768_bp256`, `ky1024_bp384` and `ky1024_bp512` based on `brainpoolP256r1`, `brainpoolP384r1` and `brainpoolP512r1`. Since `Brainpool` is not efficient, widely used, and has no security advantages, this article only tests PGC encryption algorithms `ky768_cv25519` and `ky1024_cv448` based on `X25519` and `X448`.

## What You Need

- GnuPG 2.5.0 of later

## ML-KEM-768 + X25519

### Generate Key

```bash
gpg --quick-gen-key --batch --passphrase='' "X25519 User <x25519@example.com>" Ed25519 cert 1y

gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback DDE2BD47E5196CED794FDF17D9FFA236037FC097 ed25519 sign,auth 1y
gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback DDE2BD47E5196CED794FDF17D9FFA236037FC097 cv25519 encrypt 1y

# ML-KEM-768 + X25519
gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback DDE2BD47E5196CED794FDF17D9FFA236037FC097 ky768_cv25519 encrypt 1y
```

### List Key

```bash
$ gpg -K DDE2BD47E5196CED794FDF17D9FFA236037FC097
sec   ed25519/D9FFA236037FC097 2024-07-13 [C] [expires: 2024-07-13]
      Key fingerprint = DDE2 BD47 E519 6CED 794F  DF17 D9FF A236 037F C097
uid                 [ultimate] X25519 User <x25519@example.com>
ssb   ed25519/A2E4D93F5E67CDE2 2024-07-13 [SA] [expires: 2024-07-13]
      Key fingerprint = 7660 3CDA E782 AEE7 C42A  EF74 A2E4 D93F 5E67 CDE2
ssb   cv25519/1D07C17D30191210 2024-07-13 [E] [expires: 2024-07-13]
      Key fingerprint = F3AE 317F F319 12E7 E84B  9AC9 1D07 C17D 3019 1210
ssb   ky768_cv25519/5C8EC98545A74E6C 2024-07-13 [E] [expires: 2024-07-13]
      Key fingerprint = 5C8EC 98545 A74E6 C442A 76F3F 553F9 D39F0 FDF9A 2B7CD 8CA05
```

### Specify PGC key for encryption

```bash
$ echo "Hello, PGC!" > secret.txt
$ gpg --encrypt --yes -r 5C8EC98545A74E6C! secret.txt
$ gpg --decrypt --passphrase='' --pinentry-mode loopback secret.txt.gpg
gpg: encrypted with ky768_cv25519 key, ID 5C8EC98545A74E6C, created 2024-07-13
      "X25519 User <x25519@example.com>"
Hello, PGC!
$ stat --printf="%s\t%n\n" secret.txt*
12      secret.txt
1274    secret.txt.gpg
```

### Specify X25519 key for encryption

```bash
$ echo "Hello, PGC!" > secret.txt
$ gpg --encrypt --yes -r 1D07C17D30191210! secret.txt
$ gpg --decrypt --passphrase='' --pinentry-mode loopback secret.txt.gpg
gpg: encrypted with cv25519 key, ID 1D07C17D30191210, created 2024-07-13
      "X25519 User <x25519@example.com>"
Hello, PGC!
$ stat --printf="%s\t%n\n" secret.txt*
12      secret.txt
189     secret.txt.gpg
```

### Automatically select encryption key

```bash
$ echo "Hello, PGC!" > secret.txt
$ gpg --encrypt --yes -r D9FFA236037FC097 secret.txt
$ gpg --decrypt --passphrase='' --pinentry-mode loopback secret.txt.gpg
gpg: encrypted with ky768_cv25519 key, ID 5C8EC98545A74E6C, created 2024-07-13
      "X25519 User <x25519@example.com>"
Hello, PGC!
$ stat --printf="%s\t%n\n" secret.txt*
12      secret.txt
1274    secret.txt.gpg
```

### Delete Key

```bash
gpg --delete-key --batch --yes DDE2BD47E5196CED794FDF17D9FFA236037FC097
```

## ML-KEM-1024 + X448

### Generate Key

```bash
gpg --quick-gen-key --batch --passphrase='' "X448 User <x448@example.com>" Ed448 cert 1y

gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback 194E2C38F0C4354C4172DC586E3455C57BA2F61AB06EFC243A5D386ED86242B7 ed448 sign,auth 1y
gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback 194E2C38F0C4354C4172DC586E3455C57BA2F61AB06EFC243A5D386ED86242B7 cv448 encrypt 1y

# ML-KEM-1024 + X448
gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback 194E2C38F0C4354C4172DC586E3455C57BA2F61AB06EFC243A5D386ED86242B7 ky1024_cv448 encrypt 1y
```

### List Key

```bash
$ gpg -K 194E2C38F0C4354C4172DC586E3455C57BA2F61AB06EFC243A5D386ED86242B7
sec   ed448/194E2C38F0C4354C 2024-07-13 [C] [expires: 2024-07-13]
      Key fingerprint = 194E2 C38F0 C4354 C4172 DC586 E3455 C57BA 2F61A B06EF C243A
uid                 [ultimate] X448 User <x448@example.com>
ssb   ed448/96EA0B90619ACFCD 2024-07-13 [SA] [expires: 2024-07-13]
      Key fingerprint = 96EA0 B9061 9ACFC D5709 BC773 F6300 D1B38 457BC 19792 C4763
ssb   cv448/36FC63E177ECC404 2024-07-13 [E] [expires: 2024-07-13]
      Key fingerprint = 36FC6 3E177 ECC40 4030B C0004 D91F7 4E1F7 E5590 CEC6A A096F
ssb   ky1024_cv448/373E994D9B9AC9AE 2024-07-13 [E] [expires: 2024-07-13]
      Key fingerprint = 373E9 94D9B 9AC9A EC476 3FA66 89794 AA808 894BE 4FDF4 020EE
```

### Specify PGC key for encryption

```bash
$ echo "Hello, PGC!" > secret.txt
$ gpg --encrypt --yes -r 373E994D9B9AC9AE! secret.txt
$ gpg --decrypt --passphrase='' --pinentry-mode loopback secret.txt.gpg
gpg: encrypted with ky1024_cv448 key, ID 373E994D9B9AC9AE, created 2024-07-13
      "X448 User <x448@example.com>"
Hello, PGC!
$ stat --printf="%s\t%n\n" secret.txt*
12      secret.txt
1778    secret.txt.gpg
```

### Specify X448 key for encryption

```bash
$ echo "Hello, PGC!" > secret.txt
$ gpg --encrypt --yes -r 36FC63E177ECC404! secret.txt
$ gpg --decrypt --passphrase='' --pinentry-mode loopback secret.txt.gpg
gpg: encrypted with cv448 key, ID 36FC63E177ECC404, created 2024-07-13
      "X448 User <x448@example.com>"
Hello, PGC!
$ stat --printf="%s\t%n\n" secret.txt*
12      secret.txt
212     secret.txt.gpg
```

### Automatically select encryption key

```bash
$ echo "Hello, PGC!" > secret.txt
$ gpg --encrypt --yes -r 194E2C38F0C4354C secret.txt
$ gpg --decrypt --passphrase='' --pinentry-mode loopback secret.txt.gpg
gpg: encrypted with ky1024_cv448 key, ID 373E994D9B9AC9AE, created 2024-07-13
      "X448 User <x448@example.com>"
Hello, PGC!
$ stat --printf="%s\t%n\n" secret.txt*
12      secret.txt
1778    secret.txt.gpg
```

### Delete Key

```bash
gpg --delete-key --batch --yes 194E2C38F0C4354C4172DC586E3455C57BA2F61AB06EFC243A5D386ED86242B7
```

## ML-DSA-65 + Ed25519

```bash
# Not working yet
gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback DDE2BD47E5196CED794FDF17D9FFA236037FC097 dil3_ed25519 sign 1y
```

## ML-DSA-87 + Ed448

```bash
# Not working yet
gpg --quick-add-key --batch --passphrase='' --pinentry-mode loopback 194E2C38F0C4354C4172DC586E3455C57BA2F61AB06EFC243A5D386ED86242B7 dil5_ed448 sign 1y
```

## Wrapping Up

OpenPGP currently does not fully support the PGC algorithm. It only supports encryption algorithms and lacks digital signature algorithms. It only supports generating PGC keys in the `quick-gen-key` and `quick-add-key` interfaces, but does not support generating PGC keys in the interactive interface.

The current state of OpenPGP's PGC implementation is quite exciting, considering that the algorithm is still in the draft stage. Let's look forward to the PGC algorithm being fully supported in OpenPGP, making our future more secure.

## Reference

- https://github.com/gpg/gnupg/blob/master/tests/openpgp/samplekeys/README
- https://github.com/gpg/gnupg/blob/master/common/openpgpdefs.h
- https://www.ietf.org/archive/id/draft-wussler-openpgp-pqc-04.html
- https://csrc.nist.gov/pubs/fips/203/ipd
- https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.203.ipd.pdf
- https://github.com/nccgroup/fips203
- https://dev.gnupg.org/source/gnupg/browse/master/NEWS
- https://dev.gnupg.org/T6815
- https://dev.gnupg.org/T7189
- https://lists.gnupg.org/pipermail/gnupg-announce/2024q3/000484.html
