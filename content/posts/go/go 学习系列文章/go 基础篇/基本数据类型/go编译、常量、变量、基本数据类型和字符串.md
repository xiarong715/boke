---
title: "Go 编译、常量、变量、基本数据类型和字符串"
date: 2023-05-29T16:06:21+08:00
draft: false
tags: ["go", "go编译", "go常量", "go变量", "go基本数据类型", "go字符串"]
categories: ["Golang"]
---

> Go 学习记录。有输入，有输出，知识才能内化成自己的。为月薪`30K+`奋斗。



###  go 编译

```shell
# GO 1.11后，加入了模块特性
go mod init gopher	# 初始化模块
go build			# 编译模块

# The -mod flag controls whether go.mod may be automatically updated and whether the vendor directory is used
# -mod=mod tells the go command to ignore the vendor directory and to automatically update go.mod, for example, when an imported package is not provided by any known module.
# https://go.dev/ref/mod#build-commands
go build --mod mod -o ./app/certimanager ./app/*.go		# 在源码目录外编译
```

##### go run

像执行脚本一样执行`go`代码

```shell
go run main.go
```

##### go install

编译，并把编译的目标文件拷贝到`$GOPATH/bin`中。

```shell
go install							# 在项目目录下
go install --mod mod ./app/*.go		# 在项目目录外
```

##### 交叉跨平台编译

在`MAC`平台下，可编译出`linux`和`windows`平台的可执行文件。

```shell
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build
```

##### go 语言文件的基本结构

```go
// 声明包名。入口函数main函数必须在main包中。
package main

// 导入fmt包，双引号包裹包名
import "fmt"

// 函数外只能放置标识符（常量、变量、函数和类型）的声明（包括初始化），不能放置语句。

// 程序的入口函数
func main() {
    fmt.Println("hello gopher")
}
```

### 变量

go变量要先声明再使用

##### 声明变量

```go
var str string	// 声明一个保存字符串类型的变量str
var isOK bool
var age int
```

##### 注意事项

1.函数外的每个语句必须以关键字开始（`var`、`const`、`func`等）

2.`:=`不能在函数外使用

3.`_`多用于占位，标识值可以忽略

```go
package main

var (
	age int
    str string
    isOK bool
)

func main() {
    age = 18
    str = "hello gopher"
    isOK = true
}
```

### 字符串

go语言中字符串由双引号包裹。

```go
str1 := "hello"
str2 := "world"
str3 := "gopher"
```

go语言中由单引号包裹的是字符。

```go
s1 := "1"
s2 := "你"
s3 := "h"

// 字节：1个字节=8bit（8个二进制位）
// 一个字符'A'占用一个字节空间
// 一个utf8纺码的汉字一般占3个字节

```

小天才在B站上找了【李文周】老师的Go语言视频教程，非常的好。我强烈推荐一下。

[视频链接](https://www.bilibili.com/video/BV16E411H7og?p=1)

[博客链接](https://www.liwenzhou.com)



刷李文周老师的博客，写实例代码。每段代码我至少敲了3次：第1次整理好思路注释，盲敲；第2次比对我的代码和示例的区别，优化思考；第3次把注释删掉，完全从头到尾撸1~2遍。



>  楼主也无好项目推荐，目前缺项目，李文周老师那个日志收集我做过了，感觉还是缺点

>  缺项目什么意思？没有商业项目做，想丰富一下简历吗？实在没有新项目做，可以把之前做的项目用go语言重构哇，再扎实一下基础知识。



你读的代码越多，你能写的就越多。**没有任何想法是新的或原创的；**所有东西都是从别的地方借来的。

时间管理就是学会**如何把时间用在你想做的事情上，而不是用在你不想做的事情上。**



如果你正在申请或准备申请 Go 开发工作，请找到五个或十个你能切实看到自己面试的空缺职位。他们需要什么具体的技能或知识？你有吗？什么经验是必要的？你有这些经验吗？如果没有，你怎么能得到它？如果他们要求相关的技能，如**网络**、**数据库**或**云计算**，你是否觉得你已经掌握了这些东西？你可以做什么来提高你在这些领域的知识？



我们都没有我们想要的那么多时间或精力；关键是知道如何确定我们所拥有的优先次序。在今天的 15 分钟里，你打算学习什么？准备好一份主题、任务和项目清单，这样你就可以直接进入工作状态。在你涉及到的事情上打勾；每一个勾号都使你离你想去的地方更近一步。



计划你一天中每小时要做的事情，这听起来有点像强迫症，但相信我。它是有效的。



如果你是一个初学者，首先要集中精力，尽可能多地练习简单地编写 Go 代码。一旦你觉得对 Go 有足够的信心并能熟练使用，就开始填补知识空白。阅读 [Go 规范](https://link.juejin.cn?target=https%3A%2F%2Fgolang.org%2Fref%2Fspec)，探索你不知道的东西，或者你没有信心向别人解释的东西。学习和练习这些东西。

看看 Go 的[标准库](https://link.juejin.cn?target=https%3A%2F%2Fpkg.go.dev%2Fstd)。你对它的所有内容都熟悉吗？如果没有，请深入了解。阅读文档。你认为你了解 [fmt](https://link.juejin.cn?target=https%3A%2F%2Fpkg.go.dev%2Ffmt) 库吗？继续阅读，直到你发现至少有一个你不知道的东西。在一个程序中使用它。

练习建立更大的项目，设计包的 APIs，和编写库。弄清楚什么使程序可读、灵活、可扩展、可维护、可扩展。研究其他人的项目。他们是如何工作的？你可以把哪些经验应用到你自己的代码中？

"阅读对心灵的作用就像运动对身体的作用一样"，这两件事是一起的。如果你忽视了身体，头脑就会受到影响，反之亦然。





写进展。

计划每天的每一小时。
