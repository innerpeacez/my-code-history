#### 配置全局代理

http 代理

```shell
git config http.proxy http://127.0.0.1:8088
```

https 代理

```shell
git config https.proxy http://127.0.0.1:8088
```

#### 查看全局代理情况

```shell
git config --get --global http.proxy
```

```shell
git config --get --global https.proxy
```

#### 取消全局代理

```shell
git config --unset --global http.proxy
```

```shell
git config --unset --global https.proxy
```

