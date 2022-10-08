# 1. Hello World！

```go
package main  
  
import "fmt"  
  
func main() {  
   /* 这是我的第一个简单的程序 */  
   fmt.Println("Hello, World!")  
}
```
## 1.1 GO语言组成部分

让我们来看下以上程序的各个部分：

1.  第一行代码 `package main` 定义了包名。你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。
2.  下一行 `import "fmt"` 告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入/输出）的函数。
3.  下一行 `func main()` 是程序开始执行的函数。main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 init() 函数则会先执行该函数）。
4.  下一行 `/*...*/ `是注释，在程序执行时将被忽略。单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释。多行注释也叫块注释，均已以 `/* `开头，并以 `*/` 结尾，且不可以嵌套使用，多行注释一般用于包的文档描述或注释成块的代码片段。
5.  下一行 `fmt.Println(...)` 可以将字符串输出到控制台，并在最后自动增加换行字符 `\n`。  
    使用 `fmt.Print("hello, world\n")` 可以得到相同的结果。  Print 和 Println 这两个函数也支持使用变量，如：fmt.Println(arr)。如果没有特别指定，它们会以默认的打印格式将变量 arr 输出到控制台。
6.  当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）。
## 1.2 运行 & 编译

运行

```go
$ go run hello.go
Hello, World!
```

编译 & 运行
```go
$ go build hello.go 
$ ls
hello    hello.go
$ ./hello 
Hello, World!
```

## 1.3 注意
需要注意的是 `{` 不能单独放在一行，所以以下代码在运行时会产生错误：
```go

package main  
  
import "fmt"  
  
func main()    
{  // 错误，{ 不能在单独的行上  
    fmt.Println("Hello, World!")  
}
```

# 2. 变量的 3 种方式
## 2.1  第一种，指定变量类型，如果没有初始化，则变量默认为零值

```go
package main  
import "fmt"  
func main() {  
  
    // 声明一个变量并初始化  
    var a = "RUNOOB"  
    fmt.Println(a)  // RUNOOB
  
    // 没有初始化就为零值  
    var b int  
    fmt.Println(b)  // 0
  
    // bool 零值为 false  
    var c bool  
    fmt.Println(c)  // false
}
```
-   数值类型（包括complex64/128）为 **0**
-   布尔类型为 **false**
-   字符串为 **""**（空字符串）
-   以下几种类型为 **nil**：
```go
var a *int
var a []int
var a map[string] int
var a chan int
var a func(string) int
var a error // error 是接口
```

## 2.2 第二种，根据值自行判定变量类型
```go
package main  
import "fmt"  
func main() {  
    var d = true  
    fmt.Println(d)  
}
```

## 2.3 使用 := 快速声名

`intVal := 1` 相等于：

```go
var intVal int 
intVal =1

```
可以将 `var f string = "Runoob"` 简写为 `f := "Runoob"`
```go
package main  
import "fmt"  
func main() {  
    f := "Runoob" // var f string = "Runoob"  
  
    fmt.Println(f)  
}
```

## 2.4 多变量声明
```go
package main  
  
var x, y int  
var (  // 这种因式分解关键字的写法一般用于声明全局变量  
    a int  
    b bool  
)  
  
var c, d int = 1, 2  
var e, f = 123, "hello"  
  
//这种不带声明格式的只能在函数体中出现  
//g, h := 123, "hello"  
  
func main(){  
    g, h := 123, "hello"  
    println(x, y, a, b, c, d, e, f, g, h)  
}

```
以上实例执行结果为：
`0 0 0 false 1 2 123 hello 123 hello`

## 2.5 注意事项
- `a := 50` 或 `b := false`。`a` 和 `b` 的类型（`int` 和 `bool`）将由编译器自动推断。这是使用变量的首选形式，**但是它只能被用在函数体内，而不可以用于全局变量的声明与赋值**。
- 如果你想要交换两个变量的值，则可以简单地使用 `a, b = b, a`，**两个变量的类型必须是相同**。
- 空白标识符` _` 也被用于抛弃值，如值 `5` 在`_, b = 5, 7` 中被抛弃。

# 3. GO 常量
```go
const identifier [type] = value
```

## 3.1 简单应用

```go
package main  
  
import "fmt"  
  
func main() {  
   const LENGTH int = 10  
   const WIDTH int = 5    
   var area int  
   const a, b, c = 1, false, "str" //多重赋值  
  
   area = LENGTH * WIDTH  
   fmt.Printf("面积为 : %d", area)  
   println()  
   println(a, b, c)    
}
```

```go
面积为 : 50
1 false str
```

## 3.2 枚举的定义方式

```go 
package main  
  
import "unsafe"  
const (  
    a = "abc"  
    b = len(a)  
    c = unsafe.Sizeof(a)  
)  
  
func main(){  
    println(a, b, c)  
}
```

# 4. GO 语言运算符
**举例**： 下表列出了所有Go语言的算术运算符。假定 `A` 值为 `10`，`B` 值为 `20`。

## 4.1 算术运算符
| **运算符** | **描述** |      **实例**      |
|:----------:|:--------:|:------------------:|
|     +      |   相加   | A + B 输出结果 30  |
|     -      |   相减   | A - B 输出结果 -10 |
|     *      |   相乘   | A * B 输出结果 200 |
|     /      |   相除   |  B / A 输出结果 2  |
|     %      |   求余   |  B % A 输出结果 0  |
|     ++     |   自增   |  A++ 输出结果 11   |
|     --     |   自减   |   A-- 输出结果 9   |


