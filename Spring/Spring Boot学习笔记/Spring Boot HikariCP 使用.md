## 简介

SpringBoot2.0 之后使用 HikariCP 连接池更加方便了，因为 SB2.0 之后， HikariCP  连接池代替了 Tomcat 连接池已经成为了 SB2.0 的默认连接池。[Springboot 2.0选择HikariCP作为默认数据库连接池的五大理由](http://blog.didispace.com/Springboot-2-0-HikariCP-default-reason/) ，`spring-boot-starter-data-jpa `依赖中集成了HikariCP,所以不用在引入Hikari的相关依赖了。下面开始正题。

## 环境

- Maven
- JDK 8+
- SpringBoot 2+

## 相关依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.13</version>
</dependency>
```

## 相关配置

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&useSSL=false&zeroDateTimeBehavior=convertToNull
    username: root
    password: root
    type: com.zaxxer.hikari.HikariDataSource
    hikari:
      minimum-idle: 5 # 最小链接数
      maximum-pool-size: 15 # 最大连接数
      auto-commit: true # 自动提交
      idle-timeout: 30000 # 最大空闲时间（毫秒）
      pool-name: DataPoolHikari # 链接池名
      max-lifetime: 180000 # 最大生命周期（毫秒）
      connection-timeout: 30000 # 连接超时时间（毫秒）
      connection-test-query: SELECT 1 # 心跳检测
```

## 详细配置信息



<center>
    <img style="border-radius: 0.5125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"
    src="../../images/1546595420415.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">enjoy coding！</div>
</center>

