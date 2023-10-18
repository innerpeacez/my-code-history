# 集合框架

*核心集合接口*封装了不同类型的集合，如下图所示。这些接口允许独立于它们的表示细节来操作集合。核心集合接口是 Java 集合框架的基础。如下图所示，核心集合接口形成层次结构。

![两棵接口树，一棵以Collection开头，包括Set、SortedSet、List、Queue，一棵以Map开头，包括SortedMap。](https://docs.oracle.com/javase/tutorial/figures/collections/colls-coreInterfaces.gif)

Set 是一种特殊的 Collection, SortedSet 是一种特殊的 Set。

以下列表描述了核心集合接口：

- **Collection** 集合框架的根接口。集合表示一组元素的对象，接口是所有集合实现的 `Collection` 最小公分母，用于传递集合并在需要最大通用性时操作集合。某些类型的集合允许重复元素，而其他类型则不允许。有些是有序的，有些是无序的。Java 平台不提供此接口的任何直接实现，但提供更具体的子接口的实现，例如`Set`和`List`。

- **Set**
- **List**
- **Queue** 用户处理保存多个元素的集合，除了 Collection 接口提供的操作以外，Queue 接口提供了额外的插入，提取和检查操作。
  - 队列通常（但不一定）以FIFO（先进先出）的方式对元素进行排序。其中优先队列更具提供的比较器或元素的自然顺序进行排序。
  - 无论如何排序，队列的头部都是通过调用 remove() 或者 poll() 方法来删除元素。
  - 在 FIFO 队列中，所以新增的原始都会被插入到队尾，其他类型的队列可能使用不同的规则，但是必须要指定排序方法。

- **Deque**
- **Map**

最后两种核心集合接口是仅仅是有序的Set和Map

- **SortedSet**
- **SortedMap**