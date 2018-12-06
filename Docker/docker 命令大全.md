### 拉取镜像

```bash
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

[选项]
-a, --all-tags                Download all tagged images in the repository
​     --disable-content-trust   Skip image verification (default true)
​     --platform string         Set platform if server is multi-platform capable

### 运行容器

操作

```dockerfile
docker run [OPTIONS] IMAGE [COMMAND][ARG...]
```

| Options                                  | 说明                                |
| ---------------------------------------- | ----------------------------------- |
| -d, --detach                             | 后台执行容器                        |
| -e, --env list                           | 配置容器的环境变量                  |
| -i, --interactive                        | 保持stdin (常与 –t一起使用进入容器) |
| --name                                   | 容器名                              |
| --network string                         | 配置容器网络                        |
| -p, --publish list<br/>-P, --publish-all | 把容器端口映射到主机端口            |
| -t, --tt                                 | 为容器创建tty                       |
| -v, --volume list<br/>--volumes-from     | 把挂载绑定到容器目录                |

例子

```bash
docker run -it --rm ubuntu:16.04 bash
```

- `-it`：这是两个参数，一个是 `-i`：交互式操作，一个是 `-t` 终端。我们这里打算进入 `bash` 执行一些命令并查看返回结果，因此我们需要交互式终端。
- `--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 `docker rm`。我们这里只是随便执行个命令，看看结果，不需要排障和保留结果，因此使用 `--rm` 可以避免浪费空间。
- `ubuntu:16.04`：这是指用 `ubuntu:16.04` 镜像为基础来启动容器。
- `bash`：放在镜像名后的是**命令**，这里我们希望有个交互式 Shell，因此用的是 `bash`。

### 列出镜像

```bash
docker image ls [OPTIONS][REPOSITORY[:TAG]]
```

```
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --help            Print usage
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs
```

#### 列出已经下载了的镜像（列出顶级镜像）

```bash
docker image ls
```

```bash
REPOSITORY      TAG                 IMAGE ID            CREATED             SIZE
mysql           latest              6a834f03bd02        5 weeks ago         484 MB
tomcat-app      v1                  a29e200a18e9        2 years ago         358 MB
pause-amd64     3.0                 99e59f495ffa        2 years ago         747 kB
```

列表包含了 `仓库名`、`标签`、`镜像 ID`、`创建时间` 以及 `所占用的空间`。

其中仓库名、标签在之前的基础概念章节已经介绍过了。**镜像 ID** 则是镜像的唯一标识，一个镜像可以对应多个**标签**。因此，在上面的例子中，我们可以看到 `ubuntu:16.04` 和 `ubuntu:latest` 拥有相同的 ID，因为它们对应的是同一个镜像。

注意：size一般大于docker repo中的大小，那是由于上传镜像镜像仓库的时候进行了压缩处理，并且size的总和也不代表硬盘的使用大小，因为Docker使用Union FS进行分层存储，不同的镜像使用相同的层都是同一层，相同的层只保存一份。

#### 列出虚悬镜像

```bash
docker image ls -f dangling=true
```

注意：虚悬镜像产生的原因是下载了相同版本（tag）的镜像，新旧镜像同名，旧镜像名称被取消，docker pull 和docker build 时都有可能产生虚悬镜像，虚悬镜像已经失去了存在价值，可以使用下面命令进行删除

```bash
docker image prune
```

列出中间层镜像（列出所有镜像）

```bash
docker image ls -a
```

中间层镜像主要是加速镜像构建，重复利用资源

#### 列出部分镜像

```bash
docker image ls [仓库名（REPOSITORY）]
docker image ls [仓库名（REPOSITORY）]：[标签（TAG）]
```

#### 列表过滤器参数（--filter 缩写 -f）

查看`mongo:3.2` 之后建立的镜像

```bash
$ docker image ls -f since=mongo:3.2
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              6a834f03bd02        3 days ago          198 MB
nginx               latest              a29e200a18e9        3 days ago          182 MB
```

### 制作镜像

退出容器

```
ctrl p ctrl q
```

### Dockerfile

使用dockerfile构建

```shell
docker build -f Dockerfile -t testdocker:v2 .
```

dockerfile常用指令

| 指令        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| FROM        | 指定所创建镜像的基础镜像                                     |
| RUN         | 运行命令                                                     |
| CMD         | 指定启动容器时默认执行的命令                                 |
| LABEL       | 指定生成镜像的元数据标签信息                                 |
| EXPOSE      | 声明镜像内服务所监听的端口                                   |
| ENV         | 指定环境变量                                                 |
| ADD         | 赋值指定的<src>路径下的内容到容器中的<dest>路径下，<src>可以为URL；如果为tar文件，会自动解压到<dest>路径下 |
| ENTRYPOINT  | 指定镜像的默认入口                                           |
| VOLUME      | 创建数据挂载点                                               |
| USER        | 指定运行容器时的用户名或UID                                  |
| WORKDIR     | 配置工作目录                                                 |
| ARG         | 指定镜像内使用的参数(例如版本号信息等)                       |
| ONBUILD     | 配置当前所创建的镜像作为其他镜像的基础镜像时，所执行的创建操作的命令 |
| STOPSIGNAL  | 容器退出的信号                                               |
| HEALTHCHECK | 如何进行健康检查                                             |
| SHELL       | 指定使用SHELL时的默认SHELL类型                               |

