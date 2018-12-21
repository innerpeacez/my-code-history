## Maven生命周期

- CleanLifecycle：在构建之前完成清理
- DefaulLifecycle：构建核心部分，包括：编译，测试，打包，部署等
- SiteLifecycle：生成项目报告，站点，发布站点

注意：不同的生命周期是相互独立的。可以独立完成。

![1545387985681](../../images/1545387985681.png)

### CleanLifecycle生命周期

#### clean生命周期的各个阶段：

- pre-clean：执行一些需要在clean之前完成的工作
- clean：移除所有上一次构建生成的文件
- post-clean：执行一些需要在clean之后立刻完成的工作

#### 命令：

- mvn pre-clean
- mvn clean
- mvn post-clean