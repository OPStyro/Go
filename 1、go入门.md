# 一、安装与运行环境

## 1.1 Linux 安装 Go

如果你能够自己下载并编译 Go 的源代码来说是非常有教育意义的，你可以根据这个页面找到安装指南和下载地址：[Download the Go distribution](https://golang.google.cn/doc/install)。

1.1.1.设置 Go 环境变量

我们在 Linux 系统下一般通过文件 `$HOME/.bashrc` 配置自定义环境变量，根据不同的发行版也可能是文件 `$HOME/.profile`，然后使用 gedit 或 vi 来编辑文件内容。

```bash
 export GOROOT=$HOME/go
```

为了确保相关文件在文件系统的任何地方都能被调用，你还需要添加以下内容： 

```bash
 export PATH=$PATH:$GOROOT/bin
```

在开发 Go 项目时，你还需要一个环境变量来保存你的工作目录。

```bash
export GOPATH=$HOME/Applications/Go
```

$GOPATH 可以包含多个工作目录，取决于你的个人情况。如果你设置了多个工作目录，那么当你在之后使用 go get（远程包安装命令）时远程包将会被安装在第一个目录下。

在完成这些设置后，你需要在终端输入指令 source .bashrc 以使这些环境变量生效。然后重启终端，输入 go env 和 env 来检查环境变量是否设置正确。

### 1.1.2 安装 C 工具

Go 的工具链是用 C 语言编写的，因此在安装 Go 之前你需要先安装相关的 C 工具。如果你使用的是 Ubuntu 的话，你可以在终端输入以下指令（ 译者注：由于网络环境的特殊性，你可能需要将每个工具分开安装 ）。

```bash
 sudo apt-get install bison ed gawk gcc libc6-dev make
```

你可以在其它发行版上使用 RPM 之类的工具。

### 1.1.3 获取 Go 源代码

从 [官方页面](https://golang.google.cn/dl/) 或 [国内镜像](http://www.golangtc.com/download) 下载 Go 的源码包到你的计算机上，然后将解压后的目录 `go` 通过命令移动到 `$GOROOT` 所指向的位置。

```bash
 wget https://storage.googleapis.com/golang/go<VERSION>.src.tar.gz
 tar -zxvf go<VERSION>.src.tar.gz
 sudo mv go $GOROOT
```

### 1.1.4 构建 Go

在终端使用以下指令来进行编译工作。

```bash
 cd $GOROOT/src
 ./all.bash
```

在完成编译之后（通常在 1 分钟以内，如果你在 B 型树莓派上编译，一般需要 1 个小时），你会在终端看到如下信息被打印：

**注意事项**

在测试 `net/http` 包时有一个测试会尝试连接 `google.com`，你可能会看到如下所示的一个无厘头的错误报告：

```http
 ‘make[2]: Leaving directory `/localusr/go/src/pkg/net’
```

如果你正在使用一个带有防火墙的机器，我建议你可以在编译过程中暂时关闭防火墙，以避免不必要的错误。

解决这个问题的另一个办法是通过设置环境变量 `$DISABLE_NET_TESTS` 来告诉构建工具忽略 `net/http` 包的相关测试：

```bash
 export DISABLE_NET_TESTS=1
```

如果你完全不想运行包的测试，你可以直接运行 `./make.bash` 来进行单纯的构建过程。

### 1.1.5 测试安装

使用你最喜爱的编辑器来输入以下内容，并保存为文件名 `hello_world1.go`。

```go
package main

import "fmt" //表示引入了一个包，类似于python 引入改包后就可以使用包内函数

func main() {
	/* {不能单独放在一行，func是一个关键词，后面是一个函数，main主函数，即是程序的入口 */
	/* 这是我的第一个简单的程序,这是注释 */
	fmt.Println("Hello, World!")
} //fmt 表示调用Printlt输出hello word.
```

切换相关目录到下，然后执行指令 `go run hello_world1.go`，将会打印信息：`Hello world`。

### 1.1.6 验证安装版本

你可以通过在终端输入指令 `go version` 来打印 Go 的版本信息。

如果你想要通过 Go 代码在运行时检测版本，可以通过以下例子实现。

示例 2.2 [version.go](https://learnku.com/docs/the-way-to-go/install-go-on-linux/examples/chapter_2/version.go)

```go
 package main

 import (
     "fmt"
     "runtime"
 )

 func main() {
     fmt.Printf("%s", runtime.Version())
 }
```

这段代码将会输出 `go1.4.2` 或类似字符串。

## 1.2 在 windows 上安装 Go

你可以在 下载页面 页面下载到 Windows 系统下的一键安装包。

前期的 Windows 移植工作由 Hector Chu 完成，但目前的发行版已经由 Joe Poirier 全职维护。

在完成安装包的安装之后，你只需要配置 $GOPATH 这一个环境变量就可以开始使用 Go 语言进行开发了，其它的环境变量安装包均会进行自动设置。在默认情况下，Go 将会被安装在目录 c:\go 下，但如果你在安装过程中修改安装目录，则可能需要手动修改所有的环境变量的值。

如果你想要测试安装，则可以使用指令 go run 运行 hello_world1.go。

如果发生错误 fatal error: can’t find import: fmt 则说明你的环境变量没有配置正确。

如果你想要在 Windows 下使用 cgo （调用 C 语言写的代码），则需要安装 MinGW，一般推荐安装 TDM-GCC。如果你使用的是 64 位操作系统，请务必安装 64 位版本的 MinGW。安装完成进行环境变量等相关配置即可使用。

在 Windows 下运行在虚拟机里的 Linux 系统上安装 Go：

如果你想要在 Windows 下的虚拟机里的 Linux 系统上安装 Go，你可以选择使用虚拟机软件 VMware，下载 VMware player，搜索并下载一个你喜欢的 Linux 发行版镜像，然后安装到虚拟机里，安装 Go 的流程参考第 2.3 节中的内容。

注意：引用包出现报错：

```go
关闭 go mod 模式
用 gopath 模式引入包从src目录下开始引入，需要
go env -w GO111MODULE=off

main.go:5:2: package project/project/funccall/dbutile is not in GOROOT (D:\go\src\project\project\funccall\dbutile)
```



## 1.3 安装目录清单

你的 Go 安装目录（`$GOROOT`）的文件夹结构应该如下所示：

README.md, AUTHORS, CONTRIBUTORS, LICENSE

/bin：包含可执行文件，如：编译器，Go 工具
/doc：包含示例程序，代码工具，本地文档等
/lib：包含文档模版
/misc：包含与支持 Go 编辑器有关的配置文件以及 cgo 的示例
/os_arch：包含标准库的包的对象文件（.a）
/src：包含源代码构建脚本和标准库的包的完整源代码（Go 是一门开源语言）
/src/cmd：包含 Go 和 C 的编译器和命令行脚本

## 1.4 Go 运行时(runtime)

尽管 Go 编译器产生的是本地可执行代码，然而这些代码仍旧运行在 Go 的 runtime（这部分的代码可以在 runtime 包中找到）当中。这个 runtime 类似 Java 和 .NET 语言所用到的虚拟机，它负责管理包括内存分配、垃圾回收（第 11.8 节）、栈处理、goroutine、channel、切片（slice）、map 和反射（reflection）等等。

runtime 主要由 C 语言编写（自 Go 1.5 起开始自举），并且是每个 Go 包的最顶级包。你可以在目录 [`$GOROOT/src/runtime`](https://github.com/golang/go/tree/master/src/runtime) 中找到相关内容。

垃圾回收器 Go 拥有简单却高效的标记 - 清除回收器。它的主要思想来源于 IBM 的可复用垃圾回收器，旨在打造一个高效、低延迟的并发回收器。目前 gccgo 还没有回收器，同时适用于 gc 和 gccgo 的新回收器正在研发中。使用一门具有垃圾回收功能的编程语言不代表你可以避免内存分配所带来的问题，分配和回收内容都是消耗 CPU 资源的一种行为。

Go 的可执行文件都比相对应的源代码文件要大很多，这恰恰说明了 Go 的 runtime 嵌入到了每一个可执行文件当中。当然，在部署到数量巨大的集群时，较大的文件体积也是比较头疼的问题。但总得来说，Go 的部署工作还是要比 Java 和 Python 轻松得多。因为 Go 不需要依赖任何其它文件，它只需要一个单独的静态文件，这样你也不会像使用其它语言一样被各种不同版本的依赖文件混淆。

## 1.5 Go 解释器

因为 Go 具有像动态语言那样快速编译的能力，自然而然地就有人会问 Go 语言能否在 REPL（read-eval-print loop）编程环境下实现。Sebastien Binet 已经使用这种环境实现了一个 Go 解释器，你可以在这个页面找到：github.com/sbinet/igo。

# 二、好用的相关工具

应用程序的开发过程中调试是必不可少的一个环节，因此有一个好的调试器是非常重要的，可惜的是，Go 在这方面的发展还不是很完善。目前可用的调试器是 gdb，最新版均已内置在集成开发环境 LiteIDE 和 GoClipse 中，但是该调试器的调试方式并不灵活且操作难度较大。

如果你不想使用调试器，你可以按照下面的一些有用的方法来达到基本调试的目的：

1、在合适的位置使用打印语句输出相关变量的值（print/println 和 				fmt.Print/fmt.Println/fmt.Printf）。

2、在 fmt.Printf 中使用下面的说明符来打印有关变量的相关信息：

%+v 打印包括字段在内的实例的完整信息
%#v 打印包括字段和限定类型名称在内的实例的完整信息
%T 打印某个类型的完整说明

3、使用 panic 语句（第 13.2 节）来获取栈跟踪信息（直到 panic 时所有被调用函数的列表）。

4、使用关键字 defer 来跟踪代码执行过程（第 6.4 节）。



## 2.1 生成代码文档

`go doc` 工具会从 Go 程序和包文件中提取顶级声明的首行注释以及每个对象的相关注释，并生成相关文档。

它也可以作为一个提供在线文档浏览的 web 服务器，[golang.org](http://golang.org/) 就是通过这种形式实现的。

**一般用法**

- go doc package 获取包的文档注释，例如：go doc fmt 会显示使用 godoc 生成的 fmt 包的文档注释。

- go doc package/subpackage 获取子包的文档注释，例如：go doc container/list。
- go doc package function 获取某个函数在某个包中的文档注释，例如：go doc fmt Printf 会显示有关 fmt.Printf() 的使用说明。

这个工具只能获取在 Go 安装目录下 ../go/src 中的注释内容。此外，它还可以作为一个本地文档浏览 web 服务器。在命令行输入 godoc -http=:6060，然后使用浏览器打开 localhost:6060 后，你就可以看到本地文档浏览服务器提供的页面。

godoc 也可以用于生成非标准库的 Go 源码文件的文档注释（第 9.6 章）。

如果想要获取更多有关 godoc 的信息，请访问该页面：golang.org/cmd/godoc/（在线版的第三方包 godoc 可以使用 Go Walker）。

# 三、基本结构和基本类型

## 3.1 Go 程序的基本结构和要素

示例：hello.go

```go
package main

import "fmt"

func main() {
    fmt.Println("hello, world")
}
```

## 3.2 包的概念、导入与可见性

包是结构化代码的一种方式：每个程序都由包（通常简称为 pkg）的概念组成，可以使用自身的包或者从其它包中导入内容。

如同其它一些编程语言中的类库或命名空间的概念，每个 Go 文件都属于且仅属于一个包。一个包可以由许多以 .go 为扩展名的源文件组成，因此文件名和包名一般来说都是不相同的。

你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main 表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。

一个应用程序可以包含不同的包，而且即使你只使用 main 包也不必把所有的代码都写在一个巨大的文件里：你可以用一些较小的文件，并且在每个文件非注释的第一行都使用 package main 来指明这些文件都属于 main 包。如果你打算编译包名不是为 main 的源文件，如 pack1，编译后产生的对象文件将会是 pack1.a 而不是可执行程序。另外要注意的是，所有的包名都应该使用小写字母。

------

**标准库**

在 Go 的安装文件里包含了一些可以直接使用的包，即标准库。在 Windows 下，标准库的位置在 Go 根目录下的子目录 pkg\windows_386 中；在 Linux 下，标准库在 Go 根目录下的子目录 pkg\linux_amd64 中（如果是安装的是 32 位，则在 linux_386 目录中）。一般情况下，标准包会存放在 $GOROOT/pkg/$GOOS_$GOARCH/ 目录下。

Go 的标准库包含了大量的包（如：fmt 和 os），但是你也可以创建自己的包（第 9 章）。

如果想要构建一个程序，则包和包内的文件都必须以正确的顺序进行编译。包的依赖关系决定了其构建顺序。

属于同一个包的源文件必须全部被一起编译，一个包即是编译时的一个单元，因此根据惯例，每个目录都只包含一个包。

------

**如果对一个包进行更改或重新编译，所有引用了这个包的客户端程序都必须全部重新编译。**

Go 中的包模型采用了显式依赖关系的机制来达到快速编译的目的，编译器会从后缀名为 `.go` 的对象文件（需要且只需要这个文件）中提取传递依赖类型的信息。

如果 `A.go` 依赖 `B.go`，而 `B.go` 又依赖 `C.go`：

- 编译 `C.go`, `B.go`, 然后是 `A.go`.
- 为了编译 `A.go`, 编译器读取的是 `B.o` 而不是 `C.o`.

这种机制对于编译大型的项目时可以显著地提升编译速度。

------

**每一段代码只会被编译一次**

一个 Go 程序是通过 import 关键字将一组包链接在一起。

import "fmt" 告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入 / 输出）的函数。包名被封闭在半角双引号 "" 中。如果你打算从已编译的包中导入并加载公开声明的方法，不需要插入已编译包的源代码。

如果需要多个包，它们可以被分别导入：

```go
import "fmt"
import "os"
```

或：

```go
import "fmt"; import "os"
```

但是还有更短且更优雅的方法（被称为因式分解关键字，该方法同样适用于 const、var 和 type 的声明或定义）：

```go
import (
   "fmt"
   "os"
)
```

它甚至还可以更短的形式，但使用 gofmt 后将会被强制换行：

```go
import ("fmt"; "os")
```

当你导入多个包时，最好按照字母顺序排列包名，这样做更加清晰易读。

如果包名不是以 . 或 / 开头，如 "fmt" 或者 "container/list"，则 Go 会在全局文件进行查找；如果包名以 ./ 开头，则 Go 会在相对目录中查找；如果包名以 / 开头（在 Windows 下也可以这样使用），则会在系统的绝对路径中查找。

导入包即等同于包含了这个包的所有的代码对象。

除了符号 _，包中所有代码对象的标识符必须是唯一的，以避免名称冲突。但是相同的标识符可以在不同的包中使用，因为可以使用包名来区分它们。

包通过下面这个被编译器强制执行的规则来决定是否将自身的代码对象暴露给外部文件：

------

**可见性规则**



# 四、变量

```go
package main

import "fmt"

//全局变量
//glo := "Global"	--全局变量不能这么定义
var Glo1 = "Global"

//一次性定义变量
var (
	Glo2 = "Global2"
	n8   = 1
)

//fmt.Println("Glo = ", glo)
func main() {
	//局部变量
	var loc = "location"
	fmt.Println("loc = ", loc)
	//全局
	fmt.Println("Glo = ", Glo1, Glo2, n8)

	//第一种：变量的使用方法
	var num int = 13
	fmt.Println("num =", num)

	//第二种：指定变量的类型，但不赋值，使用默认值
	var num2 int
	fmt.Println("num2 =", num2)

	//第三种：如果没有写变量的类型，那么根据=后面的值进行判断变量的类型	（自动类型判断）
	var num3 = "wjs"
	fmt.Println("num3 =", num3)

	//第四种：省略var,注意  := 不能写为 =
	sex := "男"
	fmt.Println("sex = ", sex)
	fmt.Println("-----------------------------------------------------------------------------------------------")

	//声明多个变量
	var n1, n2, n3 int
	fmt.Println(n1, n2, n3)

	var name, age, height = "wjs", 25, "170cm"
	fmt.Println(name, age, height)

}
```

# 五、数据类型

## 5.1 整数类型

### 5.1.1 有符号整数类型

符号就是表示数字前面的正负号

​	0：表示正数		1：表示复数

| 类型  | 有无符号 | 占用存储空间 | 表数范围                |
| ----- | -------- | ------------ | ----------------------- |
| int8  | 有       | 1字节        | -128~127                |
| int16 | 有       | 2字节        | -32768~32767            |
| int32 | 有       | 4字节        | -2的31次方~-2的31次方-1 |
| int64 | 有       | 8字节        | -2的63次方~-2的63次方-1 |

PS：127怎么算出来的

​		011111111 ---> 二进制 ---->转为十进制：127

PS：-128怎么算出来的

​		10000000 --->二进制 ---->-看就是负数

​		减1：01111111

​		取反：10000000 ----> 得到的一个正数

​		加负号：-128

**代码测试超出范围：**

```go
package main

import "fmt"

func main() {
	//定义一个整数类型
	var num int8 = 230
	fmt.Println(num)
}

// 输出的结果:报错了
PS E:\goStudy\src\project\project\datatype> go run main.go
# command-line-arguments
.\main.go:7:17: cannot use 230 (untyped int constant) as int8 value in variable declaration (overflows)
```



### 5.1.2 无符号类型

 无符号：没有负数类型

| 类型   | 有无符号 | 占用存储空间 | 表数范围                                          |
| ------ | -------- | ------------ | ------------------------------------------------- |
| unit8  | 无       | 1字节        | -2的31次方~-2的31次方-1   -2的63次方~-2的63次方-1 |
| unit16 | 无       | 2字节        | 2的16次方-1                                       |
| rune   | 有       | 等价int31    | -2的31次方-1 ~ 2的31次方-1                        |
| byte   | 无       | 等价unit8    | 0~255                                             |



### 5.1.3 其他类型

| 类型  | 有无符号 | 占用存储空间                 | 表数范围                   |
| ----- | -------- | ---------------------------- | -------------------------- |
| int   | 有       | 32位系统4字节  64位系统8字节 | -128~127                   |
| uint  | 无       | 32位系统4字节  64位系统8字节 | 2的32次方-1    2的64次方-1 |
| int32 | 有       | 4字节                        | -2的31次方~-2的31次方-1    |
| int64 | 有       | 8字节                        | -2的63次方~-2的63次方-1    |

```go
package main

import (
	"fmt"
	"unsafe"		//导包
)

func main() {
	//定义一个整数类型
	var num int8 = 23
	fmt.Println(num)

	//printf函数的作用：格式化，把变量的类型打印出来
	var a = "wjs"
	fmt.Printf("a的变量类型：%T", a) //可以在标准库里面看
	fmt.Println()
	fmt.Println("a的字节数: ", unsafe.Sizeof(a)) //unsafe.Sizeof：打印占用空间的大小，字节
}

```



## 5.2 浮点类型

| 类型    | 存储空间 | 表数范围               |
| ------- | -------- | ---------------------- |
| float32 | 4字节    | -3.403E38 ~ 3.403E38   |
| float64 | 8字节    | -1.798E308 ~ 1.798E308 |

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	//printf函数的作用：格式化，把变量的类型打印出来
	var a = "wjs"
	fmt.Printf("a的变量类型：%T", a) //可以在标准库里面看
	fmt.Println()
	fmt.Println("a的字节数: ", unsafe.Sizeof(a)) //unsafe.Sizeof：打印占用空间的大小，字节

	//浮点数
	var b = 2.1
	fmt.Println("b的字节数和类型:", unsafe.Sizeof(b))
	fmt.Printf("b的变量类型：%T", b)
}
```

## 5.3 数据类型之间的转换

### 5.3.1 整数类型转换字符串

```go
package main

import "fmt"

func main() {
	var n1 = 10
	var n2 = fmt.Sprintf("n2 %T", n1)
	fmt.Printf("n2的数据类型: %T;n2 = %v", n2, n2)
}
PS E:\goStudy\src\project\errorharding\main> go run .\main.go
n2的数据类型: string;n2 = n2 int
```



# 六、运算符

##  6.1 算数运算符

```go
+ -	*	/	%	++	--

import "fmt"

func main() {
	//字符串拼接
	var s1 = "aaa" + "bbb"
	fmt.Println(s1)
}

%：取模公式：a%b=a-a/b*b,或者a/b取余数

注意：++或者-- 只能放在数字的后面：1++,不能写在数字的前面
```



## 6.2 赋值运算符

```
=	+=	-=	/=	%=
```



## 6.3 关系运算符

```
==	!=	>	<	>=	<=
```



## 6.4逻辑运算符

```
&&	||	!
```



## 6.5 位运算符

```go
&	|	^
```



## 6.6 其他运算符

```go
&	*

&：取存储空间对应的地址
*: 取指针变量对应的值，指针就是内存地址
package main

import "fmt"

func main() {
	//取存储地址
	var age = 1
	fmt.Println("age对应存储的地址：", &age)

	//取存储地址中的值
	var ptr *int = &age
	fmt.Println("ptr这个指针指向的值：", *ptr)
}

PS E:\goStudy\src\project\project\operator> go run .\main.go
age对应存储的地址： 0xc00000e0d0
ptr这个指针指向的值： 1
```

# 七、流程控制

## 7.1 if / else

语法：

```go
if 条件表达式{
    逻辑代码
}if else 条件表达式{
    逻辑代码
}if else 条件表达式{
    逻辑代码
}else {
    逻辑代码
}

```



## 7.2 switch

语法：

```go
switch 表达式{
    case 值1，值2,...：
    	语句块
    case 值3,值4,...：
    	语句块
    ...
    default:
    	语句块
}
```

语法2：不推荐这样使用

```go
switch {
	case 条件表达式：
		语句块
	case 条件表达式：
		语句块
}
```

例如：

```go
switch a := 7;{	//不能使用var,用 “;”表示结束
    case a > 6：
    	fmt.Println("A")
    	fallthrough			//穿透到下面的语句块，只能穿透一次
    case a > 8：
    	fmt.Println("B")
}
PS:
	最终的结果：A和B
```



## 7.3 for 循环

语法：

```go
for (初始化表达式;布尔表达式;迭代因子){
    循环体;
}
```

例如：

```go
package main

import "fmt"

var sum = 0

func main() {
	for i := 1; i <= 4; i++ {	//不能用var定义变量
		sum += i
	}
	fmt.Println(sum)
}
```

案例：

```go
package main

import "fmt"

func main(){
	
}
```



# 八、函数

函数规范：

1. ​	驼峰命名：addNum
2. 首字母大写该函数可以被本包文件和其他包文件使用（类似public）
3. 首字母小写只能被本包文件使用，其他包文件不能使用（类似private）
4. 

语法及调用：

```go
func 函数名 (形参列表)(返回值类型列表){
	执行语句
	return + 返回值列表
}

调用函数：
func main() {
    sum += 函数名(值1，值2)
}
```

例如：

```go
package main

import "fmt"

func sum(s1 int, s2 int) (int) {
	a := 0
	a = s1 + s2
	return a
}
func main() {
	s3 := 0
	s3 += sum(10, 20)
	fmt.Println(s3)
}
PS:30
```

## 8.1 可变参数

```go
package main

import "fmt"

//定义一个函数，参数为可变参数...参数的数量是可变的
//arges...int 可以传入任意多个数量的int类型的数据，传入0个，1个
func sum(arges ...int) {
	for i := 0; i < len(arges); i++ {
		//函数内部处理的时候，将可变参数当做切片来处理
		//遍历可变参数
		fmt.Println(arges[i])
	}
}

func main() {
	sum()
	fmt.Println("--------------------------------------------")
	sum(3)
	fmt.Println("--------------------------------------------")
	sum(12, 34, 543, 466)
}

PS E:\goStudy\src\project\project\func> go run .\fun1.go
--------------------------------------------
3
--------------------------------------------
12
34
543
466
PS E:\goStudy\src\project\project\func> 
```

## 8.2 注意俩个函数的传值

注意：这样是无法改变num1的值

基本数据类型和数组都是值传递的，即进行值拷贝，在函数内修改，不会影响到原来的值

```go
package main

import "fmt"

func test(num int) {
	num = 30
	fmt.Println("num = ", num)
}

func main() {
	var num1 = 10
	test(num1)
	fmt.Println("num1 = ", num1)
}
PS E:\goStudy\src\project\project\func> go run .\fun1.go
num =  30
num1 =  10
```



通过指针的方式改变main函数里面的值

以值传递方式的数据类型，如果希望在函数内的变量能修改函数外的变量，可以传入变量的地址&，函数内以指针的方式操作变量。从效果来看是类似引用传递

```go
package main

import "fmt"

func test(num *int) {
	*num = 30			//指针
	fmt.Println("num = ", num)
}

func main() {
	var num1 = 10
	test(&num1)
	fmt.Println("num1 = ", num1)
}

PS E:\goStudy\src\project\project\func> go run .\fun1.go
num =  0xc0000a6058
num1 =  30
```

## 8.3 函数调用函数

```go
package main

import (
	"fmt"
)

func test(num int) {
	fmt.Println("num = ", num)
}

// 定义函数，把另一个函数作为形参
func test2(num int, num2 float32, testfunc func(int)) {
	fmt.Println("----------test2")
	//fmt.Println(num)
	//fmt.Println(num2)
	//fmt.Println(testfunc)
}

func main() {
	//函数也是一种数据类型，可以赋值给一个变量
	a := test                                        //变量就是一个函数类型的变量
	fmt.Printf("a的类型是：%T,test函数的类型是：%T \n", a, test) //a的类型是：func(int),test函数的类型是：func(int)

	//通过该变量可以对函数调用
	a(10) //等价于	test(10)

	//调用test2(10,3.19,test)
	test2(10, 3.19, test)
	test2(10, 3.19, a)
}

PS E:\goStudy\src\project\project\func> go run .\fun1.go
a的类型是：func(int),test函数的类型是：func(int) 
num =  10
----------test2
----------test2
```

## 8.4 自定义数据类型

数据类型转换

```go
package main

import (
	"fmt"
)

func main() {
	//自定义数据类型 myint
	type myint int		
	var n1 myint = 30
	fmt.Println("n1 = ", n1)

	var n2 = 10
	n2 = int(n1)		//数据类型转换
	fmt.Println("n2 = ", n2)
}
```

## 8.5 自定义函数类型

```go
type myfunc func(int)	//自定义函数类型
func test2(num int, num2 float32, testfunc myfunc) {	//调用函数：myfunc
	fmt.Println("----------test2")
	fmt.Println(num)
	fmt.Println(num2)
	fmt.Println(testfunc)
}
```



## 8.6 支持对函数的返回值命名



```go
package main

import "fmt"

func test(n1 int, n2 int) (s1 int, s2 int) {//顺序无所谓，不用一一对应

	s1 = n1 + n2
	s2 = n1 - n2
	fmt.Printf("s1 = %v,s2 = %v", s1, s2)
	return			//这里返回的是：s1,s2
}

func main() {
	test(10, 20)
}
```

## 8.7 包的引入

连接数据库的包

```go
package dbutile	//在一个目录下，其他的go文件这个的package必须是一样的，建议是文件夹的名字

import (
	"fmt"
)
	
func GetConnetDB() {		//函数名或者变量名：首字母必须大写才能被其他包引用
	fmt.Println("连接数据库")
}
```

主函数调用连接数据库的包

```go
package main

import (
	"fmt"
	"project/project/funccall/dbutile"	//注意路径
)

func main() {
	fmt.Println("执行主函数：main")
    dbutile.GetConnetDB()		//包名.函数名()
}

```

同一个包下，函数名必须不一样

# 九、错误处理机制

1、panic：错误异常，恐慌

## 9.1 错误的捕获机制/错误处理

引入的机制：defer + recover 机制处理，在标准库中是可以看到的

优点：提高语言的健壮性

[Go语言标准库文档中文版 | Go语言中文网 | Golang中文社区 | Golang中国 (studygolang.com)](https://studygolang.com/pkgdoc)

语法：defer + recover

```go
func recover() interface{}

内建函数recover允许程序管理恐慌过程中的Go程。在defer的函数中，执行recover调用会取回传至panic调用的错误值，恢复正常执行，停止恐慌过程。若recover在defer的函数之外被调用，它将不会停止恐慌过程序列。在此情况下，或当该Go程不在恐慌过程中时，或提供给panic的实参为nil时，recover就会返回nil。
```

例如：

```go
```







