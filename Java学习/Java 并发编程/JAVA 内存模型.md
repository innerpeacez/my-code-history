### 简介

JAVA内存模型规范了JAVA虚拟机与计算机内存是如何协同工作的



### JAVA内存模型把Java虚拟机最主要的两个部分

![Java Memory Model](../images/java-memory-model-1-1551772627390.png)

- 线程栈（Thread Stack）
- 堆（Heap）



#### 哪些对象保存在栈中

- 原始类型的局部变量
- 引用类型的局部变量的引用
- 函数中的局部变量

#### 哪些对象保存在堆中

- 引用类型的本地变量的引用指向的对象
- 成员变量（原始类型，引用类型）

### 计算机内存模型

![enter image description here](../images/java-memory-model-4.png)

### Java内存模型和硬件内存架构之间的桥接

![enter image description here](../images/java-memory-model-5.png)

### volatile 关键字

![enter image description here](../images/java-memory-model-6.png)



参考：

- http://ifeve.com/java-memory-model-6/