## 4.2 关系运算符
| 运算符 |                              描述                              |       实例        |
|:------:|:--------------------------------------------------------------:|:-----------------:|
|   ==   |     检查两个值是否相等，如果相等返回 True 否则返回 False。     | (A == B) 为 False |
|   !=   |   检查两个值是否不相等，如果不相等返回 True 否则返回 False。   | (A != B) 为 True  |
|   >    |   检查左边值是否大于右边值，如果是返回 True 否则返回 False。   | (A > B) 为 False  |
|   <    |   检查左边值是否小于右边值，如果是返回 True 否则返回 False。   |  (A < B) 为 True  |
|   >=   | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 | (A >= B) 为 False |
|   <=   | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 | (A <= B) 为 True  |

```go
   if( a == b ) {  
      fmt.Printf("第一行 - a 等于 b\n" )  
   } else {  
      fmt.Printf("第一行 - a 不等于 b\n" )  
   }  
   if ( a < b ) {  
      fmt.Printf("第二行 - a 小于 b\n" )  
   } else {  
      fmt.Printf("第二行 - a 不小于 b\n" )  
   }
```

## 4.3 逻辑运算符
| 运算符 | 描述                                                                      | 实例              |
|:--------:|:---------------------------------------------------------------------------:|:-------------------:|
| &&     | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。  | (A && B) 为 False |
| \|\|     | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。 | (A \|\| B) 为 True  |
| !      | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。   | !(A && B) 为 True |

```go
package main  
  
import "fmt"  
  
func main() {  
   var a bool = true  
   var b bool = false  
   if ( a && b ) {  
      fmt.Printf("第一行 - 条件为 true\n" )  
   }  
   if ( a || b ) {  
      fmt.Printf("第二行 - 条件为 true\n" )  
   }  
   /* 修改 a 和 b 的值 */  
   a = false  
   b = true  
   if ( a && b ) {  
      fmt.Printf("第三行 - 条件为 true\n" )  
   } else {  
      fmt.Printf("第三行 - 条件为 false\n" )  
   }  
   if ( !(a && b) ) {  
      fmt.Printf("第四行 - 条件为 true\n" )  
   }  
}
```

# 5. GO 条件语句
## 5.1 if
```go
if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
}
```

```go
package main  
  
import "fmt"  
  
func main() {  
   /* 定义局部变量 */  
   var a int = 10  
   
   /* 使用 if 语句判断布尔表达式 */  
   if a < 20 {  
       /* 如果条件为 true 则执行以下语句 */  
       fmt.Printf("a 小于 20\n" )  
   }  
   fmt.Printf("a 的值为 : %d\n", a)  
}
```

## 5.2 if..else
```go

if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
} else {
  /* 在布尔表达式为 false 时执行 */
}
```

```go
package main  
  
import "fmt"  
  
func main() {  
   /* 局部变量定义 */  
   var a int = 100;  
   
   /* 判断布尔表达式 */  
   if a < 20 {  
       /* 如果条件为 true 则执行以下语句 */  
       fmt.Printf("a 小于 20\n" );  
   } else {  
       /* 如果条件为 false 则执行以下语句 */  
       fmt.Printf("a 不小于 20\n" );  
   }  
   fmt.Printf("a 的值为 : %d\n", a);  
  
}
```
## 5.3 switch
```go
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

```go
package main  
  
import "fmt"  
  
func main() {  
   /* 定义局部变量 */  
   var grade string = "B"  
   var marks int = 90  
  
   switch marks {  
      case 90: grade = "A"  
      case 80: grade = "B"  
      case 50,60,70 : grade = "C"  
      default: grade = "D"    
   }  
  
   switch {  
      case grade == "A" :  
         fmt.Printf("优秀!\n" )      
      case grade == "B", grade == "C" :  
         fmt.Printf("良好\n" )        
      case grade == "D" :  
         fmt.Printf("及格\n" )        
      case grade == "F":  
         fmt.Printf("不及格\n" )  
      default:  
         fmt.Printf("差\n" );  
   }  
   fmt.Printf("你的等级是 %s\n", grade );        
}
```

### 5.3.1 Notice 
与`C++` 比较，没有使用 `break` 关键字

### 5.3.2 fallthrough

使用 `fallthrough` 会强制执行后面的 `case` 语句，`fallthrough` 不会判断下一条 `case` 的表达式结果是否为 `true`。
```go
package main  
  
import "fmt"  
  
