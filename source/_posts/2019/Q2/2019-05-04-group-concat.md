---
title: MySQL Aggregate Function GROUP_CONCAT()
description: MySQL Aggregate Function GROUP_CONCAT()
date: 2019-05-04 14:00:31
tags:
  - Programming
  - SQL
categories: [Programming, SQL]
permalink: group-concat
---

# Group concat

The [MySQL GROUP_CONCAT() function](https://mariadb.com/kb/en/library/group_concat/) is an aggregate function that concatenates strings from a group into a single string with various options.

## MySQL

```sql
select parent_name, group_concat(child_name)
from children
group by parent_name;

select parent_name,
    group_concat(child_name order by child_name asc separator ';')
from children
group by parent_name;
```

## PostgreSQL

```sql
select parent_name,
    array_to_string(array_agg(child_name), ',')
from children
group by parent_name;

select parent_name,
    array_to_string(array_agg(child_name order by child_name ASC), ',')
from children
group by parent_name;

SELECT a.farm_id,
    string_agg(DISTINCT a.name, ',') as animals,
    string_agg(DISTINCT t.name, ',') as tools
FROM
    animal a, tool t
WHERE a.farm_id = t.farm_id
GROUP BY a.farm_id;

FARM_ID    ANIMALS        TOOLS
1          cow,horse      axe,hammer,spade
```

## Oracle

```sql
SELECT parent_name,
    LISTAGG(child_name, ',') WITHIN GROUP (ORDER BY child_name)
FROM children
GROUP BY parent_name;

CREATE OR REPLACE
FUNCTION get_comma_separated_value (input_val in number)
  RETURN VARCHAR2
IS
  return_text  VARCHAR2(10000) := NULL;
BEGIN
  FOR x IN (SELECT col2 FROM table_name WHERE col1 = input_val) LOOP
    return_text := return_text || ',' || x.col2 ;
  END LOOP;
  RETURN LTRIM(return_text, ',');
END;

select parent_name, get_comma_separated_value(child_name)
from children;
```

## SQLite

```sql
select parent_name, group_concat(child_name)
from children group by parent_name;

select parent_name, group_concat(child_name, ';')
from children group by parent_name;

select parent_name, group_concat(child_name)
from
  (select parent_name, child_name from children order by child_name)
group by parent_name;
```

## HSQLDB

```sql
select parent_name, group_concat(child_name)
from children
group by parent_name;

select parent_name,
  group_concat(child_name order by child_name SEPARATOR ';')
from children
group by parent_name;
```
