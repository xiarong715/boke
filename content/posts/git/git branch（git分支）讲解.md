---
title: "Git Branch（git分支）讲解"
date: 2023-05-24T17:37:16+08:00
draft: false
tags: ["git", "git branch"]
categories: ["Git"]
---

一、前言
		在开发中，建议经常创建分支、合并分支和删除分支。当发布一个软件的稳定版本后，发现有一个`bug`，这时创建一个分支解决`bug`，等测试完成，解决`bug`后，合并新分支到`master`分支，最后删除新分支。

​		当开发一个新功能时，创建新分支，在新分支中添加新功能，测试没问题后，合并到`master`分支，最后删除新分支。`git`分支的好处是，在不影响稳定代码的情况下，修补`bug`，添加新功能；



二、`git`分支的使用

1.当修补`bug`或添加新功能时，创建分支`testing`

```shell
git branch testing		# 创建新分支testing
git checkout testing	# 切换到新分支testing

# 或

# 创建新分支并切到新分支testing （一步操作顶两步操作，当分支存在时会出错；切换到存在的分支时，使用 git checkout testing）
git checkout -b testing
```



2.测试完成后，合并分支

```shell
# 首先切换到要合并的分支
git checkout master

# 合并分支
git merge testing

# 解决合并冲突，当两个分支中修改同一文件的同一地方，合并分支时就会冲突：如index.html
# 首先，打开文件index.html会看到 <<<<<<<<  ==========  >>>>>>>>>>> 的内容，这些内容就是冲突的位置，手动合并冲突的内容；
# 然后
git add index.html
git commit -a -m "merge testing, change for xxx bug"
```



3.合并完成后，分支testing的使命完成，删除分支testing

```shell
git branch -d testing
```



三、git 分支管理
```shell
git branch  			# 查看所有分支，带*号的分支，表示当前所在的分支；
git branch -v 			# 查看所有分支最后一次提交的对象信息
git branch --merged		# 查看哪些分支已被合并到当前分支，此时可以没有带*号的分支，它们已合并到当前分支了，删除也不会有损失；
git branch -d testing	# 删除已被合并的分支
git branch --no-merged  # 查看哪些分支没有被合并到当前分支，此时删除这些分支会报错，提示这此分支没有被合并；
git branch -d lsy		# 删除没有合并的分支，报错，因为那样做会丢失数据 （强制删除：git branch -D lsy   慎重！！！！）	
```



四、利用分支进行开发的工作流程

长期分支

特性分支



五、远程分支

```shell
# 推送本地分支到远程仓库
git fetch origin 				# 获取远程仓库origin的最新数据
git push origin master			# 推送master分支到远程仓库origin的master分支 （同 git push origin master:master）
git push origin master:ownfix	# 推送master分支到远程仓库origin的ownfix分支
git merge origin/serverfix		# 合并远程分支到（本地）当前分支

# 从远程分支分化一个新分支，创建本地分支serverfix，内容和远程分支origin/serverfix一样，并切换到远程分支
git checkout -b serverfix origin/serverfix
```



```shell
# 跟踪远程分支
# 从远程分支分化的本地分支，称为跟踪分支，在跟踪分支里输入git push，git会自动推断向哪个远程仓库的哪个分支推送数据，输入git pull，git会自动推断向哪个远程仓库的哪个分支拉取数据，并且合并到本地当前分支中；
# 当克隆一个远程仓库时，git会自动创建一个master本地分支，跟踪origin/master远程分支，（如：git checkout -b master origin/master的作用）；
# git push、git pull 推送拉取数据
	
git checkout -b ownfix origin/ownfix 	# 跟踪其他分支，本地分支ownfix跟踪origin/ownfix远程分支
git checkout -b sf origin/serverfix		# 设置本地分支与远程分支不同的名字
git checkout --track origin/serverfix	# 创建本地分支serverfix跟踪远程分支origin/serverfix; 1.6.2版本的git，可使用--track参数
```



```shell
# 手动设置本地分支跟踪远程分支
git branch --set-upstream-to=origin/master master	# 本地master分支跟踪远程分支origin/master
```

```shell
# 删除远程分支
# git push <远程仓库名> <本地仓库分支名>:<远程仓库分支名>
# git push <远程仓库名> <来源地>:<目的地>
git push origin :serverfix			# 删除远程分支serverfix
```



六、分支的衍合(rebase)
```shell
git checkout serverfix			# 切换到serverfix分支
git rebase master				# 把server分支在master和serverfix的祖先结点上的操作，在master上重演一遍。	

# 也可用
git rebase master serverfix		# 与上面两条命令得到的结果相同
```

```shell
# 快进主干分支master
git checkout master				# 切换到master分支
git merge serverfix				# 合并serverfix到master，因为serverfix和master在一条线上，master指针会直接移到serverfix指向的对象。（称为快进）。
```

```shell
# 当从主干分支分出两条分支，且两条分支有不同与主干分支的祖先结点，但又只想衍合其中一条分支(serverfix)
#			master
#			|
#	--------|		 |---------testing
#			|--------|
#					 |---------serverfix        (意思一下，不够形象)

# 找出serverfix在serverfix与testing共有的祖先结点上的操作，在master上重演一遍。
git rebase --onto master serverfix testing	

#			master
#			|------------------serverfix'
#	--------|		 |---------testing
#			|--------|
#					 |x-x-x-x-xserverfix（没了）


git checkout master
git merge serverfix			# 快进主干分支master

#					           master
#	--------|------------------serverfix'
#			|
#			|		 |---------testing
#			|--------|
#					 |x-x-x-x-xserverfix（没了）
```

```shell
#又想合并另一个分支(testing)
git rebase master testing
	
#					          master
#	--------|-----------------serverfix'---------testing'
#			|
#			|		 
#			|--------|x-x-x-x-testing（没了）

git checkout master
git merge testing		# 快进主干分支master
	
#										         master
#	--------|-----------------serverfix'---------testing'
```



```shell
# 最后删除分支
git branch -d serverfix
git branch -d testing

#	--------|------------------------------------master
```



六、设置远程分支
设置本地分支`addlibsoxr-v0`追踪到远程分支`origin/addlibsoxr-v0`上

```shell
git push --set-upstream origin/addlibsoxr-v0 addlibsoxr-v0
```



七、重命名分支

```shell
git branch -M main 	# 重命名分支为main
git branch -m main	# -M  -m 作用相同
```

可通过`git branch --help`查看用法。
