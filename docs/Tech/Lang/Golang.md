---
title: Golang笔记
---

# Golang

<!-- more -->

## 安装&配置

### 安装环境报错

go安装环境报错：

```shell
Installing github.com/uudashr/gopkgs/v2/cmd/gopkgs FAILED
```

解决：

```shell
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
```

---

### Go配置

```shell
sudo vim /etc/profile
```

在文件尾添加

```shell
export GOPATH=/home/zhang-yue/工作路径
export GOROOT=/usr/zhang-yue/go安装路径
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

关闭后通过

```shell
source /etc/profile
go version
```

重启即可

## Go语法基础
下面开始快速入门golang(从入门到入坟)

- golang语句后不必加分号

### 包
- 每个Go程序都是由包构成 程序从main包开始运行。

- 在Go中，如果一个名字以大写字母开头，那么它就是可导出的。(例如fmt.Println P大写)

- 在导入一个包时，您只能引用已导出的名字，任何未导出的名字在该包外均无法访问

### 函数
- 声明格式

    ```go
    func add(x,y int) int {
        return x+y
    }
    ```
   > 注意：与C不同的是 Go在声明函数时函数返回值类型在最后 具体原因在[这里](https://blog.go-zh.org/gos-declaration-syntax)

    >(主要是为了增加可读性吧)

- 函数可以有任意多的返回值 且返回值可被命名

    对于没有参数的```return```语句 将直接返回已命名的返回值 在长程序中应避免直接返回

    例如

    ```go
    func add(x,y int) (sum int) {
        sum = x+y
        return
    }
    ```

### 变量
- 在函数外必须使用```var```关键字声明变量

- 在函数内可使用```:=```简洁赋值语句代替```var```声明

- 与C不同的是，Go在不同类型变量间赋值时必须使用显式转换

### 常量
- 常量声明使用```const```关键字

- 常量可以是字符、字符串、布尔值或数值

- 常量不能用```:=```声明

- 数值常量是高精度的值

    ```go
    const (
        Big = 1 << 100
        //Big的二进制是1后面跟着100个0
        //直接输出将会溢出
    )
    ```

### 判断
#### if

- 基本格式

    ```go
    if x < 0 {
        //do something
    }
    ```

    与C不同的是:表达式外无需小括号

- if语句可以在条件表达式前执行一个简单语句
    该语句声明的变量作用域在if内

    ```go
    if x := 5; x < 10 {
        //do something
    }
    ```

#### switch

- 基本格式

    ```go
    package main

    import (
        "fmt"
        "runtime"
    )

    func main() {
        fmt.Print("Go runs on ")
        switch os := runtime.GOOS; os {
        case "darwin":
            fmt.Println("OS X.")
        case "linux":
            fmt.Println("Linux.")
        default:
            // freebsd, openbsd,
            // plan9, windows...
            fmt.Printf("%s.\n", os)
        }
    }
    ```

- Go只运行选定的case，相当于在每个case后自动加了break
但可使用fallthrough强制执行后续case

- case后无需为常量 取值不必为整数

- case语句从上到下匹配

- 没有条件的switch相当于一长串的if-then-else

    ```go
    package main

    import (
        "fmt"
        "time"
    )

    func main() {
        t := time.Now()
        switch {
        case t.Hour() < 12:
            fmt.Println("Good morning!")
        case t.Hour() < 17:
            fmt.Println("Good afternoon.")
        default:
            fmt.Println("Good evening.")
        }
    }
    ```

#### defer
- defer语句会将函数推迟到外层函数返回之后执行 但其参数会立即求值

- 推迟的函数调用会被压入一个栈中 在外层函数返回后按照后进先出顺序调用

### 循环

#### for
- 基本格式

    ```go
    for i:=0; i<10; i++ {
        sum += i
    }
    ```

    与C语言不同，Go的for语句后的三个部分外没有小括号


#### "while"

- C的while在Go中叫做for

- 基本格式

    ```go
    for sum<1000 {
        sum += sum
    }
    ```

    在"while"条件前后加上两个分号即为标准的for语句

    ```go
    for ;sum<1000; {
        sum += sum
    }
    ```

#### 无限循环

- 只需省略循环条件

    ```go
    for {

    }
    ```

### 结构体

- 结构体指针允许隐式间接引用(这是什么鬼话 看下面例子

    例如

    有一个指向结构体的指针p 那么在C语言中可以通过```(*p).X```来访问其字段X

    在go中允许通过```p.x```来访问

- 这边放一个结构体的例子用来看语法

    ```go
    package main

    import "fmt"

    type Vertex struct {
        X, Y int
    }

    var (
        v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
        v2 = Vertex{X: 1}  // Y:0 被隐式地赋予
        v3 = Vertex{}      // X:0 Y:0
        p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
    )

    func main() {
        fmt.Println(v1, p, v2, v3)
    }
    ```

### 数组与切片
- 数组声明

    ```go
    var a [10]int
    ```

- 切片声明

    ```go
    primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
    ```
    切片提供动态大小

    例中1:4为半开区间 左闭右开

- 切片不存储任何数据 更改切片会修改底层数组

- 切片拥有**长度**和**容量**。

    切片的长度就是它所包含的元素个数。

    切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

    切片 s 的长度和容量可通过表达式```len(s)```和```cap(s)```来获取。

    ```go
    func main() {
	s := []int{2, 3, 5, 7, 11, 13}
    //len=6 cap=6 [2 3 5 7 11 13]

	// 截取切片使其长度为 0
	s = s[:0]
    //len=0 cap=6 []

	// 拓展其长度
	s = s[:4]
    //len=4 cap=6 [2 3 5 7]

	// 舍弃前两个值
	s = s[2:]
    //len=2 cap=4 [5 7]
    }
    ```

    拓展长度时超过容量会引发**panic**

- 切片的零值为```nil```

    ```nil```切片的长度和容量均为0且没有底层数组

- 可用make函数创建切片

    ```go
    a := make([]int, 5) //len(a)=5

    a := make([]int, 0, 5) //len(a)=0,cap(a)=5
    ```

- 切片可包含任意类型

    切片的切片

    ```go
    package main

    import (
        "fmt"
        "strings"
    )

    func main() {
        // 创建一个井字板（经典游戏）
        board := [][]string{
            []string{"_", "_", "_"},
            []string{"_", "_", "_"},
            []string{"_", "_", "_"},
        }

        // 两个玩家轮流打上 X 和 O
        board[0][0] = "X"
        board[2][2] = "O"
        board[1][2] = "X"
        board[1][0] = "O"
        board[0][2] = "X"

        for i := 0; i < len(board); i++ {
            fmt.Printf("%s\n", strings.Join(board[i], " "))
        }
    }
    ```



### Panic

[参考博客](https://www.cnblogs.com/yuxiaoba/p/9813605.html)

```go
package main

