---
title: "Docker Compose 使用"
tags: ["docker", "docker-compose"]
categories: ["Docker", "Docker-compose"]
date: 2020-12-29T21:54:23+08:00
draft: false
---

1.docker-compose的作用

方便管理docker容器

2.下载安装

docker-compose会依赖一些库

```shell
yum install -y py-pip python-dev libffi-dev openssl-dev gcc libc-dev make
```

下载docker-compose

```shell
curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

curl -L "https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-$(uname -s)-$(uname -m)" -o /user/local/bin/docker-compose  (待验证)
```

添加执行权限

```shell
chmod +x /usr/local/bin/docker-compose
```

测试

```shell
docker-compose version

## 显示以下信息，表明docker-compose安装完成
docker-compose version 1.26.1, build f216ddbf
docker-py version: 4.2.2
CPython version: 3.7.7
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
```

3.docker-compose.yml的编写

```yml
version: '2.2'
services:
  spkv01:
    image: 'centos7_spk:v1.0'
    container_name: 'spk-01'
    hostname: 'spk-01'
    privileged: 'true'
    ports: 
      - '2225:22'
      - '5200:8080'
    # network_mode: 'bridge'
    networks:
      my_net:
        ipv4_address: '172.18.0.128'
  spkv02:
    image: 'centos7_spk:v1.0'
    container_name: 'spk-02'
    hostname: 'spk-02'
    privileged: 'true'
    ports: 
      - '2226:22'
      - '5300:8080'
    # network_mode: 'bridge'
    networks: 
      my_net:
        ipv4_address: '172.18.0.129'

networks:
  my_net:
    driver: bridge
    ipam:
      config: 
        - subnet: '172.18.0.0/16'
          gateway: '172.18.0.1'
```

4.基本命令

```shell
# 停止容器并删除容器
docker-compose down
# 创建容器并后台运行
docker-compose up -d
# 查看容器的log
docker-compose logs
# 启动容器
docker-compose start
# 停止容器
docker-compose stop
```

