<center>
    <img style="border-radius: 0.5125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 0px 10px 0 rgba(34,36,38,.08);"
    src="../images/1561780948294.png">
    <br>
    <div style="color:orange; border-bottom: 0px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">boom</div>
</center>

#### 安装 crontab

```shell
yum install crontabs
```

#### 启动/关闭

```shell
service crond start // 启动服务
service crond stop // 关闭服务
service crond restart // 重启服务
service crond reload // 重新载入配置
```

#### 查看 crontab 服务是否已设置为开机启动

```shell
systemctl list-unit-files | grep enable | grep crond
```

#### 将 crontab 加入开机自动启动 

```shell
chkconfig crond on
// 或者
systemctl enable crond.service
```

#### 查看 crontab 状态

```shell
service crond status // 查看crontab服务状态
```

#### 编写定时任务

- 命令格式

```shell
min hour day month dayofweek command
 分  时   天    月    星期几      命令
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
> ​      command：指定要执行的命令

- 编辑命令两种方式
  1. 在命令行输入: crontab -e 然后添加相应的任务，wq存盘退出
  2. 直接编辑/etc/crontab 文件，即vi /etc/crontab，添加相应的任务

- 时间格式

> ​      \* ：表示任意的时刻；如小时位 * 则表示每个小时
>
> ​      n ：表示特定的时刻；如小时位 5 就表示5时
>
> ​      n,m ：表示特定的几个时刻；如小时位 1,10 就表示1时和10时
>
> ​      n－m ：表示一个时间段；如小时位 1-5 就表示1到5点
>
> ​      */n : 表示每隔多少个时间单位执行一次；如小时位 */1 就表示每隔1个小时执行一次命令，也可以写成 1-23/1

#### 小栗子

```shell
* 1 * * * ~/clear_cache.sh ：从 1:00 到 1:59 每隔1分钟执行一次脚本
0 * * * * ~/clear_cache.sh ：每个小时的 0 分钟执行一次脚本
*/10 * * * * ~/clear_cache.sh ：每隔10分执行一次脚本
```