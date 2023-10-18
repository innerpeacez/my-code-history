```
kubectl delete namespace cert --grace-period=0 --force
```





```shell
export NS=xxx && for resource in `kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get -o name -n $NS`; do kubectl patch $resource -p '{"metadata": {"finalizers": []}}' --type='merge' -n $NS; done
```

