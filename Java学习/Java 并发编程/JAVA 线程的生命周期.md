

#### 生命周期

![img](../images/9bbc6fa7fb4d631484aa953626cf6ae5.png)

1. **初始状态**，指的是线程已经被创建，但是还不允许分配CPU执行。这个状态属于编程语言特有的，不过这里所谓的被创建，仅仅是在编程语言层面被创建，而在操作系统层面，真正的线程还没有创建。
2. **可运行状态**，指的是线程可以分配CPU执行。在这种状态下，真正的操作系统线程已经被成功创建了，所以可以分配CPU执行。
3. 当有空闲的CPU时，操作系统会将其分配给一个处于可运行状态的线程，被分配到CPU的线程的状态就转换成了**运行状态**。
4. 运行状态的线程如果调用一个阻塞的API（例如以阻塞方式读文件）或者等待某个事件（例如条件变量），那么线程的状态就会转换到**休眠状态**，同时释放CPU使用权，休眠状态的线程永远没有机会获得CPU使用权。当等待的事件出现了，线程就会从休眠状态转换到可运行状态。
5. 线程执行完或者出现异常就会进入**终止状态**，终止状态的线程不会切换到其他任何状态，进入终止状态也就意味着线程的生命周期结束了。



#### Java语言中线程共有六种状态，分别是：

1. NEW（初始化状态）
2. RUNNABLE（可运行/运行状态）
3. BLOCKED（阻塞状态）
4. WAITING（无时限等待）
5. TIMED_WAITING（有时限等待）
6. TERMINATED（终止状态）

![img](../images/3f6c6bf95a6e8627bdf3cb621bbb7f8c.png)

Java线程中的BLOCKED、WAITING、TIMED_WAITING是一种状态，即前面我们提到的休眠状态。也就是说**只要Java线程处于这三种状态之一，那么这个线程就永远没有CPU的使用权**。



#### JAVA中各种状态的转换

- 从NEW到RUNNABLE状态
  - 线程调用 start() 方法
- RUNNABLE与BLOCKED的状态转换
  - synchronized修饰的方法、代码块同一时刻只允许一个线程执行，其他线程只能等待，这种情况下，等待的线程就会从RUNNABLE转换到BLOCKED状态

- RUNNABLE与WAITING的状态转换
  - 获得synchronized隐式锁的线程，调用无参数的Object.wait()方法
  - 调用无参数的Thread.join()方法
  - 调用LockSupport.park()方法
- RUNNABLE与TIMED_WAITING的状态转换
  - 调用**带超时参数**的Thread.sleep(long millis)方法；
  - 获得synchronized隐式锁的线程，调用**带超时参数**的Object.wait(long timeout)方法；
  - 调用**带超时参数**的Thread.join(long millis)方法；
  - 调用**带超时参数**的LockSupport.parkNanos(Object blocker, long deadline)方法；
  - 调用**带超时参数**的LockSupport.parkUntil(long deadline)方法。
- 从RUNNABLE到TERMINATED状态
  - 线程执行完 run() 方法后，会自动转换到TERMINATED状态
  - 如果执行run()方法的时候异常抛出，也会导致线程转换到TERMINATED状态
  - 调用stop()或者interrupt()方法，stop()方法已经废弃，不建议使用

**stop()和interrupt()方法的主要区别**

stop()方法会真的杀死线程，不给线程喘息的机会，如果线程持有synchronized隐式锁，也不会释放，那其他线程就再也没机会获得synchronized隐式锁，这实在是太危险了。所以该方法就不建议使用了，类似的方法还有suspend() 和 resume()方法，这两个方法同样也都不建议使用了，所以这里也就不多介绍了。

而interrupt()方法就温柔多了，interrupt()方法仅仅是通知线程，线程有机会执行一些后续操作，同时也可以无视这个通知。被interrupt的线程，是怎么收到通知的呢？一种是异常，另一种是主动检测。

当线程A处于WAITING、TIMED_WAITING状态时，如果其他线程调用线程A的interrupt()方法，会使线程A返回到RUNNABLE状态，同时线程A的代码会触发InterruptedException异常。上面我们提到转换到WAITING、TIMED_WAITING状态的触发条件，都是调用了类似wait()、join()、sleep()这样的方法，我们看这些方法的签名，发现都会throws InterruptedException这个异常。这个异常的触发条件就是：其他线程调用了该线程的interrupt()方法。

当线程A处于RUNNABLE状态时，并且阻塞在java.nio.channels.InterruptibleChannel上时，如果其他线程调用线程A的interrupt()方法，线程A会触发java.nio.channels.ClosedByInterruptException这个异常；而阻塞在java.nio.channels.Selector上时，如果其他线程调用线程A的interrupt()方法，线程A的java.nio.channels.Selector会立即返回。

上面这两种情况属于被中断的线程通过异常的方式获得了通知。还有一种是主动检测，如果线程处于RUNNABLE状态，并且没有阻塞在某个I/O操作上，例如中断计算圆周率的线程A，这时就得依赖线程A主动检测中断状态了。如果其他线程调用线程A的interrupt()方法，那么线程A可以通过isInterrupted()方法，检测是不是自己被中断了。

