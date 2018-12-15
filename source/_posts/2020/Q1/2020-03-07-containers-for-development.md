---
title: Containers for development
description: Containers for development
date: 2020-03-07 10:24:32
tags:
  - Cloud
  - Container
  - Linux
categories: [Cloud, Container]
permalink: containers-for-development
---

# Containers for development

## Base images

- https://docs.docker.com/engine/reference/builder/
- https://hub.docker.com/_/alpine
- https://hub.docker.com/_/debian
- https://hub.docker.com/_/ubuntu
- https://hub.docker.com/_/centos
- https://hub.docker.com/_/microsoft-windows-nanoserver
- https://hub.docker.com/_/microsoft-windows-servercore
- https://mcr.microsoft.com/v2/windows/servercore/tags/list
- https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/base-image-lifecycle
- https://www.azul.com/downloads/zulu-community/
- https://cdn.azul.com/zulu/bin/zulu8.44.0.11-ca-jre8.0.242-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu11.37.17-ca-jre11.0.6-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu8.44.0.11-ca-jdk8.0.242-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu11.37.17-ca-jdk11.0.6-linux_x64.tar.gz
- https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
- http://releases.ubuntu.com/xenial/ubuntu-16.04.6-server-amd64.iso
- http://releases.ubuntu.com/bionic/ubuntu-18.04.4-live-server-amd64.iso
- http://mirror.centos.org/centos-7/7/isos/x86_64/CentOS-7-x86_64-DVD-1908.iso
- http://mirror.centos.org/centos-8/8/isos/x86_64/CentOS-8.1.1911-x86_64-dvd1.iso

```shell
docker pull alpine:3
docker pull debian:9
docker pull debian:10
docker pull ubuntu:18.04
docker pull ubuntu:20.04
docker pull centos:7
docker pull centos:8
docker pull registry.suse.com/suse/sles12sp5
docker pull registry.suse.com/suse/sle15
docker pull registry.access.redhat.com/ubi7/ubi
docker pull registry.access.redhat.com/ubi8/ubi
docker pull mcr.microsoft.com/windows/nanoserver:1809
docker pull mcr.microsoft.com/windows/nanoserver:1909
docker pull mcr.microsoft.com/windows/servercore:ltsc2019

docker image inspect alpine:3
    "Created": "2020-01-18T01:19:37.187497623Z",
    "Architecture": "amd64",
    "Os": "linux",
    "Size": 5591300,
```

## SLE 15 based images

### Java 8 - 8u242-jdk

#### Dockerfile

```docker
FROM registry.suse.com/suse/sle15

ARG JDK_8_URL="https://cdn.azul.com/zulu/bin/zulu8.44.0.11-ca-jdk8.0.242-linux_x64.tar.gz"
ARG MAVEN_URL="https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz"

WORKDIR /root

ENV \
    LC_ALL="en_US.UTF-8" \
    TZ="Asia/Shanghai" \
    JAVA_HOME="/opt/jdk-8" \
    PATH="/opt/maven/bin:/opt/jdk-8/bin:/usr/sbin:/usr/bin:/sbin:/bin"

RUN set -x \
    && zypper remove --no-confirm container-suseconnect || true \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem/                    SLE-15-SP1-Module-Basesystem \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem-Updates/            SLE-15-SP1-Module-Basesystem-Updates \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools/             SLE-15-SP1-Module-Development-Tools \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools-Updates/     SLE-15-SP1-Module-Development-Tools-Updates \
    && zypper update --no-recommends --no-confirm \
    && zypper install --auto-agree-with-licenses --no-confirm --no-recommends timezone curl ca-certificates-mozilla gzip tar which \
    && zypper removerepo -a \
    && mkdir -p /opt/jdk-8 /opt/maven \
    && curl -sSL ${JDK_8_URL} | tar -C /opt/jdk-8 --strip-components=1 -xz \
    && curl -sSL ${MAVEN_URL} | tar -C /opt/maven --strip-components=1 -xz \
    && rm -fr /root/bin /opt/jdk-8/src.zip /opt/jdk-8/demo /opt/jdk-8/man /opt/jdk-8/sample \
    && mvn -version
```

#### docker build

```bash
IMG=songdongsheng/openjdk
TAG=8u242-jdk

docker build --compress --tag ${IMG}:${TAG} -f Dockerfile.8u242-jdk .

# docker image inspect ${IMG}:${TAG} | less

docker images | grep ${IMG}

docker push ${IMG}:${TAG}
```

