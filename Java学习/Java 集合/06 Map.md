# Map

## Map 接口

Map 是将键映射到值的对象。Map 中不能包含重复的 key: 每个键只能对应一个值。Map 接口包括用于基本操作（如`put`、`get`、`remove`、 `containsKey`、`containsValue`、`size`和`empty`）、批量操作（如`putAll`和`clear`）和集合视图（如`keySet`、`entrySet`和`values`）的方法。

Java 包含三个通用`Map`实现： `HashMap`、 `TreeMap`和 `LinkedHashMap`。`HashSet`的行为和性能与`LinkedHashSet` 、`TreeSet`类似。





## Map 实现方式

#### 常规实现

- HashMap
- TreeMap
- LinkedHashMap

#### 特定实现

- EnumMap
- WeakHashMap
- IdentityHashMap

#### 并发实现

- ConcurrentHashMap



