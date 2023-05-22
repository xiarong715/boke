---
title: "Linux 任务前后台切换"
date: 2021-02-12T18:19:40+08:00
tags: ["linux"]
categories: ["Linux"]
draft: false

---

1. 查看后台运行的任务

`jobs`查看当前有多少任务在后台运行

```shell
jobs
```

2. 切换后台任务到前台

`fg(foreground)`将后台中的任务调至前台继续运行

```shell
fg
```

3. 前台任务切换到后台

`ctrl + z`可以将一个正在前台执行的任务放到后台，并且暂停运行

```shell
ctrl + z
```

4. 后台暂停的任务继续在后台运行

`bg(background)`将后台暂停的任务切换为运行状态

```shell
bg
```

5. 在后台运行任务

`hugo &`将`hugo`任务在后台运行

```shell
hugo &
```

