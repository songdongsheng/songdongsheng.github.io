---
title: OpenJDK Life Cycle
excerpt: OpenJDK Life Cycle
date: 2019-08-31 23:15:57
tags:
  - Java
categories: [Programming, Java]
---

# OpenJDK Life Cycle

+ https://wiki.openjdk.java.net/display/jdk8u
+ https://wiki.openjdk.java.net/display/JDKUpdates/JDK11u

## Oracle Java SE

+ https://www.oracle.com/java/technologies/java-se-support-roadmap.html
+ Java 8, March 2014 to December 2030
+ Java 11, September 2018 to September 2026
+ Java 17, September 2021 to September 2029
+ Java 21, September 2023 to September 2031

## Azul Zulu

+ https://www.azul.com/products/azul-support-roadmap/
+ https://www.azul.com/products/zulu-community/
+ https://www.azul.com/products/zulu-community/free-jdk-comparison-matrix/
+ https://www.azul.com/downloads/zulu-community/
+ Java 8, Dec 2030
+ Java 11, Sep 2026
+ Java 17, Sep 2029
+ Java 21, Sep 2031

```bash
podman run --rm --pull always docker.io/azul/zulu-openjdk:8 java -version
podman run --rm --pull always docker.io/azul/zulu-openjdk:11 java -version

podman run --rm --pull always docker.io/azul/zulu-openjdk:17-jre java -version
podman run --rm --pull always docker.io/azul/zulu-openjdk:17 java -version
```

## RedHat

+ https://access.redhat.com/articles/1299013
+ https://developers.redhat.com/products/openjdk/download/
+ OpenJDK 8, May 2026
+ OpenJDK 11, October 2024
+ OpenJDK 17, October 2027

## Eclipse Temurin (Adoptium’s JDKs)

+ https://adoptium.net/support/
+ https://adoptium.net/supported-platforms/
+ https://adoptium.net/temurin/releases/
+ https://adoptium.net/jmc/
+ https://adoptium.net/temurin/archive/
+ https://hub.docker.com/_/eclipse-temurin?tab=tags
+ OpenJDK 8, At Least May 2026
+ OpenJDK 11, At Least Oct 2024
+ OpenJDK 17, At Least Oct 2027

```bash
podman run --rm --pull always docker.io/library/eclipse-temurin:11-jre java -version
podman run --rm --pull always docker.io/library/eclipse-temurin:11-jdk java -version

podman run --rm --pull always docker.io/library/eclipse-temurin:17-jre java -version
podman run --rm --pull always docker.io/library/eclipse-temurin:17-jdk java -version

docker run --rm --pull always eclipse-temurin:11-jre-nanoserver java -version
docker run --rm --pull always eclipse-temurin:11-jdk-nanoserver java -version

docker run --rm --pull always eclipse-temurin:17-jre-nanoserver java -version
docker run --rm --pull always eclipse-temurin:17-jdk-nanoserver java -version
```

## Amazon Corretto

+ https://aws.amazon.com/corretto/faqs/#support
+ https://github.com/corretto/corretto-8
+ https://github.com/corretto/corretto-11
+ https://github.com/corretto/corretto-17
+ Java 8, at least April 2026
+ Java 11, at least July 2027
+ Java 17, at least July 2029

## Microsoft Build of OpenJDK
+ https://docs.microsoft.com/en-us/java/openjdk/support
+ https://docs.microsoft.com/en-us/java/openjdk/download
+ https://docs.microsoft.com/en-us/java/openjdk/download-major-urls
+ https://docs.microsoft.com/en-us/java/openjdk/containers
+ https://docs.microsoft.com/en-us/java/openjdk/java-jlink-runtimes
+ OpenJDK 11 LTS, September, 2024
+ OpenJDK 17 LTS, September, 2027

## Tencent Kona

+ https://github.com/Tencent/TencentKona-8/
+ https://github.com/Tencent/TencentKona-11/
+ https://github.com/Tencent/TencentKona-17/
+ https://github.com/Tencent/TencentKona-8/releases/tag/Fiber-8.0.8-GA
+ https://github.com/Tencent/TencentKona-11/releases/tag/kona11.0.13-fiber
+ https://github.com/Tencent/TencentKona-11/releases/tag/kona11.0.8-vector-api

```bash
podman run --rm --pull always docker.io/konajdk/konajdk:8 java -version
podman run --rm --pull always docker.io/konajdk/konajdk:11 java -version
```
