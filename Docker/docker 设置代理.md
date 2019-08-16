创建目录

```shell
mkdir /etc/systemd/system/docker.service.d
```

写入代理配置

```powershell
vim /etc/systemd/system/docker.service.d/http-proxy.conf


[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80/" # 代理的地址
```

docker daemon 刷新配置

```shell
sudo systemctl daemon-reload
```

查看配置是否生效

```shell
sudo systemctl show --property Environment docker
```

重启docker

```shell
sudo systemctl restart docker
```