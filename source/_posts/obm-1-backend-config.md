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
├─conf
├─middleware
├─models
├─pkg
├─routes
└─runtime
```

| Folder Name  | Description    |
| ------------ | -------------- |
| `conf`       | 配置文件夹     |
| `middleware` | 中间件         |
| `models`     | 数据库模型     |
| `pkg`        | 第三方包       |
| `routes`     | 路由           |
| `runtime`    | 运行时存放数据 |

创建`main.go`

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Test")
}
```

在当前目录下运行

```bash
❯ go mod init your-repo-url
```

如`go mod init github.com/Jecosine/obm-back-end`

项目目录下会生成`go.mod`文件

## 安装Gin

````shell
go get -u github.com/gin-gonic/gin
````

等待其安装即可

```shell
go: downloading golang.org/x/sys v0.0.0-20200323222414-85ca7c5b95cd
go: github.com/golang/protobuf upgrade => v1.4.3
go: gopkg.in/yaml.v2 upgrade => v2.4.0
go: golang.org/x/sys upgrade => v0.0.0-20201231184435-2d18734c6014
go: github.com/modern-go/reflect2 upgrade => v1.0.1
go: github.com/leodido/go-urn upgrade => v1.2.1
go: github.com/go-playground/validator/v10 upgrade => v10.4.1
go: github.com/json-iterator/go upgrade => v1.1.10
go: github.com/ugorji/go/codec upgrade => v1.2.2
go: github.com/modern-go/concurrent upgrade => v0.0.0-20180306012644-bacd9c7ef1dd
go: downloading golang.org/x/sys v0.0.0-20201231184435-2d18734c6014
go: google.golang.org/protobuf upgrade => v1.25.0
go: golang.org/x/crypto upgrade => v0.0.0-20201221181555-eec23a3978ad
```

`go.mod`中会多出以下配置

```
require (
	github.com/gin-gonic/gin v1.6.3
	github.com/go-playground/validator/v10 v10.4.1 // indirect
	github.com/go-sql-driver/mysql v1.5.0 // indirect
	github.com/golang/protobuf v1.4.3 // indirect
	github.com/jinzhu/gorm v1.9.16
	github.com/json-iterator/go v1.1.10 // indirect
	github.com/leodido/go-urn v1.2.1 // indirect
	github.com/modern-go/concurrent v0.0.0-20180306012644-bacd9c7ef1dd // indirect
	github.com/modern-go/reflect2 v1.0.1 // indirect
	github.com/ugorji/go v1.2.2 // indirect
	golang.org/x/crypto v0.0.0-20201221181555-eec23a3978ad // indirect
	golang.org/x/sys v0.0.0-20201231184435-2d18734c6014 // indirect
	google.golang.org/protobuf v1.25.0 // indirect
	gopkg.in/yaml.v2 v2.4.0 // indirect
)
```

## 安装`github.com/unknwon/com`

是的, 我**没有打错**,就是`unknwon`, com包集合了Go语言中常用的函数

```shell
go get -u github.com/unknwon/com
```

## 添加项目配置

​		一般在项目中, 将配置数据写入程序中是不优雅不灵活的实现, 同时, 这对需要部署你项目的其他用户并不友好. 在这种情况下我们可以使用配置文件, 存放用户环境的数据, 在项目中获取配置的值. 

拉取`go-ini/ini`依赖

```bash
❯ go get -u github.com/go-ini/ini
go: downloading github.com/go-ini/ini v1.62.0
go: github.com/go-ini/ini upgrade => v1.62.0
```

创建配置文件 `conf/app.ini`

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
TABLE_PREFIX = obm_
```

在`pkg`目录下创建`setting`文件夹, 在下面创建`setting.go`, 测试一下读取配置文件信息

`./pkg/setting/setting.go`

```go
package setting

import (
	"log"

	"github.com/go-ini/ini"
)

var (
	// Cfg config file
	Cfg *ini.File
	// RunMode run mode
	RunMode string
	// Name app name
	Name string
	// HTTPPort running port
	HTTPPort int
)
// Only for test
func TestSetup() {
	var e error
	Cfg, e = ini.Load("conf/app.ini")
	if e != nil {
		log.Fatalf("[ERROR] Fail to load conf/app.ini: %v", e)
	}
	RunMode = Cfg.Section("").Key("RUN_MODE").MustString("debug")
	Name = Cfg.Section("app").Key("NAME").MustString("Obm")
	HTTPPort = Cfg.Section("server").Key("HTTP_PORT").MustInt(8888)
	log.Printf("RUN_MODE: %v\nNAME: %v\nHTTP_PORT: %v", RunMode, Name, HTTPPort)
}
```

然后编辑`go.mod`, 由于此时还没有将仓库推送到远程, 所以需要先做一下替换, 使其指向本地目录. 此后每个新建的包都需要进行此操作, 最后的`go.mod`文件见[代码仓库](https://github.com/Jecosine/obm-back-end)

```yml
module github.com/Jecosine/obm-back-end

go 1.15

require (
	github.com/go-ini/ini v1.62.0
)

replace github.com/Jecosine/obm-back-end/pkg/setting => ./pkg/setting

```

编辑`your-project-name/main.go`

```go
package main

import (
	"fmt"

	"github.com/Jecosine/obm-back-end/pkg/setting"
)

