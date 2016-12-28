+++
title = "Analytics"
date = "2016-12-22T20:00:12-08:00"
tags = ["Analytics", "Technical"]
categories = ["Technical"]

+++
Understanding how a software product is used and performs in production is incredibly useful. SaaS companies use real time analytics tools to make informed development decisions. But on-prem enterprise product teams mostly operate in the blind: relying on vanilla logs, inaccurate surveys and random guesswork in making decisions.

There are several analytics solutions out there. But let's consider a simple approach towards collecting and analyzing data from on-prem software:

Instrumentation - [Dropwizard Metrics](http://metrics.dropwizard.io/)  
ETL - [Logstash](https://www.elastic.co/products/logstash)  
Time Series Storage - [InfluxDB](https://www.influxdata.com/time-series-platform/influxdb/)  
Visualization - [Grafana](http://grafana.org/)

## Instrumentation

The first step is instrumenting the code to collect relevant information. Metrics is a simple yet powerful option in this regard. It is written in Java with ports available for most languages.

It provides five main metrics:

* _Counters_: Counts things such as number of hits, total events processed etc.
* _Gauges_: Instant measurements of a value such as queue size, number of pending jobs etc.
* _Meters_: Measures throughput (such as requests per second).
* _Histogram_: Measures the statistical distribution of values (min, max, mean, median, percentiles: 75th, 90th, 95th..)
* _Timers_: Measures both the rate that a particular piece of code is called and the distribution of its duration.  

Additionally, Metrics also provides neat ways of reporting the data including CSV, SLF4J and Graphite reporters. Here is a snippet of data collected using the Metrics SLF4J reporter backed by Log4j:

```
2016-07-14 14:01:41 INFO  metrics:108 - type=COUNTER, name=ui.settings_page.count, count=2
2016-07-14 14:01:41 INFO  metrics:108 - type=METER, name=events.requests.meter, count=2, mean_rate=1.9872438815244944, m1=0.0, m5=0.0, m15=0.0, rate_unit=events/second
2016-07-14 14:02:10 INFO  metrics:108 - type=TIMER, name=events.requests.timer, count=9, min=1002.1139999999999, max=1004.454, mean=1003.142253407935, stddev=0.8211239140876241, median=1003.1129999999999, p75=1003.673, p95=1004.454, p98=1004.454, p99=1004.454, p999=1004.454, mean_rate=0.90735593456149, m1=0.8, m5=0.8, m15=0.8, rate_unit=events/second, duration_unit=milliseconds
```

## ETL

[Logstash](https://www.elastic.co/products/logstash) comes with plugins for [ingesting](https://www.elastic.co/guide/en/logstash/current/input-plugins.html), [transforming](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html) and [outputting](https://www.elastic.co/guide/en/logstash/current/output-plugins.html) data in different formats.

It includes plugins for [Log4j](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-log4j.html) and [InfluxDB](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-influxdb.html) that can be leveraged to transform Log4j files and push them into an InfluxDB database.

## Data Storage

[InfluxDB](https://www.influxdata.com/time-series-platform/influxdb/) is a time series database written in Go. It can handle large amounts of data and includes RESTful API's to insert and query data.

It includes InfluxQL: a [language](https://docs.influxdata.com/influxdb/v1.1/query_language/) similar to SQL to manipulate the data and includes a  web interface for easy access.

## Data Visualization

The ultimate utility comes from visualizing the data we've gathered. [Grafana](http://grafana.org/) is one of the best open source tools for visualizing data. It ships with in-built support for [InfluxDB data sources](http://docs.grafana.org/datasources/influxdb/) amongst others.

Check out [play.grafana.org](http://play.grafana.org/) for a showcase of all its features including a sample [dashboard](http://play.grafana.org/dashboard/db/influxdb-templated-queries) backed by an InfluxDB data source.

## Conclusion

The combination of Metrics, Logstash, InfluxDB and Grafana offers a highly powerful yet simple pipeline to collect and analyze relevant application data enabling engineering teams to make informed decisions.
