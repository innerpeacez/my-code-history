### Helm Hook架构图

![helm_arch.jpg](http://dockone.io/uploads/article/20170916/46d748c8534621eb3040a4745ba90153.jpg)

### Helm组件

Helm有以下两个组成部分：

Helm Client是用户命令行工具，其主要负责如下：

- 本地chart开发
- 仓库管理
- 与Tiller sever交互
- 发送预安装的chart
- 查询release信息
- 要求升级或卸载已存在的release
- Tiller Server是一个部署在Kubernetes集群内部的server，其与Helm client、Kubernetes API server进行交互。

Tiller server主要负责如下：

- 监听来自Helm client的请求
- 通过chart及其配置构建一次发布
- 安装chart到Kubernetes集群，并跟踪随后的发布
- 通过与Kubernetes交互升级或卸载chart

简单的说，client管理charts，而server管理发布release。

### Helm安装