---
title: "Centos7 问题与解决"
date: 2023-05-18T17:05:03+08:00
draft: false
---

1. 解压tar.bz2

   问题：`tar (child): lbzip2: Cannot exec: No such file or directory` 

   解决：

   ```shell
   yum -y install bzip2
   ```

2. `windows`和`linux`下回车符不一致

   问题：`$'\r': command not found`

   原因：`windows` 与 `unix`下的文件格式不一致，在`windows`下换行符是`\r\n`，在`unix`下换行符是`\n`，`windows`文件在`unix`中解析失败；

   解决：

   方法1：`set ff=unix`

   ```shell
   vi hello.sh
   # Esc 进入命令行模式
   # : 底命令行模式， 输入 set ff=unix
   ```

   方法2：把`windows`格式文件，转换为`unix`格式；

   ```shell
   yum install doc2unix	# download doc2unix translate tool.
   doc2unix hello.sh		# translate windows format file to unix format file.
   ```

3. hello
