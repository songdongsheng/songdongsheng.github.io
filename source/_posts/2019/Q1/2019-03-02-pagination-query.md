---
title: Pagination query
description: Pagination query
date: 2019-03-02 19:27:18
tags:
  - Programming
  - SQL
categories: [Programming, SQL]
permalink: pagination-query
---

# Pagination query

Paging queries consume lot's of resources and are not scalable very well, should be avoided.

## Oracle

    SELECT ID, JSON FROM (
        SELECT ROWNUM RN, ID, JSON FROM MY_BASS ORDER BY ID ASC
    ) AS T WHERE
    RN BETWEEN ((@PageNumber - 1) * @RowspPage + 1) AND (@PageNumber * @RowspPage);

## PostgreSQL and MySQL

- https://www.postgresql.org/docs/10/static/queries-limit.html
- https://dev.mysql.com/doc/refman/5.7/en/select.html


    SELECT ID, JSON FROM MY_BASS ORDER BY ID
    LIMIT @RowspPage OFFSET (@PageNumber - 1) * @RowspPage);

## SQL Server (2012 +)

- https://social.technet.microsoft.com/wiki/contents/articles/23811.paging-a-query-with-sql-server.aspx
- http://www.mssqlgirl.com/paging-function-performance-in-sql-server-2012.html


    SELECT ID, JSON FROM MY_BASS ORDER BY ID
    OFFSET ((@PageNumber - 1) * @RowspPage) ROWS
    FETCH NEXT @RowspPage ROWS ONLY;

## DB2 (DB2 9.7.4 +)

- https://www.ibm.com/developerworks/data/library/techarticle/dm-1006db2expressc972/dm-1006db2expressc972-pdf.pdf
- https://www.itjungle.com/2016/01/12/fhg011216-story03/


    SELECT ID, JSON FROM MY_BASS ORDER BY ID
    LIMIT @RowspPage OFFSET (@PageNumber - 1) * @RowspPage);
