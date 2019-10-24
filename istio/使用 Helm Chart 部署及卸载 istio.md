### 部署 istio 

1.添加 istio 官方的 helm 仓库

```shell
helm repo add istio   https://storage.googleapis.com/istio-release/releases/1.3.3/charts/
```

2.是否添加成功

```shell
helm search repo istio
```

> NAME                    CHART VERSION   APP VERSION     DESCRIPTION                        
> istio/istio             1.3.3           1.3.3           Helm chart for all istio components
> istio/istio-cni         1.3.3           1.3.3           Helm chart for istio-cni components
> istio/istio-init        1.3.3           1.3.3           Helm chart to initialize Istio CRDs

3.创建 istio-system 命名空间

```shell
kubectl create ns istio-system
```

4.创建 istio 所需的 crd 文件

```shell
helm install istio-init istio/istio-init -n istio-system
```

5.检查 CRD 文件是否创建完成，输出为23

```shell
kubectl get crds | grep 'istio.io' | wc -l
```

> *23*

6.部署 istio

```shell
helm install istio istio/istio -n istio-system
```

> NAME: istio
> LAST DEPLOYED: Thu Oct 24 12:05:06 2019
> NAMESPACE: istio-system
> STATUS: deployed
> REVISION: 1
> NOTES:
> Thank you for installing Istio.
>
> Your release is named Istio.
>
> To get started running application with Istio, execute the following steps:
> 1. Label namespace that application object will be deployed to by the following command (take default namespace as an example)
>
>  kubectl label namespace default istio-injection=enabled
> ​ kubectl get namespace -L istio-injection
>
> 2. Deploy your applications
>
> $ kubectl apply -f <your-application>.yaml
>
> For more information on running Istio, visit:
> https://istio.io/

### 卸载 istio

1.卸载 istio 

```shell
helm -n istio-system uninstall istio
```

2.删除 istio crd 文件

```shell
helm -n istio-system uninstall istio-init
```

### 总结

上述步骤使用的是 istio 官方提供的默认配置，如果你想要自定配置，可以阅读 values.yaml 文件后，通过 `--set` 的方式修改。