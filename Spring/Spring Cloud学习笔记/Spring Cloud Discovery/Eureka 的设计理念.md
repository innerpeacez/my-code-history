#### 作为一个注册中心，需要解决的问题

1. 服务实例如何注册到注册中心
   1. 普通 java 应用，通过 `Eureka Client API` 注册
   2. spring cloud 应用 ，通过`spring-cloud-start-netflix-eureka-client`， 基于 sb 自动装配完成自动注册
2. 服务实例如何从注册中心剔除
   1. client 端主动剔除：实例关闭时，调用 de-register 方法，剔除自身
   2. server 端主动剔除：为了解决 client 端由于某种原因，调用剔除失败，server与client 建立心跳机制，心跳失败的会被 server 自动剔除

3. 服务实例信息一致性问题

   ​	在 Eureka Server Cluster 中，需要保证多个 Server 上注册的应用实例信息一致，Eureka 采用了 AP 模型。

   - Consistency 数据一致性：多副本数据保持一致

   - Availiability 可用性 ：任何情况下，都有一个副本能都正差提供服务

   - Partition Tolerance 分区容错性 ：每个副本都是一样的，即便副本之间通信出现故障，集群仍然可用
