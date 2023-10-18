## Happens-before

划重点：前面一个操作的结果对后续操作是可见的，Happens-before 规则基本上都关于可见性

```java
class VolatileExample {
  int x = 0;
  volatile boolean v = false;
  public void writer() {
    x = 42;
    v = true;
  }

  public void reader() {
    if (v == true) {
      //x的值是多少呢？
    }
  }
}
```

线程A调用 writer() 方法，线程B调用reader()方法。x 的值为多少，为什么？

#### 1. 程序的顺序性规则（Program Order Rule）

一个线程中，按照代码编写的顺序，前面的操作 Happens-Before 于后面的任意操作

程序中 x=42 会在 v=true 之前执行。

#### 2. volatile 变量规则（Volatile Variable Rule）

一个 volatile 变量的写操作 Happens-Before 于后续对这个 volatile 变量的读操作

#### 3. 传递性（Transitivity）

A 操作 Happens-Before 于 B 操作，B 操作 Happens-Before 于 C 操作, 那么 A 操作 Happens-Before 于 C 操作

#### 4. 管程中的锁规则（Monitor Lock Rule）

一个锁的解锁 Happens-Before 与后续对个锁的加锁操作

```java
public class Test{
    private int x = 0;
    public void initX{
        synchronized(this){ //自动加锁
            if(this.x < 10){
                this.x = 10;
            }
        } //自动释放锁
    }
}
```

#### 5. 线程 start() 规则（Thread Start Rule）

父线程 A 中启动子线程 B 后，父线程 A 在启动子线程 B 之前的操作对子线程 B 可见

```java
//在线程A中初始化线程B
Thread threadB = new Thread(()->{
    //此处的变量x的值是多少呢？答案是100
});
//线程A在启动线程B之前将共享变量x的值修改为100
x = 100;
//启动线程B
threadB.start();
```

#### 6. 线程 join() 规则（Thread Termination Rule）

父线程 A 等待子线程 B 完成操作之后，子线程 B 的操作对父线程 A 可见

```java
Thread threadB = new Thread(()-{
    //在线程B中，将共享变量x的值修改为100
    x = 100;
});
//在线程A中启动线程B
threadB.start();
//在线程A中等待线程B执行完成
threadB.join();
//此处访问共享变量x的值为100
```

#### 7. 线程 **interrupt()** 规则（Thread Interruption Rule）

对线程A 中断子线程 B 的操作完成时，父线程 A 的操作对子线程 B 可见

```java
//在线程A中将x变量的值初始化为0
private int x = 0;

public void execute(){
  //在线程A中初始化线程B
  Thread threadB = new Thread(()->{
    //线程B检测自己是否被中断
    if (Thread.currentThread().isInterrupted()){
      //如果线程B被中断，则此时X的值为100
      System.out.println(x);
    }
  });
  //在线程A中启动线程B
  threadB.start();
  //在线程A中将共享变量X的值修改为100
  x = 100;
  //在线程A中中断线程B
  threadB.interrupt();
}
```

#### 8. 对象终结规则 （Finalizer Rule）

一个对象的初始化完成 Happens-Before 于它的 finalize() 方法的开始。

```java
public class TestThread {

   public TestThread(){
       System.out.println("构造方法");
   }

    @Override
    protected void finalize() throws Throwable {
        System.out.println("对象销毁");
    }

    public static void main(String[] args){
        new TestThread();
        System.gc();
    }
}
```

运行结果如下所示。

```java
构造方法
对象销毁
```