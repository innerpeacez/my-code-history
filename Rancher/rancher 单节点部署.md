![1561779593413](../images/1561779593413.png)

#### 安装：

```shell
yum install crontabs
```

#### 启动/关闭命令：

```shell
service crond start // 启动服务
service crond stop // 关闭服务
service crond restart // 重启服务
service crond reload // 重新载入配置
service crond start //手动启动crontab服务
```

#### 查看 crontab 服务是否已设置为开机启动:

```shell
systemctl list-unit-files | grep enable | grep crond
```

#### 加入开机自动启动:

```shell
chkconfig crond on
或者
systemctl enable crond.service
```

#### 查看 crontab 状态:

```shell
service crond status // 查看crontab服务状态
```

#### 编写定时任务

#####  1、编辑命令

- 命令格式：

```shell
 min hour day month dayofweek command
 分钟 小时  天   月     星期几    命令
```

> ​      min：每个小时的第几分钟执行该任务；取值范围0-59
>
> ​      hour：每天的第几个小时执行该任务；取值范围0-23
>
> ​      day：每月的第几天执行该任务；取值范围1-31
>
> ​      month：每年的第几个月执行该任务；取值范围1-12
>
> ​      dayofweek：每周的第几天执行该任务；取值范围0-6，0表示周末
>
> ​      command：指定要执行的程序路径

- 在命令行输入: `crontab -e` 然后添加相应的任务，wq存盘退出  或 直接编辑`/etc/crontab` 文件，即vi /etc/crontab，添加相应的任务

- 时间格式：

> ​      \* ：表示任意的时刻；如小时位 * 则表示每个小时
>
> ​      n ：表示特定的时刻；如小时位 5 就表示5时
>
> ​      n,m ：表示特定的几个时刻；如小时位 1,10 就表示1时和10时
>
> ​      n－m ：表示一个时间段；如小时位 1-5 就表示1到5点
>
> ​      */n : 表示每隔多少个时间单位执行一次；如小时位 */1 就表示每隔1个小时执行一次命令，也可以写成 1-23/1

#####   2、小栗子

```shell
0 1 * * * ~/clear_cache.sh ：每天1点执行清理cache脚本
* 1 * * * ~/clear_cache.sh ：从每天1点到2点，每分钟执行一次脚本
0 1 * * 1 ~/clear_cache.sh ：每周一的1点执行脚本
```