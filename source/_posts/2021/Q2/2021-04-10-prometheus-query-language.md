---
title: Prometheus Query Language
description: Prometheus Query Language
date: 2021-04-10 16:06:43
tags:
    - Cloud
    - Kubernetes
categories: [Cloud, Kubernetes]
permalink: prometheus-query-language
---

# Prometheus Query Language

## Querying Prometheus reference

- [Querying Prometheus](https://prometheus.io/docs/prometheus/latest/querying/basics/)
- [Prometheus Operators](https://prometheus.io/docs/prometheus/latest/querying/operators/#vector-matching)
- [Prometheus Functions](https://prometheus.io/docs/prometheus/latest/querying/functions/)
- [Prometheus HTTP API](https://prometheus.io/docs/prometheus/latest/querying/api/)
- [Prometheus data model](https://prometheus.io/docs/concepts/data_model/)
- [Prometheus metric and label naming](https://prometheus.io/docs/practices/naming/)
- [Prometheus Recording rules](https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/)
- [Prometheus Security Model](https://prometheus.io/docs/operating/security/)
- [Prometheus regular expressions - RE2 syntax](https://github.com/google/re2/wiki/Syntax)
- [PromQL tutorial for beginners and humans](https://valyala.medium.com/promql-tutorial-for-beginners-9ab455142085)
- [PromQL Cheat Sheet](https://promlabs.com/promql-cheat-sheet/)
- [PromLens - The power tool for querying Prometheus](https://promlens.com/)

## Querying Prometheus status

### Querying Prometheus config

```shell
$ curl -sSL http://localhost:9090/api/v1/status/config | jq .
{
  "status": "success",
  "data": {
    "yaml": "global:\n  scrape_interval: 15s\n  scrape_timeout: 10s\n  evaluation_interval: 15s\n  query_log_file: query_log.txt\nscrape_configs:\n- job_name: prometheus-localhost-9090\n  honor_timestamps: true\n  scrape_interval: 15s\n  scrape_timeout: 10s\n  metrics_path: /metrics\n  scheme: http\n  static_configs:\n  - targets:\n    - localhost:9090\n- job_name: simulator-localhost-8181\n  honor_timestamps: true\n  scrape_interval: 15s\n  scrape_timeout: 10s\n  metrics_path: /metrics\n  scheme: http\n  static_configs:\n  - targets:\n    - localhost:8181\n"
  }
}
```

### Querying Prometheus flags

```shell
$ curl -sSL http://localhost:9090/api/v1/status/flags | jq .
{
  "status": "success",
  "data": {
    "alertmanager.notification-queue-capacity": "10000",
    "alertmanager.timeout": "",
    "config.file": "prometheus.yml",
    "enable-feature": "",
    "log.format": "logfmt",
    "log.level": "info",
    "query.lookback-delta": "5m",
    "query.max-concurrency": "20",
    "query.max-samples": "50000000",
    "query.timeout": "2m",
    "rules.alert.for-grace-period": "10m",
    "rules.alert.for-outage-tolerance": "1h",
    "rules.alert.resend-delay": "1m",
    "scrape.adjust-timestamps": "true",
    "storage.remote.flush-deadline": "1m",
    "storage.remote.read-concurrent-limit": "10",
    "storage.remote.read-max-bytes-in-frame": "1048576",
    "storage.remote.read-sample-limit": "50000000",
    "storage.tsdb.allow-overlapping-blocks": "false",
    "storage.tsdb.max-block-duration": "6m",
    "storage.tsdb.min-block-duration": "2h",
    "storage.tsdb.no-lockfile": "false",
    "storage.tsdb.path": "data/",
    "storage.tsdb.retention": "0s",
    "storage.tsdb.retention.size": "0B",
    "storage.tsdb.retention.time": "1h",
    "storage.tsdb.wal-compression": "true",
    "storage.tsdb.wal-segment-size": "0B",
    "web.config.file": "",
    "web.console.libraries": "console_libraries",
    "web.console.templates": "consoles",
    "web.cors.origin": ".*",
    "web.enable-admin-api": "true",
    "web.enable-lifecycle": "false",
    "web.external-url": "",
    "web.listen-address": "0.0.0.0:9090",
    "web.max-connections": "512",
    "web.page-title": "Prometheus Time Series Collection and Processing Server",
    "web.read-timeout": "5m",
    "web.route-prefix": "/",
    "web.user-assets": ""
  }
}
```

### Querying Prometheus runtime information

```shell
$ curl -sSL http://localhost:9090/api/v1/status/runtimeinfo | jq .
{
  "status": "success",
  "data": {
    "startTime": "2021-04-10T03:11:30.8916173Z",
    "CWD": "C:\\opt\\prometheus",
    "reloadConfigSuccess": true,
    "lastConfigTime": "2021-04-10T03:11:31Z",
    "corruptionCount": 0,
    "goroutineCount": 30,
    "GOMAXPROCS": 8,
    "GOGC": "",
    "GODEBUG": "",
    "storageRetention": "1h"
  }
}
```

### Querying Prometheus build information

```shell
$ curl -sSL http://localhost:9090/api/v1/status/buildinfo | jq .
{
  "status": "success",
  "data": {
    "version": "2.25.2",
    "revision": "bda05a23ada314a0b9806a362da39b7a1a4e04c3",
    "branch": "HEAD",
    "buildUser": "root@de38ec01ef10",
    "buildDate": "20210316-18:20:38",
    "goVersion": "go1.15.10"
  }
}
```

### Querying Prometheus TSDB Stats

```shell
$ curl -sSL http://localhost:9090/api/v1/status/tsdb | jq .
{
  "status": "success",
  "data": {
    "headStats": {
      "numSeries": 635,
      "numLabelPairs": 398,
      "chunkCount": 1656,
      "minTime": 1618024298730,
      "maxTime": 1618030645198
    }
  }
}
```

### Clean tombstones of Prometheus

```shell
$ curl -sSL -X POST http://localhost:9090/api/v1/admin/tsdb/clean_tombstones | jq .
{
  "status": "error",
  "errorType": "unavailable",
  "error": "admin APIs disabled"
}

# prometheus --storage.tsdb.retention.time=1h --web.enable-admin-api
$ curl -i -sSL -X POST http://localhost:9090/api/v1/admin/tsdb/clean_tombstones
HTTP/1.1 204 No Content
Date: Sat, 10 Apr 2021 05:03:29 GMT
```

## Querying metric metadata

### List metrics

```shell
$ curl -sSL http://localhost:9090/api/v1/metadata?limit=3 | jq .
{
  "status": "success",
  "data": {
    "go_goroutines": [
      {
        "type": "gauge",
        "help": "Number of goroutines that currently exist.",
        "unit": ""
      }
    ],
    "go_memstats_mallocs_total": [
      {
        "type": "counter",
        "help": "Total number of mallocs.",
        "unit": ""
      }
    ],
    "prometheus_tsdb_head_series": [
      {
        "type": "gauge",
        "help": "Total number of series in the head block.",
        "unit": ""
      }
    ]
  }
}
```

### List metrics - jq

```shell
$ curl -sSL http://localhost:9090/api/v1/metadata | jq '.data.system_cpu_usage'
[
  {
    "type": "gauge",
    "help": "The \"recent cpu usage\" for the whole system",
    "unit": ""
  }
]
```

### List metrics - keys

```shell
$ curl -sSL http://localhost:9090/api/v1/metadata | jq '.data | to_entries | .[].key' | tr -d '"'
api_north_requests_recv_total
connected_device_gauge
go_gc_duration_seconds
go_goroutines
go_info
go_memstats_alloc_bytes
go_memstats_alloc_bytes_total
go_memstats_buck_hash_sys_bytes
go_memstats_frees_total
go_memstats_gc_cpu_fraction
go_memstats_gc_sys_bytes
go_memstats_heap_alloc_bytes
go_memstats_heap_idle_bytes
go_memstats_heap_inuse_bytes
go_memstats_heap_objects
go_memstats_heap_released_bytes
go_memstats_heap_sys_bytes
go_memstats_last_gc_time_seconds
go_memstats_lookups_total
go_memstats_mallocs_total
go_memstats_mcache_inuse_bytes
go_memstats_mcache_sys_bytes
go_memstats_mspan_inuse_bytes
go_memstats_mspan_sys_bytes
go_memstats_next_gc_bytes
go_memstats_other_sys_bytes
go_memstats_stack_inuse_bytes
go_memstats_stack_sys_bytes
go_memstats_sys_bytes
go_threads
http_server_requests_seconds
http_server_requests_seconds_max
jvm_buffer_count_buffers
jvm_buffer_memory_used_bytes
jvm_buffer_total_capacity_bytes
jvm_classes_loaded_classes
jvm_classes_unloaded_classes_total
jvm_gc_live_data_size_bytes
jvm_gc_max_data_size_bytes
jvm_gc_memory_allocated_bytes_total
jvm_gc_memory_promoted_bytes_total
jvm_memory_committed_bytes
jvm_memory_max_bytes
jvm_memory_used_bytes
jvm_threads_daemon_threads
jvm_threads_live_threads
jvm_threads_peak_threads
jvm_threads_states_threads
logback_events_total
net_conntrack_dialer_conn_attempted_total
net_conntrack_dialer_conn_closed_total
net_conntrack_dialer_conn_established_total
net_conntrack_dialer_conn_failed_total
net_conntrack_listener_conn_accepted_total
net_conntrack_listener_conn_closed_total
process_cpu_seconds_total
process_cpu_usage
process_max_fds
process_open_fds
process_resident_memory_bytes
process_start_time_seconds
process_uptime_seconds
process_virtual_memory_bytes
prometheus_api_remote_read_queries
prometheus_build_info
prometheus_config_last_reload_success_timestamp_seconds
prometheus_config_last_reload_successful
prometheus_engine_queries
prometheus_engine_queries_concurrent_max
prometheus_engine_query_duration_seconds
prometheus_engine_query_log_enabled
prometheus_engine_query_log_failures_total
prometheus_http_request_duration_seconds
prometheus_http_requests_total
prometheus_http_response_size_bytes
prometheus_notifications_alertmanagers_discovered
prometheus_notifications_dropped_total
prometheus_notifications_queue_capacity
prometheus_notifications_queue_length
prometheus_remote_storage_highest_timestamp_in_seconds
prometheus_remote_storage_samples_in_total
prometheus_remote_storage_string_interner_zero_reference_releases_total
prometheus_rule_evaluation_duration_seconds
prometheus_rule_group_duration_seconds
prometheus_sd_consul_rpc_duration_seconds
prometheus_sd_consul_rpc_failures_total
prometheus_sd_discovered_targets
prometheus_sd_dns_lookup_failures_total
prometheus_sd_dns_lookups_total
prometheus_sd_failed_configs
prometheus_sd_file_read_errors_total
prometheus_sd_file_scan_duration_seconds
prometheus_sd_kubernetes_events_total
prometheus_sd_received_updates_total
prometheus_sd_updates_total
prometheus_target_interval_length_seconds
prometheus_target_metadata_cache_bytes
prometheus_target_metadata_cache_entries
prometheus_target_scrape_pool_exceeded_target_limit_total
prometheus_target_scrape_pool_reloads_failed_total
prometheus_target_scrape_pool_reloads_total
prometheus_target_scrape_pool_sync_total
prometheus_target_scrape_pool_targets
prometheus_target_scrape_pools_failed_total
prometheus_target_scrape_pools_total
prometheus_target_scrapes_cache_flush_forced_total
prometheus_target_scrapes_exceeded_sample_limit_total
prometheus_target_scrapes_sample_duplicate_timestamp_total
prometheus_target_scrapes_sample_out_of_bounds_total
prometheus_target_scrapes_sample_out_of_order_total
prometheus_target_sync_length_seconds
prometheus_template_text_expansion_failures_total
prometheus_template_text_expansions_total
prometheus_treecache_watcher_goroutines
prometheus_treecache_zookeeper_failures_total
prometheus_tsdb_blocks_loaded
prometheus_tsdb_checkpoint_creations_failed_total
prometheus_tsdb_checkpoint_creations_total
prometheus_tsdb_checkpoint_deletions_failed_total
prometheus_tsdb_checkpoint_deletions_total
prometheus_tsdb_compaction_chunk_range_seconds
prometheus_tsdb_compaction_chunk_samples
prometheus_tsdb_compaction_chunk_size_bytes
prometheus_tsdb_compaction_duration_seconds
prometheus_tsdb_compaction_populating_block
prometheus_tsdb_compactions_failed_total
prometheus_tsdb_compactions_skipped_total
prometheus_tsdb_compactions_total
prometheus_tsdb_compactions_triggered_total
prometheus_tsdb_data_replay_duration_seconds
prometheus_tsdb_head_active_appenders
prometheus_tsdb_head_chunks
prometheus_tsdb_head_chunks_created_total
prometheus_tsdb_head_chunks_removed_total
prometheus_tsdb_head_gc_duration_seconds
prometheus_tsdb_head_max_time
prometheus_tsdb_head_max_time_seconds
prometheus_tsdb_head_min_time
prometheus_tsdb_head_min_time_seconds
prometheus_tsdb_head_samples_appended_total
prometheus_tsdb_head_series
prometheus_tsdb_head_series_created_total
prometheus_tsdb_head_series_not_found_total
prometheus_tsdb_head_series_removed_total
prometheus_tsdb_head_truncations_failed_total
prometheus_tsdb_head_truncations_total
prometheus_tsdb_isolation_high_watermark
prometheus_tsdb_isolation_low_watermark
prometheus_tsdb_lowest_timestamp
prometheus_tsdb_lowest_timestamp_seconds
prometheus_tsdb_mmap_chunk_corruptions_total
prometheus_tsdb_out_of_bound_samples_total
prometheus_tsdb_out_of_order_samples_total
prometheus_tsdb_reloads_failures_total
prometheus_tsdb_reloads_total
prometheus_tsdb_retention_limit_bytes
prometheus_tsdb_size_retentions_total
prometheus_tsdb_storage_blocks_bytes
prometheus_tsdb_symbol_table_size_bytes
prometheus_tsdb_time_retentions_total
prometheus_tsdb_tombstone_cleanup_seconds
prometheus_tsdb_vertical_compactions_total
prometheus_tsdb_wal_completed_pages_total
prometheus_tsdb_wal_corruptions_total
prometheus_tsdb_wal_fsync_duration_seconds
prometheus_tsdb_wal_page_flushes_total
prometheus_tsdb_wal_segment_current
prometheus_tsdb_wal_truncate_duration_seconds
prometheus_tsdb_wal_truncations_failed_total
prometheus_tsdb_wal_truncations_total
prometheus_tsdb_wal_writes_failed_total
prometheus_web_federation_errors_total
prometheus_web_federation_warnings_total
promhttp_metric_handler_requests_in_flight
promhttp_metric_handler_requests_total
system_cpu_count
system_cpu_usage
```

### List labels

```shell
$ curl -sSL 'http://localhost:9090/api/v1/labels' | jq .{
  "status": "success",
  "data": [
    "__name__",
    "api",
    "app",
    "area",
    "branch",
    "call",
    "code",
    "config",
    "dialer_name",
    "endpoint",
    "event",
    "exception",
    "goversion",
    "handler",
    "id",
    "instance",
    "interval",
    "job",
    "le",
    "level",
    "listener_name",
    "method",
    "name",
    "outcome",
    "quantile",
    "reason",
    "revision",
    "role",
    "scrape_job",
    "slice",
    "state",
    "status",
    "uri",
    "version"
  ]
}
```

### List values of label

```shell
$ curl -sSL http://localhost:9090/api/v1/label/api/values | jq .
{
  "status": "success",
  "data": [
    "afi-nbi",
    "afi-sbi",
    "bdt-nbi",
    "bdt-sbi",
    "cp-nbi",
    "cp-sbi",
    "ee-nbi",
    "ee-sbi",
    "qos-nbi",
    "qos-sbi"
  ]
}
```

```shell
$ curl -sSL http://localhost:9090/api/v1/label/app/values | jq .
{
  "status": "success",
  "data": [
    "afi",
    "bdt",
    "cp",
    "ee",
    "qos"
  ]
}
```

## Querying metric values

### Simple query

#### Counter

```shell
$ curl -sSL 'http://localhost:9090/api/v1/query?query=api_north_requests_recv_total' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "afi-nbi",
          "app": "afi",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031222.5,
          "1380800"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "bdt-nbi",
          "app": "bdt",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031222.5,
          "2071200"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "cp-nbi",
          "app": "cp",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031222.5,
          "2761600"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "ee-nbi",
          "app": "ee",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031222.5,
          "11046400"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "qos-nbi",
          "app": "qos",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031222.5,
          "3452000"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031222.5,
          "690400"
        ]
      }
    ]
  }
}
```

#### Gauge

```shell
$ curl -sSL 'http://localhost:9090/api/v1/query?query=connected_device_gauge' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "afi-sbi",
          "app": "afi",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031307.066,
          "200"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "bdt-sbi",
          "app": "bdt",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031307.066,
          "300"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "cp-sbi",
          "app": "cp",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031307.066,
          "400"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "ee-sbi",
          "app": "ee",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031307.066,
          "1600"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "qos-sbi",
          "app": "qos",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031307.066,
          "500"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031307.066,
          "100"
        ]
      }
    ]
  }
}
```


### Query by labels

#### Query Counter by labels

```shell
# api_north_requests_recv_total{api=~"bdt-nbi|cp-nbi|ee-nbi|qos-nbi|afi-nbi"}
$ curl -sSL 'http://localhost:9090/api/v1/query?query=api_north_requests_recv_total%7Bapi%3D~"bdt-nbi|cp-nbi|ee-nbi|qos-nbi|afi-nbi"%7D' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "afi-nbi",
          "app": "afi",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031396.263,
          "19200"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "bdt-nbi",
          "app": "bdt",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031396.263,
          "28800"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "cp-nbi",
          "app": "cp",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031396.263,
          "38400"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "ee-nbi",
          "app": "ee",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031396.263,
          "153600"
        ]
      },
      {
        "metric": {
          "__name__": "api_north_requests_recv_total",
          "api": "qos-nbi",
          "app": "qos",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031396.263,
          "48000"
        ]
      }
    ]
  }
}
```

#### Query Gauge by labels

```shell
# connected_device_gauge{api=~"bdt-sbi|cp-sbi|ee-sbi|qos-sbi|afi-sbi"}
$ curl -sSL 'http://localhost:9090/api/v1/query?query=connected_device_gauge%7Bapi%3D~"bdt-sbi|cp-sbi|ee-sbi|qos-sbi|afi-sbi"%7D' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "afi-sbi",
          "app": "afi",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031544.665,
          "200"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "bdt-sbi",
          "app": "bdt",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031544.665,
          "300"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "cp-sbi",
          "app": "cp",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031544.665,
          "400"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "ee-sbi",
          "app": "ee",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031544.665,
          "1600"
        ]
      },
      {
        "metric": {
          "__name__": "connected_device_gauge",
          "api": "qos-sbi",
          "app": "qos",
          "instance": "localhost:8181",
          "job": "simulator-localhost-8181"
        },
        "value": [
          1618031544.665,
          "500"
        ]
      }
    ]
  }
}
```

### Aggregation query

#### Aggregation of Counter

```shell
# sum(api_north_requests_recv_total{api=~"bdt-nbi|cp-nbi|ee-nbi|qos-nbi|afi-nbi"})
$ curl -sSL 'http://localhost:9090/api/v1/query?query=sum(api_north_requests_recv_total%7Bapi%3D~"bdt-nbi|cp-nbi|ee-nbi|qos-nbi|afi-nbi"%7D)' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618031709.761,
          "1188000"
        ]
      }
    ]
  }
}

# sum(api_north_requests_recv_total)
$ curl -sSL 'http://localhost:9090/api/v1/query?query=sum(api_north_requests_recv_total)' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618031709.873,
          "1227600"
        ]
      }
    ]
  }
}
```

#### Aggregation of Gauge

```shell
# sum(connected_device_gauge{api=~"bdt-sbi|cp-sbi|ee-sbi|qos-sbi|afi-sbi"})
$ curl -sSL 'http://localhost:9090/api/v1/query?query=sum(connected_device_gauge%7Bapi%3D~"bdt-sbi|cp-sbi|ee-sbi|qos-sbi|afi-sbi"%7D)' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618031937.161,
          "3000"
        ]
      }
    ]
  }
}

# sum(connected_device_gauge)
$ curl -sSL 'http://localhost:9090/api/v1/query?query=sum(connected_device_gauge)' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618031937.277,
          "3100"
        ]
      }
    ]
  }
}
```

### Rate query (Counter)

```shell
# sum(rate(api_north_requests_recv_total{api=~"bdt-nbi|cp-nbi|ee-nbi|qos-nbi|afi-nbi"}[60s]))
$ curl -sSL 'http://localhost:9090/api/v1/query?query=sum(rate(api_north_requests_recv_total%7Bapi%3D~"bdt-nbi|cp-nbi|ee-nbi|qos-nbi|afi-nbi"%7D%5B60s%5D))' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618032295.164,
          "3000.066668148181"
        ]
      }
    ]
  }
}

# sum(rate(api_north_requests_recv_total[60s]))
$ curl -sSL 'http://localhost:9090/api/v1/query?query=sum(rate(api_north_requests_recv_total%5B60s%5D))' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618032295.276,
          "3100.0688904197873"
        ]
      }
    ]
  }
}
```

### Rate subtraction query

```shell
# sum(rate(api_north_requests_recv_total{api="cp-nbi"}[60s])) - sum(rate(api_north_requests_recv_total{api="bdt-nbi"}[60s]))
curl -sSL 'http://localhost:9090/api/v1/query?query=sum%28rate%28api_north_requests_recv_total%7Bapi%3D%22cp-nbi%22%7D%5B60s%5D%29%29%20-%20sum%28rate%28api_north_requests_recv_total%7Bapi%3D%22bdt-nbi%22%7D%5B60s%5D%29%29' | jq .
{
  "status": "success",
  "data": {
    "resultType": "vector",
    "result": [
      {
        "metric": {},
        "value": [
          1618032478.932,
          "99.99555575307767"
        ]
      }
    ]
  }
}
```
