# opensuse-loki

Minimal openSUSE documentation for running loki grafana

https://github.com/grafana/loki#loki-like-prometheus-but-for-logs


# Howto:

1) install grafana and prometheus on a vm  or your system


2) Install loki https://github.com/grafana/loki/blob/master/docs/installation/local.md

3) Create config file with:

`mkdir /etc/loki`

`touch /etc/loki/local-config.yaml`

paste following content into /etc/loki/local-config.yaml
```
auth_enabled: false

server:
  http_listen_port: 3100

ingester:
  lifecycler:
    address: 127.0.0.1
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1
    final_sleep: 0s
  chunk_idle_period: 5m
  chunk_retain_period: 30s

schema_config:
  configs:
  - from: 2018-04-15
    store: boltdb
    object_store: filesystem
    schema: v9
    index:
      prefix: index_
      period: 168h

storage_config:
  boltdb:
    directory: /tmp/loki/index

  filesystem:
    directory: /tmp/loki/chunks

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h

chunk_store_config:
  max_look_back_period: 0

table_manager:
  chunk_tables_provisioning:
    inactive_read_throughput: 0
    inactive_write_throughput: 0
    provisioned_read_throughput: 0
    provisioned_write_throughput: 0
  index_tables_provisioning:
    inactive_read_throughput: 0
    inactive_write_throughput: 0
    provisioned_read_throughput: 0
    provisioned_write_throughput: 0
  retention_deletes_enabled: false
  retention_period: 0
```


4) Run loki
`loki  -config.file=/etc/loki/local-config.yaml`


## Configure promtail

1) install https://github.com/grafana/loki/blob/master/docs/clients/promtail/installation.md


2) `mkdir /etc/promtail`
   create a file like `/etc/promtail/job0.yaml`
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://localhost:3100/api/prom/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log

```

run with ` promtail -config.file /etc/promtail/job0.yaml`

(I assume both are on same host)


