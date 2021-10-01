---
title: Generate CA certificates for Java
excerpt: Generate CA certificates for Java
date: 2019-11-16 09:19:03
tags:
  - Java
categories: [Programming, Java]
---

# Generate CA certificates for Java

## packages

```bash
# dpkg -l | grep ca-certificates
ii  ca-certificates         20190110    all     Common CA certificates
ii  ca-certificates-java    20190405    all     Common CA certificates (JKS keystore)
```

## generate cacerts

```bash
storepass='changeit'
JAR=/usr/share/ca-certificates-java/ca-certificates-java.jar

rm -f /etc/ssl/certs/java/cacerts
find /etc/ssl/certs -name \*.pem | \
while read filename; do
    alias=$(basename $filename .pem | tr A-Z a-z | tr -cs a-z0-9 _)
    alias=${alias%*_}
    if [ -n "$FIXOLD" ]; then
        echo "-${alias}"
        echo "-${alias}_pem"
    fi
    echo "+${filename}"
done | \
java -Xmx64m -jar $JAR -storepass "$storepass"
```

## results

```bash
ls -la /etc/ssl/certs/java/cacerts
sha256sum /etc/ssl/certs/java/cacerts

# ls -la /etc/ssl/certs/java/cacerts
-rw-r--r-- 1 root root 150689 2019-11-15 21:24 /etc/ssl/certs/java/cacerts

# sha256sum /etc/ssl/certs/java/cacerts
0d0041de796beaecfd8a2f859e3c642839fa9d6567d6da394fed2d001076c765  /etc/ssl/certs/java/cacerts
```
