---
title: "Go const与iota"
date: 2023-06-02T09:31:36+08:00
draft: false
tags: ["go", "golang", "const", "iota"]
categories: [Golang]
---

const 与 iota的搭配使用。

const关键字，用于定义常量，常量在定义时初始化，之后不能再被修改。iota第一次出现时值为0，每增加一行，其值加1。const常量没有初始化表达式时，表达式同邻近的上一行。



以上是基本原则。举几个例子説明。

```go
func test1() {
    const (
        d1 = iota	// 0
        d2			// 1
        d3			// 2
        d4			// 3
    )
    fmt.Println(d1,d2,d3,d4)		// 0 1 2 3
}
```

```go
func test2() {
    const 
}
```

