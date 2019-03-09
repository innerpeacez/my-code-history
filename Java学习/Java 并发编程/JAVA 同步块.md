### Java 中同步关键字（**synchronized**）



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

synchronized 关键字使用在实例方法上,也就是对象方法，一个 new 出来的对象的方法上使用了 synchronized 关键修饰，那么这个方法就是同步方法，针对这个对象调用该同步方法时，每次只允许一个线程调用该同步方法，当有其他线程调用这个同步方法时，将被阻塞，直到上一个线程将结果刷新到主内存完全退出时，其他线程才能调用该同步方法，保证的线程安全性。

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

每次执行的结果都是符合预期1000。