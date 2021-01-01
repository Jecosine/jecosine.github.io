---
title: '[在线书籍管理平台] - 1 - 后台Gin项目框架搭建'
tags:
  - online-book-management
categories:
  - project
  - go
date: 2021-01-02 02:48:33
---

## 项目目录初始化

```
obm-back-end
├─config
├─middleware
├─models
├─pkg
├─routes
└─runtime
```

| Folder Name  | Description    |
| ------------ | -------------- |
| `config`     | 配置文件夹     |
| `middleware` | 中间件         |
| `models`     | 数据库模型     |
| `pkg`        | 第三方包       |
| `routes`     | 路由           |
| `runtime`    | 运行时存放数据 |

## 添加项目配置

拉取`go-ini/ini`依赖

```shell
> go get -u github.com/go-ini/ini
go: downloading github.com/go-ini/ini v1.62.0
go: github.com/go-ini/ini upgrade => v1.62.0
```

创建`config/app.ini`

```ini
RUN_MODE = debug

[app]
NAME = My Online Book Manager

[server]
HTTP_PORT = 8888

[database]
TYPE = mysql
USER = obm
PASSWORD = your-password
HOST = localhost:port
NAME = obm
```

