---
title: "Go plugin 用法"
tags: ["golang", "go plugin"]
categories: ["Golang"]
date: 2020-12-20T18:12:33+08:00
draft: false
---

1.编写plugin部分

创建文件testplugin.go

```go
package main

import "fmt"

func init() {
	fmt.Println("load plugin")
}

// Hello hello
func Hello() {
	fmt.Println("Hello Plugin")
}
```

note: plugin的包名必须指定为main

2.编译plugin部分

```shell
go build --buildmode=plugin testplugin.go
```

编译后会生testplugin.so

3.编写主程序部分

创建文件main.go

```go
package main

import "plugin"

func main() {
	p, err := plugin.Open("plugin/testplugin.so")
	if err != nil {
		panic(err)
	}
	f, err := p.Lookup("Hello")
	if err != nil {
		panic(err)
	}
	f.(func())()
}
```

4.编译主程序

使用模块编译主程序

```shell
// 初始化模块goplugin
go mod init goplugin
// 编译
go build
```

编译后生成goplugin二进制文件

5.测试

```shell
./goplugin
```

打印结果为：

```shell
load plugin
Hello Plugin
```

