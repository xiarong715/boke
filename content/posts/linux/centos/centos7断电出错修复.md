---
title: "Centos7断电出错修复"
date: 2023-05-18T16:56:07+08:00
draft: false
---

[参考](https://developer.aliyun.com/article/759518)

1.centos7断电后出现错误：

现象

`Centos7：“Entering emergency mode. Exit the shell to continue”`错误解决方法

输入以下命令

```shell
xfs_repair -v -L /dev/dm-0
```

经过一段时间修复，reboot 重启就能正常启动

