----------

### [GitHub地址](https://github.com/innerpeacez/leetcode)

----------

### 题目

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。

### 示例

**示例 1:**

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

**示例 2:**

```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

### 代码

这个算法String已经实现了

```java
public static int strStr(String haystack, String needle) {
    return haystack.indexOf(needle);
}
```

所以来看一下String的indexOf()方法的源码吧。

```java
/**
 * Code shared by String and StringBuffer to do searches. The
 * source is the character array being searched, and the target
 * is the string being searched for.
 *
 * @param   source       源字符串对应的char[]
 * @param   sourceOffset 源字符串的偏移量
 * @param   sourceCount  源字符串的长度
 * @param   target       目标字符串对应的char[]
 * @param   targetOffset 目标字符串的偏移量
 * @param   targetCount  目标字符串的长度
 * @param   fromIndex    从源字符串的哪个索引位置开始搜索
 */
static int indexOf(char[] source, int sourceOffset, int sourceCount,
        char[] target, int targetOffset, int targetCount,
        int fromIndex) {
    // 判断开始索引是否大于source的长度
    if (fromIndex >= sourceCount) {
        // 如果大于，判断target是否为"",是:返回sourceCount；否：-1
        return (targetCount == 0 ? sourceCount : -1);
    }
    // 调整开始索引小于0的fromIndex为0.
    if (fromIndex < 0) {
        fromIndex = 0;
    }
    // 如果target为"",返回开始的搜索的位置的索引
    if (targetCount == 0) {
        return fromIndex;
    }
	
    // target偏移后的第一个字符
    char first = target[targetOffset];
    // 遍历的最大索引位置
    int max = sourceOffset + (sourceCount - targetCount);
    // 从source偏移后的位置开始遍历搜索
    for (int i = sourceOffset + fromIndex; i <= max; i++) {
        // 搜索source上等于target第一个字符的索引位置
        if (source[i] != first) {
            while (++i <= max && source[i] != first);
        }

        // 判断第一个字符后的其余字符
        if (i <= max) {
            // 为了不影响到i（source第一个字符的位置），创建一个新的变量j记录第一个字符后的一个索引位置。
            int j = i + 1;
            // source遍历结束位置
            int end = j + targetCount - 1;
            // 用j和k双指针遍历source和target，判断其余字符是否相等。
            for (int k = targetOffset + 1; j < end && source[j]
                    == target[k]; j++, k++);
		
          	// j==end证明target在source上被找到了
            if (j == end) {
                // 找到了整个字符串，返回第一个字符对应的索引
                return i - sourceOffset;
            }
        }
    }
    // 找不到返回-1
    return -1;
}
```