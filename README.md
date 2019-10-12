# opensuse-loki

Minimal openSUSE documentation for running loki grafana

https://github.com/grafana/loki#loki-like-prometheus-but-for-logs


# Howto:

### 1) install grafana and prometheus on a vm  or your system

You can get the rpm via this repo:
(`https://build.opensuse.org/package/show/server:monitoring`)

### 2) Install loki and promtail from rpms packages from repo:
`https://build.opensuse.org/package/show/server:monitoring/loki`

Add the repository for your system and install the pkg
`zypper in loki`


### 3) Activate systemd services:

In this example we assume for simplicity that promtail run in same machine like loki and grafana. ( this can be changed)


`systemctl start loki`

`systemctl enable loki`


    loki is the main server, responsible for storing logs and processing queries.
___
`systemctl start promtail`

`systemctl enable promtail`


    promtail is the agent, responsible for gathering logs and sending them to Loki.

___
### 4) Adapt config files:


`/etc/loki/loki.yaml`

and

`/etc/loki/promtail.yaml`

By defaul the promtail.yaml config file should monitor the logs file which is already a valid example to get started


For more info visit: 
https://github.com/grafana/loki#loki-like-prometheus-but-for-logs

