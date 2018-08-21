# Project

## 数据库设计

### 用户表：user

| 字段         | 约束                                       |
| ---------- | ---------------------------------------- |
| id         | int primary key auto_increment           |
| username   | varchar(20) unique                       |
| password   | varchar(50)                              |
| sex        | tinyint(2) unsigned not null default default 0 comment '0,male,1,female' |
| age        | int                                      |
| tel        | varchar(20)                              |
| created_at | int                                      |
| admin      | tinyint(2) unsigned not null default default 0 comment '0,not admin,1,admin' |
| score      | int                                      |

### 技术：technology,游记：travels

| 字段            | 约束                                 |
| ------------- | ---------------------------------- |
| id            | int pk auto_increment              |
| author        | foreign key user.id                |
| tag           | technology/travels                 |
| title         | varchar                            |
| sumamry       | varchar                            |
| content       | ueditor                            |
| state         | tinyint default 0 (待审核，审核通过，审核不通过) |
| created_at    | int                                |
| view_time     | int                                |
| like_times    | int                                |
| collect_times | int                                |
|               |                                    |
|               |                                    |
|               |                                    |

### 收藏：collection

| 字段         | 约束            |
| ---------- | ------------- |
| id         | int pk        |
| use_id     | fk user.id    |
| post_id    | fk article.id |
| created_at | int           |

### 评论：comment

| 字段   | 约束   |
| ---- | ---- |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |

###  回复：reply

|      |      |
| ---- | ---- |
|      |      |
|      |      |
|      |      |



## 接口定义：

### url接口：`http://www.baidu.com/s?key=前锋`

`http://` 协议

`www.baidu.com` 域名

`/s` 路径

`?key=` 参数

### 接口：`/？page=`

描述：首页

html界面：index.html

方法：GET

参数：page 分页

后端函数：index

### 接口：`/user/register/`

描述：注册

html界面：user/register.html

方法：POST

参数：

后端函数：register

### 接口：`/user/login/`

描述：登录

html界面：user/login.html

方法：POST

参数：

后端函数：login

### 接口：`/user/publish/`

描述：发布

html界面：user/publish.html

方法：POST

参数：

后端函数：publish

### 接口：`/user/modify/`

描述：修改

html界面：user/modify.html 

方法：POST

参数：

后端函数：modify

### 接口：`/user/delete/`

描述：删除

html界面：user/delete/

方法：DELETE

参数：

后端函数：delete

### 接口：`/article/`

描述：

html界面：

方法：

参数：

后端函数：

### 接口：

描述：

html界面：

方法：

参数：

后端函数：

## 变量定义：

### session:

| 变量       | 描述   |
| -------- | ---- |
| username | 用户名  |
|          |      |
|          |      |
|          |      |