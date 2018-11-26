## @SpringBootApplication

这个注解可以说是SpringBoot中最为出名的注解了，点进这个注解看到其实这个注解是一个复合注解。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
      @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

   ...

}
```

下面是其中三个比较重要的注解

### @SpringBootConfiguration

看看这个注解的源码

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}
```

这个注解其实只是将@Configuration再包裹了一层。本质上还是@Configuration这个注解。每一个标注了@Configuration这个注解的类都是一个Spring IOC容器的配置类，说的容易理解一点，标注了这个注解的类就相当于之前的xml文件，而标注了这个注解的类里面一般会有@Bean这个注解，@Bean这个注解就相当于xml文件中的<bean></bean>配置的每一个bean。

#### @Configuration

看看@Configuration这个注解的源码

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    ...
}
```

可以看到@Component，标注了这个注解的类，会在Spring IOC容器初始化的时候去扫描

#### @Bean

相比于xml方式中的<bean></bean>的这种方式。

```xml
<bean id="bean1" class="bean1.class">
	<property ref="bean2" name="bean2">
</bean>
<bean id="bean2" class="bean2.class"></bean>
```

@Bean注解存在一个问题，如果有多个类都依赖一个类，每个需要依赖某个类的类中都要使用这个注解进行注入，就可能出现这个类多实例。当然这个问题Spring 给出的策略是没有实例的bean进行注入，如果已经有了就返回之前的bean实例

### @EnableAutoConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
    ...
}
```

#### @Import

之前xml的配置文件，往往会将不同功能的配置卸载不同的配置文件中

```java
spring-application.xml
spring-application-dubbo.xml
spring-application-database.xml
...
```

然后通过<import /> 引入配置，这个注解对应的就是引入配置的能力。

这个注解配合AutoConfigurationImportSelector这个类，将符合条件的@Configuration配置都注入到Spring IOC容器中

### @ComponentScan

寻找标注有@Component。@Controller，@Service，@Mapping，@Repository等注解的类，将其注入IOC容器