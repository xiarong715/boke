---
title: "Go 语言环境搭建"
tags: ["golang"]
categories: ["Golang"]
date: 2020-11-26T21:43:24+08:00
---

1.下载

打开网址，选择稳定版本下载

https://go.dev/dl/

https://golang.org/dl/       (有墙打不开)

https://golang.google.cn/dl

https://studygolang.com/dl

```shell
// 二进制文件包下载
wget https://golang.google.cn/dl/go1.15.4.linux-amd64.tar.gz
wget https://studygolang.com/dl/golang/go1.15.4.linux-amd64.tar.gz

// 源码包下载
wget https://golang.google.cn/dl/go1.15.4.src.tar.gz
```

2.二进制文件安装

```shell
tar -xvzf go1.15.4.linux-amd64.tar.gz -C /usr/local
```

3.源码编译安装

源码需要有go环境，可以用yum安装一个，因为系统的版本更新滞后，所以有必要下载较新的版本安装

```shell
yum install -y go
```

```shell
解压
	tar -xvzf go1.15.4.src.tar.gz -C /tools
编译
	cd /tools/go/src
	./all.bash
```

4.环境配置

```shell
vi /etc/profile 编辑文件添加下面内容：

export GO111MODULE=on				# enable go moudle
export GOPROXY=https://goproxy.cn	# go package proxy
export GOROOT=/usr/local/go			# go root directory
export GOPATH=$HOME/go				# go path directory
export PATH=$PATH:$GOROOT/bin
```

```shell
source /etc/profile	# 生效配置文件
```

5.测试

显示版本信息，说明go环境搭建完成。

```shell
go version			# 打印出go的版本号表明安装成功
# go version go1.15.4 linux/amd64
```



