---
title: "Shell 一个功能脚本的分析与语法讲解"
date: 2023-05-31T11:56:12+08:00
draft: false
tags: ["shell"]
categories: ["Shell"]
---

## 脚本展示。用指定的方式编译并执行一个`go`程序。

`build_run.sh`用于编译并执行一个`go`程序。获取项目文件夹名，转化为小写，作为模块名。当项目中没有`go.mod`文件时，执行`go mod init module_name`生成`go.mod`。添加模块路径到`go.work`文件中，创建目标生成的目录。编译并执行`go`程序。

```shell
#!/bin/bash

PWD=`pwd`
MODULE_FILE=$PWD/go.mod
DIR_NAME=$(basename ${PWD})
MODULE_NAME=${DIR_NAME,,}   # 大写全部转小写

# 如果没有go.mod文件
# 获取文件夹名，变小写当做模块名
if [ ! -f $MODULE_FILE ]; then
    go mod init $MODULE_NAME
fi

go work use .

# 没有build/则创建
BUILD_NAME=build
if [ ! -d ${BUILD_NAME} ]; then
    mkdir ${BUILD_NAME}
fi

go build -o ${BUILD_NAME}/          # 输出的目录可自动创建，上面创建目录的过程可省略。

exec ${PWD}/${BUILD_NAME}/${MODULE_NAME}

```

## 功能拆解与分析

一行一行讲解功能与语法。

```shell
#!/bin/bash		# 给系统提示，用/bin/bash来解释文件内容
```

给系统提示，用`/bin/bash`来解释该文件内容。若不在文件首行写上`#!/bin/bash`，则可用`/bin/bash build_run.sh`来执行脚本。

若是`python`脚本，则可在文件首行加上`#!/bin/python3`，也可直接用`/bin/python3 xxxx.py`执行`python`脚本。



---

```shell
PWD=`pwd`		# 获取当前所在目录，并赋给PWD变量
```

`pwd`：用尖括号括起来的命令，代表执行该命令。执行`pwd`命令，获取当前所在的目录。PWD=`pwd`获取当前目录并赋给变量PWD。



---

```shell
MODULE_FILE=$PWD/go.mod		# 当前路径下go.mod文件的路径赋给变量MODULE_FILE
```

`$PWD`：引用变量`PWD`，获取`PWD`变量的值。`$PWD/go.mod`路径赋给`MODULE_FILE`变量。



---

```shell
DIR_NAME=$(basename ${PWD})	# 获取所在路径的最后一级目录名，赋给DIR_NAME变量。
```

**$() 和``。**在bash中，$()和反引号都用作命令替换。命令替换用来重组命令行，首先执行括起来的命令，然后用其结果替换，再组成新的命令行。

`$(basename ${PWD})`：执行`basename ${PWD}`命令，再用结果作替换，同用尖括号括起来的`basename`。获取所在路径的最后一级目录名，赋给DIR_NAME变量。



获取最后一级目录名，也有其他的方式。

**${}**。作用是获取变量的结果。可在其中加入匹配操作，对变量的值操作。`$var`和`${var}`没有区别，只是`${var}`能精确界定变量名的范围。

注意：${}是对变量取值，因此${}中必须是变量。只有变量名，或包含对变量的操作。