func main() {  
  
    switch {  
    case false:  
            fmt.Println("1、case 条件语句为 false")  
            fallthrough  
    case true:  
            fmt.Println("2、case 条件语句为 true")  // 执行
            fallthrough  
    case false:  
            fmt.Println("3、case 条件语句为 false")  // 执行
            fallthrough  
    case true:  
            fmt.Println("4、case 条件语句为 true")  // 执行
    case false:  
            fmt.Println("5、case 条件语句为 false")  
            fallthrough  
    default:  
            fmt.Println("6、默认 case")  
    }  
}
```

从以上代码输出的结果可以看出：`switch` 从`第一个判断表达式为 true` 的 `case`  开始执行，如果 `case` 带有 `fallthrough`，程序会继续执行`下一条 case`，且它不会去判断下一个 `case` 的表达式是否为 `true`。

## 5.4 select
- `select` 是 `Go` 中的一个控制结构，类似于用于**通信的**  `switch` 语句。
- 每个 `case` 必须是一个**通信**操作，要么是发送要么是接收。
- `select` **随机**执行一个**可运行**的 `case`。
- 如果没有 `case` 可运行，它将**阻塞**，直到有 `case` 可运行。
- 一个默认的子句应该总是可运行的

语法
```go
select {  
    case communication clause  :  
       statement(s);        
    case communication clause  :  
       statement(s);  
    /* 你可以定义任意数量的 case */  
    default : /* 可选 */  
       statement(s);  
}
```

```go
package main  
  
import "fmt"  
  
func main() {  
   var c1, c2, c3 chan int  
   var i1, i2 int  
   select {  
      case i1 = <-c1:  
         fmt.Printf("received ", i1, " from c1\n")  
      case c2 <- i2:  
         fmt.Printf("sent ", i2, " to c2\n")  
      case i3, ok := (<-c3):  // same as: i3, ok := <-c3  
         if ok {  
            fmt.Printf("received ", i3, " from c3\n")  
         } else {  
            fmt.Printf("c3 is closed\n")  
         }  
      default:  
         fmt.Printf("no communication\n")  
   }      
}
```

### 5.4.1 go chan 类型补充
- 引用类型
- 零值 nil
- <发送/写>型 chan<-
- <接收/读>型 <-chan
- 双向型 chan
- 构造/初始化 make()
- 关闭 close()
- 判等 ==
- <发送/写>数据 chan <- send_date
- <接收/读>数据 recv_data := <- chan
- chan 关闭或有数据,读操作不阻塞
- chan 未关闭且无数据,读操作阻塞
- 
```go
package main

import "fmt"

func main() {

	//双向型 chan, 零值 nil
	var ch chan int
	//输入型 chan->
	var ci chan<- int
	//输出型 <-chan
	var co <-chan int
	//make()
	cc := make(chan int, 10)
	ch = cc
	//双向型赋值给单向型正确
	ci = ch
	co = ch
	//单向型赋值给双向型错误
	//ch = ci //❌
	//ch = co //❌
	fmt.Println(ci, co, ch) //0xc00008c000 0xc00008c000 0xc00008c000

	//可以赋值 -> 类型兼容 -> 可以判等
	b1 := ch == cc
	b2 := ci == nil
	b3 := ch == ci
	b4 := ch == co
	//不可以赋值 -> 类型不兼容 -> 不可以判等
	// b5 := ci == co //❌
	fmt.Println(ci, co, cc, ch) //0xc00008c000 0xc00008c000 0xc00008c000 0xc00008c00
	fmt.Println(b1, b2, b3, b4) //true false true true

	//chan 发送/写
	ch <- 1
	//chan 接收/读
	out := <-ch
	fmt.Println(out) // 1

	ch <- 1
	//close(), chan 关闭
	close(ch) //关闭后读操作不阻塞
	//chan 关闭后,还有数据.读操作,返回 数据 和 true
	out1, ok1 := <-ch
	//chan 关闭后,没有数据.读操作,返回 零值 和 false
	out2, ok2 := <-ch
	fmt.Println(out1, ok1, out2, ok2) // 1 true 0 fals

	ch = make(chan int, 0)
	//chan 没关闭,无数据,读操作阻塞
	out = <-ch

	fmt.Println("上一句阻塞了!!!打印不出这行了?!!!")

}


```


```go
0xc00008c000 0xc00008c000 0xc00008c000
0xc00008c000 0xc00008c000 0xc00008c000 0xc00008c000
true false true true
1
1 true 0 false
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.main()
	i:/github.com/gkt_cc_go/src/example/chan/chan.go:68 +0x508
exit status 2
```

# 6. GO 循环语句

## 5.1 for 循环
### 5.1.1 init; condition; post

```go
for init; condition; post { }
```

计算 1 到 10 的数字之和：

```go
package main  
  
import "fmt"  
  
func main() {  
        sum := 0  
        for i := 0; i <= 10; i++ {  // for init; condition; post
                sum += i  
        }  
        fmt.Println(sum)  
}
```

### 5.1.2 conditon
`init` 和 `post` 参数是可选的，我们可以直接省略它，类似 `While` 语句。

```go
for condition { }
```
以下实例在 `sum` 小于 `10` 的时候计算 `sum` 自相加后的值：
```go
package main  
  
import "fmt"  
  
func main() {  
        sum := 1  
        for ; sum <= 10; {  
                sum += sum  
        }  
        fmt.Println(sum)  
  
        // 这样写也可以，更像 While 语句形式  
        for sum <= 10{  
                sum += sum  
        }  
        fmt.Println(sum)  
}
```

### 5.1.3 For-each range 循环

这种格式的循环可以对`字符串`、`数组`、`切片`等进行`迭代`输出元素。


```go
package main  
import "fmt"  
  
