## Package 包 

main包是程序入口，每个程序只有一个main包

### 导包的两种方式：

- 打包导入

```go
import (
	"fmt"
	"math"
)
```

- 多语句导入

```go
import "fmt"
import "math"
```

### 导出名：

​	大写的方法名为导出名，小写不是。

## 函数：

- 分开写参数类型：

```go
func add(x int,y int) int {
	return x + y
}
```

- 相同参数类型：

```go
func add(x ,y int) int {
	return x + y
}
```

- 多返回值

```go
func addAndSub(x int, y int) (int, int) {
	var add = x + y
	var sub = x - y
	return add, sub
}
```



- 命名返回值

```go
func split(num int) ( /*十位数*/ dig2 /*个位数*/, dig1 int) {
	dig1 = num * 4 / 9
	dig2 = num - dig1
	return dig2, dig1
}
```

函数定义用func关键字 ，可以有多个参数，写法类似C（参数名 参数类型），返回值类型写在参数后面

## 变量

### 定义变量

使用`var`关键字定义变量，并初始化。

```go
var name string = innerpeacez
```

定义多个变量

```go
var (
	v1 = 10
	v2 = 20
)
```

### 短声明变量

使用`:=`符号，简洁的声明变量并赋值，`:=`不可以使用再函数（`func`）之外。函数外的每一个语句必须以关键字开始（`var`,`func`等）



### 基本变量类型

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
     // 代表一个Unicode码

float32 float64

complex64 complex128
```

### 基本类型零值（默认值）

![1542629367057](F:\innerpeacez_github\my-code-history\images\1542629367057.png)



### 类型转换

表达式 `T(v)` 将值 `v` 转换为类型 `T`。

```go
var _int int = 10
var _float float64 = float64(_int)
var _uint uint = uint(_float)

_int2 := 10
_float2 := float64(_int2)
_uint2 := uint(_float2)

_int3 := 10
_float3 := _int3
_uint3 := _float3
```

### 类型推断

在定义一个变量但不指定其类型时（使用没有类型的 `var` 或 `:=` 语句）， 变量的类型由右值推导得出。

## 常量

使用`const`关键字定义；常量可以是字符、字符串、布尔或数字类型的值；常量不能使用 `:=` 语法定义。未指定类型的常量，由上下文类型推断决定其类型。

```go
import "fmt"

const Pi = 3.141592653

func main() {
   const HelloWorld = "你好,世界"
   // PI = 1234,常量无法修改左值
   fmt.Println(Pi)
   fmt.Println(HelloWorld)
}
```

定义多个常量

```go
const (
	Name = "innerpeacez"
	Age  = 14
)
```

## for循环

`go`中只有一种循环结构----`for`循环,不能使用`()`,必须有`{}`

```go
sum := 0
for i := 0; i < 10; i++ {
   sum += i
}
fmt.Println(sum)
```

没有前置和后置的`for`，相当于`java`中的`while`

```go
sum := 0
for sum < 100 {
   sum += sum
}
fmt.Println(sum)
```

死循环（省略了循环条件）

```go
for {
}
```
## if判断

不能使用()，必须有`{}`，其他和java相似

```go
x := 0
if x < 100 {
    fmt.Print("x 小于100")
}
```

`go`中`if`在执行前可以执行一个简单的语句，执行语句与判断条件之间用`;`隔开，执行语句的作用域为当前`if`

```go
if y := 500; y > 100 {
   fmt.Println("y 大于100")
}
```

同样也可以有`if..else...`

## switch语句

switch执行顺序，从上到下，满足条件则停止

```go
today := time.Now().Weekday()

switch time.Saturday {
case today:
   fmt.Println("今天")
case today + 1:
   fmt.Println("明天")
case today + 2:
   fmt.Println("后天")
default:
   fmt.Println("好多天后")
}
```

没有条件的语句，可以在case代码块写条件，这要就可以执行多条件

```go
t := time.Now()
switch {
case t.Hour() < 12:
   fmt.Println("Good morning!")
case t.Hour() < 17:
   fmt.Println("Good afternoon.")
default:
   fmt.Println("Good evening.")
}
```

## defer语句

defer 语句会延迟函数的执行直到上层函数返回。

延迟调用的参数会立刻生成，但是在上层函数返回前函数都不会被调用。

延迟的函数调用被压入一个栈中。当函数返回时， 会按照后进先出的顺序调用被延迟的函数调用。

```go
func main() {
   defer fmt.Println("world")
   fmt.Println("hello")
}
```

## 指针

Go语言中存在指针，指针保存了变量的内存地址。

```go
func main() {

   i := 100

   // & 符号会生成一个指向其作用对象的指针。也就是底层的内存地址
   p := &i

   fmt.Println(p)
   // * 符号表示指针指向的底层的值。
   fmt.Print(*p)
    
   *p = 1000
   fmt.Print(*p)
   fmt.Println(i)
}
```

注意：

- `&` 符号会生成一个指向其作用对象的指针。
- `*` 符号表示指针指向的底层的值。

## 结构体

- 结构体（`struct`）就是一个字段的集合。
- 结构体字段使用点号来访问。

```go
func main() {
   fmt.Println(Var{1, 2})

   _var := Var{1, 2}
   X := _var.X
   Y := _var.Y
   fmt.Println(X)
   fmt.Println(Y)
}
```

结构体也可以使用指针

```go
_var2 := Var{1, 2}

q := _var2  // 将_var2的值赋值给q
p := &_var2 // p指向_var2

q.X = 2

p.Y = 3
fmt.Println(q)
fmt.Println(p)
fmt.Println(_var2)
fmt.Println(*p)
```

结构体文法

```go
v1 = Vertex{1, 2}  // 类型为 Vertex
v2 = Vertex{X: 1}  // Y:0 被省略
v3 = Vertex{}      // X:0 和 Y:0
v4 = &Vertex{1, 2} // 类型为 *Vertex
```

## 数组

### 定义数组

```go
func main() {

   var arr [2] int
   arr[1] = 10
   arr[0] = 20

   fmt.Println(arr)
}
```