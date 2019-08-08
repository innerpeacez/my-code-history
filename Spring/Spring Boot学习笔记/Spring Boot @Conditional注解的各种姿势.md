#### @ConditionalOnProperty 注解

使用这个注解，我们可以通过配置文件的方式自定义我们的 Bean 是否被 Spring 初始化

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.TYPE, ElementType.METHOD })
@Documented
@Conditional(OnPropertyCondition.class)
public @interface ConditionalOnProperty {

   String[] value() default {};
   
   String prefix() default "";

   String[] name() default {};

   String havingValue() default "";
   
   boolean matchIfMissing() default false;

}
```

**参数的详细解释**

| args           | description                                                  |
| -------------- | ------------------------------------------------------------ |
| value          | name 的别名，等价于 name                                     |
| prefix         | name 或者 value 的前缀                                       |
| name           | 配置时的所匹配的 key, 如果你指定了 prefix="app.config"，name="my-config“, 则最后应该匹配的为 app.config.my-config 这个 key ,需要注意的是应该使用 “-” 取代大写，而不是：name="myConfig" |
| havingValue    | 期望配置 key 所指定的值，如果没有指定这个参数，则值不能为 false |
| matchIfMissing | 如果配置未匹配到指定的 name ,是否加载bean ？默认为 false,不加载 |

**注意：**这些参数中我觉得有意思的参数时 havingValue ，因为如果我指定了 `havingValue = “false”`, 且我配置文件中也配置了对应的 name = false ，这也算是匹配到了对应的值。

**小栗子：**

@ConditionalOnProperty 配置是否在服务启动时，执行这个 ApplicationRunner 。

```java
@Component
@ConditionalOnProperty(prefix = "innerpeacez", name = "runner.enabled", matchIfMissing = true, havingValue = "false")
public class MyRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) {
        System.out.printf("Hello , @ConditionalOnProperty %s \n", LocalDateTime.now());
    }
}
```

配置文件 application.yml

```yaml
innerpeacez:
  runner:
    enabled: false
```