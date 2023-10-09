---
title: "Sql使用记录"
date: 2023-06-19T10:43:29+08:00
draft: false
---

## go sql使用记录

```go
// 带有？，需要准备数据，就用先用 prepare方法
stmt, err := sqliteDB.Prepare("DELETE FROM revoke where jwt2=?")

// 之后，传入参数，执行操作
res, err := stmt.Exec(jwt2)
```

```go
// 直接查询
rows, err := sqlite.Query("SELECT * FROM revoke where jwt2=?", jwt2)
rows.Next() // 是否有结果
```

