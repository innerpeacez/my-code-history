##### 代码：

```shell
sudo sysctl -w vm.drop_caches=3
sudo sysctl -w vm.drop_caches=1
echo `date -R` >> ~/clear_cache_logs.txt
free -lh >> ~/clear_cache_logs.txt
```

清理内存 `cache` ，并将清理时间和内存剩余情况日志输入到`~/clear_cache_logs.txt`文件中，方便查看，可以结合`crontab`做定时清理内存`cache`的定时任务。