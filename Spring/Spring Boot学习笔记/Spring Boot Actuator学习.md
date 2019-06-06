Spring Boot 对于 `Production-Ready` 特性的实现包含在了 `spring-boot-starter-actuator` 依赖中。

特点

- 监控和管理发布生产的应用
- 通过HTTP或者JMX的方式监控和管理
- Endpoints 类型有 审计(Audition)，健康(Health)，指标收集(Metric Gathering)等
- 自动运行

在使用 Spring Boot Actuator 之前需要做一些事情

应为spring boot actuator 并不是默认存在，所以需要引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

默认只有两个 `Endpoint` 被开启（`health,info`）,如果想要开启所有的`Actuator Endpoints` ,需要在`application.yml` 文件中添加如下配置： 

```yml
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

