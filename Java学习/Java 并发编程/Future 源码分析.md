#### Future 接口

```java
package java.util.concurrent;

public interface Future<V> {

    boolean cancel(boolean mayInterruptIfRunning);

    boolean isCancelled();

    boolean isDone();

    V get() throws InterruptedException, ExecutionException;

    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutException;
}
```

- cancel(boolean mayInterruptIfRunning) :取消任务
- isCancel() :判断任务是否已经取消
- isDone() :判断任务是否已经结束
- get() :获取任务执行的结果
- get(long timeout, TimeUnit unit) : 获取任务执行结果，支持超时