#### Eureka Server 核心类

- InstanceInfo 注册的服务实例类
- IeaseInfo 应用实例的租约信息
- ServiceInstance  约定服务发现实例的通用信息类
- InstanceStatus 标识服务实例状态

#### InstanceInfo

##### InstanceStatus

这个类是InstanceInfo 中的一个枚举类，规定了实例的以下五种状态

| Statu          | Description                              |
| -------------- | ---------------------------------------- |
| UP             | 已经准备好接收调用                       |
| DOWN           | 健康检查回调失败，外部调用可能会出现问题 |
| STARTING       | 正在初始化，不能被调用                   |
| OUT_OF_SERVICE | 暂停的服务                               |
| UNKNOWN        | 状态不合法                               |

以下是源码

```java
public enum InstanceStatus {
    UP, // Ready to receive traffic
    DOWN, // Do not send traffic- healthcheck callback failed
    STARTING, // Just about starting- initializations to be done - do not
    // send traffic
    OUT_OF_SERVICE, // Intentionally shutdown for traffic
    UNKNOWN;

    public static InstanceStatus toEnum(String s) {
        if (s != null) {
            try {
                return InstanceStatus.valueOf(s.toUpperCase());
            } catch (IllegalArgumentException e) {
                // ignore and fall through to unknown
                logger.debug("illegal argument supplied to InstanceStatus.valueOf: {}, defaulting to {}", s, UNKNOWN);
            }
        }
        return UNKNOWN;
    }
}
```