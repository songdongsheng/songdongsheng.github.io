# 安装配置

## 初始化版本库

    npm config set registry https://mirrors.huaweicloud.com/repository/npm/
    npm config set disturl https://mirrors.huaweicloud.com/nodejs
    npm config set sass_binary_site https://mirrors.huaweicloud.com/node-sass
    npm config set phantomjs_cdnurl https://mirrors.huaweicloud.com/phantomjs
    npm config set chromedriver_cdnurl https://mirrors.huaweicloud.com/chromedriver
    npm config set operadriver_cdnurl https://mirrors.huaweicloud.com/operadriver
    npm config set electron_mirror https://mirrors.huaweicloud.com/electron/
    npm config set python_mirror https://mirrors.huaweicloud.com/python

    npm cache clean -f

    SET NODE_HOME=C:\opt\node-v14
    %NODE_HOME%\node.exe %NODE_HOME%\node_modules\npm\bin\npm-cli.js install -g npm

    npm install -g npm

    npm install -g gitbook-cli
    npm install -g hexo-cli

    hexo init songdongsheng.github.io
    cd songdongsheng.github.io

    npm install --save
    npm install --save hexo hexo-beautify hexo-browsersync hexo-deployer-git hexo-generator-archive hexo-generator-feed hexo-generator-search hexo-generator-sitemap hexo-renderer-jade hexo-renderer-sass

## 使用旧版本库

    npm install -g hexo-cli

    git clone -b hexo git@github.com:songdongsheng/songdongsheng.github.io.git
    cd songdongsheng.github.io

    git submodule update --init --force --remote --recursive

## 安装与更新 npm 包

    npm install --save
    npm update
    npm outdated
    npm audit
    npm audit fix

## 目录与标签

+ Programming
    - Concurrent
    - Language
        - Java
            - Hibernate JPA
            - MyBATIS
            - JDBC
            - Spring MVC
            - Spring Cloud
            - Jersey
            - Apache CXF
            - Ehcache
            - Java EE
                - Weblogic
                - WebSphere
                - TomEE
        - C
        - C#
        - Python
        - Go
        - shell
        - Tex
    - Database
        - SQL
            - Oracle
            - SQL Server
            - PostgreSQL
            - MySQL
        - NoSQL
            - Cassandra
            - HBase
            - MongoDB
            - ElasticSearch
            - Redis
        - NewSQL
            - Cloud Spanner - ANSI 2011 SQL
            - OceanBase - Oracle & MySQL
            - Amazon Aurora - MySQL, PostgreSQL
            - ClustrixDB - MySQL
            - YugaByteDB - PostgreSQL (BETA)
            - CockroachDB - PostgreSQL
            - SequoiaSQL - PostgreSQL/MySQL - 巨杉数据库
            - CrateDB - IoT
            - TiDB - MySQL
            - Trafodion
        - MQ
            - RabbitMQ
            - RocketMQ
            - Kafka
            - Pulsar
+ Processor
    - x86
    - arm
    - RISC-V
+ Operating system
    - Linux
        - Debian
        - Ubuntu
        - RHEL
        - CentOS
        - SLES
    - Windows
    - macOS
+ System architecture
    - SOA
    - RESTful
    - MSA - Microservices Architecture - 微服务
+ Bigdata
    - Hadoop
    - Hbase
+ Operation and monitoring
    - DevOps
        - GitHub
        - GitLab
        - Jenkins
        - SonarQube
        - Jira
        - Puppet
        - InfluxDB
        - Grafana
        - Nagios
        - Prometheus
    - Container
        - LXC
        - Docker
    - Virtualization - Virtual machines
        - VMware
        - KVM
        - Xen
    - Cluster
        - Linux IPVS (LVS)
        - Keepalived
        - HAProxy
        - Nginx
    - Distributed storage
        - GlusterFS
        - Ceph Storage Cluster
+ Misc
    - Trends


## 配置文件简介

    hexo 有两个配置文件，一个在 hexo 项目的根目录，另一个在主题文件夹的根目录，文件名都是均为 _config.yml 。

## 使用主题

