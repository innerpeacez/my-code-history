### 自定义属性

我们在使用`Spring boot`的时候，一些特殊情况下会需要用到一些自定义属性，当然`Spring Boot`已经为我们考虑好了，我们只要按着配置规则进行配置就可以了，本文讲的是自定义属性的配置。

#### @Value

首先说一下之前我们怎么获取配置文件中的自定义属性

* 在application.properties文件中加入自定义属性

```java
com.innerpeacez.name=innerpeacez
com.innerpeacez.address=hangzhou
com.innerpeacez.age=18
com.innerpeacez.desc=${com.innerpeacez.age} years old,${com.innerpeacez.name} is studying spring boot in ${com.innerpeacez.address}
```

* 使用`@Value`注解获取属性值

```java
@Component
@Data
public class InnerpeacezInfo {
    @Value("${com.innerpeacez.name}")
    private String name;
    @Value("${com.innerpeacez.address}")
    private String address;
    @Value("${com.innerpeacez.age}")
    private int age;
    @Value("${com.innerpeacez.desc}")
    private String desc;
}
```

测试类：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest
public class PropertiesTests {

    private final Logger logger = LoggerFactory.getLogger(PropertiesTests.class);

    @Autowired
    private InnerpeacezInfo innerpeacezInfo;

    @Test
    public void testCustomProperties() {
        logger.info("------------------------------------------------------");
        logger.info(innerpeacezInfo.toString());
        logger.info("------------------------------------------------------");
    }
}
```

测试结果：

```java
2018-08-29 14:22:49.967  INFO 7900 --- [           main] c.zhw.study.springboot.PropertiesTests   : ------------------------------------------------------
2018-08-29 14:22:49.967  INFO 7900 --- [           main] c.zhw.study.springboot.PropertiesTests   : InnerpeacezInfo(name=innerpeacez, address=hangzhou, age=18, Desc=18 years old,innerpeacez is studying spring boot in hangzhou)
2018-08-29 14:22:49.967  INFO 7900 --- [           main] c.zhw.study.springboot.PropertiesTests   : ------------------------------------------------------
```



#### @ConfigurationProperties

接下来就是通过`Spring Boot`提供的@ConfigurationProperties注解来完成自定义属性。

* 需要引入configuration相关Maven依赖

```java
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-configuration-processor</artifactId>
   <optional>true</optional>
</dependency>
```

- 在application.properties文件中加入自定义属性

```java
com.innerpeacez.name=innerpeacez
com.innerpeacez.address=hangzhou
com.innerpeacez.age=18
com.innerpeacez.desc=${com.innerpeacez.age} years old,${com.innerpeacez.name} is studying spring boot in ${com.innerpeacez.address}
```

- 使用`@ConfigurationProperties`注解获取属性值

```java
@ConfigurationProperties("com.innerpeacez")
@Component
@Data
public class InnerpeacezInfo {
    private String name;
    private String address;
    private int age;
    private String desc;
}
```

测试类和测试结果都是和上面的一样的。



### 自定义文件

自定义属性的时候我们是将属性写在`application.properties`文件中的。因为`Spring Boot`启动的时候会自动加载这个文件，如果我们不想使用这个文件我们应该怎么做?

* 首先自定义一个`innerpeacezinfo.properties`配置文件，内容和上面的`application.properties`文件一样

```java
com.innerpeacez.name=innerpeacez
com.innerpeacez.address=hangzhou
com.innerpeacez.age=18
com.innerpeacez.desc=${com.innerpeacez.age} years old,${com.innerpeacez.name} is studying spring boot in ${com.innerpeacez.address}
```

#### @PropertySource

* 使用`@PropertySource`注解加载我们自定义的配置文件

```java
@ConfigurationProperties("com.innerpeacez")
@PropertySource("classpath:innerpeacezinfo.properties")
@Component
@Data
public class InnerpeacezInfo {
    private String name;
    private String address;
    private int age;
    private String desc;
}
```

测试类和测试结果都是和上面的一样的。



### 注意

配置文件中是不区分大小写的，但是好像不能有`boolean`类型的元素，`@Value`注解时直接会报错，而使用`Spring Boot`的方式加载时，不会报错，会给个默认值false