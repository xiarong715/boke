---
title: "Shell 常用用法"
date: 2023-06-01T14:51:55+08:00
draft: false
tags: ["shell"]
categories: ["Shell"]
---



### ${@:n}

从第n个参数到最后一个参数的集合。

```shell
# test.sh

echo $@
echo ${@:2}		# 打印从第2个参数到最后一个参数
echo ${@:3}		# 打印从第3个参数到最后一个参数
```

```shell
# 第一个参数是命令本身，test.sh
test.sh 1 2 3 4 5

# 2 3 4 5
# 3 4 5
```

