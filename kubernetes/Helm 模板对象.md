## Helm 模板中可以访问的顶级对象

- Release
- Values
- Chart
- Files
- Capabilities
- Template

### Release对象

| 对象名            | 描述                                                 |
| ----------------- | ---------------------------------------------------- |
| Release.Name      | Release 名称                                         |
| Release.Time      | Release 时间                                         |
| Release.Namespace | Release 的 namespace                                 |
| Release.Service   | Release 服务的名称（总是Tiller）                     |
| Release.Revision  | Release 版本号，从 1 开始，每次 helm upgrade 时递增1 |
| Release.IsUpgrade | 如果当前操作是升级或回滚，则设置为true。             |
| Release.IsInstall | 如果当前操作是安装，则设置为true。                   |

### Values对象

从 `values.yaml` 文件和用户提供的文件传入模板的值。默认情况下，Values 是空的。在values.yaml文件中，可以定义任何想要给模板文件访问的内容。

#### values.yaml文件

##### values对象的值四种来源

1. chart包values.yaml文件
2. 如果包含子chart包，父chart包中的value.yaml中定义的值，将覆盖子chart包。
3. 可以使用`-f`参数传递值到chart包的values.yaml中。如：(`helm install -f myvals.yaml ./mychart`)
4. 使用命令行传入指定的值，如：（`helm install --set foo=bar ./mychart`）

##### 四种值的优先级：

4>3>2>1，高优先级将覆盖低优先级中定义的相同`key`对应的`value`。

##### 删除value.yaml中的key

如果您需要从默认值中删除一个键，可以覆盖该键的值为 null，在这种情况下，Helm 将从覆盖值合并中删除该键。

### Chart对象

`Chart.yaml` 文件的内容。Chart.yaml中的定义任何数据都使用Chart对象访问。如`{{.Chart.Name}}-{{.Chart.Version}}`

#### Chart.yaml中可以包含以下字段

```yaml
apiVersion: The chart API version, always "v1" (required)
name: The name of the chart (required)
version: A SemVer 2 version (required)
kubeVersion: A SemVer range of compatible Kubernetes versions (optional)
description: A single-sentence description of this project (optional)
keywords:
  - A list of keywords about this project (optional)
home: The URL of this project's home page (optional)
sources:
  - A list of URLs to source code for this project (optional)
maintainers: # (optional)
  - name: The maintainer's name (required for each maintainer)
    email: The maintainer's email (optional for each maintainer)
    url: A URL for the maintainer (optional for each maintainer)
engine: gotpl # The name of the template engine (optional, defaults to gotpl)
icon: A URL to an SVG or PNG image to be used as an icon (optional).
appVersion: The version of the app that this contains (optional). This needn't be SemVer.
deprecated: Whether this chart is deprecated (optional, boolean)
tillerVersion: The version of Tiller that this chart requires. This should be expressed as a SemVer range: ">2.0.0" (optional)
```

### Files对象

用于访问在chart包中自定义的一些文件，无法使用它来访问模板文件。

| 对象名         | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| Files.Get      | 按名称获取文件的函数，如：（`.Files.Get config.ini`）获取config.ini文件 |
| Files.GetBytes | 将文件内容作为字节数组而不是字符串获取的函数。这对于像图片这样的东西很有用。 |

### Capabilities

提供了有于 Kubernetes 集群支持的功能的信息。

| 对象名                                | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| Capabilities.APIVersions              | 集群版本信息                                                 |
| Capabilities.APIVersions.Has $version | 指示是否在群集上启用了版本（batch/ v1）                      |
| Capabilities.KubeVersion              | 提供了查找Kubernetes版本的方法。可以获取到 `Major`，`Minor`，`GitVersion`，`GitCommit`，`GitTreeState`，`BuildDate`，`GoVersion`，`Compiler`和`Platform`。 |
| Capabilities.TillerVersion            | 提供了查找Tiller版本的方法。可以获取到`SemVer`, `GitCommit`, and `GitTreeState`. |

### Template对象

包含当前正在执行的模板的信息。

| 对象名   | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| Name     | 当前模板的`namespace`文件路径（例如mychart / templates / template.yaml） |
| BasePath | 当前 chart 模板目录的`namespace`路径（例如 mychart/templates）。 |

