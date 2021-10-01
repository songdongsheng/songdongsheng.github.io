---
title: Manually generate the Java root certificate
excerpt: Manually generate the Java root certificate
date: 2018-12-30 15:29:17
tags:
  - Programming
  - Java
  - Security
categories: [Programming, Java]
---

# Manually generate the Java root certificate

## Related packages

```shell
# dpkg -l | grep ca-certificates
ii  ca-certificates         20161130+nmu1+deb9u1    all     Common CA certificates
ii  ca-certificates-java    20170929~deb9u3         all     Common CA certificates (JKS keystore)
```

## Shell script

```shell
storepass='changeit'
JAR=/usr/share/ca-certificates-java/ca-certificates-java.jar

rm -f /etc/ssl/certs/java/cacerts
find /etc/ssl/certs -name \*.pem | \
while read filename; do
    echo "+${filename}"
done | \
java -Xmx64m -jar $JAR -storepass "$storepass"
```