### 下载主题

    # https://hexo.io/zh-cn/docs/
    # https://hexo.io/themes/

    # http://git-scm.com/docs/git-submodule
    git submodule add --force --depth 12 -b master https://github.com/YenYuHsuan/hexo-theme-beantech themes/beantech
    git submodule add --force --depth 12 -b master https://github.com/chaooo/hexo-theme-BlueLake themes/BlueLake
    git submodule add --force --depth 12 -b main   https://github.com/nexmoe/hexo-theme-yet-the-books.git themes/books
    git submodule add --force --depth 12 -b master https://github.com/littlewin-wang/hexo-theme-casual themes/casual
    git submodule add --force --depth 12 -b master https://github.com/wd/hexo-fabric.git themes/fabric
    git submodule add --force --depth 12 -b master https://github.com/iTimeTraveler/hexo-theme-hiero.git themes/hiero
    git submodule add --force --depth 12 -b master https://github.com/iTimeTraveler/hexo-theme-hipaper themes/hipaper
    git submodule add --force --depth 12 -b master https://github.com/first19326/hexo-liveforcode themes/liveforcode
    git submodule add --force --depth 12 -b master https://github.com/miccall/hexo-theme-Mic_Theme themes/mic
    git submodule add --force --depth 12 -b master https://github.com/theme-next/hexo-theme-next themes/next
    git submodule add --force --depth 12 -b master https://github.com/sabrinaluo/hexo-theme-replica themes/replica
    git submodule add --force --depth 12 -b master https://github.com/SumiMakito/hexo-theme-typography themes/typography
    git submodule add --force --depth 12 -b master https://github.com/fooying/hexo-theme-xoxo-plus themes/xoxo-plus

    :set tabstop=8 softtabstop=0 expandtab shiftwidth=4 smarttab
    :set tabstop=8
    :set shiftwidth=8
    :set expandtab
    :set retab

    $ du -ms themes/*
    1       themes/aath
    15      themes/beantech
    2       themes/BlueLake
    4       themes/books
    1       themes/casual
    1       themes/fabric
    4       themes/hiero
    4       themes/hipaper
    98      themes/liveforcode
    6       themes/mic
    2       themes/next
    1       themes/replica
    4       themes/typography
    1       themes/xoxo-plus

    # This repository has been archived by the owner. It is now read-only.
    git submodule add --force -b develop https://github.com/lewis-geek/hexo-theme-Aath themes/aath
    git submodule add --force -b master https://github.com/pinggod/hexo-theme-apollo  themes/apollo
    git submodule deinit  --force themes/aath
    git submodule deinit  --force themes/apollo

    git submodule status
    git submodule update --init --force --remote --recursive

    git submodule deinit -f themes/apollo
    git rm -f themes/apollo
    rm -fr .git/modules/themes/apollo

    搜索插件 algolia https://www.algolia.com/
    评论插件 来必力  https://www.livere.com/

### 配置主题

    修改 hexo 项目的根目录的 _config.yml，配置 theme 为新主题的名称。

## 常用命令

### 列出站点信息

    hexo list page|post|route|tag|category

### 站点本地预览

    hexo server

### 生成静态文件

    hexo generate

### 部署静态文件

    # dig www.songdongsheng.info +nostats +nocomments +nocmd

    hexo deploy

## 基于容器构建

    podman run --rm -it -h hexo-builder -v `pwd`:/root -w /root -p 4000:4000 \
        --pull always ubuntu:20.04

    podman run --rm -it -h hexo-builder -v `pwd`:/root -w /root -p 4000:4000 \
        --pull always songdongsheng.info/library/ubuntu:20.04

    apt-get update && apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends ca-certificates curl git g++ libsass-dev vim-tiny xz-utils

    NODE_DIST_FILE=$(curl -sL https://nodejs.org/dist/latest-v14.x/SHASUMS256.txt.asc  | grep node-.*-linux-x64.tar.xz | awk '{print $2}' | head -1)
    mkdir -p /opt/node-14 && curl -sL https://nodejs.org/dist/latest-v14.x/${NODE_DIST_FILE} | tar -xvJf - -C /opt/node-14 --strip-components=1

    groupadd -g 1200 dongsheng || addgroup -g 1200 dongsheng
    useradd -g dongsheng -G wheel -u 1200 -s /bin/bash -m dongsheng || useradd -g dongsheng -G sudo -u 1200 -s /bin/bash -m dongsheng || (adduser -G dongsheng -u 1200 -D -s /bin/sh dongsheng && addgroup dongsheng wheel)

    export PATH=/opt/node-14/bin:/usr/sbin:/usr/bin:/sbin:/bin

    node --version
    npm --version
    npm version

    git config --global --replace-all safe.directory /root
    git submodule update --init --force --remote --recursive

    npm install --unsafe-perm --global hexo@4.2.1 && npm list --global
    npm install --unsafe-perm && hexo generate

    # https://github.com/sass/node-sass/releases/tag/v4.14.1 Linux 64-bit with Unsupported runtime
    hexo server

    hexo generate
    hexo deploy

    npm install -g npm
    hexo init songdongsheng.one


    # https://mermaid-js.github.io/
    npm install -g hexo-cli mermaid-cli
