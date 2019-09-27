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