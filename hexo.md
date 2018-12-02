# 安装配置
## 初始化版本库
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
    npm outdated
    npm update
    npm audit fix

## 目录与标签

+ programming
    - concurrent
    - language
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
    - database
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
+ processor
    - x86
    - arm
    - RISC-V
+ operating system
    - Linux
        - Debian
        - Ubuntu
        - RHEL
        - CentOS
        - SLES
    - windows
    - macOS
+ System architecture
    - SOA
    - RESTful
    - MSA - Microservices Architecture - 微服务
+ bigdata
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
    - cluster
        - Linux IPVS (LVS)
        - Keepalived
        - HAProxy
        - Nginx
+ misc
    - trends

## 目录

## 配置文件简介
    hexo 有两个配置文件，一个在 hexo 项目的根目录，另一个在主题文件夹的根目录，文件名都是均为 _config.yml 。

# 使用主题
## 下载主题
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

## 配置主题
    修改 hexo 项目的根目录的 _config.yml，配置 theme 为新主题的名称。

# 常用命令
## 列出站点信息
    hexo list page|post|route|tag|category

## 站点本地预览
    hexo server

## 生成静态文件
    hexo generate

## 部署静态文件
    # dig www.songdongsheng.info +nostats +nocomments +nocmd

    hexo deploy
