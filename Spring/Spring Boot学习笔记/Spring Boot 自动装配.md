#### 神奇的@SpringBootApplication

@SpringBootApplication

- @SpringBootConfiguration
  - @Configuration
    - @Component
- @EnableAutoConfiguration
  - @AutoConfigurationPackage
  - @Import
- @ComponentScan

可以开启自动装配的两个注解

- @SpringBootApplication
- @EnableAutoConfiguration

两者的区别在于@SpringBootApplication注解时派生注解，其中包含了@Configuration注解，但是@EnableAutoConfiguration注解中并没有@Configuration注解。

JavaConfig 配置方式

@Bean 可以使用再 @Component 和 @Configuration 两种注解注释的类中，两者的区别在于，@Bean 存在与@Configuration 注释的类中时，会被CGLIB 的动态代理方式进行增强。

官方给这两种模式起了两个名字

- lite mode 轻量模式
- full mode 全量模式

所以SpringBoot 中的CGLIB增强只是针对与@Configuration注解的。

自动装配

@ComponentScan 注解扫描到类会被spring ioc 容器初始化







自动装配中两个常见的注解

- @ConditionalOnMissingBean
- @ConditionalOnClass

