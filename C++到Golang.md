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
      "math/rand"//只需要一个大包的子包时，我们不需要导入所有的子包。我们可以简单地子包导入。
  )
  ```

  