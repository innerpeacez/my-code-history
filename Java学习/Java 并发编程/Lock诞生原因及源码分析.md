#### 诞生原因

Java 提供了 syncronized 后，又提供了 Lock 原因在于 syncronized 在获取到不到锁是将会一直阻塞，如果一个线程获取到了 A 锁，尝试获取 B 锁时，另一个线程已经获取到了 B 锁，尝试获取 A 锁。这是 syncronized 不会释放自身持有的锁，而是会一直阻塞，这样就会导致死锁的产生

而解决这种问题的方法，便由 Lock 提供

```java
// 支持中断的API
void lockInterruptibly() 
throws InterruptedException;
// 支持超时的API
boolean tryLock(long time, TimeUnit unit) 
throws InterruptedException;
// 支持非阻塞获取锁的API
boolean tryLock();
```

1. **能够响应中断**。synchronized的问题是，持有锁A后，如果尝试获取锁B失败，那么线程就进入阻塞状态，一旦发生死锁，就没有任何机会来唤醒阻塞的线程。但如果阻塞状态的线程能够响应中断信号，也就是说当我们给阻塞的线程发送中断信号的时候，能够唤醒它，那它就有机会释放曾经持有的锁A。这样就破坏了不可抢占条件了。
2. **支持超时**。如果线程在一段时间之内没有获取到锁，不是进入阻塞状态，而是返回一个错误，那这个线程也有机会释放曾经持有的锁。这样也能破坏不可抢占条件。
3. **非阻塞地获取锁**。如果尝试获取锁失败，并不进入阻塞状态，而是直接返回，那这个线程也有机会释放曾经持有的锁。这样也能破坏不可抢占条件。

```java
public interface Lock {

    void lock();

    void lockInterruptibly() throws InterruptedException;

    boolean tryLock();

    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

    void unlock();

    Condition newCondition();
}
```

