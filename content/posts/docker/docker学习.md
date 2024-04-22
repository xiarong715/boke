---
title: "Docker 学习"
tags: ["docker"]
categories: ["Docker"]
date: 2020-12-23T20:54:22+08:00
draft: false
---

1.`docker`的编写

```dockerfile
FROM registry.lan:5000/ipanel/centos7-dev

MAINTAINER iPanel Cloud Team
LABEL Vendor="iPanel" \
      License=MIT \
      Version=2.0
ADD CI /root/CI
ADD mkyaffs2image /usr/sbin/mkyaffs2image
ADD rc.local /etc/rc.d/rc.local
RUN chmod -v +x /root/CI -R 
RUN chmod -v +x /usr/sbin/mkyaffs2image 
RUN rpm --rebuilddb \
	&& rpm --import \
		http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-7 \
	&& rpm --import \
		https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7 \
	&& yum -y install \
			--setopt=tsflags=nodocs \
			--disableplugin=fastestmirror \
		ncurses-devel zlib-devel bzip2 git openssl file gettext glibc.i686 libstdc++.i686 dos2unix squashfs-tools\
```

`docker file` 命令讲解：

```dockerfile
FROM centos:7								# 引入基础镜像
	
	LABEL Maintainer="iPanel Cloud Team"	\	# MAINTAINER 指令设置生成镜像的 Author 字段（已废弃，现使用LABEL Maintainer="iPanel Cloud Team"）
		  Vendor="iPanel" \						# 设置你需要的任何元数据
		  License=MIT \
		  Version=2.0
		  
	EXPOSE 8088						# 声明端口，告诉运维人员容器开启了哪些端口，则可以在docker container 启动时 docker run -p 808888:8088
	
	ADD run.sh /run.sh 				# 添加文件到系统中
	
	RUN mkdir /fs					# 在系统运行命令
	RUN chmod 777 /fs
	ENV								# 你可以使用ENV来为容器中安装的程序更新 PATH 环境变量,
									# 如ENV PATH /usr/local/nginx/bin:$PATH来确保CMD ["nginx"]能正确运行。
	VOLUME 							# 挂载宿主机文件到容器中
	COPY 
	ENTRYPOINT ["/usr/sbin/init"]	#
	CMD ["systemctl restart autofs"]	# CMD指令用于执行目标镜像中包含的软件和任何参数，如部署某个服务
```



2.编译镜像

编译镜像的命令为`docker build`，命令的使用方法为：`docker build [OPTIONS] PATH`。

```shell
docker build -t test/first_image:v1.0 .

# -t , --tag list : 使用name:tag的方式对镜像命名；
# .   : 使用当前路径中的Dockerfile编镜像。
```



3.运行镜像

```shell
# 运行镜像，并进入shell.
docker container run -it test/first_image:0.1 /bin/bash
# or
docker run -it test/first_image:0.1 /bin/bash

# 或
docker run -itd test/first_image:0.1 /bin/bash
docker exec -it test/first_image:0.1 /bin/bash
```

```shell
docker run -itd --name first_image test/first_image:v1.0 /bin/bash  (后台运行)
docker exec -it first_image /bin/bash     (容器必须存在)（进入其shell）

# 在first_image容器中执行/eccenc
docker exec first_image /eccenc

docker run -itd --name rtlbuildee_ubuntu registry.lan:5000/zapci:3
docker exec -it rtlbuildee_ubuntu /bin/bash  	# 进入rtlbuildee_ubuntu容器的终端
docker exec -it redis-test /bin/bash			# 进入 redis-test 容器的终端
```

会进入到容器的`shell`；

```shell
docker run -ti --privileged=true -d -p 2255:22 --name rtlbuildee_ubuntu --restart=always 51e7336035ad4b2aba84207b1554af54 /sbin/init
```

```shell
docker run -ti --privileged=true -d -p 2255:22 --name rtlbuildee_ubuntu --restart=always 51e7336035ad4b2aba84207b1554af54
```

通过宿主机`ssh root@ip:2255`登录容器。