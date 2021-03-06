ansible-role-telemetry-server
=============================

The monitoring service keeps track of all available and used system resources,
allowing us to spot bottlenecks, memory leaks and errors early. It is configured
to display all the data in a convenient web interface for easy access, and
reports once certain values exceed predefined thresholds.

It consists of three seperate services: the Data collector, time series database
and graphical frontend.

Requirements
------------

None

Role Variables
--------------

```yaml
monitoring_domain: "monitoring.example.com"
grafana_url: "https://{{ monitoring_domain }}"

monitoring_admin_name: "admin"
monitoring_admin_password: "changeme"

monitoring_agent_name: "agent"
monitoring_agent_password: "changeme"

grafana_admin_name: "{{ monitoring_admin_name }}"
grafana_admin_password: "{{ monitoring_admin_password }}"
```


Time series database
--------------------

The time series database is responsible for storing all collected metrics for
each host, indexed by hostname, metric name, metric type and timestamp. We are
using [InfluxDB](https://github.com/influxdata/influxdb) for this purpose, since
it specialises on time-series data unlike competing products like PostgreSQL and
Elasticsearch, while being easier to setup and maintain than Prometheus. Other
feature of the time series database include the querying of the data, deletion
of old datasets, real time transformation of the data (such as integrals,
differentials, sums) etc.

Graphical Frontend
------------------

The graphical Frontend is used to display the collected metrics, and allow the
user to build queries to retrieve data in a specialized fashion. We are using
[Grafana](https://github.com/grafana/grafana) for this. Its main purpose is
drawing graphs, but it also deals with rights management by prodviding a login
iterface. Also it offers the option to define Alarms on certain thresholds e.g.
System Load or Hard disk usage.

To set up the dashboard in Grafana, the InfluxDB database needs to be added first. On initial run Grafana will prompt you for a datasource. You can enter the following information:

```
Name: [Any]
Default: true
URL: http://influxdb:8086
Access: Server

Influx DB Database: telegraf
Influx DB User: [monitoring_admin_name]
Influx DB Password: [monitoring_admin_password]

Min Time Interval: 10s
```

Dependencies
------------

None

License
-------

MIT