func main() {  
        strings := []string{"google", "runoob"}  
        for i, s := range strings {  
                fmt.Println(i, s)  
        }  
  
  
        numbers := [6]int{1, 2, 3, 5}  
        for i,x:= range numbers {  
                fmt.Printf("第 %d 位 x 的值 = %d\n", i,x)  
        }    
}

```

```go
0 google
1 runoob
第 0 位 x 的值 = 1
第 1 位 x 的值 = 2
第 2 位 x 的值 = 3
第 3 位 x 的值 = 5
第 4 位 x 的值 = 0
第 5 位 x 的值 = 0
```

## 5.2 GO 循环关键字

### 5.2.1 break

case 1: 用于循环语句中跳出循环，并开始执行循环之后的语句。
case 2: `break` 在 `switch`（开关语句）中在执行一条 `case` 后跳出语句的作用。
case 3 : 在多重循环中，可以用标号 label 标出想 `break` 的循环。

#### case 1

```go
package main  
  
import "fmt"  
  
func main() {  
   /* 定义局部变量 */  
   var i, j int  
  
   for i=2; i < 10; i++ {  
      for j=2; j <= (i/j); j++ {  
         if(i%j==0) {  
            break; // 如果发现因子，则不是素数  
         }  
      }  
      if(j > (i/j)) {  
         fmt.Printf("%d  是素数\n", i);  
      }  
   }    
}
```

```go
2  是素数
3  是素数
5  是素数
7  是素数
```

#### case 3
```go
package main  
  
import "fmt"  
  
func main() {  
  
   // 不使用标记  
   fmt.Println("---- break ----")  
   for i := 1; i <= 3; i++ {  
      fmt.Printf("i: %d\n", i)  
      for i2 := 11; i2 <= 13; i2++ {  
         fmt.Printf("i2: %d\n", i2)  
         break  
      }  
   }  
  
   // 使用标记  
   fmt.Println("---- break label ----")  
   re:  
      for i := 1; i <= 3; i++ {  
         fmt.Printf("i: %d\n", i)  
         for i2 := 11; i2 <= 13; i2++ {  
         fmt.Printf("i2: %d\n", i2)  
         break re  
      }  
   }  
}
```

```go
---- break ----
i: 1
i2: 11
i: 2
i2: 11
i: 3
i2: 11
---- break label ----
i: 1
i2: 11
```

### 5.2.2 continue

Go 语言的 continue 语句 有点像 break 语句。但是 continue 不是跳出循环，而是跳过当前循环执行下一次循环语句。
`for` 循环中，执行 `continue` 语句会触发 `for` 增量语句的执行。
在多重循环中，可以用标号 label 标出想 `continue` 的循环。

```go
package main  
  
import "fmt"  
  
func main() {  
  
    // 不使用标记  
    fmt.Println("---- continue ---- ")  
    for i := 1; i <= 3; i++ {  
        fmt.Printf("i: %d\n", i)  
            for i2 := 11; i2 <= 13; i2++ {  
                fmt.Printf("i2: %d\n", i2)  
                continue  
            }  
    }  
  
    // 使用标记  
    fmt.Println("---- continue label ----")  
    re:  
        for i := 1; i <= 3; i++ {  
            fmt.Printf("i: %d\n", i)  
                for i2 := 11; i2 <= 13; i2++ {  
                    fmt.Printf("i2: %d\n", i2)  
                    continue re  
                }  
        }  
}
```

```go
---- continue ---- 
i: 1
i2: 11
i2: 12
i2: 13
i: 2
i2: 11
i2: 12
i2: 13
i: 3
i2: 11
i2: 12
i2: 13
---- continue label ----
i: 1
i2: 11
i: 2
i2: 11
i: 3
i2: 11
```

### 5.2.3 goto
Go 语言的 `goto` 语句可以无条件地转移到过程中指定的行。
`goto` 语句通常与条件语句配合使用。可用来实现条件转移， 构成循环，跳出循环体等功能。
但是，在结构化程序设计中一般不主张使用 `goto` 语句， 以免造成程序流程的混乱，使理解和调试程序都产生困难。
```go
package main  
  
import "fmt"  
  
func main() {  
   /* 定义局部变量 */  
   var a int = 10  
  
   /* 循环 */  
   LOOP: for a < 20 {  
      if a == 15 {  
         /* 跳过迭代 */  
         a = a + 1  
         goto LOOP  
      }  
      fmt.Printf("a的值为 : %d\n", a)  
      a++      
   }    
}
```


```go
a的值为 : 10
a的值为 : 11
a的值为 : 12
a的值为 : 13
a的值为 : 14
a的值为 : 16
a的值为 : 17
a的值为 : 18
a的值为 : 19
```

# 5. GO 函数

语法

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

-   `func`：函数由 `func` 开始声明
-   `function_name`：函数名称，**参数列表和返回值类型构成了函数签名**。
-   `parameter list`：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
-   `return_types`：返回类型，函数返回一列值。`return_types` 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
-   `函数体`：函数定义的代码集合。

```go
/* 函数返回两个数的最大值 */  
func max(num1, num2 int) int {  
   /* 声明局部变量 */  
   var result int  
  
   if (num1 > num2) {  
      result = num1  
   } else {  
      result = num2  
   }  
   return result  
}
```

## 5.1 GO 函数可以返回多个参数值
```go
package main  
  
import "fmt"  
  
