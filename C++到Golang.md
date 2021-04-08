## **从C++到Golang**

### 基本程序结构

```go
package main //包,表明代码所在模块
import "fmt" //引入代码依赖
func main(){
    fmt.Println("Hello World!")
}
```

+ 必须是main包：package main
+ 必须是main方法：func main()
+ 文件名不一定是main.go

```go
package main 
import (
	"fmt"
    "os"
)
func main(){
    fmt.Println(os.Args)
    if len(os.Args)>1{
        fmt.Println(osArgs[1])
    }
    os.Exit(-1)
}
```
+ main函数不支持任何返回值，不支持传入参数
+ 通过os.Exit来返回状态
+ 在程序中通过os.Args获取命令行参数
### 函数多返回值
```
//go语言内置支持多返回值
package main

import (
    "fmt"
)

func vals() (int, int) {
    return 3, 9
}

func main() {
    a, b := vals()
    // _ 下划线来忽略其他返回值
    _, c := vals()
}
```
### 包导入语法

> Go 支持直接导入每个包以及分组导入。

+ 点操作

  ```go
  import(."fmt") // . 相当于C++的using namespace fmt;
  fmt.Println("Hello World!") => Println("Hello World!")
  ```

+ 别名操作

  ```go
  import(f "fmt") //相当于C++的namespace f = fmt;
  fmt.Println("Hello World!") => f.Println("Hello World!")
  //可能导致名称空间冲突。
  ```

+ _操作

  ```go
  import( _ "os") //不可以调用包内的其他方法。
  /*
  当我们在 Go 中导入一些软件包，但是没有在任何地方使用它时，Go编译报错。空白标识符可用于忽略该标识符。
  当导入一个包时，它所有的init()函数就会被执行，但有些时候并非真的需要使用这些包，仅仅是希望它的init()函数被执行而已。
  */
  ```

+ 嵌套导入

  ```go
  import (
      "fmt"
      "math/rand"//只需要一个大包的子包时，我们不需要导入所有的子包，可以简单地子包导入。
  )
  ```
### 基本数据类型
```
bool
string //值类型,默认初始化为"",len为实际字符个数,没有\0
int int8 int16 int32 int64 //int的大小是和操作系统位数相关的,32位占4,64位占8字节
uint uint8 uint16 uint32 uint64 uintptr
byte //字节是一个8位无符号整型数据,可用来表示ASI码
rune //alias for int32
float32 float64
complex64 complex128
```
### 类型转换
+ 不允许隐式类型转换
### 指针类型
> https://golang.co/pointers-in-golang/
+ 不支持指针运算
+ 可以直接新建指针

### 变量赋值
```
var x int
var x int = 0
var x = 0
var x, y int = 0, 1
x := 10
x,y:=10,10
z := math.Min(x, y)//运行时计算
```
+ 可以自动类型推断
+ 可以同时对多个变量进行同时赋值
+ 类型预定义值math.MaxInt64,math.MaxFloat64
### 算数运算符
```
+
-
*
/
%
++ //只支持后置++,--
--
```
### 基础数据结构

+ 数组
```
myarray := [...]int{10, 20, 30}
```
+ slice动态数组,由三部分构成：指针、长度、容量
> https://www.cnblogs.com/sparkdev/p/10704614.html
+ map
``` 
var m map[string] int //默认值为nil空指针
m := make(map[string] int,100) //初始化实例，100为存储大小,可不传
```
+ struct
```
type Employee struct {  
    firstName, lastName string
    age                 int
}
emp1 := Employee{
        firstName: "Sam",
        age:       25,
        salary:    500,
        lastName:  "Anderson",
}
emp2 := Employee{"Thomas", "Paul", 29, 800}
```
+ interface
```
接口类型，是一种抽象的类型，它不会暴露它所代表的对象的内部值和对象支持的操作方法。接口只会通过声明方法(行为)来展示它是用来做什么，并不知道它具体是什么。
```
### 用==比较数组
+ 相同维数相同长度才可以比较
+ 每个元素都相同才相等
### if条件
+ 条件必须为bool值
+ 支持变量赋值
```
if a := 1==1;a{
  //a可以使用
}else{

}
```
### switch条件
> https://www.golangprograms.com/golang-switch-case-statements.html
+ 条件不限制为常量或者整数
+ 单个case可以有多个选项支持,逗号分隔
+ 不需要break退出case
+ 继续判断使用fallthrough关键字
```
switch i{
  case 0,2:
    t.Log("Even")
  case 1,3:
    t.Log("Odd")
  default:

}
```
### for循环
+ 没有while
+ 支持C++格式for循环
```
for i := 0; i < 3; i++ {

}
for ;i <= 10; {
        fmt.Printf("%d ", i)
        i += 2
}
for i <= 10 {
        fmt.Printf("%d ", i)
        i += 2
}
for{
  //while(true){}    
}

```
### 不同接口interface类型，实现多态
> https://golangbot.com/polymorphism/
### defer/panic/recover:良好的错误处理
+ defer
> return之后添加一个函数调用。因此，defer通常用来释放函数内部变量。
> https://golangbot.com/defer/
+ panic
> 中断函数执行
> 会执行defer函数
+ recover
> 内建函数,令panic的协程恢复，仅在defer函数中有效
```
package main
import "fmt"

func Demo(i int) {
    //定义10个元素的数组
    var arr [10]int
    //错误拦截要在产⽣错误前设置
    defer func() {
        //设置recover拦截错误信息
        err := recover()
        //产⽣panic异常 打印错误信息
        if err != nil {
            fmt.Println(err)
        }
    }()
    //根据函数参数为数组元素赋值
    //如果i的值超过数组下标 会报错误：数组下标越界
    arr[i] = 10
}

func main() {
    Demo(10)
    //产⽣错误后 程序继续
    fmt.Println("程序继续执⾏...")
}
```
### Golang协程：Goroutine
> https://www.cnblogs.com/liang1101/p/7285955.html
### CSP并发模型
> https://blog.csdn.net/ScarletMeCarzy/article/details/106447870
### 垃圾回收机制
> https://cloud.tencent.com/developer/article/1766782
### Golang学习网站

> 并发性和处理机制 
> https://golangbot.com/learn-golang-series/ 
> 数据结构与算法
> https://www.golangprograms.com/go-language.html