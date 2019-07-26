简介：

主要原因是，我不会 vim ，在 linux 上修改 charts 的很蹩脚，所以就想着能不能再windows上执行 helm 命令，将 charts install linux 上搭建的 kubernetes 集群上，答案当然是可以的。

本文环境

- kubernetes 1.15
- helm 3 alpha1
- kubectl 

Helm 3 是什么？

kubectl 是什么？



调教经历

安装helm3

下载

设置 path



```yaml
apiVersion: v1
generated: "2019-07-23T09:39:54.849920932+08:00"
repositories:
- caFile: ""
  cache: /root/.helm/repository/cache/stable-index.yaml
  certFile: ""
  keyFile: ""
  name: stable
  password: ""
  url: https://kubernetes-charts.storage.googleapis.com
  username: ""
```

helm init 

安装 kubectl 

下载

设置path

配置 kubernetes config 文件

 