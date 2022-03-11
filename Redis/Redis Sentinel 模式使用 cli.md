sentinel 模式下 slave 节点是无法写入数据的，此时如果需要使用 cli 往 redis 写入数据，可以通过



```bash
SENTINEL masters ：列出所有被监视的主服务器，以及这些主服务器的当前状态；
SENTINEL slaves <master name> ：列出给定主服务器的所有从服务器，以及这些从服务器的当前状态；
SENTINEL get-master-addr-by-name <master name> ： 返回给定名字的主服务器的 IP 地址和端口号。 如果这个主服务器正在执行故障转移操作， 或者针对这个主服务器的故障转移操作已经完成， 那么这个命令返回新的主服务器的 IP 地址和端口号；
SENTINEL reset <pattern> ： 重置所有名字和给定模式 pattern 相匹配的主服务器。 pattern 参数是一个 Glob 风格的模式。 重置操作清楚主服务器目前的所有状态， 包括正在执行中的故障转移， 并移除目前已经发现和关联的， 主服务器的所有从服务器和 Sentinel ；
SENTINEL failover <master name> ： 当主服务器失效时， 在不询问其他 Sentinel 意见的情况下， 强制开始一次自动故障迁移。 （不过发起故障转移的 Sentinel 会向其他 Sentinel 发送一个新的配置，其他 Sentinel 会根据这个配置进行相应的更新）

SENTINEL MONITOR <name> <ip> <port> <quorum> 这个命令告诉sentinel去监听一个新的master
SENTINEL REMOVE <name> 命令sentinel放弃对某个master的监听
SENTINEL SET <name> <option> <value> 这个命令很像Redis的CONFIG SET命令，用来改变指定master的配置。支持多个<option><value>。例如以下实例：SENTINEL SET objects-cache-master down-after-milliseconds 1000
```



1. 任意进入一个 redis 节点，查看所有master节点

   ```bash
   sentinel masters 
   ```

2. 找到master之后，在进入master

   ```bash
   redis-cli -h <ip> -p <port> -a <password>
   ```

3. 此时即可使用 redis-cli 中的命令了 GET SET 等等