[变量匹配参考](https://www.jb51.net/article/275532.htm) 

[${}、$()、``、$(())参考](https://blog.csdn.net/sinat_34241861/article/details/121457548)

```shell
# ${}  变量操作符
# #左匹配符，%右匹配符。##最大化左匹配符，%%最大化右匹配符。  */ 代表截掉匹配到的左边的部分和字符/。 /* 代表截掉右边的和字符/
# ${PWD##*/}		# 从左边开始，最大化匹配字符/，然后截掉左边内容（包括字符/）,返回余下的右侧部分。
# 例如
PWD=/root/work/app
echo ${PWD##*/}		# 最大化匹配/，截掉匹配到的左边的部分和字符/。获取最后一级目录名 app
```

```shell
# 扩展
FILE=/root/work/app/hello.go
echo ${FILE##*.}	# 最大化左匹配.。go

FILE=/root/work/app/hello.tar.gz
echo ${FILE#*.}		# 左匹配.。tar.gz

FILE=/root/work/app/hello.tar.gz
echo ${FILE%/*}		# 右匹配/。/root/work/app

FILE=hello.tar.gz
echo ${FILE%%.*}	# 最大化右匹配.。hello
```

**$(())**。作用是进行整数运算。在$(())中的变量名称，可在变量前加$符号，也可不加。

```shell
a=2 b=3
echo $((a+b))		# 5
echo $(($a+$b))		# 5
```

$(())也可以将其他进制转化为十进制展示出来。$((N#xx))，N表示进制，xx表示在该进制下的数。

```shell
echo $((2#110))		# 6
echo $((16#2a))		# 42
echo $((8#12))		# 10
```



**`basename`与`dirname`**

`basename`截取文件名或最后一级目录名。

`dirname`截取文件（或目录）所在的路径。

```shell
echo $(basename /root/work/app/hello.go)	# hello.go			截取文件名hello.go
echo $(basename /root/work/app)				# app				截取最后一级目录名app

echo $(dirname /root/work/app/hello.go)		# /root/work/app	截取hello.go文件所在的路径/root/work/app
echo $(dirname /root/work/app)				# /root/work		截取app目录所在的路径/root/work
```



---

```shell
MODULE_NAME=${DIR_NAME,,}					# 把DIR_NAME变量的值全部转化为小写，并赋给变量MODULE_NAME
```

把`DIR_NAME`变量的值全部转化为小写，并赋给变量`MODULE_NAME`。

[参考](https://zhuanlan.zhihu.com/p/548290013)

`${DIR_NAME,,}`：,,用于把字符全部转化为小写。

把`DIR_NAME`变量的值全部转化为小写，并赋给变量`MODULE_NAME`

```shell
# 扩展
STR="hello world"
echo ${STR^}		# 首字母转化为大写。Hello world
echo ${STR^^}		# 全部字母转化为大写。HELLO WORLD

STR="GOOD STUDY"
echo ${STR,}		# 首字母转化为小写。gOOD STUDY
echo ${STR,,}		# 全部字母转化为小写。good study
```

```shell
# 使用tr命令实现大小写转化
STR="hello world"
echo `echo $STR | tr [a-z] [A-Z]`	# HELLO WORLD

STR="GOOD STUDY"
echo `echo $STR | tr [A-Z] [a-z]`	# good study
```

```shell
# 使用awk命令搭配tolower()、toupper()函数实现大小写转化
STR="hello world"
echo `echo $STR | awk '{print toupper($0)}'`	# HELLO WORLD

STR="GOOD STUDY"
echo `echo $STR | awk '{print tolower($0)}'`	# good study
```



---

```shell
if [ ! -f $MODULE_FILE ]; then
	go mod init $MODULE_NAME
if
```

判断项目目录下`go.mod`文件是否存在。如果不存在则用`go mod init $MODULE_NAME`，创建`go.mod`，并且使用文件夹名的小写形式作为模块名。`-f`检测常规文件是否存在，`-d`检测目录是否存在。`!`感叹号是取`not`运算。

```shell
# /root/work目录不存在时，输出提示
[ ! -d /root/work ] && echo "/root/work directory does not exist."

```



多个文件检测，可使用`-a`，或`[[ && ]]`来检测多个文件。

```shell
if [ -f /etc/resolv.conf -a -f /etc/hosts]; then
	echo "both files exits."
fi

if [[ -f /etc/resolv.conf && -f /etc/hosts ]]; then
	echo "both files exits."
fi
```



---

```shell
go work use .		# 把当前模块加入go.work中，go项目特定的命令

BUILD_NAME=build
if [ ! -d ${BUILD_NAME} ]; then  # 检测不存在build文件夹时，创建build文件夹
	mkdir ${BUILD_NAME}
fi

go build -o ${BUILD_NAME}/	# 编译go程序，生成的二进制文件会放入${BUILD_NAME}文件夹中，二进制文件名为模块名
```



---

```shell
exec ${PWD}/${BUILD_NAME}/${MODULE_NAME}	# 执行go程序
```

[exec参考](https://www.jianshu.com/p/60a3dae7694f)

`exec`执行命令时，不会启用新的`shell`进程。

`source`和`.`也不会启用新的`shell`进程，在当前`shell`中执行，设定的局部变量在执行完命令后仍然有效。

`bash`或`sh`执行时，会另起一个`shell`进程，其继承父`shell`进程的环境变量，其子`shell`进程的变量执行完后，不影响父`shell`进程。

`exec`是用被执行的命令行替换掉当前的`shell`进程，且`exec`命令后的其他命令将不再执行。

例如，在当前`shell`中执行`exec ls`，表示执行`ls`这条命令来替换当前的`shell`，即为执行完后会退出当前`shell`。

为了避免父`shell`被退出，一般将`exec`命令放到一个子`shell`脚本中，在父`shell`中调用这个子`shell`脚本，调用处可以用`bash xx.sh`（`xx.sh`为存放`exec`命令的脚本），这样会为`xx.sh`建立一个子`shell`去执行，当执行`exec`后该子`shell`进程就被替换成相应的`exec`的命令。

其中有一个例处：当`exec`命令对文件描述符操作的时候，就不会替换当前`shell`，而是会继续执行后面的命令。

```shell
# ...
exec 3>&1 4>&2		# exec 对文件描述符操作
ls					# 会被执行
exec mkdir good		# 替换当前shell
go work use .		# 不会被执行
# ...
```

**文件描述符**
shell中描述符一共有12个

0 代表标准输入

1 代表标准输出

2 错误输出

其他 3-9 都是空白描述符



`exec`执行有两个作用：`exec`执行的命令会替代当前命令，之后的命令不再被执行。`exec`可对文件描述符重定向。
