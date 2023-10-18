## Synchroniezed

```java
class X {
  // 修饰非静态方法
  synchronized void foo() {
    // 临界区
  }
  // 修饰静态方法
  synchronized static void bar() {
    // 临界区
  }
  // 修饰代码块
  Object obj = new Object()；
  void baz() {
    synchronized(obj) {
      // 临界区
    }
  }
}  
```

Java的一条隐式规则：

- 当修 synchronized 饰静态方法的时候，锁定的是当前类的Class对象，在上面的例子中就是Class X
- 当修 synchronized 饰非静态方法的时候，锁定的是当前实例对象 this

#### 同步代码块

```java
package com.zhw.java.study.syncronized;

public class Test1 {

    public void test1() {
        synchronized (this) {
            System.out.println("hello synchronized code block");
        }
    }

    public static void main(String[] args) {
        new Test1().test1();
    }
}
```



#### 同步方法

```java
package com.zhw.java.study.syncronized;

public class Test2 {

    public synchronized void test2() {
        System.out.println("hello synchronized method");
    }

    public static void main(String[] args) {
        new Test2().test2();
    }
}
```