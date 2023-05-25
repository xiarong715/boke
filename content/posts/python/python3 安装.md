---
title: "Python3 安装"
date: 2023-05-12T16:59:24+08:00
draft: false
tags: ["python3"]
categories: ["Python3"]
---

一、下载

查看python 版本：http://npm.taobao.org/mirrors/python/

```shell
wget https://registry.npmmirror.com/-/binary/python/3.9.13/Python-3.9.13.tar.xz
```

二、编译与安装

```shell
$ tar -xvf Python-3.9.13.tar.xz
$ cd Python-3.9.13
$ yum install openssl-devel 			# install openssl
$ ./configure --with-ssl
$ make && make install
```

三、安装pip 

pip 是 Python 包管理工具。`pip3.9 list`可查看已安装的包，包括有`pip`、`setuptools`、`numpy`等其他包；

```shell
$ pip3.9 list						# check package installed
$ pip3.9 install --upgrade xxx		# upgrade a package
$ pip3.9 install --upgrade pip		# upgrade pip
```



三种安装方法：

1.源文件编译安装或`whl`文件安装

官网：[pypi.org](https://pypi.org/project/pip/)

**源文件编译安装**

源文件地址：

https://files.pythonhosted.org/packages/4b/30/e15b806597e67057e07a5acdc135216ccbf76a5f1681a324533b61066b0b/pip-22.2.2.tar.gz

```shell
$ tar -xvzf pip-22.2.2.tar.gz
$ cd pip-22.2.2
$ python3.9 setup.py build
$ python3.9 setup.py install
```

**`whl`文件安装**

`whl`文件地址

https://files.pythonhosted.org/packages/1f/2c/d9626f045e7b49a6225c6b09257861f24da78f4e5f23af2ddbdf852c99b8/pip-22.2.2-py3-none-any.whl

```shell
$ pip3.9 install pip-22.2.2-py3-none-any.whl
```

要求有pip的一个版本，这种方式用于更新pip

2.python安装pip

```shell
$ yum upgrade python3-setuptools
$ yum install python3-pip
```

3.脚本安装

```shell
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python3.9 get-pip.py
```

四、`python`多版本管理

​	不同的项目，依赖不同`python`版本，那么存在同一台机器安装多个`python`版本，版本管理将是一个问题。官方提出一个虚拟环境的解决方案，每个项目可使用一个独立的`python`环境。

```shell
$ python3.6 -m venv tutorial-env		# create a virtual environment with python3.6 in tutorial-env directory
$ python3.9 -m venv tutorial-env		# create a virtual environment with python3.9 in tutorial-env directory
```



