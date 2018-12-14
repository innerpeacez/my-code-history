## Gateway中的路由断言种类

```java
Loaded RoutePredicateFactory [After]
Loaded RoutePredicateFactory [Before]
Loaded RoutePredicateFactory [Between]
Loaded RoutePredicateFactory [Cookie]
Loaded RoutePredicateFactory [Header]
Loaded RoutePredicateFactory [Host]
Loaded RoutePredicateFactory [Method]
Loaded RoutePredicateFactory [Path]
Loaded RoutePredicateFactory [Query]
Loaded RoutePredicateFactory [ReadBodyPredicateFactory]
Loaded RoutePredicateFactory [RemoteAddr]
Loaded RoutePredicateFactory [Weight]
Loaded RoutePredicateFactory [CloudFoundryRouteService]
```

- [After]路由断言: 当请求进来的当前时间在配置的UTC时间之后，则会匹配成功，否则路由不能匹配成功
- [Before]路由断言： 当请求进来的当前时间在配置的UTC时间之前，则会匹配成功，否则路由不能匹配成功
- [Between]路由断言： 当请求进来的当前时间在配置的UTC时间之间，则会匹配成功，否则路由不能匹配成功
- [Cookie]路由断言： 当请求中携带的cookie和Cookie断言中设置的的key和value一致，则匹配路由成功，否则不成功。
- [Header]路由断言： 根据配置的路由header信息进行匹配，匹配成功则转发，否则不转发
- [Host]路由断言： 当请求中的Host与Host断言匹配时，则路由成功，否则失败。
- [Method]路由断言： 根据路由信息配置的method对请求方法是GET或者POST等请求类型进行匹配，匹配成功则路由，否则不路由。
- [Path]路由断言： 根据请求的路由路径jin
- [Query]路由断言： 请求的参数的key和value进行匹配，成功则路由，否则不路由。
- [RemoteAddr]路由断言： 对请求地址的ip进行断言，ip匹配成功则路由成功，否则失败。