func swap(x, y string) (string, string) {  
   return y, x  
}  
  
func main() {  
   a, b := swap("Google", "Runoob")  
   fmt.Println(a, b)  
}
```

```go
Runoob Google
```

## 5.2  函数参数

### 5.2.1 值传递
`值传递`是指在调用函数时将实际参数 `复制` 一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
**默认情况下，Go 语言使用的是值传递，即在调用过程中不会影响到实际参数**。

```go
/* 定义相互交换值的函数 */
func swap(x, y int) int {
   var temp int

   temp = x /* 保存 x 的值 */
   x = y    /* 将 y 值赋给 x */
   y = temp /* 将 temp 值赋给 y*/

   return temp;
}
```

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200

   fmt.Printf("交换前 a 的值为 : %d\n", a )
   fmt.Printf("交换前 b 的值为 : %d\n", b )

   /* 通过调用函数来交换值 */
   swap(a, b)

   fmt.Printf("交换后 a 的值 : %d\n", a )
   fmt.Printf("交换后 b 的值 : %d\n", b )
}

/* 定义相互交换值的函数 */
func swap(x, y int) int {
   var temp int

   temp = x /* 保存 x 的值 */
   x = y    /* 将 y 值赋给 x */
   y = temp /* 将 temp 值赋给 y*/

   return temp;
}
```

```go
交换前 a 的值为 : 100
交换前 b 的值为 : 200
交换后 a 的值 : 100
交换后 b 的值 : 200
```

### 5.2.2 引用传递
`引用传递` 是指在调用函数时将 `实际参数的地址` 传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

引用传递指针参数传递到函数内，以下是交换函数 swap() 使用了引用传递：
```go
/* 定义交换值函数*/
func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保持 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int= 200

   fmt.Printf("交换前，a 的值 : %d\n", a )
   fmt.Printf("交换前，b 的值 : %d\n", b )

   /* 调用 swap() 函数
   * &a 指向 a 指针，a 变量的地址
   * &b 指向 b 指针，b 变量的地址
   */
   swap(&a, &b)

   fmt.Printf("交换后，a 的值 : %d\n", a )
   fmt.Printf("交换后，b 的值 : %d\n", b )
}

func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保存 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

```go
交换前，a 的值 : 100
交换前，b 的值 : 200
交换后，a 的值 : 200
交换后，b 的值 : 100
```

## 5.3 函数的用法

### 5.3.1 函数作为实参
Go 语言可以很灵活的创建 `函数`，并作为 `另外一个函数` 的 `实参`。

```go
package main  
  
import (  
   "fmt"  
   "math"  
)  
  
func main(){  
   /* 声明函数变量 */  
   getSquareRoot := func(x float64) float64 {  
      return math.Sqrt(x)  
   }  
  
   /* 使用函数 */  
   fmt.Println(getSquareRoot(9))  
  
}
```

```go
3
```

### 5.3.2 函数闭包
Go 语言支持`匿名函数`，可 `作为闭包`。
`匿名函数` 是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。
以下实例中，我们创建了函数 `getSequence()` ，返回另外一个函数。该函数的目的是在`闭包中递增 i 变量`，代码如下：
```go
package main  
  
import "fmt"  
  
func getSequence() func() int {  
   i:=0  
   return func() int {  
      i+=1  
     return i    
   }  
}  
  
func main(){  
   /* nextNumber 为一个函数，函数 i 为 0 */  
   nextNumber := getSequence()    
  
   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */  
   fmt.Println(nextNumber())  
   fmt.Println(nextNumber())  
   fmt.Println(nextNumber())  
     
   /* 创建新的函数 nextNumber1，并查看结果 */  
   nextNumber1 := getSequence()    
   fmt.Println(nextNumber1())  
   fmt.Println(nextNumber1())  
}
```

```go
1
2
3
1
2
```

### 5.3.3 方法

Go 语言中同时有 `函数` 和 `方法`。
一个 `方法` 就是一个包含了`接受者` 的 `函数`，`接受者` 可以是 `命名类型` 或者 `结构体类型` 的 `一个值` 或者是 `一个指针` 。
所有给定类型的方法属于该类型的 `方法集`。

语法格式如下：
```go
func (variable_name variable_data_type) function_name() [return_type]{
   /* 函数体*/
}
```

```go
package main  
  
import (  
   "fmt"    
)  
  
/* 定义结构体 */  
type Circle struct {  
  radius float64  
}  
  
func main() {  
  var c1 Circle  
  c1.radius = 10.00  
  fmt.Println("圆的面积 = ", c1.getArea())  
}  
  
//该 method 属于 Circle 类型对象中的方法  
func (c Circle) getArea() float64 {  
  //c.radius 即为 Circle 类型对象中的属性  
  return 3.14 * c.radius * c.radius  
}
```

```go
圆的面积 =  314
```

Go 没有面向对象，而我们知道常见的 Java。
C++ 等语言中，实现类的方法做法都是编译器隐式的给函数加一个 this 指针，而在 Go 里，这个 this 指针需要明确的申明出来，其实和其它 OO 语言并没有很大的区别。

在 C++ 中是这样的:
```c++
class Circle {
  public:
    float getArea() {
       return 3.14 * radius * radius;
    }
  private:
    float radius;
}

