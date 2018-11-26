# Spring Boot 多环境配置

我们在开发的过程中，免不了使用不同的数据源地址，服务端口等等。比如：开发环境，生产环境，测试环境。如果每次都通过修改application.yml文件或者application.properties文件来切换环境，这是非常繁琐的且很容易反生错误的事。

对于多环境的配置，各种项目构建工具或是框架的基本思路是一致的，通过配置多份不同环境的配置文件，再通过打包命令指定需要打包的内容之后进行区分打包，Spring Boot也不例外，或者说更加简单。

在Spring Boot中多环境配置文件名需要满足application-{profile}.properties或者application-{profile}.yml的格式，其中{profile}对应你的环境标识，比如：

**properties文件：**

* application-dev.properties：开发环境

* application-test.properties：测试环境

* application-prod.properties：生产环境

**yml文件：**

* application-dev.yml：开发环境

* application-test.yml：测试环境

* application-prod.yml：生产环境

具体哪个的配置文件会被加载，需要在application.properties或者application.yml文件中通过spring.profiles.active属性来设置，其值对应{profile}值。

如：

properties文件：

```java
spring.profiles.active=prod
```

yml文件：

```java
spring:
  profiles:
    active: prod
```

就会加载application-prod.properties配置文件内容


下面，以不同环境配置不同的服务端口为例，进行多环境测试。

针对各环境新建不同的配置文件application-dev.properties、application-test.properties、application-prod.properties或者application-dev.yml、application-test.yml、application-prod.yml

目录结构如下：

![TIM截图20180824150109](C:\Users\Administrator\Desktop\innerpeacez\images\TIM截图20180824150109.png)

或者

![1535094943495](C:\Users\Administrator\Desktop\innerpeacez\images\1535094943495.png)

在这三个文件均都设置不同的server.port属性，如：dev环境设置为8080，test环境设置为8081，prod环境设置为8082

application.properties中设置spring.profiles.active=dev，就是说默认以dev环境设置

**测试不同配置的加载**

- 执行java -jar xxx.jar，可以观察到服务端口被设置为8080，也就是默认的开发环境（dev）
- 执行java -jar xxx.jar --spring.profiles.active=test，可以观察到服务端口被设置为8081，也就是测试环境的配置（test）
- 执行java -jar xxx.jar --spring.profiles.active=prod，可以观察到服务端口被设置为8082，也就是生产环境的配置（prod）

**按照上面的实验，可以如下总结多环境的配置思路：**

* application.properties或者application.yml中配置通用内容，并设置spring.profiles.active=dev或者spring，以开发环境为默认配置
* application-{profile}.properties中配置各个环境不同的内容
* 通过命令行方式去激活不同环境的配置