---
title: 在线书籍管理平台项目要求
categories:
- project
- go
tags:
- online-book-management
---

## 项目需求

- 好看
- 好看！
- 好看！！
- 基于RBAC的权限管理
- 在线阅读支持的文档格式
- 对大文件进行分片读取

## 技术栈

- **前端**
  - Vue3
  - Element UI
- **后台**
  - Go
  - Gin

## 实体

| Entity      | Description                                               |
| ----------- | --------------------------------------------------------- |
| `user`      | 用户实体                                                  |
| `role`      | 权限角色                                                  |
| `privilege` | 权限实体                                                  |
| `file`      | 书本实体，~~为什么不叫`book`?说不定之后可以做个文件存储~~ |
| `category`  | 分类,  树状结构, 有父级关系                               |
| `tag`       | 标签, 用于区分书的系列 ( 大概 )                           |



## 路由设计

初版路由都由`/v1/`作为共同前缀, 关于错误码的定义请参考[此处](), 实现请参考 [此处]()

### 用户相关

#### **添加用户**

**URI** 

`POST`  `/v1/user/add`

**说明**

添加用户, 需要权限?

**参数体**

| Name       | Required | Description                    |
| ---------- | -------- | ------------------------------ |
| `id`       | `true`   | 用户id                         |
| `username` | `true`   | 用户自设用户名(不超过20个字符) |
| `password` | `true`   | 加密后的密码, 32位             |
| `email`    | `true`   | 联系邮箱, 之后的验证           |

**响应体**

| Name     | Description |
| -------- | ----------- |
| `status` | 响应状态码  |
| `msg`    | 响应消息    |
| `data`   | 响应消息体  |

**请求样例**

```json
{
	"id": 123456,
	"username": "username",
	"password": "098f6bcd4621d373cade4e832627b4f6",
	"email": "test@jecosine.com",
	"avatar": "https://your.avatar.url/avatar.webp"
}
```

**响应实例**

```json
{
	"status": 200,
	"msg": "OK",
	"data": null
}
```



#### 修改用户

**URI** 

`POST`  `/v1/user/save`

**说明**

修改用户信息, 普通用户只能修改自己的信息

**参数体**

| Name       | Required | Description                    |
| ---------- | -------- | ------------------------------ |
| `id`       | `true`   | 用户id                         |
| `username` | `false`  | 用户自设用户名(不超过20个字符) |
| `password` | `false`  | 加密后的密码, 32位             |
| `email`    | `false`  | 联系邮箱, 之后的验证           |

**响应体**

| Name     | Description |
| -------- | ----------- |
| `status` | 响应状态码  |
| `msg`    | 响应消息    |
| `data`   | 响应消息体  |



#### 删除用户

**URI** 

`POST`  `/v1/user/delete`

**说明**

修改用户信息, 普通用户只能删除自己的账户

**参数体**

| Name | Required | Description |
| ---- | -------- | ----------- |
| `id` | `true`   | 用户id      |

**响应体**

| Name     | Description |
| -------- | ----------- |
| `status` | 响应状态码  |
| `msg`    | 响应消息    |
| `data`   | 响应消息体  |



#### 查询用户

**URI** 

`GET`  `/v1/user/get/{userid}`

**说明**

修改用户信息, 普通用户只能修改自己的信息

**URL参数**

| Name     | Required | Description |
| -------- | -------- | ----------- |
| `userid` | `true`   | 用户id      |

**响应体**

| Name     | Description              |
| -------- | ------------------------ |
| `status` | 响应状态码               |
| `msg`    | 响应消息                 |
| `data`   | 响应消息体, 类型是`user` |




- `/v1/user/get/{userid}`

`user`

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

`book`

| Field Name | Type    | Description      |
| ---------- | ------- | ---------------- |
| id         | char    | book id          |
| name       | varchar | book name        |
| author     | varchar | book author name |
| uploader   | varchar | uploader name    |
| type       | char    | book type id     |
| category   | char    | category id      |


`collection`

