## MacOS DNS 配置方法

#### 1、列出所有网络连接方式

```bash
networksetup -listallnetworkservices
```

#### 1-1、配置dns

##### 给指定网络配置dns（单独配置）

```bash
networksetup -setdnsservers Wi-Fi 8.8.8.8
```

##### 1-2、配置 dns 文件（文件配置）

在 /etc/resolver 目录下创建 DNS 文件，将dns配置写入文件

```bash
nameserver 8.8.8.8
```

#### 2、查看本机使用的 DNS

```bash
scutil --dns
```

#### 3、重启网卡

