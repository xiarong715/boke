---
title: "Centos7 安装与配置使用"
date: 2021-02-11T17:08:50+08:00
draft: false
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
修改为：（不推荐，/etc/resolv.conf是网络管理器创建的，网络重启会被修改）
	nameserver 8.8.8.8
	nameserver 114.114.114.114
或
	vi /etc/sysconfig/network-scripts/ifcfg-ens33
修改为：（推荐，会把变化同步到/etc/resolv.conf中）
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
# aliyun 阿里源
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
# 或
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

```shell
# huawei yun 华为源
wget -O /etc/yum.repos.d/CentOS-Base.repo https://repo.huaweicloud.com/repository/conf/CentOS-7-reg.repo
# 或
curl -o /etc/yum.repos.d/CentOS-Base.repo https://repo.huaweicloud.com/repository/conf/CentOS-7-reg.repo

# 华为源地址：https://mirrors.huaweicloud.com/home
```

```shell
# epel (Extra Packages for Enterprise Linux) 源
mv /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel.repo.backup
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

# 或
yum -y install epel-release		# 安装 epel 源
yum repolist	# 查看仓库列表
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

开发环境安装 [参考](https://www.cnblogs.com/smomop/p/14904769.html)

```shell
yum -y groupinstall "Development tools"
```

五、自动更新系统时间

设置时区

```shell
rm -f /etc/localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

安装`ntp`并配置

```shell
yum install ntp -y		# 安装 ntp
systemctl start ntpd	# 启动 ntp
systemctl enable ntpd	# 开机启动 ntp
service ntpd status
```

修改配置文件

`vi /etc/ntp.conf`

```tex
server 0.pool.ntp.org
server 1.pool.ntp.org
server 2.pool.ntp.org
```

六、防火墙设置

[参考](https://blog.csdn.net/yybk426/article/details/94649619)

列出开放的端口
```shell
firewall-cmd --list-ports		# 列出所有开放端口
firewall-cmd --list-all			# 列出详细信息
```

添加`80、8080`端口到防火墙，即放开`80、8080`端口。

```shell
firewall-cmd --zone=public --permanent --add-port=80/tcp 
firewall-cmd --zone=public --permanent --add-port=8080/tcp
firewall-cmd --zone=public --permanent --add-port=1314/tcp
```

重启`firewalld`服务，生效配置

```shell
systemctl restart firewalld
```

七、脚本加入开机启动

```shell
# 脚本加入开机启动
echo "/bin/sh /server/scripts/inotify.sh &" >> /etc/rc.local		# /etc/rc.local脚本在开机时被执行
```

