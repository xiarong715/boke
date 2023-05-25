---
title: "Git 基础"
date: 2023-05-24T18:38:08+08:00
draft: false
tags: ["git"]
categories: ["Git"]
---

1.基础使用

```shell
# 在工作目录初始化新仓库
git init
git remote add origin https://github.com/xiarong715/hello.git 		# 添加远程仓库 origin
git remote add myorigin https://github.com/xiarong715/goStudy.git	# 添加远程仓库 myorigin
git remote set-url myorigin git@github.com:xiarong715/goStudy.git	# 修改远程仓库 myorigin的地址
git pull origin master	# 相当于 git pull origin master:master  上传到远程仓库

# 查看修改记录
git log

# 回到指定版本，如：fd789e1
git reset --hard fd789e1

# 从仓库克隆
git clone https://github.com/xiarong715/gittest.git
```

2.本地新仓库上传到`github`

参考：[a-local-floder-to-remote-git-repo](https://newbedev.com/bring-a-local-folder-to-remote-git-repo)

```shell
# 本地
cd goStudy
git init
git add .
git commit -m "first commit"
```

在github上新创建仓库

![image-20211104191242518](image-20211104191242518.png)

点击`create repository`，完成创建

```bash
$ git remote add origin https://github.com/xiarong715/goStudy.git  # 设置远程仓库路径，origin为远程仓库名，可自定义
$ git push origin master										   # 上传本地master分支到远程仓库origin的master分支
```

`github`仓库创建完成后，官方给出的操作提示。

![RTX截图未命名](image-2021111051032.jpg)

3.提交更新到仓库

工作目录下的所有文件不外乎两种状态：已跟踪和未跟踪。新创建的文件，还没纳入`git`管理，该文件就是未跟踪状态。已纳入`git`管理控制的文件，就是已跟踪状态。工作一段时间后，文件可能会有几种状态：未修改、已修改、已暂存。未修改状态就是在上次提交之后没有被修改；在上次提交之后，有修改过的文件就是已修改状态；修改之后提交到暂存区，下次提交时，会把暂存区的文件提交到仓库，暂存区的文件就是暂存状态。

几种状态的变化过程

```
         untracked file                          tracked file
			|                   |								
			|-------------------|--------------------------------|
			|                   |	                             |    
			|                   |	                             |    
			|                unmodified     modified          staged 
			|                   |---------------|----------------|
			|   git add         |               |                |
			|=================>>|               |                |
			|                   |    edit       |                |
			|                   |=============>>|                |
			|    git restore    |               |                |
			|       --staged    |               |    git add     |
			|<<=================|               |==============>>|
			|                   |               |                |
			|                   |           git commit           |
			|                   |<<==============================|
			|                   |               |                |
			|                   |               |                |
```

4.对应的基本操作


```shell
git add README					# 跟踪文件README
git restore --staged README		# 取消跟踪文件README
git add README					# 跟踪后，再次使用 git add，保存文件快照到暂存区
git commit README -m 'add README' 	# 提交暂存区的内容到本地仓库
```

```shell
git push origin master	# 提交本地仓库的master分支到远程仓库origin的master分支，同 git push origin master:master；
git push origin master:master-v0 # 本地仓库的master分支提交到远程仓库origin的master-v0分支
```

5.远程仓库添加、修改与删除

```shell
# 在本地源代码目录下执行
# 添加远程仓库地址
# git remote add reponame repo-url
git remote add origin https://github.com/xiarong715/goStudy.git  	# add remote repo origin
git remote add track https://github.com/xiarong715/goStudy.git 	# add remote repo track

# 修改已经存在的远程仓库地址
git remote set-url origin git@github.com:xiarong715/goStudy.git
git remote set-url track git@github.com:xiarong715/goStudy.git

# 删除远程仓库地址
$ git remote remove origin   # remove remote repo origin
$ git remote remove track    # remove remote repo track

# 设置好远程仓库后，可使用git push 上传到对应仓库的某个分支
# git push <远程仓库名> <本地分支>:<远程仓库分支>
$ git push origin master	# 本地master分支上传到远程origin仓库的master分支
$ git push track master		# 本地master分支上传到远程track仓库的master分支

# 为master分支设置上传到origin仓库的master分支
$ git push --set-upstream origin master
# 接下来直接使用git push，把master分支上传到origin仓库的master分支
$ git push					# 经过上一步的操作，这一步相当于： git push origin master
```

6.身份验证

`git push`上传要验证身份，因此在`repo-url`中带上用户名和密码，`github`不再支持在`url`中带上用户名和密码的形式。

```shell
git remote remove origin	# remote 之前添加的，没有带上用户名和密码。先删除再添加，或修改
git remote add origin https://xiarong715:xxxxxxxxx@github.com/xiarong715/goStudy.git
# 或
# 直接修改已存在的远程仓库的url
git remote set-url origin https://xiarong715:xxxxxxxxx@github.com/xiarong715/goStudy.git
```

参考：[git remote repo address HTTPS改为SSH](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories#switching-remote-urls-from-ssh-to-https)

`github`不再支持在`URL`中添加用户名和密码的认证形式，把`HTTPS`的`URL`改为`SSH`的形式

```shell
git remote set-url origin git@github.com:xiarong715/goStudy.git			# ssh的形式
```



使用`SSH`的形式，需要在`github`的个人设置中添加主机的`ssh public key`。参考：[`add ssh pulic key to github`](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

a.拷贝主机的`ssh public key`

```bash
vi ~/.ssh/id_rsa.pub		# 查看ssh public key
```

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDItCwXYBAhfTNPhuCdvabcHnRg2vHnzRBeQuEscNby1W4hhPrO/ZinkRWcQvA5VOioS42UL/dT1ro4PZrdJOXEgXK1NprB1KgK+OxZzXMWnNo1fQZnPnQqx6VPFgiLV1NOqWvVtJsee0fEG7qISbmCrVpxVIRjpxsg3j0iMTC5LtZvKWrWvCxTG5CwZ8Ee2TaK/yWJOpSGv6+we7jVt+QPzgxbi0wnfaXqgxVeFWM9zTRJjlUx8QAUHdfh7ZbTPqJlrgi6ebBq6RB/h5PBEkg/D+m11G8mI9rv4LNBB5RVCvPq4TsT5auEKaPYyH2H3GDV9+TYw8t5D1UoQda3OQpB root@localhost.localdomain
```

b.添加`public key`到`github`

![image-20211105161218646](image-20211105161218646.png)

![image-20211105161300116](image-20211105161300116.png)

c.粘贴第a步复制的`ssh public key`

![image-20211105161633085](image-20211105161633085.png)

![image-20211105161803843](image-20211105161803843.png)

d.测试能否登录到github.com

```bash
ssh -T git@github.com   # Attempts to ssh to GitHub
```

成功登录

![image-20211105162040215](image-20211105162040215.png)



[管理远程仓库](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)

注意：（有时行不时不行）

当 `git@github.com:xiarong715/goStudy.git`上传失败时，也可试试`https://github.com/xiarong715/goStudy.git`

```shell
git remote set-url origin https://github.com/xiarong715/goStudy.git
git push origin master
```

前提是要在github.com填写公钥；
