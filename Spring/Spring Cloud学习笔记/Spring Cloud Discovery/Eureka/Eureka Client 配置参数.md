#### 基本参数

| Args                                                 | Default   | Description                                                  |
| ---------------------------------------------------- | --------- | ------------------------------------------------------------ |
| eureka.client.availability-zones                     |           | 告知Client 有哪些 region 和 availability-zones ,支持配置修改运行时生效 |
| eureka.client.filter-only-up-instance                | true      | 是否过滤出 InstanceStatus 为UP 的实例                        |
| eureka.client.region                                 | us-east-1 | 指定该应用实例所在的 region                                  |
| eureka.client.register-with-eureka                   | true      | 是否将该应用实例注册到 Eureka Server                         |
| eureka.client.prefer-same-zone-eureka                | true      | 是否优先使用与该应用实例处于相同zone的Eureka Server          |
| eureka.client.on-demand-update-status-change         | true      | 是否将本地实例状态的更新通过 ApplicationInfoManager 实时触发同步到 Eureka Server |
| eureka.instance.metadata-map                         |           | 指定应用实例的元数据信息                                     |
| eureka.instance.prefer-ip-address                    | false     | 是否优先使用 ip 来代替 hostname 作为应用实例的hostName 字段值 |
| eureka.instance.lease-expiration-duration-in-seconds | 90        | 指定Eureka Client 时间间隔多久需要向Eureka Server 发送心跳告知实例还存活 |

#### 定时任务参数

| Args                                                         | Default | Description                                                  |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| eureka.client.cache-refresh-executor-thread-pool-size        | 2       | 刷新缓存的CacheRefreshThread的线程池大小                     |
| eureka.client.cache-refresh-executor-exponential-back-off-bound | 10      | 调度任务执行超时时下次的调度延时时间                         |
| eureka.client.heartbeat-executor-thread-pool-size            | 2       | 心跳线程HeartbeatThread的线程池大小                          |
| eureka.client.heartbeat-executor-exponential-back-off-bound  | 10      | 调度任务执行超时时下次的调度延时时间                         |
| euraka.client.eurekaserver-url-poll-interval-seconds         | 5*60    | AsyncResolver.updateTask 刷新Euraka Server地址的时间间隔     |
| eureka.client.registry-fetch-interval-seconds                | 30      | CacheRefreshThread 线程的调度频率                            |
| eureka.client.initial-instance-info-replication-interaval-secondes | 40      | InstanceInfoReplicator 将应用实例信息变更同步到 Eureka Server 的初始延时时间 |
| euraka.client.instance-info-replication-interval-seconds     | 30      | InstanceInfoReplicatitor 将实例信息变更同步到Eureka Server 的间隔时间 |
| eureka.instance.lease-renewal-interval-in-seconds            | 30      | Eureka Client 向Eureka Server 发送心跳的时间间隔             |

#### Http参数