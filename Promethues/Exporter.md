#### 简介

广义上讲所有可以向Prometheus提供监控样本数据的程序都可以被称为一个Exporter。而Exporter的一个实例称为target，如下所示，Prometheus通过轮询的方式定期从这些target中获取样本数据

![img](../images/assets%2F-LBdoxo9EmQ0bJP2BuUi%2F-LVT4hCAm7HWaP8rOjeF%2F-LPSBoeAqGV2mX0ZVbLf%2Fprometheus-exporter.png)



#### Exporter 的运行方式

- 独立使用的

以我们已经使用过的Node Exporter为例，由于操作系统本身并不直接支持Prometheus，同时用户也无法通过直接从操作系统层面上提供对Prometheus的支持。因此，用户只能通过独立运行一个程序的方式，通过操作系统提供的相关接口，将系统的运行状态数据转换为可供Prometheus读取的监控数据。

- 集成到应用中的

为了能够更好的监控系统的内部运行状态，有些开源项目如Kubernetes，ETCD等直接在代码中使用了Prometheus的Client Library，提供了对Prometheus的直接支持。这种方式打破的监控的界限，让应用程序可以直接将内部的运行状态暴露给Prometheus，适合于一些需要更多自定义监控指标需求的项目。

#### Exporter 规范

所有的Exporter程序都需要按照Prometheus的规范，返回监控的样本数据

Exporter 返回的样本数据，主要有三个部分组成

- 样本的一般注释信息（HELP）
- 样本的类型注释信息（TYPE）
- 样本

例：

```yaml
# HELP node_load1 1m load average.
# TYPE node_load1 gauge
node_load1 1.08
```

Promethues 会逐行解析样本数据，以下是解析方式

HELP

```yaml
# HELP <metrics_name> <doc_string>
```

TYPE

```yaml
# TYPE <metrics_name> <metrics_type>
```

样本

```yaml
metric_name [{label_name="label_value"}] value [timestamp]
```

如

```yaml
node_arp_entries{device="cali032d359ac3a"} 1
```

其中metric_name和label_name必须遵循PromQL的格式规范要求。value是一个float格式的数据，timestamp的类型为int64（从1970-01-01 00:00:00以来的毫秒数），timestamp为可选默认为当前时间。

#### 指定样本格式的版本

在Exporter响应的HTTP头信息中，可以通过Content-Type指定特定的规范版本，例如：

```yaml
HTTP/1.1 200 OK
Content-Encoding: gzip
Content-Length: 2906
Content-Type: text/plain; version=0.0.4
Date: Sat, 17 Mar 2018 08:47:06 GMT
```

其中version用于指定Text-based的格式版本，当没有指定版本的时候，默认使用最新格式规范的版本。同时HTTP响应头还需要指定压缩格式为gzip。

```
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=20000:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```
