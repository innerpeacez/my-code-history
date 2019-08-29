#### Eureka Server REST API

Eureka 官方的 github wiki中列出了 Eureka Server 的 [REST API](https://github.com/Netflix/eureka/wiki/Eureka-REST-operations) ,下面为翻译的中文版

| **Operation**                            | **HTTP action**                                              | **Description**                                              |
| ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 注册新的应用实例                         | POST /eureka/v2/apps/**appID**                               | Input: JSON/XMLpayload 成功返回 204                          |
| 注销应用实例                             | DELETE /eureka/v2/apps/**appID**/**instanceID**              | 成功返回 200                                                 |
| 发送应用实例心跳                         | PUT /eureka/v2/apps/**appID**/**instanceID**                 | 成功返回 200 ，如果 **instanceID** 不存在则返回 404          |
| 查询所有实例                             | GET /eureka/v2/apps                                          | Output: JSON/XML，成功返回 200                               |
| 查询所有指定 **appID** 的实例            | GET /eureka/v2/apps/**appID**                                | 成功返回 200， Output: JSON/XML                              |
| 查询指定**appID** 和**instanceID**的实例 | GET /eureka/v2/apps/**appID**/**instanceID**                 | 成功返回 200，Output: JSON/XML                               |
| 查询指定**instanceID** 的实例            | GET /eureka/v2/instances/**instanceID**                      | 成功返回 200，Output: JSON/XML                               |
| Take instance out of service             | PUT /eureka/v2/apps/**appID**/**instanceID**/status?value=OUT_OF_SERVICE | 成功返回 200，错误返回 500                                   |
| 恢复服务实例                             | DELETE /eureka/v2/apps/**appID**/**instanceID**/status?value=UP | 成功返回 200，错误返回 500                                   |
| 更新服务实例元数据                       | PUT /eureka/v2/apps/**appID**/**instanceID**/metadata?key=value | 成功返回 200，错误返回 500                                   |
| 根据 **vip** 地址查询应用实例            | GET /eureka/v2/vips/**vipAddress**                           | 成功返回 200，Output: JSON/XML ， 如果 **vip** 不存在则返回 404 |
| 根据 **svip** 地址查询应用实例           | GET /eureka/v2/svips/**svipAddress**                         | 成功返回 200，Output: JSON/XML ， 如果 **svip** 不存在则返回 404 |