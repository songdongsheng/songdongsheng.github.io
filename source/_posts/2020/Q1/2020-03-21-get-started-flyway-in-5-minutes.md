---
title: Get Started Flyway in 5 minutes
description: Get Started Flyway in 5 minutes
date: 2020-03-21 12:46:25
tags:
  - SQL
  - Programming
categories: [Programming, SQL]
permalink: get-started-flyway-in-5-minutes
---

# Get Started Flyway in 5 minutes

- install the Flyway Command-line tool
- configure it so it can talk to our database
- write our first couple of migrations

## Downloading and extracting Flyway

- https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/maven-metadata.xml
- https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/6.3.2/
- https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/6.3.2/flyway-commandline-6.3.2-windows-x64.zip
- https://mirrors.huaweicloud.com/repository/maven/org/flywaydb/flyway-commandline/6.3.2/flyway-commandline-6.3.2-windows-x64.zip

## Prepare the database

```sql
-- DROP ROLE pcc;
CREATE ROLE pcc LOGIN PASSWORD 'pcc'
     SUPERUSER INHERIT CREATEDB CREATEROLE REPLICATION;

-- DROP DATABASE pcc;
CREATE DATABASE pcc
  WITH OWNER = pcc
       ENCODING = 'UTF-8';
```

## Configuring Flyway

vi conf/flyway.conf

```
# single-schema mode
flyway.url=jdbc:postgresql://localhost:5432/pcc
flyway.user=pcc
flyway.password=pcc
```

## Creating the first migration

vi sql/V1__Create_person_table.sql

```sql
CREATE TABLE person (
    id integer NOT NULL,
    name varchar(100) NOT NULL,
    PRIMARY KEY (id)
);
```

## Migrating the database

```bash
# flyway migrate

Flyway Community Edition 6.3.2 by Redgate
Database: jdbc:postgresql://localhost:5432/pcc (PostgreSQL 12.1)
Successfully validated 1 migration (execution time 00:00.022s)
Creating Schema History table "public"."flyway_schema_history" ...
Current version of schema "public": << Empty Schema >>
Migrating schema "public" to version 1 - Create person table
Successfully applied 1 migration to schema "public" (execution time 00:00.068s)
```

## Adding a second migration

vi sql/V2__Add_people.sql

```sql
INSERT INTO person (id, name) VALUES (1, 'Axel');
INSERT INTO person (id, name) VALUES (2, 'Mr. Foo');
INSERT INTO person (id, name) VALUES (3, 'Ms. Bar');
```

## Migrating the database again

```bash
# flyway migrate
Flyway Community Edition 6.3.2 by Redgate
Database: jdbc:postgresql://localhost:5432/pcc (PostgreSQL 12.1)
Successfully validated 2 migrations (execution time 00:00.065s)
Current version of schema "public": 1
Migrating schema "public" to version 2 - Add people
Successfully applied 1 migration to schema "public" (execution time 00:00.106s)
```

## Flyway schema history table

```shell
# \d flyway_schema_history
                     Table "public.flyway_schema_history"
     Column     |            Type             | Collation | Nullable | Default
----------------+-----------------------------+-----------+----------+---------
 installed_rank | integer                     |           | not null |
 version        | character varying(50)       |           |          |
 description    | character varying(200)      |           | not null |
 type           | character varying(20)       |           | not null |
 script         | character varying(1000)     |           | not null |
 checksum       | integer                     |           |          |
 installed_by   | character varying(100)      |           | not null |
 installed_on   | timestamp without time zone |           | not null | now()
 execution_time | integer                     |           | not null |
 success        | boolean                     |           | not null |
Indexes:
    "flyway_schema_history_pk" PRIMARY KEY, btree (installed_rank)
    "flyway_schema_history_s_idx" btree (success)
```

```sql
CREATE TABLE flyway_schema_history
(
    installed_rank integer NOT NULL,
    version character varying(50),
    description character varying(200) NOT NULL,
    type character varying(20) NOT NULL,
    script character varying(1000) NOT NULL,
    checksum integer,
    installed_by character varying(100) NOT NULL,
    installed_on timestamp without time zone NOT NULL DEFAULT now(),
    execution_time integer NOT NULL,
    success boolean NOT NULL,
    CONSTRAINT flyway_schema_history_pk PRIMARY KEY (installed_rank)
);

-- DROP INDEX flyway_schema_history_s_idx;
CREATE INDEX flyway_schema_history_s_idx
    ON flyway_schema_history USING btree
    (success ASC NULLS LAST);
```
