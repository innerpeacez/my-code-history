### Dockerfile

dockerfile 可以分为以下四个部分

- 基础镜像信息
- 维护者信息
- 镜像操作指令
- 容器启动执行指令

#### 使用dockerfile构建

```shell
docker build -f Dockerfile -t testdocker:v2 . （. 不可省略）
```

#### dockerfile常用指令

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