// 其中 getArea 经过编译器处理大致变为
float getArea(Circle *const c) {
  ...
}
```

在 Go 中则是如下:

```go
func (c Circle) getArea() float64 {
  //c.radius 即为 Circle 类型对象中的属性
  return 3.14 * c.radius * c.radius
}
```

# 6. GO 数组

语法

```go
var variable_name [SIZE] variable_type
```

```go
var balance [10] float32
```


## 6.1 初始化数组

```go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

// 或者
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

如果数组长度不确定，可以使用 `...` 代替数组的长度，编译器会根据元素个数自行推断数组的长度：
```go
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
或
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
```

多维数组
```go
var variable_name [SIZE1][SIZE2]...[SIZEN] variable_type
```

```
var threedim [5][10][4] int
```

# 7. GO 切片（Slice）
`Go` 语言`切片`是`对数组的抽象`。
`Go` 数组的`长度不可改变`，在特定场景中这样的集合就不太适用，
`Go` 中提供了一种灵活，功能强悍的内置类型`切片("动态数组")`，与`数组`相比`切片`的`长度是不固定的`，可以追加元素，在追加时可能使切片的容量增大。

## 7.1 定义切片

语法

```go
var identifier [] type

```

切片不需要说明长度。
或使用 `make()` 函数来创建切片:
```go
var slice1 [] type = make([]type, len)

// 也可以简写

slice1 := make([]type, len)
```

也可以指定容量，其中 `capacity` 为可选参数。

```go
make([]T, length, capacity)
```

这里 `len` 是数组的长度并且也是切片的`初始长度`。
```go
s :=[] int{1, 2, 3} // 直接初始化切片， []是切片类型
```

```go
s := arr[:] // 初始化切片 s，是数组 arr 的引用。

s := arr[startIndex:endIndex] // 将 arr 中从下标 startIndex 到 endIndex-1 下的元素创建为一个新的切片。

s := arr[startIndex:] // 默认 endIndex 时将表示一直到arr的最后一个元素。

s := arr[:endIndex] // 默认 startIndex 时将表示从 arr 的第一个元素开始。

s1 := s[startIndex:endIndex] // 通过切片 s 初始化切片 s1。

s :=make([]int,len,cap) // 通过内置函数 make() 初始化切片s，[]int 标识为其元素类型为 int 的切片。
```

## 7.2 len() 和 cap() 函数

`切片`是`可索引`的，并且可以由 `len()` 方法获取长度。
切片提供了计算容量的方法 `cap()` 可以测量切片最长可以达到多少。
以下为具体实例：
```go

package main

import "fmt"

func main(){
	var numbers = make([]int, 3,  5) // make([]type, length, capcity)
	
	printSlice(numbers)
}


func printSlice(x []int){
	fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}

```

```go
len=3 cap=5 slice=[0 0 0]
```

## 7.3 空（nil）切片
一个切片在未初始化之前默认为 `nil`，长度为 `0`，实例如下：
```go
package main  
  
import "fmt"  
  
func main() {  
   var numbers []int  
  
   printSlice(numbers)  
  
   if(numbers == nil){  
      fmt.Printf("切片是空的")  
   }  
}  
  
func printSlice(x []int){  
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)  
}
```

```go
len=0 cap=0 slice=[]
切片是空的
```

## 7.4 切片截取

可以通过设置下限及上限来设置截取切片`[lower-bound:upper-bound]`，实例如下

```go
package main  
  
import "fmt"  
  
func main() {  
   /* 创建切片 */  
   numbers := []int{0,1,2,3,4,5,6,7,8}    
   printSlice(numbers)  
  
   /* 打印原始切片 */  
   fmt.Println("numbers ==", numbers)  
  
   /* 打印子切片从索引1(包含) 到索引4(不包含)*/  
   fmt.Println("numbers[1:4] ==", numbers[1:4])  
  
   /* 默认下限为 0*/  
   fmt.Println("numbers[:3] ==", numbers[:3])  
  
   /* 默认上限为 len(s)*/  
   fmt.Println("numbers[4:] ==", numbers[4:])  
  
   numbers1 := make([]int,0,5)  
   printSlice(numbers1)  
  
   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */  
   number2 := numbers[:2]  
   printSlice(number2)  
  
   /* 打印子切片从索引 2(包含) 到索引 5(不包含) */  
   number3 := numbers[2:5]  
   printSlice(number3)  
  
}  
  
func printSlice(x []int){  
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)  
}
```


## 7.5 append() 和 copy() 函数

**如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来**。

下面的代码描述了从拷贝切片的 `copy` 方法和向切片追加新元素的 `append` 方法。

```go
package main  
  
import "fmt"  
  
func main() {  
   var numbers []int  
   printSlice(numbers)          // len=0 cap=0 slice=[]
  
   /* 1. 允许追加空切片 */  
   numbers = append(numbers, 0)  
   printSlice(numbers)          // len=1 cap=1 slice=[0]
  
   /* 向切片添加一个元素 */  
   numbers = append(numbers, 1)  
   printSlice(numbers)            // len=2 cap=2 slice=[0 1]
  
   /* 同时添加多个元素 */  
   numbers = append(numbers, 2,3,4)  
   printSlice(numbers)         // len=5 cap=6 slice=[0 1 2 3 4]
  
   /* 创建切片 numbers1 是之前切片的两倍容量*/  
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)  
  
   /* 拷贝 numbers 的内容到 numbers1 */  
   copy(numbers1,numbers)  
   printSlice(numbers1)    // len=5 cap=12 slice=[0 1 2 3 4]
}  
  
func printSlice(x []int){  
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)  
}
```


