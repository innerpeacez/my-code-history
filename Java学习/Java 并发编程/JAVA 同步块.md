### Java 中同步关键字（**synchronized**）

> Java语言的关键字，可用来给对象和方法或者代码块加锁，当它锁定一个方法或者一个代码块的时候，同一时刻最多只有一个线程执行这段代码。当两个并发线程访问同一个对象object中的这个加锁同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块。然而，当一个线程访问object的一个加锁代码块时，另一个线程仍可以访问该object中的非加锁代码块。



<center>
    <img style="border-radius: 0.5125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"
    src="../images/webwxgetmsgimg.gif">
    <br>
    <div style="color:orange; border-bottom: 0px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 1px;">这够同步么！</div>
</center>

#### 4种不同类型的同步块

- 实例同步方法
- 类（静态）同步方法
- 实例方法中的同步代码块
- 类（静态）方法中的同步代码块

##### 实例同步方法

synchronized 关键字使用在实例方法上,也就是对象方法，一个 new 出来的对象的方法上使用了 synchronized 关键修饰，那么这个方法就是同步方法，针对这个对象调用该同步方法时，每次只允许一个线程调用该同步方法，当有其他线程调用这个同步方法时，将被阻塞，直到上一个线程将结果刷新到主内存完全退出时，其他线程才能调用该同步方法，保证了线程安全性。

```java
public class MySynchronized {

    int value = 0 ;

    public synchronized void add(int value) {
        this.value += value;
    }
}
```

测试：

```java
public void add(int value) {
    this.value += value;
}
```
先去除synchronized关键字，由于直接new Thread操作没办法保证主线程运行完时其他线程也运行完成，所以我这里使用了java 8中的并行流模拟多线程操作。

```java
@Test
public void test1() throws InterruptedException {
    MySynchronized mySynchronized = new MySynchronized();

    ArrayList<Integer> nums = new ArrayList<>();
    for (int i = 0; i < 1000; i++) {
        nums.add(1);
    }
    nums.parallelStream().forEach(num -> mySynchronized.add(num));

    System.out.println(mySynchronized.value);
}
```

多运行几次可以看出结果并不时我们预想的1000，再次测试加上synchnized关键字之后的结果。

```java
public synchronized void add(int value) {
    this.value += value;
}
```

每次执行的结果都是符合预期1000。这里同步的对象是`this`。

##### 实例中的同步代码块

```java
public class MySynchronized {

    int value = 0;

    public void add(int value) {
        synchronized (this) {
            this.value += value;
        }
    }
}
```

当方法中只有一部分代码需要保证线程安全时，就可以使用同步代码块，作用与同步方法基本相同，这里我们使用的是`this`对象来同步，`this`代表的是调用这个方法的实例对象，如果实例A的同步方法`add()`无法同步实例B的`add()`方法

##### 静态同步方法

```java
public class MySynchronized {

    static int value = 0;

    public static synchronized void add(int value) {
        value = value;
    }
}
```

使用`static`和`synchronized`关键字共同修饰的方法。针对类对象的同步，同一时间只能有一个线程调用静态同步方法，同一个类对象在`java`虚拟机中只能有一个。同步的对象是`MySynchronized.class`

##### 静态方法中的同步方法块

```
public class MySynchronized {

    static int value = 0;

    public static void add2(int value) {

        synchronized (MySynchronized.class) {
            value = value;
        }
    }
}
```

和静态同步方法类似，这里我使用的同步对象是`MySynchronized.class`，当然我们也可以使用其他对象来实现同步。

**注意：**如果一个同步方法调用另一个同步方法，同时使用的同步对象为同一个对象，则这个两个同步方法同时只能被一个线程调用。

#### 参考：

- https://baike.baidu.com/item/synchronized
- http://ifeve.com/synchronized-blocks/