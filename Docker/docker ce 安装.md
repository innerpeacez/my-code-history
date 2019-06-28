1.安装所需的包。`yum-utils`提供了`yum-config-manager` 效用，并`device-mapper-persistent-data`和`lvm2`由需要 `devicemapper`存储驱动程序。

```shell
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

2.使用以下命令设置**稳定**存储库。

```shell
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

3.安装*最新版本*的Docker CE和containerd

```shell
sudo yum install docker-ce docker-ce-cli containerd.io
```

4.启动docker

```shell
sudo systemctl start docker
```

5.将docker 加入开启启动

```shell
systemctl enable docker.service
```