### Java 8 - 8u242-jre

#### Dockerfile

```docker
FROM registry.suse.com/suse/sle15

ARG JRE_8_URL="https://cdn.azul.com/zulu/bin/zulu8.44.0.11-ca-jre8.0.242-linux_x64.tar.gz"

WORKDIR /root

ENV \
    LC_ALL="en_US.UTF-8" \
    TZ="Asia/Shanghai" \
    JAVA_HOME="/opt/jdk-8" \
    PATH="/opt/maven/bin:/opt/jdk-8/bin:/usr/sbin:/usr/bin:/sbin:/bin"

RUN set -x \
    && zypper remove --no-confirm container-suseconnect || true \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem/                    SLE-15-SP1-Module-Basesystem \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem-Updates/            SLE-15-SP1-Module-Basesystem-Updates \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools/             SLE-15-SP1-Module-Development-Tools \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools-Updates/     SLE-15-SP1-Module-Development-Tools-Updates \
    && zypper update --no-recommends --no-confirm \
    && zypper install --auto-agree-with-licenses --no-confirm --no-recommends timezone curl ca-certificates-mozilla gzip tar which \
    && zypper removerepo -a \
    && mkdir -p /opt/jdk-8 /opt/maven \
    && curl -sSL ${JRE_8_URL} | tar -C /opt/jdk-8 --strip-components=1 -xz \
    && rm -fr /root/bin /opt/jdk-8/src.zip /opt/jdk-8/demo /opt/jdk-8/man /opt/jdk-8/sample \
    && java -version
```

#### docker build

```bash
IMG=songdongsheng/openjdk
TAG=8u242-jre

docker build --compress --tag ${IMG}:${TAG} -f Dockerfile.8u242-jre .
docker tag ${IMG}:${TAG} ${IMG}

# docker image inspect ${IMG}:${TAG} | less

docker images | grep ${IMG}

docker push ${IMG}:${TAG}
```


### Java 11 - 11.0.6-jdk

#### Dockerfile

```docker
FROM registry.suse.com/suse/sle15

ARG JDK_11_URL="https://cdn.azul.com/zulu/bin/zulu11.37.17-ca-jdk11.0.6-linux_x64.tar.gz"
ARG MAVEN_URL="https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz"

WORKDIR /root

ENV \
    LC_ALL="en_US.UTF-8" \
    TZ="Asia/Shanghai" \
    JAVA_HOME="/opt/jdk-11" \
    PATH="/opt/maven/bin:/opt/jdk-11/bin:/usr/sbin:/usr/bin:/sbin:/bin"

RUN set -x \
    && zypper remove --no-confirm container-suseconnect || true \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem/                    SLE-15-SP1-Module-Basesystem \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem-Updates/            SLE-15-SP1-Module-Basesystem-Updates \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools/             SLE-15-SP1-Module-Development-Tools \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools-Updates/     SLE-15-SP1-Module-Development-Tools-Updates \
    && zypper update --no-recommends --no-confirm \
    && zypper install --auto-agree-with-licenses --no-confirm --no-recommends timezone curl ca-certificates-mozilla gzip tar which \
    && zypper removerepo -a \
    && mkdir -p /opt/jdk-11 /opt/maven \
    && curl -sSL ${JDK_11_URL} | tar -C /opt/jdk-11 --strip-components=1 -xz \
    && curl -sSL ${MAVEN_URL} | tar -C /opt/maven --strip-components=1 -xz \
    && rm -fr /root/bin /opt/jdk-11/lib/src.zip /opt/jdk-11/demo /opt/jdk-11/man /opt/jdk-11/jmods \
    && mvn -version
```

#### docker build

```bash
IMG=songdongsheng/openjdk
TAG=11.0.6-jdk

docker build --compress --tag ${IMG}:${TAG} -f Dockerfile.11.0.6-jdk .

# docker image inspect ${IMG}:${TAG} | less

docker images | grep ${IMG}

docker push ${IMG}:${TAG}
```

### Java 11 - 11.0.6-jre

#### Dockerfile