# 8. GO Range 语言范围

`Go` 语言中 `range` 关键字用于 `for` 循环中迭代`数组(array)`、`切片(slice)`、`通道(channel)`或`集合(map)`的元素。
在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 `key-value` 对。

```go
package main  
import "fmt"  
func main() {  
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似  
    nums := []int{2, 3, 4}  
    sum := 0  
    for _, num := range nums {  
        sum += num  
    }  
    fmt.Println("sum:", sum)  
   
    //在数组上使用range将传入index和值两个变量。
    
    //上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。  
    for i, num := range nums {  
        if num == 3 {  
            fmt.Println("index:", i)  
        }  
    }


    //range也可以用在map的键值对上。  
    kvs := map[string]string{"a": "apple", "b": "banana"}  
    for k, v := range kvs {  
        fmt.Printf("%s -> %s\n", k, v)  
    }  

    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。  
    for i, c := range "go" {  
        fmt.Println(i, c)  
    }  
}
```


```go
sum: 9
index: 1
a -> apple
b -> banana
0 103
1 111
```

# 9. GO Map 集合

`Map` 是一种`无序`的`键值对`的`集合`。
`Map` 最重要的一点是通过 `key` 来快速检索数据，`key` 类似于索引，指向数据的值。
`Map` 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，`Map` 是无序的，我们无法决定它的返回顺序，这是因为 `Map` 是使用 `hash` 表来实现的。

## 9.1 定义
可以使用内建函数 `make` 也可以使用 `map` 关键字来定义 `Map`:
```go

/* 1. 声明变量， 默认 map 是 nil*/
var map_variable map[key_data_type]value_data_type


/* 2. 使用 make 函数*/
map_variable := make(map[key_data_type]value_data_map)

```

如果不初始化 `map`，那么就会创建一个 `nil map`。`nil map` 不能用来存放键值对
```go
package main  
  
import "fmt"  
  
func main() {  
    var countryCapitalMap map[string]string /*创建集合 */  
    countryCapitalMap = make(map[string]string)  
  
    /* map插入key - value对,各个国家对应的首都 */  
    countryCapitalMap [ "France" ] = "巴黎"  
    countryCapitalMap [ "Italy" ] = "罗马"  
    countryCapitalMap [ "Japan" ] = "东京"  
    countryCapitalMap [ "India " ] = "新德里"  
  
    /*使用键输出地图值 */  
    for country := range countryCapitalMap {  
        fmt.Println(country, "首都是", countryCapitalMap [country])  
    }  
  
    /*查看元素在集合中是否存在 */  
    capital, ok := countryCapitalMap [ "American" ] /*如果确定是真实的,则存在,否则不存在 */  
    /*fmt.Println(capital) */  
    /*fmt.Println(ok) */  
    if (ok) {  
        fmt.Println("American 的首都是", capital)  
    } else {  
        fmt.Println("American 的首都不存在")  
    }  
}
```

```go
France 首都是 巴黎
Italy 首都是 罗马
Japan 首都是 东京
India  首都是 新德里
American 的首都不存在
```

## 9.2 delete() 函数

```go
package main  
  
import "fmt"  
  
func main() {  
        /* 创建map */  
        countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}  
  
        fmt.Println("原始地图")  
  
        /* 打印地图 */  
        for country := range countryCapitalMap {  
                fmt.Println(country, "首都是", countryCapitalMap [ country ])  
        }  
  
        /*删除元素*/ 
        delete(countryCapitalMap, "France")  
        fmt.Println("法国条目被删除")  
  
        fmt.Println("删除元素后地图")  
  
        /*打印地图*/  
        for country := range countryCapitalMap {  
                fmt.Println(country, "首都是", countryCapitalMap [ country ])  
        }  
}
```

```go
原始地图
India 首都是 New delhi
France 首都是 Paris
Italy 首都是 Rome
Japan 首都是 Tokyo
法国条目被删除
删除元素后地图
Italy 首都是 Rome
Japan 首都是 Tokyo
India 首都是 New delhi
```

# 10. GO 接口
`Go` 语言提供了另外一种`数据类型`即`接口`，
它把`所有的具有共性的方法`定义在一起，任何其他类型只要实现了这些`方法`就是实现了这个`接口`。

语法
```go
/* 接口 */
type interface_name interface{
	method_name1 [return type]
	method_name2 [return type]

	...
	method_namen [return type]
}

/* 定义结构体 */
type struct_name struct{
	/* code */
}

/* 实现接口方法 */

func (struct_name_varible struct_name) method_name1() [return type]{
	/* code */
}

func (struct_name_varible struct_name) method_name2() [return type]{
	/* code */
}

```

