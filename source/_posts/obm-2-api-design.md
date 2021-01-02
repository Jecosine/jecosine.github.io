---
title: '[在线书籍管理平台] - 2 - API服务三层架构'
tags:
  - online-book-management
categories:
  - project
  - go
date: 2021-01-02 02:48:33
---

## 前言

曾经使用过Java的`Spring MVC`框架的读者可能对三层架构有一定印象

`Controller`层: 控制器,层 与用户直接交互的表现层

`Service`层: 为`Controller`层提供服务, 同时与数据持久层交互

`DAO`层: 数据持久层, 负责数据库操作

`Entity`: 实体定义

在笔者的理解中, Go项目包与这三层的对应关系可能是:

`models` <=> `Entity` + `DAO`

`services` <=> `Service`

`routers`  <=>  `Controller`

接下来我们就先从`routers`开始吧

## 接口定义

为了方便之后对api进行更新迭代, 我们姑且将当前版本的api称作`v1`

新建路径`./routers/api/v1`

当前项目结构如下

```
obm-back-end
├─conf
├─middleware
├─models
├─pkg
│  ├─e
│  └─setting
├─routes
│  └─api
│      └─v1
└─runtime
```

项目详细的接口定义请看 [此处]()

接下来就是传说中的 `CURD` 了

### user

创建`./routers/api/v1/user.go`

```

```

