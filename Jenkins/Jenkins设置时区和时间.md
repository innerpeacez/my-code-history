Jenkins时区和时间问题：

方案一(最简单有效)：

在【系统管理】-【脚本命令行】里运行

```
System.setProperty('org.apache.commons.jelly.tags.fmt.timeZone', 'Asia/Shanghai')
```

方案二：

改容器时区
```
docker run ... -e JAVA_OPTS=-Duser.timezone=Asia/Shanghai
```