实例
```go
package main
import ("fmt")


/* 定义接口集合 */
type Phone interface {
call()
}

  
/* 定义 Nokia 类 */
type NokiaPhone struct {
}
/* 实现 Nokia 的 call 方法 */
func (nokiadphone NokiaPhone) call() {
	fmt.Println("I am Nokia, I can call you!")
}

  
/* 定义 Iphone 类*/
type Iphone struct {
}
/* 实现 Iphone 的 call 方法 */
func (iphone Iphone) call() {
	fmt.Println("I am iphone, I can call you!")
}

  
func main() {

	var phone Phone

	phone = new(NokiaPhone)
	phone.call() // I am Nokia, I can call you!

	phone = new(Iphone)
	phone.call() // I am iPhone, I can call you!

}
```

# 11. GO 错误处理
`Go` 语言通过内置的`错误接口`提供了非常简单的`错误处理机制`。
`error类型`是一个`接口类型`，这是它的定义：
```go
type error interface{
	Error() string
}
```

我们可以在编码中通过实现 `error` 接口类型来生成错误信息。
函数通常在最后的返回值中返回错误信息。使用`errors.New` 可返回一个错误信息：
```go
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New("math: square root of negative number")
    }
    // 实现
}
```

```go
package main  
  
import (  
    "fmt"  
)  
  
// 定义一个 DivideError 结构  
type DivideError struct {  
    dividee int  
    divider int  
}  
  
// 实现 `error` 接口  
func (de *DivideError) Error() string {  
    strFormat := `  
    Cannot proceed, the divider is zero.  
    dividee: %d  
    divider: 0  
`  
    return fmt.Sprintf(strFormat, de.dividee)  
}  
  
// 定义 `int` 类型除法运算的函数  
func Divide(varDividee int, varDivider int) (result int, errorMsg string) {  
    if varDivider == 0 {  
            dData := DivideError{  
                    dividee: varDividee,  
                    divider: varDivider,  
            }  
            errorMsg = dData.Error()  
            return  
    } else {  
            return varDividee / varDivider, ""  
    }  
  
}  
  
func main() {  
  
    // 正常情况  
    if result, errorMsg := Divide(100, 10); errorMsg == "" {  
            fmt.Println("100/10 = ", result)  
    }  
    // 当除数为零的时候会返回错误信息  
    if _, errorMsg := Divide(100, 0); errorMsg != "" {  
            fmt.Println("errorMsg is: ", errorMsg)  
    }  
  
}
```

```go
100/10 =  10
errorMsg is:  
    Cannot proceed, the divider is zero.
    dividee: 100
    divider: 0
```



# 12. GO 并发
`Go` 语言支持并发，我们只需要通过 `go` 关键字来开启 `goroutine` 即可。
`goroutine` 是`轻量级线程`，`goroutine` 的调度是由 `Golang` 运行时进行管理的。
`goroutine` 语法格式：
```go

go 函数名（ 参数列表 ）

```

`Go` 允许使用 `go` 语句开启一个新的`运行期线程`， 即 `goroutine`，以一个不同的、新创建的 `goroutine` 来执行一个函数。 
同一个程序中的所有 goroutine 共享同一个地址空间。

```go
package main  
  
import (  
        "fmt"  
        "time"  
)  
  
func say(s string) {  
        for i := 0; i < 5; i++ {  
                time.Sleep(100 * time.Millisecond)  
                fmt.Println(s)  
        }  
}  
  
func main() {  
        go say("world")  
        say("hello")  
}
```

执行以上代码，你会看到输出的 hello 和 world 是没有固定先后顺序。因为它们是两个 `goroutine` 在执行：

```go
world
hello
hello
world
world
hello
hello
world
world
hello
```

# 13. GO channel 通道
`通道（channel`是用来传递数据的一个数据结构。
通道可用于两个 `goroutine` 之间通过传递一个`指定类型的值`来同步`运行`和`通讯`。
操作符 `<-` 用于指定通道的`方向`，`发送`或`接收`。如果未指定方向，则为`双向通道`。

```go
package main

  

import ("fmt")


func sum(s []int, c chan int) {

	sum := 0

	for _, v := range s {
		sum += v
	}
	
	c <- sum // 把 sum 发送到通道 c
}

  

func main() {

	s := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}

	c := make(chan int)

	length := len(s)
	
	go sum(s[:length/2], c)
	
	go sum(s[length/2:], c)
	
	x, y := <-c, <-c  // 从通道 c 中接收
	
	fmt.Println(x, y, x+y)
}
```

## 13.1 chan 设置通道缓冲区大小

通道可以设置缓冲区，通过 `make` 的第二个参数指定缓冲区大小：
```
ch := make(chan int, 100)
```

带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，**就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据**。

不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

**注意**：如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果缓冲区已满，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直**阻塞**。

```go
package main  
  
import (  
        "fmt"  
)  
  
func fibonacci(n int, c chan int) {  
        x, y := 0, 1  
        for i := 0; i < n; i++ {  
                c <- x  
                x, y = y, x+y  
        }  
        close(c)  
}  
  
func main() {  
        c := make(chan int, 10)  
        go fibonacci(cap(c), c)  
        // range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个  
        // 数据之后就关闭了通道，所以这里我们 range 函数在接收到 10 个数据  
        // 之后就结束了。如果上面的 c 通道不关闭，那么 range 函数就不  
        // 会结束，从而在接收第 11 个数据的时候就阻塞了。  
        for i := range c {  
                fmt.Println(i)  
        }  
}
```
