#### 查看登录信息

```shell
cat .docker/config.json 
```

#### 镜像的离线导入与导出

导出：

```shell
docker save <imageId> -o <saveName>.tar
```

导入：

```shell
docker load -i <saveName>.tar
```

