---
title: "Docker 安装与使用"
tags: ["docker"]
categories: ["Docker"]
date: 2023-03-14T17:35:08+08:00
---

[参考1](https://blog.csdn.net/ityum/article/details/125320467) 编写`Dockerfile`，命令作用

[参考2](https://blog.csdn.net/inthat/article/details/124060033) 镜像构建过程，命令作用

1. `docker`安装

   ```shell
   yum update
   curl -fsSL https://get.docker.com/ | sh    # install lastest docker
   yum install docker   					   # it's not lastest docker
   ```

2. `docker`配置与启动

   设置云镜像地址

   ```shell
   vi /etc/docker/daemon.json
   ```

   ```json
   {
   	"registry-mirrors":["https://nbamvavl.mirrors.aliyuns.com"]
   }
   ```

   

   ```shell
   systemctl daemon-reload   # enable daemon.json
   systemctl restart docker  # restart docker
   systemctl enable docker   # start docker when system start
   ```

3. `docker`的使用

   编写`Dockerfile`

   [命令作用说明](https://docs.docker.com/engine/reference/builder/)

   ```dockerfile
   FROM golang:alpine   # 基础镜像 golang，版本为 alpine
   
   # 为我们的镜像设置必要的环境变量
   ENV GO111MODULE=on \
       CGO_ENABLED=0 \
       GOOS=linux \
       GOARCH=amd64 \
       GOPROXY=https://goproxy.cn,direct
   
   # 移动到工作目录：/build
   WORKDIR /build
   
   # 将代码复制到容器中
   # COPY source dest
   # COPY 宿主机目录 镜像目录
   COPY . .
   
   # 将我们的代码编译成二进制可执行文件app
   RUN go build -o app .
   
   # 移动到用于存放生成的二进制文件的 /dist 目录
   WORKDIR /dist
   
   # 将二进制文件从 /build 目录复制到这里
   # 镜像内拷贝
   RUN cp /build/app .
   
   # 声明服务端口
   EXPOSE 8088
   
   # 启动容器时运行的命令
   CMD ["/dist/app"]
   ```
   
   构建镜像
   
   ```shell
   docker build . -t goweb_app
   docker build . -f ./dockerfile/Dockerfile -t goweb_app2  // 指定Dockerfile文件
   ```
   
   运行镜像
   
   ```shell
   docker run -p 8081:8088 goweb_app		# docker容器的 8088端口绑定到宿主机的8081端口
   ```



4. 分阶段构建镜像

   使用带有`golang`环境的镜像，是为了编译`go`程序。我们可以在一个镜像中编好`go`程序，再拷贝到一个只支持`go`程序运行的镜像中，这样的极大的减小镜像的大小。

   ```shell
   # 阶段镜像命令为 builder
   FROM golang:alpine AS builder
   
   # 为我们的镜像设置必要的环境变量
   ENV GO111MODULE=on \
       CGO_ENABLED=0 \
       GOOS=linux \
       GOARCH=amd64 \
       GOPROXY=https://goproxy.cn,direct
   
   # 移动到工作目录：/build
   WORKDIR /build
   
   # 将宿主机代码复制到容器中
   # COPY source dest
   # COPY 宿主机目录 镜像目录
   COPY . .
   
   # 将我们的代码编译成二进制可执行文件 app
   RUN go build -o app .
   
   ###################
   # 接下来创建一个小镜像
   ###################
   FROM scratch
   
   WORKDIR /dist
   
   # 从builder镜像中把/build/app 拷贝到当前目录/dist下
   COPY --from=builder /build/app .
   
   # 需要运行的命令
   ENTRYPOINT ["/dist/app"]
   ```

   构建镜像

   ```
   docker build -t user.manager:v1.0 .
   ```

   运行镜像

   ```shell
   docker run -ti --privileged=true -d -p 2200:22 --name lsy_python3.6 --restart=always lsy/ubuntu-python3.6:v1.0
   ```

   

5. 镜像构建过程

   镜像构建过程一共有3步：

   ①基于一个镜像，启动一个`docker`容器。

   ②在容器中进行一系列操作，比如，执行命令，安装软件。这个操作产生的文件变更，都会记录在容器的存储层中。

   ③将容器存储层的变更`commit`到新的镜像层中，并添加到原镜像上。





事很难，还是不想做，还是没环境。

富人想的是如何赚钱。