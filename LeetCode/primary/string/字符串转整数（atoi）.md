----------

### [GitHub地址](https://github.com/innerpeacez/leetcode)

----------

### 题目

实现 `atoi`，将字符串转为整数。

在找到第一个非空字符之前，需要移除掉字符串中的空格字符。如果第一个非空字符是正号或负号，选取该符号，并将其与后面尽可能多的连续的数字组合起来，这部分字符即为整数的值。如果第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

字符串可以在形成整数的字符后面包括多余的字符，这些字符可以被忽略，它们对于函数没有影响。

当字符串中的第一个非空字符序列不是个有效的整数；或字符串为空；或字符串仅包含空白字符时，则不进行转换。

若函数不能执行有效的转换，返回 0。

**说明：**

假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,  2^31 − 1]。如果数值超过可表示的范围，则返回  INT_MAX (2^31 − 1) 或 INT_MIN (−2^31) 。

### 示例

**示例 1:**

```
输入: "42"
输出: 42
```

**示例 2:**

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

**示例 3:**

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

**示例 4:**

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

**示例 5:**

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

### 代码

因为之前有看过Integer的源码，所以这个问题很多想法都是参照Integer的parseInt()这个方法的，LeetCode 85% 。

```java
public static boolean isValidSign(char ch) {
    return ch == '+' || ch == '-';
}

public static boolean isVaildNumber(char ch) {
    return ch >= '0' && ch <= '9';
}

public static int myAtoi(String str) {
    String trim = str.trim();
    boolean negative = false;
    int len = trim.length();
    int result = 0;
    if (len <= 0) return result;
    char firstChar = trim.charAt(0);
    long res = 0;

    if (isValidSign(firstChar) || isVaildNumber(firstChar)) {
        if (isVaildNumber(firstChar)) {
            res = Integer.valueOf(String.valueOf(firstChar));
            result = (int) res;
        }
        if (isValidSign(firstChar)) {
            if (len == 1) return 0;
            if (firstChar == '-') negative = true;
        }
        for (int i = 1; i < trim.length(); i++) {
            if (isVaildNumber(trim.charAt(i))) {
                res = res * 10 + Integer.valueOf(String.valueOf(trim.charAt(i)));
                if (res > Integer.MAX_VALUE) {
                    if (negative) {
                        if (-res <= Integer.MIN_VALUE) {
                            return Integer.MIN_VALUE;
                        }
                    }
                    return Integer.MAX_VALUE;
                }
                result = (int) res;
            }else {
                break;
            }
        }
    }
    return negative ? -result : result;
}
```

下面也把Integer的parseInt()方法的源码贴出来吧。

```java
public static int parseInt(String s, int radix)
            throws NumberFormatException
{
    /*
     * WARNING: This method may be invoked early during VM initialization
     * before IntegerCache is initialized. Care must be taken to not use
     * the valueOf method.
     */

    if (s == null) {
        throw new NumberFormatException("null");
    }

    if (radix < Character.MIN_RADIX) {
        throw new NumberFormatException("radix " + radix +
                                        " less than Character.MIN_RADIX");
    }

    if (radix > Character.MAX_RADIX) {
        throw new NumberFormatException("radix " + radix +
                                        " greater than Character.MAX_RADIX");
    }

    int result = 0;
    boolean negative = false;
    int i = 0, len = s.length();
    int limit = -Integer.MAX_VALUE;
    int multmin;
    int digit;

    if (len > 0) {
        char firstChar = s.charAt(0);
        if (firstChar < '0') { // Possible leading "+" or "-"
            if (firstChar == '-') {
                negative = true;
                limit = Integer.MIN_VALUE;
            } else if (firstChar != '+')
                throw NumberFormatException.forInputString(s);

            if (len == 1) // Cannot have lone "+" or "-"
                throw NumberFormatException.forInputString(s);
            i++;
        }
        multmin = limit / radix;
        while (i < len) {
            // Accumulating negatively avoids surprises near MAX_VALUE
            digit = Character.digit(s.charAt(i++),radix);
            if (digit < 0) {
                throw NumberFormatException.forInputString(s);
            }
            if (result < multmin) {
                throw NumberFormatException.forInputString(s);
            }
            result *= radix;
            if (result < limit + digit) {
                throw NumberFormatException.forInputString(s);
            }
            result -= digit;
        }
    } else {
        throw NumberFormatException.forInputString(s);
    }
    return negative ? result : -result;
}
```