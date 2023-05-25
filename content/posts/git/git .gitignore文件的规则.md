---
title: "Git .gitignore文件的规则"
date: 2023-05-25T14:59:52+08:00
draft: false
---

一、`.gitignore的作用`

提交到`github`时，会忽略`.gitignore`中配置的文件或目录，这些文件或目录是临时产生的，对项目没影响。

二. `.gitignore`的规则

1. `#`开头的为注释

   

2. 忽略文件和目录

   ```.gitigonre
   # 注释
   target
   ```

   如文件结构：

   ```
   FastAPI
   	target
   	py
   		target
   			tool.md
   ```

   会忽略根目录和子目录下的 `target` 目录和 `target`文件。

   

3. 仅忽略同名文件，不忽略同名目录

   ```
   target
   !target/
   ```

   只会忽略同名的文件，不会忽略同名的目录

   

4. 仅忽略同名目录

   ```
   target/
   ```

   只忽略target目录

   

[`.gitignore`参考](https://blog.csdn.net/nyist_zxp/article/details/119887324)
