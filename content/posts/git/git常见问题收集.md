---
title: "Git 常见问题收集"
date: 2023-05-25T14:02:05+08:00
draft: false
tags: ["git", "git常见问题"]
categories: ["Git"]
---

1.`git`首次安装，下载项目时出现

**git: 'remote-https' is not a git command. See 'git --help'.**

```shell
yum install libcurl-devel
# recomplie git again
```



2.在`windows`上产生的文件，上传到`github`时

**warning:  in the working copy of ‘xxx.html’, LF will be replaced by CRLF the next time Git touches it**

[参考](https://blog.csdn.net/Babylonxun/article/details/126598477)

`Dos/Windows`平台默认的换行符：回车（`CR`）+ 换行（`LF`），即`\r\n`。

`Mac/Linux`平台默认的换行符：换行（`LF`），即`\n`。

企业服务器一般都是`Linux`系统，所以会有替换换行符的需求。



方法一：

适用于`Windows`系统，且一般为`Windows`默认设置。在`Windows`上编写代码，在提交到服务器时，对换行符进行`CRLF`-`LF`的转换，检出时又会进行`LF`-`CRLF`的转换

```shell
$ git config --global core.autocrlf true
```

方法二：

适用于`Linux`系统，所有换行符都会进行`CRLF`-`LF`转换，但不会转换回`CRLF`。

```shell
$ git config --global core.autocrlf input
```

方法三：

适用于`Windows`系统，且只在`Windows`上开发的情况。在提交和检出时都不进行`CRLF`-`LF`的转换。

```shell
$ git config --global core.autocrlf false
```



文件提交时进行`safecrlf`检查：

```shell
# 拒绝提交包含混合换行符的文件
$ git config --global core.safecrlf true

# 允许提交包含混合换行符的文件
git config --global core.safecrlf false

# 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

3.`go`项目，下载第三方库，上传到自建的仓库，需删除源有仓库的信息。

问题：
```shell
git add wechat/GOPATH/src/github.com/satori/go.uuid/codec.go

# fatal: Pathspec 'wechat/GOPATH/src/github.com/satori/go.uuid/codec.go' is in submodule 'wechat/GOPATH/src/github.com/satori/go.uuid'
```

解决：
```shell
rm -rf wechat/GOPATH/src/github.com/satori/go.uuid/.git
git rm -rf --cached wechat/GOPATH/src/github.com/satori/go.uuid
git add wechat/GOPATH/src/github.com/satori/go.uuid
```

