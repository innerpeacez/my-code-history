### HTTP 请求返回状态

#### HTTP状态代码分为五类，用于标识其含义：

- 1xx：信息 - 收到请求，返回了更多信息。
- 2xx：成功 - 请求已成功接收，理解和接受。
- 3xx：重定向 - 必须在其他地方执行请求才能完成。
- 4xx：客户端错误 - 请求包含**客户端**必须解决的无效信息。尝试相同的请求将导致相同的错误。
- 5XX：服务器错误-请求不能被所满足的**服务器**。如果稍后尝试，相同的请求可能会成功（或产生4xx错误）。

#### 常用状态码

| 码                | 含义                                                         |
| ----------------- | ------------------------------------------------------------ |
| 200好的           | 请求成功了                                                   |
| 201创建           | 成功，并创建了一个新资源。应该包括资源的Location头。         |
| 202接受           | 该请求已收到但尚未完成。请参见[异步操作](https://github.com/rancher/api-spec/blob/master/specification.md#asynchronous-actions) |
| 400错误请求       | 该请求在某种程度上是畸形的。用于可由客户端更正的一般错误。   |
| 401未经授权       | 身份验证信息未发送或无效。                                   |
| 403禁止           | 不允许经过身份验证的用户访问所请求的资源。                   |
| 404未找到         | 这些不是您正在寻找的机器人。                                 |
| 422不可处理的实体 | 请求格式良好（400）并且以支持的格式（415），但是不能被处理。通常用于特定于应用程序的验证错误。 |
| 500内部服务器错误 | 服务器上的错误的通用消息。客户端应该稍后再次尝试他们的请求，此时它**可能会**成功（或产生4xx |

| 码                  | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| 301永久移动         | 永久重定向。请求的资源已**永久**移动，客户端不应再次请求此URI。如有疑问，请使用302。 |
| 302找到             | 临时重定向。请求的资源已移动，客户端应再次请求此URI以用于将来的请求。 |
| 304未修改           | 客户端请求允许使用先前版本的资源。不会发送任何回复。         |
| 405方法不允许       | 此URL不允许使用请求的HTTP方法。                              |
| 406不可接受         | 该服务不支持客户端**请求**的表示。                           |
| 409冲突             | 启用[资源版本控制](https://github.com/rancher/api-spec/blob/master/specification.md#resource-versioning)，请求的操作与当前状态冲突。 |
| 410已经走了         | 请求的资源已经离开。它不会回来。                             |
| 413实体太大         | 请求正文大于服务愿意处理的请求正文。                         |
| 414 Request-URI太长 | 请求的URL比服务接受的URL长。                                 |
| 415不支持的媒体类型 | 该服务不支持客户端**发送**的表示类型。                       |
| 418我是一个茶壶     | 该请求尝试使用不符合[RFC2324](http://tools.ietf.org/html/rfc2324)的茶壶服务来冲泡咖啡。 |
| 503服务不可用       | 由于维护或过载，无法处理该请求。                             |

### 可以调用的的API接口

| API（前缀：https://host:port/v3）        | 描述 |
| ---------------------------------------- | ---- |
| authConfigs                              |      |
| catalogs                                 |      |
| clusters                                 |      |
| clusteralerts                            |      |
| clusterevents                            |      |
| clusterloggings                          |      |
| clusterregistrationtokens                |      |
| clusterroletemplatebindings              |      |
| composeconfigs                           |      |
| dynamicschemas                           |      |
| globalroles                              |      |
| globalrolebindings                       |      |
| groups                                   |      |
| groupmembers                             |      |
| ldapconfigs                              |      |
| listenconfigs                            |      |
| nodes                                    |      |
| nodedrivers                              |      |
| nodepools                                |      |
| nodetemplates                            |      |
| notifiers                                |      |
| podsecuritypolicytemplates               |      |
| podsecuritypolicytemplateprojectbindings |      |
| preferences                              |      |
| principals                               |      |
| projects                                 |      |
| projectalerts                            |      |
| projectloggings                          |      |
| projectnetworkpolicies                   |      |
| projectroletemplatebindings              |      |
| roletemplates                            |      |
| settings                                 |      |
| subscribe                                |      |
| templates                                |      |
| templatecontents                         |      |
| templateversions                         |      |
| tokens                                   |      |
| users                                    |      |

| Operation                                                    | HTTP Method | Request URL                                                  | Description                                                  |
| ------------------------------------------------------------ | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Query](https://github.com/rancher/api-spec/blob/master/specification.md#query-operation) | GET or HEAD | [https://base/v1/{collection_name}](https://base/v1/%7Bcollection_name%7D) | Retrieve a collection of resources                           |
| [Read](https://github.com/rancher/api-spec/blob/master/specification.md#read-operation) | GET or HEAD | [https://base/v1/{collection_name}/{resource](https://base/v1/%7Bcollection_name%7D/%7Bresource) id} | Retrieve a single resource                                   |
| [Create](https://github.com/rancher/api-spec/blob/master/specification.md#create-operation) | POST        | [https://base/v1/{collection_name}](https://base/v1/%7Bcollection_name%7D) | Creates one or more resources                                |
| [Update](https://github.com/rancher/api-spec/blob/master/specification.md#update-operation) | PUT         | [https://base/v1/{collection_name}/{resource_id}](https://base/v1/%7Bcollection_name%7D/%7Bresource_id%7D) [https://base/v1/{collection_name}](https://base/v1/%7Bcollection_name%7D) | Change one or more existing resources                        |
| [Delete](https://github.com/rancher/api-spec/blob/master/specification.md#delete-operation) | DELETE      | [https://base/v1/{collection_name}/{resource_id}](https://base/v1/%7Bcollection_name%7D/%7Bresource_id%7D) [https://base/v1/{collection_name}](https://base/v1/%7Bcollection_name%7D) | Delete one or more existing resources                        |
| [Replace](https://github.com/rancher/api-spec/blob/master/specification.md#replace-operation) | REPLACE     | [https://base/v1/{collection_name}/{resource_id}](https://base/v1/%7Bcollection_name%7D/%7Bresource_id%7D) [https://base/v1/{collection_name}](https://base/v1/%7Bcollection_name%7D) | Query, Delete and Create one or more resources in one atomic transaction |
| [Action](https://github.com/rancher/api-spec/blob/master/specification.md#action-operation) | POST        | [https://base/v1/{collection_name}/{resource_id}?{action_name}](https://base/v1/%7Bcollection_name%7D/%7Bresource_id%7D?%7Baction_name%7D) [https://base/v1/{collection_name}?{action_name}](https://base/v1/%7Bcollection_name%7D?%7Baction_name%7D) | Perform an action on a resource or collection                |