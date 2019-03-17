### JAVA内存模型

稍微了解计算机的内存模型之后，我们再来看看Java的内存模型。

![Java Memory Model](../../Java%E5%AD%A6%E4%B9%A0/images/1552810356340.png)

Java内存模型将JVM划分为两个部分：

- 线程栈（Thread Stack）
- 堆（Heap）

那么Java中的变量到底保存在哪呢？这是有明确规定的。

##### 哪些保存在栈中

- 原始类型局部变量
- 引用类型局部变量的引用

##### 哪些保存在堆中

- 引用类型局部变量的引用指向的对象
- 成员变量（原始类型，引用类型）

保存在栈中的变量是线程安全的，保存在堆中的变量是线程共享的，虽然引用类型局部变量的引用保存在栈中，但是其指向的对象保存在堆中，所以也会存在线程不安全。

分别学习了计算机内存模型和Java语言的内存模型之后，但我们还不知道Java内存模型怎么在真实的计算机中运行的呢？

### Java内存模型和硬件内存架构之间的桥接

![enter image description here](../../Java%E5%AD%A6%E4%B9%A0/images/java-memory-model-5.png)



### volatile 关键字

![enter image description here](../../Java%E5%AD%A6%E4%B9%A0/images/java-memory-model-6.png)



参考：

- http://ifeve.com/java-memory-model-6/
- https://baike.baidu.com/item/java%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B/3399303