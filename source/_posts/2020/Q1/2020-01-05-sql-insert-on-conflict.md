---
title: SQL insert on conflict
excerpt: SQL insert on conflict
date: 2020-01-05 20:48:13
tags:
  - Programming
  - SQL
categories: [Programming, SQL]
---

# SQL insert on conflict

## PostgreSQL

- https://www.postgresql.org/docs/current/static/sql-insert.html

```sql
INSERT INTO MYBAAS(ID, TYPE_NAME, JSON)
VALUES(?, ?, ?)
ON CONFLICT(ID) DO UPDATE SET TYPE_NAME=?, JSON=?

INSERT INTO MYBAAS(ID, TYPE_NAME, JSON)
VALUES(?, ?, ?)
ON CONFLICT DO NOTHING;
```

## SQL Server

- https://docs.microsoft.com/en-us/sql/t-sql/statements/create-table-transact-sql

```sql
CREATE TABLE MYBAAS (
        ID          VARCHAR(64) NOT NULL,
        TYPE_NAME   VARCHAR(64) NOT NULL,
        JSON        LONGTEXT NOT NULL,
        CONSTRAINT MYBAAS_PK PRIMARY KEY (ID) WITH (IGNORE_DUP_KEY=ON)
);

CREATE TABLE IF NOT EXISTS PROPERTY_MAP (
        ID              VARCHAR(64) NOT NULL,
        PROPERTY_NAME   VARCHAR(64) NOT NULL,
        COLUMN_NAME     VARCHAR(64) NOT NULL,
        TYPE_NAME       VARCHAR(64) NOT NULL,
        CONSTRAINT PROPERTY_MAP_PK PRIMARY KEY (ID)
);

CREATE UNIQUE INDEX PROPERTY_MAP_UK01
ON PROPERTY_MAP(PROPERTY_NAME) WITH (IGNORE_DUP_KEY = ON);

CREATE UNIQUE INDEX PROPERTY_MAP_UK02
ON PROPERTY_MAP(COLUMN_NAME) WITH (IGNORE_DUP_KEY = ON);
```

## SQLite

- https://sqlite.org/lang_insert.html
- https://sqlite.org/lang_conflict.html

```sql
INSERT OR REPLACE INTO MYBAAS(ID, TYPE_NAME, JSON)
VALUES(?, ?, ?);
```

## MariaDB/MySQL

- https://mariadb.com/kb/en/library/insert-on-duplicate-key-update/
- https://mariadb.com/kb/en/library/insert-ignore/
- https://mariadb.com/kb/en/library/values/

```sql
INSERT INTO MYBAAS(ID, TYPE_NAME, JSON)
VALUES(?, ?, ?)
ON DUPLICATE KEY UPDATE TYPE_NAME=?, JSON=?
```

## Oracle

- https://docs.oracle.com/database/122/SQLRF/MERGE.htm
- http://download.oracle.com/docs/cds/database/122.zip

```sql
MERGE INTO MYBAAS D
    USING (SELECT ? AS ID, ? AS TYPE_NAME, ? AS JSON FROM DUAL) S
    ON (D.ID = S.ID)
    WHEN MATCHED THEN
        UPDATE SET D.TYPE_NAME = S.TYPE_NAME
    WHEN NOT MATCHED THEN
        INSERT (D.ID, D.TYPE_NAME, D.JSON)
        VALUES (S.ID, S.TYPE_NAME, S.JSON);
```

## DB2

```sql
MERGE INTO MYBAAS AS D
    USING (VALUES(?, ?, ?)) AS S(ID, TYPE_NAME, JSON)
    ON D.ID = S.ID
    WHEN MATCHED THEN
        UPDATE SET D.TYPE_NAME = S.TYPE_NAME, D.JSON = S.JSON
    WHEN NOT MATCHED THEN
        INSERT (ID, TYPE_NAME, JSON) VALUES (S.ID, S.TYPE_NAME, S.JSON);
```
