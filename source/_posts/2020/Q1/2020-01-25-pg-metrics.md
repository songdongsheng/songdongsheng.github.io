---
title: pg metrics
excerpt: pg metrics
date: 2020-01-25 17:26:41
tags:
  - Programming
  - SQL
categories: [Programming, SQL]
---

# pgmetrics

- https://pgmetrics.io/
- https://pgmetrics.io/docs/
- https://github.com/rapidloop/pgmetrics

pgmetrics is an open-source, zero-dependency, single-binary tool that can collect a lot of information and statistics from a running PostgreSQL server and display it in easy-to-read text format or export it as JSON and CSV for scripting.

- Subsystem Info
- Database Info
- Table Info
- Log Files
- Easy-to-use CLI
- Zero Dependency

## Sample output

```
pgmetrics run at: 20 Dec 2019 6:41:13 AM (now)

PostgreSQL Cluster:
    Name:                10/main
    Server Version:      10.10
    Server Started:      20 Dec 2019 6:32:35 AM (8 minutes ago)
    System Identifier:   6772342791073124390
    Timeline:            1
    Last Checkpoint:     20 Dec 2019 6:32:15 AM (8 minutes ago)
    Prior LSN:           0/1692308
    REDO LSN:            0/16923B0 (168 B since Prior)
    Checkpoint LSN:      0/16923B0 (0 B since REDO)
    Transaction IDs:     548 to 563 (diff = 15)
    Notification Queue:  0.0% used
    Active Backends:     1 (max 1000)
    Recovery Mode?       no

WAL Files:
    WAL Archiving?       no
    +--------------------+---------------+
    |            Setting |         Value |
    +--------------------+---------------+
    |          wal_level |       logical |
    |    archive_timeout |             0 |
    |    wal_compression |            on |
    |       max_wal_size | 2048 (32 GiB) |
    |       min_wal_size |  64 (1.0 GiB) |
    | checkpoint_timeout |           600 |
    |   full_page_writes |            on |
    |  wal_keep_segments |             8 |
    +--------------------+---------------+

BG Writer:
    Checkpoint Rate:     0.12 per min
    Average Write:       8.8 KiB per checkpoint
    Total Checkpoints:   24 sched (80.0%) + 6 req (20.0%) = 30
    Total Write:         264 KiB, @ 17 B per sec
    Buffers Allocated:   651 (5.1 MiB)
    Buffers Written:     33 chkpt (100.0%) + 0 bgw (0.0%) + 0 be (0.0%)
    Clean Scan Stops:    0
    BE fsyncs:           0
    Counts Since:        20 Dec 2019 2:28:52 AM (4 hours ago)
    +------------------------------+--------------+
    |                      Setting |        Value |
    +------------------------------+--------------+
    |               bgwriter_delay |     200 msec |
    |         bgwriter_flush_after | 64 (512 KiB) |
    |        bgwriter_lru_maxpages |          100 |
    |      bgwriter_lru_multiplier |            2 |
    |                   block_size |         8192 |
    |           checkpoint_timeout |      600 sec |
    | checkpoint_completion_target |         0.96 |
    +------------------------------+--------------+

Backends:
    Total Backends:      1 (0.1% of max 1000)
    Problematic:         0 waiting on locks, 0 waiting on other, 0 xact too long, 0 idle in xact

Locks:
    +------------+-------------+-------+
    |  Lock Type | Not Granted | Total |
    +------------+-------------+-------+
    |   relation |           0 |     4 |
    | virtualxid |           0 |     1 |
    +------------+-------------+-------+
    |            |           0 |     5 |
    +------------+-------------+-------+

Vacuum Progress:
    No manual or auto vacuum jobs in progress.
    +------------------------------+----------------+
    |                      Setting |          Value |
    +------------------------------+----------------+
    |         maintenance_work_mem | 65536 (64 MiB) |
    |                   autovacuum |             on |
    | autovacuum_analyze_threshold |             50 |
    |  autovacuum_vacuum_threshold |             50 |
    |    autovacuum_freeze_max_age |      200000000 |
    |       autovacuum_max_workers |              3 |
    |           autovacuum_naptime |         60 sec |
    |        vacuum_freeze_min_age |       50000000 |
    |      vacuum_freeze_table_age |      150000000 |
    +------------------------------+----------------+

Roles:
    +----------------------+-------+------+-------+-----------+----------+------------+---------+---------+--------------------------------------------------------------+
    |                 Name | Login | Repl | Super | Creat Rol | Creat DB | Bypass RLS | Inherit | Expires |                                                    Member Of |
    +----------------------+-------+------+-------+-----------+----------+------------+---------+---------+--------------------------------------------------------------+
    |             postgres |   yes |  yes |   yes |       yes |      yes |        yes |     yes |         |                                                              |
    |           pg_monitor |       |      |       |           |          |            |     yes |         | pg_read_all_settings, pg_read_all_stats, pg_stat_scan_tables |
    | pg_read_all_settings |       |      |       |           |          |            |     yes |         |                                                              |
    |    pg_read_all_stats |       |      |       |           |          |            |     yes |         |                                                              |
    |  pg_stat_scan_tables |       |      |       |           |          |            |     yes |         |                                                              |
    |    pg_signal_backend |       |      |       |           |          |            |     yes |         |                                                              |
    |            collector |   yes |      |       |           |          |            |     yes |         |                                                              |
    |           replicator |   yes |      |       |           |          |            |     yes |         |                                                              |
    |                  idg |   yes |      |       |           |          |            |     yes |         |                                                              |
    +----------------------+-------+------+-------+-----------+----------+------------+---------+---------+--------------------------------------------------------------+

Tablespaces:
    +------------+----------+----------+--------+
    |       Name |    Owner | Location |   Size |
    +------------+----------+----------+--------+
    | pg_default | postgres |          | 45 MiB |
    |  pg_global | postgres |          |        |
    +------------+----------+----------+--------+

Database #1:
    Name:                postgres
    Owner:               postgres
    Tablespace:          pg_default
    Connections:         0 (no max limit)
    Frozen Xid Age:      16
    Transactions:        2014 (100.0%) commits, 0 (0.0%) rollbacks
    Cache Hits:          99.6%
    Rows Changed:        ins 0.0%, upd 0.0%, del 0.0%
    Total Temp:          0 B in 0 files
    Problems:            0 deadlocks, 0 conflicts
    Totals Since:        20 Dec 2019 2:28:53 AM (4 hours ago)
    Size:                7.6 MiB

Database #2:
    Name:                collector
    Owner:               collector
    Tablespace:          pg_default
    Connections:         0 (no max limit)
    Frozen Xid Age:      16
    Transactions:        0 (0.0%) commits, 0 (0.0%) rollbacks
    Cache Hits:          0.0%
    Rows Changed:        ins 0.0%, upd 0.0%, del 0.0%
    Total Temp:          0 B in 0 files
    Problems:            0 deadlocks, 0 conflicts
    Totals Since:
    Size:                7.5 MiB

Database #3:
    Name:                replicator
    Owner:               replicator
    Tablespace:          pg_default
    Connections:         0 (no max limit)
    Frozen Xid Age:      16
    Transactions:        0 (0.0%) commits, 0 (0.0%) rollbacks
    Cache Hits:          0.0%
    Rows Changed:        ins 0.0%, upd 0.0%, del 0.0%
    Total Temp:          0 B in 0 files
    Problems:            0 deadlocks, 0 conflicts
    Totals Since:
    Size:                7.5 MiB

Database #4:
    Name:                idg
    Owner:               idg
    Tablespace:          pg_default
    Connections:         1 (no max limit)
    Frozen Xid Age:      16
    Transactions:        604 (98.1%) commits, 12 (1.9%) rollbacks
    Cache Hits:          98.4%
    Rows Changed:        ins 92.0%, upd 8.0%, del 0.0%
    Total Temp:          0 B in 0 files
    Problems:            0 deadlocks, 0 conflicts
    Totals Since:        20 Dec 2019 2:28:54 AM (4 hours ago)
    Size:                7.7 MiB
    Installed Extensions:
      +---------+---------+------------------------------+
      |    Name | Version |                      Comment |
      +---------+---------+------------------------------+
      | plpgsql |     1.0 | PL/pgSQL procedural language |
      +---------+---------+------------------------------+

Table #1 in "idg":
    Name:                idg.public.idg_state
    Columns:             3
    Manual Vacuums:      never
    Manual Analyze:      never
    Auto Vacuums:        never
    Auto Analyze:        never
    Post-Analyze:        0.0% est. rows modified
    Row Estimate:        0.0% live of total 0
    Rows Changed:        ins 0.0%, upd 0.0%, del 0.0%
    HOT Updates:         0.0% of all updates
    Seq Scans:           1, 0.0 rows/scan
    Idx Scans:           0, 0.0 rows/scan
    Cache Hits:          0.0% (idx=0.0%)
    Size:                0 B
    +----------------+-------+---------+-------+------------+-------+----------------+-------------------+
    |          Index |  Type |    Size | Bloat | Cache Hits | Scans | Rows Read/Scan | Rows Fetched/Scan |
    +----------------+-------+---------+-------+------------+-------+----------------+-------------------+
    | idg_state_pkey | btree | 8.0 KiB |       |            |     0 |            0.0 |               0.0 |
    +----------------+-------+---------+-------+------------+-------+----------------+-------------------+
```
