# 队列

## 队列接口 （The Queue Interface）

Queue 用户保存处理之前的元素集合。除了 Collection 接口提供的操作以外，队列还提供了插入，删除和检查操作。Queue 接口如下：

```java
public interface Queue<E> extends Collection<E> {

    boolean add(E e);

    boolean offer(E e);

    E remove();

    E poll();

    E element();

    E peek();
}
```

每种`Queue`方法都以两种形式存在：

(1) 如果操作失败，一种会抛出异常

(2) 如果操作失败，另一种会返回一个特殊值（要么 要么`null`，`false`取决于操作）。

接口的常规结构 如下表所示。

| Type of Operation | Throws exception | Returns special value |
| ----------------- | ---------------- | --------------------- |
| Insert            | `add(e)`         | `offer(e)`            |
| Remove            | `remove()`       | `poll()`              |
| Examine           | `element()`      | `peek()`              |

队列通常但不一定以 FIFO（先进先出）方式对元素进行排序。其中的例外是优先队列，它根据元素的值对元素进行排序。无论使用何种顺序，队列的头部是将通过调用`remove()`or `pool()`删除元素。在 FIFO 队列中，所有新元素都插入到队列的尾部。其他类型的队列可能使用不同的放置规则。每个`Queue`实现都必须指定其排序属性。

一个队列的实现可以限制它拥有的元素数量，这样的队列被称为有界的。`java.util.concurrent` 包中的一般是有界的，`java.util` 一般是无界的。

`add()`方法`Queue`继承自`Collection`，插入一个元素，除非它违反队列的容量限制，在这种情况下它会抛出`IllegalStateException`。该`offer`方法仅用于有界队列，与`add` 的不同仅在插入元素失败时返回`false`。

`remove()`和`poll()`方法都是删除并返回队列的头部。究竟删除哪个元素取决于队列的排序策略。只有当队列为空时，`remove()`和`poll()`方法的行为才会不同。在这种情况下，`remove ()` 抛出 NoSuchElementException 而 `poll()` 返回 `null`。

`element()` 和 `peek()` 返回但不删除队头元素。 他们之前的不同于 `remove()` 和 `poll()` 完全一样。队列为空时，`element()`抛出 NoSuchElementException，`peek()`返回null。

Queue 一般不允许插入 null 元素，LinkedList 实现的 Queue 是个例外，由于历史原因，它可以插入 null 元素，但应该尽量避免这一点，因为 null 值比较特殊，回合 peek() 和 poll() 出错是返回值一样。

队列实现通常不重写 `equals()` 和`hashCode()`方法，而是继承 Object 类的。

该`Queue`接口没有定义并发编程中常见的阻塞队列方法。这些等待元素出现或空间可用的方法在 java.util.concurrent.BlockingQueue 扩展的接口中定义。

# Queue 在 java 中的实现类

Queue 的实现分为通用实现和并发实现

#### 通用实现

`LinkedList` 实现了 Queue 接口，实现了 FIFO(先进先出) 队列的 add(), poll() 等方法

`PriorityQueue` 是基于堆数据结构的优先队列，该队列根据构造时指定的排序方式对元素进行排序，顺序可以是元素的自然顺序，也可以是显式 Compator 的顺序。

队列的检索操作: `poll(), remove(), peek(), element()` 都是访问的队头元素。队头元素是根据指定排序方式的最小元素。

`PriorityQueue` 及其迭代器的实现，是实现了 `Collection` 和 `Iterator` 接口的所有方法。方法迭代器中提供的迭代器不能保证以任何特定的顺序遍历 `PriorityQueue` 的元素。对于有序遍历，可以考虑使用 `Arrays.sort (pq.toArray ())`。

#### 并发实现

`java.util.concurrent` 包中包含了一些列同步的接口和类。BlockingQueue 使用以下操作扩展 Queue: 在检索元素时等待队列变为非空，在存储元素时等待队列中的空间变为可用。

BlockingQueue 有以下实现方式

- LinkedBlockingQueue 通过链表数据结构实现的可选的有界 FIFO（先进先出） 等待队列
- ArrayBlockingQueue 通过数组数据结构实现的有界 FIFO（先进先出） 等待队列
- PriorityBlockingQueue 通过堆实现的无界优先等待队列
- DelayQueue 通过堆实现的基于时间调度的阻塞队列
- SynchronousQueue 

JDK7 中还提供了一种特殊的 BlockingQueue 接口 TransferQueue, 其向队列添加元素的代码可以选择等待(阻塞)另一个线程中的代码检索该元素

, 该接口只有一种实现

- LinkedTransferQueue 通过链表数据结构实现了 TransferQueue 接口的等待队列

