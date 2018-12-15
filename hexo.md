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

    git submodule update --init --recursive

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

    git submodule add --force https://github.com/lewis-geek/hexo-theme-Aath themes/aath
    git submodule add --force https://github.com/chaooo/hexo-theme-BlueLake themes/BlueLake
    git submodule add --force https://github.com/theme-next/hexo-theme-next themes/next
    git submodule add --force https://github.com/pinggod/hexo-theme-apollo  themes/apollo
    git submodule add --force https://github.com/yanm1ng/hexo-theme-vexo    themes/vexo

    git submodule status
    git submodule update --init --recursive

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
