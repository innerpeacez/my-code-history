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

kubectl delete crd `kubectl get crd | grep istio| awk '{print $1}'`
```

### 一键部署及卸载 istio 的脚本

#### 部署脚本

```shell
#!/bin/bash

# Add istio official repo
add_repo(){
  VERSION=$1
  REPO="https://storage.googleapis.com/istio-release/releases/${VERSION}/charts/"
  helm repo add istio $REPO

  STATUS_CMD=`echo $?`
  CHECK_REPO_CMD=`helm repo list | grep $REPO | wc -l`
  echo "$STATUS_CMD"
  echo "$CHECK_REPO_CMD"
  while [[ $STATUS_CMD != 0 && $CHECK_REPO_CMD -ge 1 ]]
  do
    sleep 5
    helm repo add istio $REPO

    STATUS_CMD=`echo $?`
    CHECK_REPO_CMD=`helm repo list | grep $REPO | wc -l`
  done
}

# Create istio-system namespace
create_namespace() {
  NAMESPACE=$1
  kubectl create ns ${NAMESPACE}

  STATUS_CMD=`echo $?`
  while [[ $STATUS_CMD != 0 ]]
  do
    sleep 5
    kubectl create ns ${NAMESPACE}
    STATUS_CMD=`echo $?`
  done
}

# Create CRD need for istio
create_crd() {
  NAMESPACE=$1
  helm install istio-init istio/istio-init -n ${NAMESPACE}
  CRD_COUNT=`kubectl get crds | grep 'istio.i' | wc -l`

  while [[ ${CRD_COUNT} != 23 ]]
  do
    sleep 5
    CRD_COUNT=`kubectl get crds | grep 'istio.io' | wc -l`
  done

  echo 'Istio crd create successful'
}

# Deploy istio related components
deploy_istio() {
  NAMESPACE=$1
  VERSION=$2
  helm install istio istio/istio -n ${NAMESPACE}

  check() {
     kubectl -n ${NAMESPACE}  get deploy | grep istio | awk '{print "deployment/"$1}' | while read line ;
     do
       kubectl rollout status $line -n ${NAMESPACE};
     done
  }
  check

  echo "Istio is deployed successful"
}

main(){
  ISTIO_VERSION="1.3.3"
  ISTIO_NAMESPACE="istio-system"
  add_repo $ISTIO_VERSION
  if [[ `kubectl get ns | grep $ISTIO_NAMESPACE | wc -l ` == 0 && `kubectl get ns $ISTIO_NAMESPACE | grep -v NAME | wc -l` == 0 ]] ;then
    create_namespace $ISTIO_NAMESPACE
  fi
  create_crd $ISTIO_NAMESPACE
  deploy_istio $ISTIO_NAMESPACE $ISTIO_VERSION
}

main
```

#### 卸载脚本

```shell
#!/bin/bash

helm -n istio-system uninstall istio 
helm -n istio-system uninstall istio-init
kubectl delete crd `kubectl get crd | grep istio | awk '{print $1}'` 
kubectl delete ns istio-system
```

**注意：**卸载需谨慎，删除了 istio-system 的命名空间

### 总结

上述步骤使用的是 istio 官方提供的默认配置，如果你想要自定配置，可以阅读 values.yaml 文件后，通过 `--set` 的方式修改，或者直接修改 chart。

