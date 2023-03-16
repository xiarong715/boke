---
title: "Go 导入自定义包"
tags: ["golang", "go package", "go module"]
categories: ["Golang"]
date: 2021-04-11T11:33:28+08:00
draft: false
---

1.自定义包

为了模块化编程，需要把属于同一功能的源文件放在同一目录下，并在文件开头加上`package calc`，`calc`即为包名，包名与目录名可以不相同；

如：

`goStudy/package/calc/operation.go`

```go
package calc

func Add(a, b int) int {
	return a + b
}

func Sub(a, b int) int {
	return a - b
}

func Multi(a, b int) int {
	return a * b
}

func Division(a, b int) int {
	return a / b
}
```

`goStudy/package/snow/use.go`

```go
package snow

import (
	"fmt"
	"pack/calc"
)

func TestCalc() {
	fmt.Println(calc.Add(1, 2))
	fmt.Println(calc.Sub(2, 3))
	fmt.Println(calc.Multi(3, 4))
	fmt.Println(calc.Division(4, 5))
}
```

`goStudy/package/main.go`

```go
package main

import "pack/snow"

func main() {
	snow.TestCalc()
}
```

2.导入自定义包

 `Go version 1.11`时，go引入`go module`的功能，当`GO111MODULE=on`时，可以直接调用写好的模块；但是当`GO111MODULE`开启时，go编译器默认会在`$GOROOT/src`目录下寻找模块；

```go
package main

import "snow" // 会在$GOROOT/src/下查找snow模块

func main() {
	snow.TestCalc()
}
```

报错：main.go:3:8: package snow is not in GOROOT (E:\app\Go\src\snow)

解决办法：

1. 在package/下创建一个模块，导入自定义包时，指定模块名；

```shell
cd package
go mod init pack
```

2. 导入时指定模块名

```go
package main

import "pack/snow" // 在pack模块中查找snow包

func main() {
	snow.TestCalc()
}
```

例子只是为了演示导入自定义包；