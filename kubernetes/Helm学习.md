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



### Helm 安装





### 支持Hook类型

Helm提供了如下hook供chart开发者使用：

```
pre-install：在模板文件被渲染之后、而在Kubernetes创建任何资源创建之前执行。
post-install：在Kubernetes加载全部的资源之后执行。
pre-delete：在Kubernetes删除任何resource之前执行。
post-delete：在一个release的全部资源被删除之后执行。
pre-upgrade：在模板渲染之后，而在Kubernetes加载任何资源之前执行。
post-upgrade：在Kubernetes更新完全部resource之后执行。
pre-rollback：在模板被渲染之后、而在Kubernetes执行对全部resource的回滚之前。
post-rollback：在Kubernetes的全部resource已经被修改之后执行。
```