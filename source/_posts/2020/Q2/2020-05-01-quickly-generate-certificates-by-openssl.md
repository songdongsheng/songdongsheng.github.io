---
title: Quickly generate certificates by OpenSSL
description: Quickly generate certificates by OpenSSL
date: 2020-05-01 21:39:00
tags:
  - Programming
  - C/C++
categories: [Programming, C/C++]
permalink: quickly-generate-certificates-by-openssl
---

# Quickly generate certificates by OpenSSL

## Build OpenSSL

```shell
cd /tmp
curl -sSL https://www.openssl.org/source/openssl-1.1.1g.tar.gz | tar -xvz
cd openssl-1.1.1g

./config --prefix=/usr --openssldir=/etc/pki/tls -fPIC no-shared
make -j2 LDFLAGS="-Wl,--strip-all"
make install
```

## Check OpenSSL

```shell
# openssl version
OpenSSL 1.1.1g  21 Apr 2020

# openssl version -a
OpenSSL 1.1.1g  21 Apr 2020
built on: Fri May 1 13:12:19 2020 UTC
platform: linux-x86_64
options:  bn(64,64) rc4(16x,int) des(int) idea(int) blowfish(ptr)
compiler: gcc -fPIC -pthread -m64 -Wa,--noexecstack -Wall -O3 -fPIC -DOPENSSL_USE_NODELETE -DL_ENDIAN -DOPENSSL_PIC -DOPENSSL_CPUID_OBJ -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DKECCAK1600_ASM -DRC4_ASM -DMD5_ASM -DAESNI_ASM -DVPAES_ASM -DGHASH_ASM -DECP_NISTZ256_ASM -DX25519_ASM -DPOLY1305_ASM -DNDEBUG
OPENSSLDIR: "/etc/pki/tls"
ENGINESDIR: "/usr/lib64/engines-1.1"
Seeding source: os-specific

echo | openssl s_client -connect www.feistyduck.com:443 -tls1_2 -servername www.feistyduck.com -reconnect 2>&1  | grep -E "^New|^Reused"

echo | openssl s_client -connect docs.python.org:443 -tls1_3 -servername docs.python.org -reconnect 2>&1  | grep -E "^New|^Reused"

# echo -e "GET /3/ HTTP/1.1\r\nConnection: close\r\nHost: docs.python.org\r\n\r\n" | openssl s_client -quiet -connect docs.python.org:443 -tls1_3 -servername docs.python.org
```

## OpenSSL ciphers

```shell
# openssl ciphers -v | grep -E "^DHE-RSA-|^ECDHE-RSA-|^ECDHE-ECDSA-" | grep AEAD | sort
DHE-RSA-AES128-GCM-SHA256 TLSv1.2 Kx=DH       Au=RSA  Enc=AESGCM(128) Mac=AEAD
DHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=DH       Au=RSA  Enc=AESGCM(256) Mac=AEAD
DHE-RSA-CHACHA20-POLY1305 TLSv1.2 Kx=DH       Au=RSA  Enc=CHACHA20/POLY1305(256) Mac=AEAD
ECDHE-ECDSA-AES128-GCM-SHA256 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AESGCM(128) Mac=AEAD
ECDHE-ECDSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AESGCM(256) Mac=AEAD
ECDHE-ECDSA-CHACHA20-POLY1305 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=CHACHA20/POLY1305(256) Mac=AEAD
ECDHE-RSA-AES128-GCM-SHA256 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AESGCM(128) Mac=AEAD
ECDHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AESGCM(256) Mac=AEAD
ECDHE-RSA-CHACHA20-POLY1305 TLSv1.2 Kx=ECDH     Au=RSA  Enc=CHACHA20/POLY1305(256) Mac=AEAD
```

## Generate RSA-2048 certificate

```shell
openssl genrsa -out server.key 2048

openssl req -key server.key -x509 -days 7670 -out server.crt -subj '/CN=me' \
    -addext "subjectAltName=DNS:example.com,DNS:*.example.com,IP:127.0.0.1,IP:::1" \
    -addext "certificatePolicies=1.2.3.4"

openssl x509 -text -in server.crt | less
openssl rsa -in server.key -text -noout | less

openssl dsaparam -genkey 2048 | openssl dsa -out dsa.key
openssl dsa -in dsa.key -text -noout | less
```

## Generate ECDSA certificate

```shell
export algorithm=secp521r1
export hash=sha512
export algorithm=secp384r1
export hash=sha384

openssl ecparam -genkey -out server-$algorithm.key -name $algorithm

openssl req -key server-$algorithm.key -subj '/CN=me' \
    -new -x509 -${hash} -days 7670 -extensions v3_ca -out server-$algorithm.crt \
    -addext "subjectAltName=DNS:example.com,DNS:*.example.com,DNS:*.example.net,IP:127.0.0.1,IP:::1"

openssl pkey -in server-$algorithm.key -text -noout
openssl x509 -in server-$algorithm.crt -text -noout
openssl x509 -purpose -in server-$algorithm.crt
```

## Generate EdDSA certificate

```shell
export algorithm=ed448
export algorithm=ed25519

openssl genpkey -algorithm $algorithm -out server-$algorithm.key

openssl req -key server-$algorithm.key -subj '/CN=me' \
    -new -x509 -days 7670 -extensions v3_ca -out server-$algorithm.crt \
    -addext "subjectAltName=DNS:example.com,DNS:*.example.com,DNS:*.example.net,IP:127.0.0.1,IP:::1"

openssl pkey -in server-$algorithm.key -text -noout
openssl x509 -in server-$algorithm.crt -text -noout
openssl x509 -purpose -in server-$algorithm.crt
```
