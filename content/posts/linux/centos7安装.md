---
title: "Centos7 安装"
date: 2021-02-11T17:08:50+08:00
draft: flase
tags: ["linux", "centos7"]
categories: ["Linux", "Centos7"]
---

一、下载

[镜像下载地址](http://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)

```shell
http://mirrors.aliyun.com/centos/7.7.1908/isos/x86_64/CentOS-7-x86_64-DVD-1908.iso
http://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-Minimal-2009.iso
```

二、安装

1.分区（新手方案）

​	启动分区`/boot` ：`512M`或`1024M`

`	swap`分区 ：内存的2倍

​	根分区 `/`    ：剩余的空间

2.设置静态`IP`及`DNS`

设置静态`IP`

```shell
vi /etc/sysconfig/network-scripts/ifcfg-ens33
修改为：
	BOOTPROTO=static      # 静态IP
	ONBOOT=yes			  # 开机启动
	
	IPADDR=192.168.80.130 # IP
	NETMASK=255.255.255.0 # 子网掩码
	GATEWAY=192.168.80.2  # 网关
```

添加`DNS`

```tex
vi /etc/resolv.conf
修改为：
	nameserver 8.8.8.8
	nameserver 114.114.114.114
或
	vi /etc/sysconfig/network-scripts/ifcfg-ens33
修改为：
	DNS1=8.8.8.8
	DNS2=114.114.114.114
```

三、更换源

备份:

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

下载 `repo` 文件:

```shell
// aliyun 阿里源
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
或
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

// huawei yun 华为源
wget -O /etc/yum.repos.d/CentOS-Base.repo https://repo.huaweicloud.com/repository/conf/CentOS-7-reg.repo
或
curl -o /etc/yum.repos.d/CentOS-Base.repo https://repo.huaweicloud.com/repository/conf/CentOS-7-reg.repo
```

更新缓存:

```shell
yum clean all
yum makecache
```

四、安装`C/C++`编译器

```shell
yum install gcc gcc-c++
```

五、自动更新系统时间

安装`ntp`并配置

```shell
yum install ntp -y		# 安装 ntp
systemctl start ntpd	# 启动 ntp
systemctl enable ntpd	# 自启动 ntp
service ntpd status
```

修改配置文件

`vi /etc/ntp.conf`

```tex
server 0.pool.ntp.org
server 1.pool.ntp.org
server 2.pool.ntp.org
```

