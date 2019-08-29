Java 语言本身提供的定时任务相关的类，大致有以下几种，并且这几个都存在与 `java.util` 包中

![1567041635951](../images/1567041635951.png)

- Timer：Task的调度类，和TimerTask一样，暴露给最终用户使用的类，通过schedule方法安排Task的执行计划。该类通过TaskQueue和TimerThread类完成Task的调度。
- TimerTask：实现了Runnable接口，表明每一个任务均为一个独立的线程。通过run()方法提供用户定制自己任务。该类有一个比较重要的成员变量nextExecutionTime ，表示下一次执行该任务的时间。以后会看到，Timer机制就是靠这个值安排Task执行的。
- TimerThread：继承于Thread，是真正执行Task的类。
- TaskQueue：一个存储Task的数据结构，内部由一个最小堆实现，堆的每个成员为一个TimeTask，每个Task依靠其 nextExecutionTime值进行排序，也就是说，nextExecutionTime最小的任务在队列的最前端，从而能够现实最早执行。

