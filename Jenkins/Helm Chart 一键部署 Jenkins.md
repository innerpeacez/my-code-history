#### Jenkins 

Jenkins是一款开源 CI&CD 软件，用于自动化各种任务，包括构建、测试和部署软件。目前提供超过1000个插件来支持构建、部署、自动化， 满足任何项目的需要。

Jenkins 支持各种运行方式，可通过系统包、Docker 或者通过一个独立的 Java 程序。本文记录通过使用 Helm Chart 将 Jenkins 部署到 Kubernetes 集群中

#### 本文环境

- jenkins 2.190.3
- Kubernetes v1.14.8
- helm 3.0 release

#### 部署

1. 部署 Kubernetes
2. 配置 helm 及 kubectl 环境
3. 部署 Jenkins 

##### 部署 Kubernetes 

本文不再阐述 kubernetes 部署细节，有需要可以参考[一步步部署 Kubernetes](https://github.com/opsnull/follow-me-install-kubernetes-cluster).

##### 配置 helm 及 kubectl 环境

本文不再阐述 helm 及 kubectl 环境配置，需要自行安装.

##### 部署 Jenkins 

1. 添加 helm repo 

   ```shell
   helm repo add stable https://kubernetes-charts.storage.googleapis.com/
   ```

2. 查看目前的最近版本

   ```shell
   $ helm search repo stable/jenkins -l
   NAME          	CHART VERSION	APP VERSION	DESCRIPTION
   stable/jenkins	1.9.11       	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.10       	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.9        	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.8        	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.7        	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.6        	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.5        	lts        	Open source continuous integration server. It s...
   stable/jenkins	1.9.4        	lts        	Open source continuous integration server. It s...
   ```

3. 部署 jenkins 

   chart 中对应的镜像版本为 lts ,这个版本的镜像 jenkins 一直有在更新。所以我们只需要部署目前最新的 chart 即可，默认是没有指定 storageClass 的，需要指定。

   ```shell
   $ helm install jenkins stable/jenkins -n <namespace> \
               --set persistence.storageClass=<storageClass>
   ```

   注意：namespace 为命名空间， storageClass 为存储类

   只需这一条命令 jenkins 就已经部署完成了，并且chart 中默认安装了一些常用插件，比如 kubernetes ,并且生成了对应的配置，chart 真香：）

#### 总结

本文只是简单介绍了使用 chart 的部署过程，下一篇将会介绍 kubernetes 插件。