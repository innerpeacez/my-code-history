<center>
    <img style="border-radius: 0.5125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"
    src="../images/bb283e21bade4a66914e445a071a18b.jpg">
</center>

#### Service 简介：

Kubernetes `Service` 定义了这样一种抽象：一个 `Pod` 的逻辑分组，一种可以访问它们的策略 —— 通常称为微服务。 这一组 `Pod` 能够被 `Service` 访问到，通常是通过 [`Label Selector`](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)（查看下面了解，为什么可能需要没有 selector 的 `Service`）实现的。

对 Kubernetes 集群中的应用，Kubernetes 提供了简单的 `Endpoints` API，只要 `Service` 中的一组 `Pod`发生变更，应用程序就会被更新。 对非 Kubernetes 集群中的应用，Kubernetes 提供了基于 VIP 的网桥的方式访问 `Service`，再由 `Service` 重定向到 backend `Pod`。

#### Service 的五种类型 

- ClusterIP
- NodePort
- LoadBalancer
- ExternalName
- None

<center>
    <img style="border-radius: 0.5125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"
    src="../images/1558083780164.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">enjoy coding！</div>
</center>

##### ClusterIP

###### 简介：

通过集群内部的虚拟IP暴露服务，这种类型只在集群内部可达，无法被外部的客户端调用。

###### 设置方式：

```yaml
kind: Service
apiVersion: v1
metadata:
  name: test
spec:
  type: ClusterIP
```

不设置 type 类型时，默认为ClusterIP。

##### NodePort

###### 简介：

建立在 `ClusterIP` 之上，通过每个 Node 的IP和静态端口暴露服务，外部客户端通过访问 `NodePort` ，`NodePort`  将请求路由到集群内的 `ClusterIP` 上访问服务。

部署k8s集群时可以预留指定的端口范围，默认为：30000~32767。

###### 设置方式：

```yaml
kind: Service
apiVersion: v1
metadata:
  name: test
spec:
  type: NodePort
```

建议不需要自定义`port`,而是让集群自动分配一个 `NodePort` , 否则可能会导致端口冲突。

##### LoadBalancer

###### 简介：

建立在 NodePort 之上，通过外部的云服务提供厂商提供的负载均衡器路由至集群内的NodePort上。

###### 设置方式：

```yaml
kind: Service
apiVersion: v1
metadata:
  name: test
spec:
  type: LoadBalancer
```

##### ExternalName

###### 简介：

通过`service` 的 `externalName` 字段指定外部服务的访问地址，使得集群内部的服务可以访问到集群外部的服务。

这种类型的 `service` 不会产生 `ClusterIP`和 `NodePort` ,一般不需要使用 `labelSelector` 所以也没有 `EndPoints`。也称为 `Headless Service`。

###### 设置方式：

```yaml
kind: Service
apiVersion: v1
metadata:
  name: test
spec:
  type: ExternalName
  externalName: xxx.com
```

##### None

###### 简介：

Headless Service，不会产生 `ClusterIP` , 如果配置了 `labelSelector` 匹配到了对应的 Pod ，则会生成 `EndPoints`

###### 设置方式

```yaml
kind: Service
apiVersion: v1
metadata:
  name: test
spec:
  clusterIP: None
```

#### 参考

- <https://jimmysong.io/kubernetes-handbook/concepts/service.html>
- 《kubernetes 进阶实战》