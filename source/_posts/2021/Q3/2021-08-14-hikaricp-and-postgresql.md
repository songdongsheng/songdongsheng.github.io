---
title: HikariCP and PostgreSQL
excerpt: HikariCP and PostgreSQL
date: 2021-08-14 10:45:29
tags:
    - Programming
    - Java
categories: [Programming, Java]
---

# HikariCP and PostgreSQL

## Introduction

HikariCP is a simple, solid, high-performance, "zero-overhead" production ready JDBC connection pool at last. At roughly 130Kb, the library is very light.

## Artifacts

### Java 11

```xml
<dependency>
   <groupId>com.zaxxer</groupId>
   <artifactId>HikariCP</artifactId>
   <version>5.0.0</version>
</dependency>
```

### Java 8

```xml
<dependency>
   <groupId>com.zaxxer</groupId>
   <artifactId>HikariCP</artifactId>
   <version>4.0.3</version>
</dependency>
```

## About Pool Sizing

Configuring a connection pool is something that developers often get wrong. There are several, possibly counter-intuitive for some, principles that need to be understood when configuring the pool.

When we look at what the major bottlenecks for a database are, they can be summarized as three basic categories: CPU, Disk, Network. We could add Memory in there, but compared to Disk and Network there are several orders of magnitude difference in bandwidth.

Don't over-provision your database. The formula below is provided by the PostgreSQL project as a starting point, but I believe it will be largely applicable across databases. You should test your application, i.e. simulate expected load, and try different pool settings around this starting point:

```java
connections = ((core_count * 2) + effective_spindle_count)
```

Don't be tricked into thinking, "SSDs are faster and therefore I can have more threads". That is exactly 180 degrees backwards. Faster, no seeks, no rotational delays means less blocking and therefore fewer threads [closer to core count] will perform better than more threads. More threads only perform better when blocking creates opportunities for executing.

## Production Ready Configuration

```xml
<property name="hibernate.hikari.jdbcUrl" value="<jdbcUrl>" />
<property name="hibernate.hikari.username" value="<username>" />
<property name="hibernate.hikari.password" value="<password>" />
<property name="hibernate.hikari.autoCommit" value="true" />
<property name="hibernate.hikari.minimumIdle" value="10" />
<property name="hibernate.hikari.maximumPoolSize" value="25" />
<property name="hibernate.hikari.keepaliveTime" value="60000" />
<property name="hibernate.hikari.idleTimeout" value="120000" />
<property name="hibernate.hikari.leakDetectionThreshold" value="150000" />
<property name="hibernate.hikari.maxLifeTime" value="180000" />
<property name="hibernate.hikari.connectionTimeout" value="3000" />
<property name="hibernate.hikari.validationTimeout" value="2500" />
<property name="hibernate.hikari.registerMbeans" value="true" />
```

## Workaround for Missing Configurations

If missing the following configuration items：

```xml
<property name="hibernate.hikari.keepaliveTime" value="60000" />
<property name="hibernate.hikari.idleTimeout" value="120000" />
<property name="hibernate.hikari.leakDetectionThreshold" value="150000" />
<property name="hibernate.hikari.maxLifeTime" value="180000" />
```

We can workaround it by tunning in the PostgreSQL server side:

```sql
alter system set tcp_keepalives_idle=10;
alter system set tcp_keepalives_interval=2;
alter system set tcp_keepalives_count=3;
alter system set idle_in_transaction_session_timeout='10s';

SELECT pg_reload_conf();
```

In sessions connected via a Unix-domain socket, [TCP keep-alive parameters](https://www.postgresql.org/docs/10/runtime-config-connection.html) are ignored and always reads as zero.

```bash
$ psql postgres

postgres=# show tcp_keepalives_idle;
 tcp_keepalives_idle
---------------------
 0
(1 row)

postgres=# show tcp_keepalives_interval;
 tcp_keepalives_interval
-------------------------
 0
(1 row)

postgres=# show tcp_keepalives_count;
 tcp_keepalives_count
----------------------
 0
(1 row)

postgres=# show idle_in_transaction_session_timeout;
 idle_in_transaction_session_timeout
-------------------------------------
 0
(1 row)

postgres=# alter system set tcp_keepalives_idle=10;
ALTER SYSTEM
postgres=# alter system set tcp_keepalives_interval=2;
ALTER SYSTEM
postgres=# alter system set tcp_keepalives_count=3;
ALTER SYSTEM
postgres=# alter system set idle_in_transaction_session_timeout='10s';
ALTER SYSTEM
postgres=#
postgres=# SELECT pg_reload_conf();
 pg_reload_conf
----------------
 t
(1 row)

postgres=# show idle_in_transaction_session_timeout;
 idle_in_transaction_session_timeout
-------------------------------------
 10s
(1 row)

postgres=# show tcp_keepalives_count;
 tcp_keepalives_count
----------------------
 0
(1 row)

postgres=# show tcp_keepalives_interval;
 tcp_keepalives_interval
-------------------------
 0
(1 row)

postgres=# show tcp_keepalives_idle;
 tcp_keepalives_idle
---------------------
 0
(1 row)
```

Let's check these parameters with TCP connection:

```bash
$ psql --host=127.0.0.1

postgres=# show tcp_keepalives_idle;
 tcp_keepalives_idle
---------------------
 10
(1 row)

postgres=# show tcp_keepalives_interval;
 tcp_keepalives_interval
-------------------------
 2
(1 row)

postgres=# show tcp_keepalives_count;
 tcp_keepalives_count
----------------------
 3
(1 row)

postgres=# show idle_in_transaction_session_timeout;
 idle_in_transaction_session_timeout
-------------------------------------
 10s
(1 row)
```

## Get Rid of Staled Connections

```sql
select clock_timestamp(), datname, pid, state, application_name, state_change from pg_stat_activity;

SELECT pg_terminate_backend(pid)
    FROM pg_stat_activity
    WHERE datname = 'regress'
      AND pid <> pg_backend_pid()
      AND state in ('idle', 'idle in transaction', 'idle in transaction (aborted)', 'disabled')
      AND application_name !~ '(?:psql)|(?:pgAdmin.+)'
      AND state_change < current_timestamp - INTERVAL '5' MINUTE;

select statement_timestamp(), datname, pid, state, application_name, state_change from pg_stat_activity;

select clock_timestamp(), datname, pid, state, application_name, state_change from pg_stat_activity;
```

## Reference

+ [HikariCP Configurations](https://github.com/brettwooldridge/HikariCP#gear-configuration-knobs-baby)
+ [HikariCP Latest Release](https://github.com/brettwooldridge/HikariCP/releases/latest)
+ [Down the Rabbit Hole](https://github.com/brettwooldridge/HikariCP/wiki/Down-the-Rabbit-Hole)
+ [About Pool Sizing](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing)
+ [Dropwizard Metrics](https://github.com/brettwooldridge/HikariCP/wiki/Dropwizard-Metrics)
+ [Dropwizard HealthChecks](https://github.com/brettwooldridge/HikariCP/wiki/Dropwizard-HealthChecks)
+ [MBean (JMX) Monitoring and Management](https://github.com/brettwooldridge/HikariCP/wiki/MBean-(JMX)-Monitoring-and-Management)
+ [PostgreSQL 12 Connection Configurations](https://www.postgresql.org/docs/12/runtime-config-connection.html)
+ [Spring Boot Application Dependencies ](https://docs.spring.io/spring-boot/docs/current/reference/html/dependency-versions.html#dependency-versions)
+ [Spring Boot Application Properties - spring.datasource.hikari.*](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties)
