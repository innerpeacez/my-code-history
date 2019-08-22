#### LeaseManager

服务发现的几个重要的操作，定义在LeaseManager类中

- 服务注册（register）：注册服务实例信息
- 服务下线（cancel）：删除服务实例信息
- 服务租约（renew）：与Eureka Server进行心跳，维持租约
- 服务剔除（evict）：剔除租约过期的服务实例信息

```java
public interface LeaseManager<T> {

    void register(T r, int leaseDuration, boolean isReplication);
    
    boolean cancel(String appName, String id, boolean isReplication);
    
    boolean renew(String appName, String id, boolean isReplication);
    
    void evict();
}
```

#### LookupService

定义了Eureka Client 从Eureka Server 获取服务实例的查询方法

- getApplication()：根据 appName 获取应用信息
- getApplications()： 获取所有的应用信息
- getInstancesById()：根据应用 id 获取对应的所有实例
- getNextServerFromEureka()：根据 vhost 获取下一个服务实例

```java
public interface LookupService<T> {

    Application getApplication(String appName);

    Applications getApplications();

    List<InstanceInfo> getInstancesById(String id);

    InstanceInfo getNextServerFromEureka(String virtualHostname, boolean secure);
}
```

