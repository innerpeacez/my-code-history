# 列表(List)

列表是有序的集合，列表中可以包含重复的元素，除了从 Collection 接口继承的操作以外，列表还包含以下操作：

- 位置访问：根据元素在列表中的位置来操作元素。包括 get, set, add, addAll, remove 这些方法
- 搜索：根据元素寻找其在列表中的值。包括 indexOf, lastIndexOf
- 迭代：扩展 Iterator 来利用列表的顺序特性。listIterator 方法提供了这种行为
- Range-view：subList 方法获取列表指定方位类元素。

## 列表通用实现：ArrayList, LinkedList

### ArrayList

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    @java.io.Serial
    private static final long serialVersionUID = 8683452581122892189L;

    /**
     * 默认初始化容量为10
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * 共享空实例
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     * 用于默认大小的空实例的共享空数组实例。将其与 EMPTY_ELEMENTDATA 区分开来，以了解添加第一个元素时要膨胀多少。
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
     * 存储 ArrayList 元素的数组缓冲区。 ArrayList 的容量就是这个数组缓冲区的长度。 
     * 添加第一个元素时，任何具有 elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA 的空 ArrayList 都将扩展为 DEFAULT_CAPACITY。
     */
    transient Object[] elementData; 

    /**
     * 元素个数
     */
    private int size;

    /**
     * 构造一个指定容量的空列表
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
          	// 指定容量大于0时，创建指定容量的空数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            // 指定容量为0时，使用共享空数组
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            // 指定容量小于0时，抛出异常
            throw new IllegalArgumentException("Illegal Capacity: "+ initialCapacity);
        }
    }

    /**
     * 构造一个初始容量为10的空列表
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * 构造一个包含指定集合元素的列表，顺序按照指定集合元素的迭代顺序
     */
    public ArrayList(Collection<? extends E> c) {
      	// 指定的集合转为数组
        Object[] a = c.toArray();
        // 列表的元素个数为指定集合的元素个数
        if ((size = a.length) != 0) {
          	// 个数不为0时
            if (c.getClass() == ArrayList.class) {
                // 集合的类型为 ArrayList 时
                elementData = a;
            } else {
                elementData = Arrays.copyOf(a, size, Object[].class);
            }
        } else {
            // 指定集合为空集合时，将列表的缓冲区修改为 EMPTY_ELEMENTDATA
            elementData = EMPTY_ELEMENTDATA;
        }
    }
  ......
}    
```

#### 集合操作

继承了 Collection 接口中的所有操作，remove() 方法会删除链表中的第一个匹配的元素，add, addAll 会在列表的末尾添加新的元素到列表中。所以可以用下面的方式连接两个列表：

```java
list1.addAll(list2)
```

Jdk 8+ 中提供了聚合同字段到新的列表中的操作

```java
List<String> list = people.stream()
.map(Person::getName)
.collect(Collectors.toList());
```

equals 方法可以判断两个列表中的元素及位置是否完全相同；

#### 位置访问

基本的位置访问操作有：get, set, add, remove. set 会覆盖掉原位置的元素， remove 会删除原位置的元素，其他的

##### get(int index)

```java
		/**
			返回指定位置的元素
		*/
		public E get(int index) {
      	// 检查索引是否正确
        Objects.checkIndex(index, size);
        // 返回索引位置的元素
        return elementData(index);
    }

    @ForceInline
    public static
    int checkIndex(int index, int length) {
        return Preconditions.checkIndex(index, length, null);
    }

    @IntrinsicCandidate
    public static <X extends RuntimeException> int checkIndex(int index, int length, BiFunction<String, List<Number>, X> oobef) {
        if (index < 0 || index >= length)
            throw outOfBoundsCheckIndex(oobef, index, length);
        return index;
    }

    @SuppressWarnings("unchecked")
    E elementData(int index) {
        return (E) elementData[index];
    }
```

##### set(int index, E element)

```java
    /**
    	替换列表中指定位置的元素
    */
    public E set(int index, E element) {
        // 检查索引是否正确
        Objects.checkIndex(index, size);
        // 获取原位置的元素
        E oldValue = elementData(index);
        // 替换为新元素
        elementData[index] = element;
        // 返回原位置的元素
        return oldValue;
    }
```

##### add(E e)

```java
		/**
			添加指定元素到列表的尾部
		*/
		public boolean add(E e) {
      	// 修改次数加1
        modCount++;
      	// 添加新元素
        add(e, elementData, size);
        return true;
    }
    
		private void add(E e, Object[] elementData, int s) {
      	// 判断是否需要扩容，当容量等于元素个数时
        if (s == elementData.length)
            elementData = grow();
        // 新元素放到列表的末尾
        elementData[s] = e;
      	// 列表元素个数加1
        size = s + 1;
    }

    private Object[] grow() {	
      	// 元素个数加1判断是否需要扩容
        return grow(size + 1);
    }

    private Object[] grow(int minCapacity) {
      	// 记录原缓冲区的容量
        int oldCapacity = elementData.length;
      	
        if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
          	// 缓冲区容量扩容为原来的1.5倍
            int newCapacity = ArraysSupport.newLength(oldCapacity,
                    minCapacity - oldCapacity, /* minimum growth */
                    oldCapacity >> 1           /* preferred growth */);
            return elementData = Arrays.copyOf(elementData, newCapacity);
        } else {
          	// 添加第一个元素时，创建默认容量（或者指定容量的）的缓冲区
            return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
        }
    }
```

##### remove(int index)

```java
    public E remove(int index) {
      	// 索引检查
        Objects.checkIndex(index, size);
        final Object[] es = elementData;
				// 记录指定位置的元素
        @SuppressWarnings("unchecked") E oldValue = (E) es[index];
        fastRemove(es, index);
				// 返回删除前的元素
        return oldValue;
    }

    private void fastRemove(Object[] es, int i) {
        modCount++;
        final int newSize;
      	// 删除中间元素时，将指定位置之后的元素整体向前移动
        if ((newSize = size - 1) > i)
            System.arraycopy(es, i + 1, es, i, newSize - i);
      	// 将最后位置的元素置null
        es[size = newSize] = null;
    }
```

#### 搜索

##### indexOf(Object o)

```java
    // 返回正序第一次出现元素的索引
		public int indexOf(Object o) {
        return indexOfRange(o, 0, size);
    }

    int indexOfRange(Object o, int start, int end) {
        Object[] es = elementData;
        // 判断元素是否为 null
        if (o == null) {
            // 迭代寻找第一个为空的元素
            for (int i = start; i < end; i++) {
                if (es[i] == null) {
                    return i;
                }
            }
        } else {
            // 不为 null 时，迭代寻找第一个出现的元素
            for (int i = start; i < end; i++) {
                if (o.equals(es[i])) {
                    return i;
                }
            }
        }
        return -1;
    }
```

##### lastIndexOf(Object o)

```java
   	// 返回倒序第一次出现元素的索引
    public int lastIndexOf(Object o) {
        return lastIndexOfRange(o, 0, size);
    }

    int lastIndexOfRange(Object o, int start, int end) {
        Object[] es = elementData;
        // 判断元素是否为 null
        if (o == null) {
            // 倒序迭代寻找第一个为空的元素
            for (int i = end - 1; i >= start; i--) {
                if (es[i] == null) {
                    return i;
                }
            }
        } else {
        		// 不为 null 时，倒序迭代寻找第一个出现的元素
            for (int i = end - 1; i >= start; i--) {
                if (o.equals(es[i])) {
                    return i;
                }
            }
        }
        return -1;
    }
```

### LinkedList



## 特殊实现： CopyOnWriteArrayList