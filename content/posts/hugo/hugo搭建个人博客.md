---
title: "Hugo 搭建个人博客"
tags: ["golang","hugo"]
categories: ["博客"]
date: 2020-11-18T21:49:37+08:00
---

1.Download and Install

需要安装git和go；

git环境搭建参考：

[git环境搭建](/posts/git/git环境搭建/)

go环境搭建参考：

[go环境搭建](/posts/go/go环境搭建)

```shell
mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install --tags extended
```

2.Create a New Site

```shell
hugo new site boke
```

3.Add a Theme

```shell
cd boke
git clone https://github.com/flysnow-org/maupassant-hugo.git themes/maupassant
echo 'theme = "maupassant"' >> config.toml
```

4.Add Some Content

```shell
hugo new content/posts/hugo搭建个人博客.md
hugo new content/post/hugo搭建个人博客.md (使用飞雪无情的主题时，在content/post/下发布文章)(待确认)
```

可把`*.md`文件放在不同的目录中，分类存放，如：`hugo new content/posts/hugo/hugo搭建个人博客.md`

打开hugo搭建个人博客.md文件，修改draft的值为false，否则不会被发布；

5.Start the Hugo server

本地运行hugo服务，编译出静态网页，并启动web服务；

```shell
hugo server -D
```

6.Customize the Theme

如果对主题不满意，可以修改主题；

7.Build static pages

编译静态网页，静态网页会放在public目录下；

```shell
hugo -D
```

8.Deploy to github.io

```shell
cd public
git init
git add *
git commit -a -m "add"

git remote add origin https://github.com/xiarong715/xiarong715.github.io.git
git pull origin master
git branch --set-upstream-to=origin/master master
git push
```

9.Visit the Site

https://xiarong715.github.io

官方参考文档：

https://gohugo.io/getting-started/quick-start/