func main() {
	setting.TestSetup()
}
```

如果可以看到正确的项目输出, 则基本的配置已经完成了.

```bash
❯ go run main.go
2021/01/02 05:45:21 
RUN_MODE: debug
NAME: My Online Book Manager
HTTP_PORT: 8888
```

## 连接数据库

拉取 mysql 的驱动依赖

```bash
go get -u github.com/go-sql-driver/mysql
```

使用`gorm`,  `gorm`是一个强大的ORM库， 相关文档可以在 [这里](https://gorm.io/) 看到

```
go get -u gorm.io/gorm
```

使用配置文件中的配置连接数据库

`models/models.go`

```go
package models

import (
	"fmt"
	"log"

	"github.com/Jecosine/obm-back-end/pkg/setting"

	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

var db *gorm.DB

// Model base model
type Model struct {
	ID string `gorm:"primary_key" json:"id"`
}

type User struct {
	ID       string `gorm:"primary_key" json:"id"`
	Username string `json:"username"`
	Password string `json:"password"`
	Email    string `json:"email"`
}

func Init() {
	var (
		e                                                       error
		dbType, dbUser, dbPasswd, dbName, dbHost, dbTablePrefix string
	)
	dbSection, e := setting.Cfg.GetSection("database")
	if e != nil {
		log.Fatal(2, "Fail to get section database in app.ini: %v", e)
	}
	dbType = dbSection.Key("TYPE").String()
	dbUser = dbSection.Key("USER").String()
	dbName = dbSection.Key("NAME").String()
	dbPasswd = dbSection.Key("PASSWORD").String()
	dbHost = dbSection.Key("HOST").String()
	dbTablePrefix = dbSection.Key("TABLE_PREFIX").String()

	db, e = gorm.Open(
		dbType,
		fmt.Sprintf("%s:%s@tcp(%s)/%s?charset=utf8&parseTime=True&loc=Local",
			dbUser,
			dbPasswd,
			dbHost,
			dbName,
		),
	)
	if e != nil {
		log.Fatalf("[ERROR] Database connecting error: %v", e)
	}
	gorm.DefaultTableNameHandler = func(db *gorm.DB, tableName string) string {
		return dbTablePrefix + tableName
	}
	// configure database
	db.SingularTable(true)
	db.LogMode(true)
	db.DB().SetMaxIdleConns(12)
	db.DB().SetMaxOpenConns(100)
}

// TestDatabase Test connection and query
func TestDatabase() ([]*User, error) {
	var users []*User
	e := db.Select("*").Find(&users).Error
	return users, e
}

// CloseDataBase close database
func CloseDataBase() {
	defer db.Close()
}

```

测试一下数据库吧! (亢奋)

在数据库中新建`obm_user`表

```sql
create table obm_user
(
    id           char(15)                           not null comment 'unique id of user',
    username     varchar(20)                        not null comment 'custom username',
    password     char(32)                           not null comment 'user password, encrypted',
    avatar       varchar(128)                       null comment 'url of avatar',
    deletedTime  datetime                           null comment 'user delete time',
    createdTime  datetime default CURRENT_TIMESTAMP not null,
    salt         varchar(32)                        null,
    modifiedTime datetime                           null,
    constraint obm_user_id_uindex
        unique (id)
);

alter table obm_user
    add primary key (id);
```

插入一些数据

```sql
INSERT INTO obm.obm_user (id, username, password, avatar, deletedTime, createdTime, salt, modifiedTime)
VALUES (
  '123456789012345',
  'jecosine',
  '098f6bcd4621d373cade4e832627b4f6',
  'test',
  null,
  '2021-01-02 20:21:16',
  null,
  null
);
```

如果你使用的是和本文一样的`app.ini`, 你需要在MySQL创建一个名为`obm`的用户, 并给予其访问`obm`数据库的权限.

参考代码如下:

```sql
create user obm;
grant all on obm.* to obm@'%';
```

在`main.go`中编写如下代码 (在此处提醒需要在`go.mod`中添加对应的包, 之后默认读者已经做了这一步)

```go
package main

import (
	"fmt"

	"github.com/Jecosine/obm-back-end/models"
	"github.com/Jecosine/obm-back-end/pkg/setting"
)

func main() {
	setting.Setup()
	models.Init()
	users, e := models.TestDatabase()
	if e != nil {
		fmt.Printf("error: %v", e)
		return
	}
	for _, user := range users {
		fmt.Println(user)
	}
}
```

如果已经正确配置好数据库和项目的话, 应该会有如下输出

```
2021/01/03 07:11:29 
RUN_MODE: debug
NAME: My Online Book Manager
HTTP_PORT: 8888

(E:/Programs/obm/obm-back-end/models/models.go:76) 
[2021-01-03 07:11:29]  [0.97ms]  SELECT * FROM `obm_user`
[1 rows affected or returned ]
&{123456789012345 jecosine 098f6bcd4621d373cade4e832627b4f6  2021-01-02T20:21:16+08:00}
```

到此, 项目的基本配置与数据库的连接就完成了, 目前为止的项目结构如图所示

```
obm-back-end
├─.vscode
├─conf
├─middleware
├─models
├─pkg
│  ├─e
│  └─setting
├─routes
└─runtime
```

接下来进行的是api服务设计, 请看 下一章 [API服务三层结构设计]()

