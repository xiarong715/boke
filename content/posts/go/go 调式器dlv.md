---
title: "Go 调式器dlv"
date: 2023-06-12T15:15:18+08:00
draft: false
tags: ["golang", "dlv", "debug"]
categories: ["Golang", "Delve"]
---

[参考](https://blog.csdn.net/DisMisPres/article/details/128866995)

## Delve 调试器

`Delve`专门为`Go`语言打造的调试工具。

### Delve安装

```shell
# 安装最新版delve
go install github.com/go-delve/delve/cmd/dlv@latest
```

### 经常使用的两种方式

```shell
dlv debug
dlv attach
```

#### dlv debug

1.创建`main.go`文件

```go
package main
import "fmt"

func main() {
    fmt.Println("hello delve")
}
```

2.命令行进入包所在的目录，输入`dlv debug`命令进入调试

```
dlv debug			# 进入调式命令行
(dlv) b main.main	# 设置断点：main.main
(dlv) bp 			# 查看所有设置的断点
(dlv) vars main		# 查看全局变量（可通过正则参数过滤）
(dlv) c				# 运行到下个断点处
(dlv) n 			# 单步执行
(dlv) args 			# 查看传入函数的参数
(dlv) locals		# 查看局部变量
(dlv) b main.go:5	# 组和使用 break 和 condition 命令设置条件断点
(dlv) print nums	# 打印 nums
(dlv) stack			# 查看栈帧信息
(dlv) q				# 退出
```

#### dlv attach

常见的`http`服务调试即可使用这种方式。

1.创建`main.go`

```go
package main

import (
	"log"
    "net/http"
)

func main() {
    http.HandleFunc("/hello", func(writer http.ResponseWriter, request *http.Request) {
        nums := make([]int, 5)
        for i := 0; i < len(nums); i++ {
            nums[i] = i * i
        }
        writer.Write([]byte("OK"))
    })
    
    err := http.ListenAndServe(":2000", nil)
    if err != nil {
        log.Fataln(err)
    }
}
```

2.`go build -gcflags="all=-N -l" main.go`生成`main`，这里一定要加`-gcflags="all=-N -l"`不然有可能代码被编译器优化，断点打不上。

3.执行`main`，得到程序的`PID`

4.`dlv attach PID`进入调试

```shell
dlv attach 22300
(dlv) b main.go:12		# 打断点
(dlv) c					# 运行到下个断点处
```

5.访问网页`http://127.0.0.1:2000/hello`，这时可以看到我们刚刚打的断点生效了。

接下来的用法同上面讲的`dlv debug`用法一致。

### 查看用法

```shell
(dlv) help
```





