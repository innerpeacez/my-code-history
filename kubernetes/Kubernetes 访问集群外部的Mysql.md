遇到个需求：kubernets 集群内服务想要访问外部的 Mysql , 之前虽然知道kuberbetes 支持这个方式，但是没有实践过，俗话说实践出真知。下面就说一下我的实践过程

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
    - port: 3306
```

```
kind: Endpoints
apiVersion: v1
metadata:
  name: mysql
subsets:
  - addresses:
      - ip: <your ip>
    ports:
      - port: 3306
```