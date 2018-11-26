### 1.Master 

master为k8s集群的控制节点，每个k8s集群需要一个master节点来负责整个集群的管理和控制，可以配置多个Master保证其高可用。

#### master节点的关键进程

- kube-apiserver：暴露k8s API
- kube-controller-manager：运行管理控制器
- kube-scheduler：Pod调度器，为Pod分配Node
- etcd：master节点上需要启动一个etcd服务，etcd是k8s默认的存储系统，保存所有资源对象的数据
- cloud-controller-manager:云控制管理器，负责与云平台交互
- addons（插件）：不同的插件对应不同的功能

#### 常用插件addons

- DNS:DNS服务
- kube-ui：容器资源监控UI
- cluster-level Logging:提供日志

### 2.Node

node为k8s集群中的工作节点，

#### node节点的关键进程

- kubelet：Pod对应容器的创建，启停；响应master完成集群控制
- kube-proxy：实现k8s svc的通信和负载均衡等
- docker engine：运行docker容器
- rkt：docker的替代方案
- supervisord:轻量级监控，监控kubelet和docker的运行
- fluentd：守护进程，可以提供日志（cluster-level logging）

### 3.Pod

#### Pod的种类

- 普通pod：一旦创建，储存在etcd中，注册到master，master将其于对应Node绑定。
- 静态pod：只在具体的Node中运行，不进行绑定，信息存放在具体的Node中

#### Pod默认策略

- pod中的某个container，k8s自动检测到，并重启pod中的所有container
- pod所在的Node宕机，k8s自动检测，并将这个pod绑定到另一个Node上，再重启pod

### 4.Replication Controller(RC)

#### Rolling Update (滚动更新)：

当前又10个旧Pod,每次停止一个Pod，同时创建一个新的Pod，运行时始终保持Pod的数量为10个，知道所有的Pod都更新完成。

### 5.Replication Set(RS)

### 6.Deployment

### 7.Horizontal Pod Autoscaler(HPA)

#### HPA Pod 负载的度量指标

- CPUUtilizationPercentage：目标Pod所有副本的CPU使用率的平均值。（CPU使用量/Pod Request）
- TPS或QPS：

### 8.Service

k8s中的服务发现机制。

三大Ip

- Node IP
- Pod IP
- Cluster IP

### 9.Volume

volume类型

- emptyDir：数据保存在Node上
- hostPath：数据挂载到宿主机
- gcePersistentDisk：数据保存在谷歌共有云提供的永久磁盘上
- awsElasticBlockStore：数据保证在亚马逊公有云提供的EBS Volume
- NFS：使用NFS网络文件系统的共享目录存储数据
- 其他Volume：（Ceph，FlusterFS）

### 10.Persistent Volume

简称PV，是k8s集群中网络存储的一块存储，独立于Pod之外，不属于任何Node，但是每一个Node都可以访问。需要和PVC文件共同使用。

#### PV的accessModes属性：

- ReadWriteOnce：单Node读写
- ReadOnlyMany：多Node只读
- ReadWriteMany：多Node读写

#### PV的状态：

- Available：空闲状态
- Bound：已经绑定到了某个PVC上
- Released：对应的PVC已经删除，但是资源还没有被集群回收
- Failed：PV自动回收失败

### 11.Namespace

不同namespace之间不可以访问。

### 12.Annotation