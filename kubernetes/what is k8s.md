### k8s可以做什么？

- 快速部署应用
- 快速扩展应用
- 无缝对接新的应用功能
- 节省资源，优化硬件资源的使用

### k8s特点

- **可移植**: 支持公有云，私有云，混合云，多重云（multi-cloud）
- **可扩展**: 模块化, 插件化, 可挂载, 可组合
- **自动化**: 自动部署，自动重启，自动复制，自动伸缩/扩展

### 部署方式

- 传统部署
- 容器部署

### 容器优势总结

- **快速创建/部署应用：**与VM虚拟机相比，容器镜像的创建更加容易。
- **持续开发、集成和部署：**提供可靠且频繁的容器镜像构建/部署，并使用快速和简单的回滚(由于镜像不可变性)。
- **开发和运行相分离：**在build或者release阶段创建容器镜像，使得应用和基础设施解耦。
- **开发，测试和生产环境一致性：**在本地或外网（生产环境）运行的一致性。
- **云平台或其他操作系统：**可以在 Ubuntu、RHEL、 CoreOS、on-prem、Google Container Engine或其它任何环境中运行。
- **Loosely coupled，分布式，弹性，微服务化：**应用程序分为更小的、独立的部件，可以动态部署和管理。
- **资源隔离**
- **资源利用：**更高效

### k8s能做什么？

- [多个进程（作为容器运行）协同工作。](http://docs.kubernetes.org.cn/312.html)（Pod）
- 存储系统挂载
- Distributing secrets
- 应用健康检测
- [应用实例的复制](http://docs.kubernetes.org.cn/314.html)
- Pod自动伸缩/扩展
- Naming and discovering
- 负载均衡
- 滚动更新
- 资源监控
- 日志访问
- 调试应用程序
- [提供认证和授权](http://docs.kubernetes.org.cn/51.html)

### Kubernetes不是什么？

- Kubernetes不限制支持应用的类型，不限制应用框架。限制受支持的语言runtimes (例如, Java, Python, Ruby)，满足[12-factor applications](https://12factor.net/) 。不区分 “apps” 或者“services”。 Kubernetes支持不同负载应用，包括有状态、无状态、数据处理类型的应用。只要这个应用可以在容器里运行，那么就能很好的运行在Kubernetes上。
- Kubernetes不提供中间件（如message buses）、数据处理框架（如Spark）、数据库(如Mysql)或者集群存储系统(如Ceph)作为内置服务。但这些应用都可以运行在Kubernetes上面。
- Kubernetes不部署源码不编译应用。持续集成的 (CI)工作流方面，不同的用户有不同的需求和偏好的区域，因此，我们提供分层的 CI工作流，但并不定义它应该如何工作。
- Kubernetes允许用户选择自己的日志、监控和报警系统。
- Kubernetes不提供或授权一个全面的应用程序配置 语言/系统（例如，[jsonnet](https://github.com/google/jsonnet)）。
- Kubernetes不提供任何机器配置、维护、管理或者自修复系统。