import (
    "fmt"
    "errors"
)

func main() {
    fmt.Println("Enter function main.")
    panic(errors.New("something wrong")) // 引发 panic
    p := recover()
    fmt.Printf("panic: %s\n", p)
    fmt.Println("Exit function main.")
}
```

此程序recover()不会执行  引发panic后(控制权会沿着调用栈反方向传播)，后续程序不再执行

`正确用法`

```go
package main

import (
    "fmt"
    "errors"
)

func main() {
    fmt.Println("Enter function main.")
    defer func(){
        fmt.Println("Enter defer function.")
        if p := recover(); p != nil {
        fmt.Printf("panic: %s\n", p)
        }
        fmt.Println("Exit defer function.")
    }()
    panic(errors.New("something wrong")) // 引发 panic。
    fmt.Println("Exit function main.")
}

//运行结果
Enter function main.
Enter defer function.
panic: something wrong
Exit defer function.
```

当一个函数即将结束执行时，写在最下面的defer函数调用会最先执行，其次是写在它上边的与它距离最近的defer函数调用，以此类推。

```go
package main

import "fmt"

func main() {
    defer fmt.Println("first defer")
    for i := 0; i < 3; i++ {
        defer fmt.Printf("defer in for [%d]\n", i)
    }
    defer fmt.Println("last defer")
}


//运行结果
last defer
defer in for [2]
defer in for [1]
defer in for [0]
first defer
```

## swagger-UI 相关
编写完注释后记得运行以下```swag init```来生成接口文档数据(忘了两次了)

> 安装swag:```go get -u github.com/swaggo/swag/cmd/swag```