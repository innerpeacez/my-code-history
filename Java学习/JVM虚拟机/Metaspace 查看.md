```shell
jstat -gcutil <pid> <time> 查看内存使用情况
jstat -options
-class  #类加载情况 加载个数和空间使用
-compiler #即时编译器信息
-gc  # GC情况 包括 young gc、full gc 次数、时间等
-gccapacity #年轻代、老年代的使用情况
-gccause #GC 统计信息和回收原因
-gcmetacapacity #显示有关metaspace大小的统计信息
-gcnew #新生代 GC 统计
-gcnewcapacity #新生代内存统计
-gcold #老年代 GC 统计
-gcoldcapacity #老年代内存使用情况
-gcutil #GC 汇总信息
-printcompilation #编译方法统计

-gcutil
S0：年轻代中第一个survivor（幸存区）已使用的占当前容量百分比
S1：年轻代中第二个survivor（幸存区）已使用的占当前容量百分比
E：年轻代中Eden（伊甸园）已使用的占当前容量百分比
O：old代已使用的占当前容量百分比
M：Metaspace已使用的占当前容量百分比
YGC：从应用程序启动到采样时年轻代中gc次数
YGCT：从应用程序启动到采样时年轻代中gc所用时间(s)
FGC：从应用程序启动到采样时old代(全gc)gc次数
FGCT：从应用程序启动到采样时old代(全gc)gc所用时间(s)
GCT：从应用程序启动到采样时gc用的总时间(s)

-gcmetacapacity
NGCMN - 最小的新生代的大小
NGCMX - 最大的新生代大小
NGC - 目前新生代的大小
S0C - survivor 0区域的容量
S1C - survivor 1区域的容量
EC - Eden区域的容量
OGCMN - 最小old代大小
OGCMX - 最大old代大小
OGC - 目前old代大小
OC - 目前old space大小
MCMN - 最小metaspace大小
MCMX - 最大metaspace大小
MC - Metaspace元数据区的 Committed Size
CCSMN - Compressed class空间的最小容量
CCSMX - Compressed class空间的最大容量
CCSC - Compressed class的Committed Size
YGC - young generation的GC次数
FGC - full GC的次数


手动dump gc 日志
jmap -dump:format=b,file=./xx.hprof <pid>

jps -lv 查看当前的 java 进程

jstack -l pid 查看线程状态，栈使用情况
jstack -m pid，打印 Java 和 Native 栈信息

jinfo -flags pid 查看 JVM 参数，其中 Non-default VM flags 是虚拟机默认设置的参数，Command line 是用户指定的参数，比如命令行启动 jar 包的时候加上的参数。
jinfo -flag 参数名 pid 可以查看指定参数的值，比如查看堆的最大值(-XX:MaxHeapSize 也就是 -Xmx ):
jinfo -flag MetaspaceSize pid 查看metaspace 初始化size 单位byte
jinfo -flag MaxMetaspaceSize pid 查看 metaspace 最大size 单位byte
```



