---
title: "Git 环境搭建"
tags: ["git"]
categories: ["Git"]
date: 2020-12-01T20:32:32+08:00
---

1.download a tarball

[下载网址](https://mirrors.edge.kernel.org/pub/software/scm/git/)

[下载最新版本git](https://www.kernel.org/pub/software/scm/git/)

[源代码包下载地址](https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.xz)

```shell
wget https://www.kernel.org/pub/software/scm/git/git-2.9.5.tar.xz
```

2.解压

```shell
tar -xvf git-2.9.5.tar.xz
```

3.configure

```shell
cd git-2.9.5
./configure
```

4.make and make install

```shell
make && make install
```

5.测试

打印出版本号代表安装成功；

```shell
git version
git version 2.9.5
```

---

problems

①`fatal error: zlib.h: No such file or directory`

```shell
yum search zlib
yum install zlib-devel -y
```

②`Can't locate ExtUtils/MakeMaker.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5`

```shell
yum install -y perl-devel
```

③`git fatal: Unable to find remote helper for 'https' or 'http'`

或 `git: 'remote-https' is not a git command. See 'git --help'.`

```shell
yum install -y libcurl-devel
yum install -y curl-devel
cd git-2.9.5
./configure
make && make install
```

④`checking for gcc... no`

```shell
yum install gcc
```





