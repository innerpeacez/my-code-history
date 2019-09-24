#### CompletableFuture

```java
//使用默认线程池
public static CompletableFuture<Void> runAsync(Runnable runnable)
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier)
//可以指定线程池  
public static CompletableFuture<Void> runAsync(Runnable runnable, Executor executor)
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier, Executor executor)  
```

runAsync 与 supplyAsync 方法的主要区别在于，runAsync 接受的 Runnale 的 run 方法是没有返回值的



#### CompletionStage

1. 串行关系

   ```java
   CompletionStage<R> thenApply(fn);
   CompletionStage<R> thenApplyAsync(fn);
   CompletionStage<Void> thenAccept(consumer);
   CompletionStage<Void> thenAcceptAsync(consumer);
   CompletionStage<Void> thenRun(action);
   CompletionStage<Void> thenRunAsync(action);
   CompletionStage<R> thenCompose(fn);
   CompletionStage<R> thenComposeAsync(fn);
   ```

2. And 聚合关系

   ```java
   CompletionStage<R> thenCombine(other, fn);
   CompletionStage<R> thenCombineAsync(other, fn);
   CompletionStage<Void> thenAcceptBoth(other, consumer);
   CompletionStage<Void> thenAcceptBothAsync(other, consumer);
   CompletionStage<Void> runAfterBoth(other, action);
   CompletionStage<Void> runAfterBothAsync(other, action);
   ```

3. OR 聚合关系

   ```java
   CompletionStage applyToEither(other, fn);
   CompletionStage applyToEitherAsync(other, fn);
   CompletionStage acceptEither(other, consumer);
   CompletionStage acceptEitherAsync(other, consumer);
   CompletionStage runAfterEither(other, action);
   CompletionStage runAfterEitherAsync(other, action);
   ```

4. 异常处理（异步线程的异常处理）

   ```java
   CompletionStage exceptionally(fn);
   CompletionStage<R> whenComplete(consumer);
   CompletionStage<R> whenCompleteAsync(consumer);
   CompletionStage<R> handle(fn);
   CompletionStage<R> handleAsync(fn);
   ```


