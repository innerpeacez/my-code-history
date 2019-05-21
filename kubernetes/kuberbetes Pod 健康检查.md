#### 判断 Pod 健康状态的两种方式

- livenessProbe 存活性探针
- readinessProbe 就绪性探针

##### 1.livenessProbe 存活性探针

用于判断容器是否存活，即 Running 状态。如果 livenessProbe 探针检测到容器不健康，则 kubelet 会杀死容器，并根据容器的重启策略进行容器的重启。如果容器没有定义 livenessProbe 探针，则 livenessProbe 返回状态永远为 Success。

##### 2.readinessProbe  就绪性探针

用于判断容器服务（service）是否可用，即 Ready 状态。如果 Ready 状态，则表示Service 与 Pod EndPoint 建立了关联，并且将其保存到 EndPoint 列表中，以供服务调用；如果状态变为 Failure , Kubernetes 会将其从 EndPoint 列表中移除，保证通过 Service 访问时不会将流量路由到不健康的 Pod 上，等到状态恢复到 Ready 状态再将其对应的 EndPoint 加入EndPoint 列表中。如果容器没有定义 readinessProbe 探针，则 readiness 返回状态永远为 Success。

#### 探针的配置方式

通过 kubectl 定期对容器进行诊断，判断容器状态是否健康或者存活。

##### livenessProbe 和 readinessProbe 探测方式

- ExecAction
- TCPSocketAction
- HTTPGetAction

###### ExecAction

在容器中执行自定义命令以判断容器是否健康，命令退出时返回码为 0 ,则表示健康，其他值表示不健康。



### 

