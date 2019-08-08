### Hooks的作用

Helm 提供了一个 hook 机制，允许 chart 开发人员在 release 的生命周期中的某些点进行干预。

- 在加载任何其他 chart 之前，在安装过程中加载 ConfigMap 或 Secret。
- 在安装新 chart 之前执行作业以备份数据库，然后在升级后执行第二个作业以恢复数据。
- 在删除 release 之前运行作业，以便在删除 release 之前优雅地停止服务。

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

### Hooks 和 release 的生命周期

Helm 为 install 生命周期定义了两个 hook：`pre-install` 和 `post-install`。如果 `foo` chart 的开发者实现了两个 hook，那么生命周期就像这样改变：

- 用户运行 `helm install foo`
- chart 被加载到 Tiller 中
- 经过一些验证后，Tiller 渲染 `foo` 模板
- Tiller 准备执行 `pre-install` hook（将 hook 资源加载到 Kubernetes 中）
- Tiller 会根据权重对 hook 进行排序（默认分配权重 0），并按相同权重的 hook 按升序排序。
- Tiller 然后装载最低权重的 hook（从负到正）
- Tiller 等待，直到 hook“准备就绪”
- Tiller 将产生的资源加载到 Kubernetes 中。请注意，如果设置 `--wait` 标志，Tiller 将等待，直到所有资源都处于就绪状态，并且在准备就绪之前不会运行 `post-install`hook。
- Tiller 执行 `post-install` hook（加载 hook 资源）
- Tiller 等待，直到 hook“准备就绪”
- Tiller 将 release 名称（和其他数据）返回给客户端
- 客户端退出

### Hooks的权重

设置方式

![1543372133900](F:\innerpeacez_github\my-code-history\images\1543372133900.png)

权重的执行顺序

- 由负到正
- 从小到大

默认权重

- 默认为0

hook的删除策略

![1543372500230](F:\innerpeacez_github\my-code-history\images\1543372500230.png)

- "hook-succeeded"  ：指定 Tiller 应该在 hook 成功执行后删除 hook。
- "hook-failed"  ：指定如果 hook 在执行期间失败，Tiller 应该删除 hook。
- "before-hook-creation" ： 指定 Tiller 应在删除新 hook 之前删除以前的 hook。