```docker
FROM registry.suse.com/suse/sle15

ARG JRE_11_URL="https://cdn.azul.com/zulu/bin/zulu11.37.17-ca-jre11.0.6-linux_x64.tar.gz"

WORKDIR /root

ENV \
    LC_ALL="en_US.UTF-8" \
    TZ="Asia/Shanghai" \
    JAVA_HOME="/opt/jdk-11" \
    PATH="/opt/jdk-11/bin:/usr/sbin:/usr/bin:/sbin:/bin"

RUN set -x \
    && zypper remove --no-confirm container-suseconnect || true \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem/                    SLE-15-SP1-Module-Basesystem \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Basesystem-Updates/            SLE-15-SP1-Module-Basesystem-Updates \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools/             SLE-15-SP1-Module-Development-Tools \
    && zypper addrepo -C -G http://example.com/SLE15/SLE-15-SP1-Module-Development-Tools-Updates/     SLE-15-SP1-Module-Development-Tools-Updates \
    && zypper update --no-recommends --no-confirm \
    && zypper install --auto-agree-with-licenses --no-confirm --no-recommends timezone curl ca-certificates-mozilla gzip tar which \
    && zypper removerepo -a \
    && mkdir -p /opt/jdk-11 \
    && curl -sSL ${JRE_11_URL} | tar -C /opt/jdk-11 --strip-components=1 -xz \
    && rm -fr /root/bin /opt/jdk-11/man \
    && java -version
```

#### docker build

```bash
IMG=songdongsheng/openjdk
TAG=11.0.6-jre

docker build --compress --tag ${IMG}:${TAG} -f Dockerfile.11.0.6-jre .
# docker tag ${IMG}:${TAG} ${IMG}

# docker image inspect ${IMG}:${TAG} | less

docker images | grep ${IMG}

docker push ${IMG}:${TAG}
```

## Ubuntu 18.04 based images

### All in one

#### Dockerfile

```docker
FROM ubuntu:18.04

ARG NODE_JS_12_URL="https://nodejs.org/dist/v12.16.1/node-v12.16.1-linux-x64.tar.xz"
ARG JDK_11_URL="https://cdn.azul.com/zulu/bin/zulu11.37.17-ca-jdk11.0.6-linux_x64.tar.gz"
ARG JDK_8_URL="https://cdn.azul.com/zulu/bin/zulu8.44.0.11-ca-jdk8.0.242-linux_x64.tar.gz"
ARG MAVEN_URL="https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz"
ARG JRUBY_URL="https://github.com/jruby/jruby/releases/download/9.2.11.0/jruby-dist-9.2.11.0-bin.tar.gz"

WORKDIR /root

ENV \
    LC_ALL="en_US.UTF-8" \
    TZ="Asia/Shanghai" \
    TERM="tmux-256color" \
    PATH="/opt/node-v12/bin:/opt/jruby/bin:/opt/maven/bin:/opt/jdk-11/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

RUN set -x \
    && apt-get update \
    && apt-get install -y --no-install-recommends dialog locales \
    && localedef -i en_US -f UTF-8 -A /etc/locale.alias en_US.UTF-8 \
    && localedef --list-archive \
    && apt-get dist-upgrade -y --no-install-recommends \
    && apt-get install -y --no-install-recommends curl ca-certificates xz-utils python3-distutils \
    && mkdir -p /opt/node-v12 /opt/jruby /opt/maven /opt/jdk-11 \
    && curl -sSL ${JDK_11_URL} | tar -C /opt/jdk-11 --strip-components=1 -xz \
    && curl -sSL ${MAVEN_URL} | tar -C /opt/maven --strip-components=1 -xz \
    && curl -sSL ${JRUBY_URL} | tar -C /opt/jruby --strip-components=1 -xz \
    && curl -sSL ${NODE_JS_12_URL} | tar -C /opt/node-v12 --strip-components=1 -xJ \
    && curl -sSL https://bootstrap.pypa.io/get-pip.py | python3 \
    && python3 -m pip install -U Pygments docutils PyYAML simplejson lxml requests pyOpenSSL cassandra-driver aioredis redis-py-cluster recommonmark confluent-kafka asyncpg pg8000 mysql-connector-python \
    && gem install asciidoctor asciidoctor-diagram asciidoctor-pdf asciimath pygments.rb rouge kramdown-asciidoc \
    && gem sources -c \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && python3 -m pip list --format=columns \
    && mvn -version \
    && jruby --version \
    && npm version
```

#### docker build

```bash
IMG=songdongsheng/development
TAG=20200307

docker build --tag ${IMG}:${TAG} .

# docker image inspect ${IMG}:${TAG} | less

docker images | grep ${IMG}

docker push ${IMG}:${TAG}
```
