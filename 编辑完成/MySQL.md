# MySQL

MySQL关系型数据库，支持SQL，支持事务

数据库>>表>>字段>>数据

## mysql数据库引擎：

### 存储引擎主要有：

MyIsam、 Mrg_Myisam、 Memory、Blackhole、CSV、Performance_Schema、Archive、Federated、 InnoDB

##### 修改存储类型：

`alter table <表名> engine=<存储类型>`

### MyISAM:

MyISAM存储引擎独立于操作系统，也就是可以在windows上使用，也可以比较简单的将数据转移到linux操作系统上去。这种存储引擎在创建表的时候，会创建三个文件，一个是**.frm文件用于存储表的定义**，一个是**.MYD文件用于存储表的数据**，另一个是**.MYI文件，存储的是索引**。操作系统对大文件的操作是比较慢的，这样将表分为三个文件，那么.MYD这个文件单独来存放数据自然可以优化数据库的查询等操作。

1. 不支持事务，但是并不代表着有事务操作的项目不能用MyIsam存储引擎，可以在service层进行根据自己的业务需求进行相应的控制。


2. 不支持外键。


3. 查询速度很快。如果数据库insert和update的操作比较多的话采用表锁效率低（最好用innodb）。


4. 对表进行加锁。

### InnoDB:

InnoDB是一个事务型的存储引擎，有行级锁定和外键约束,**MySQL的默认存储引擎**

1. 更新多的表，适合处理多重并发的更新请求。
2. 支持事务。
3. 可以从灾难中恢复（通过bin-log日志等）。
4. 外键约束。只有InnoDB支持外键。
5. 支持自动增加列属性auto_increment。

### innodb事物处理操作

将当前表文件存储类型改为 innodb: `alter table <表名> engine=innodb;`

查看当前表的提交类型：`select @@autocommit;`

改为手动提交(开启事物)：`set autocommit=0`

事务：

```mysql
begin;
<各种SQL语句>
#提交事务
commit work;
--------------------------------------
#回滚
rollback work;
```



### 事务ACID(Atomicity Consistency Isolation Durability)

##### 事务的原子性：

一个事务要么全部执行，要么不执行

##### 事务的一致性：

事务的运行不改变数据库中数据的一致性。例如,完整性约束了a+b=10,一个事务改变了a,那么b也应该随之改变.

##### 事务的独立性：

事务的独立性也有称作隔离性,是指两个以上的事务不会出现交错执行的状态.因为这样可能会导致数据不一致。

##### 事务的持久性：

事务的持久性是指事务执行成功以后,该事务所对数据库所作的更改便是持久的保存在数据库之中，不会无缘无故的回滚。

## MySQL字段类型：

### 数字类型：

| 类型           | 大小     | 范围           | 无符号范围      | 用途         |
| ------------ | ------ | ------------ | ---------- | ---------- |
| tinyint      | 1字节    | -128,127     | 0，255      | 小整数值       |
| smallint     | 2字节    | -32768,32767 | 0，65535    | 大整数值       |
| int          | 4字节    | 2-》10位置      | 4... 10 位的 | 大整数值       |
| float(m,n)   | 4个字节   |              |            | 单精度浮点型     |
| double(m,n)  | 8个字节   |              |            | 双精度浮点型     |
| decimal(m,n) | 根据存储的值 |              |            | 小数据值（更加精准） |

浮点数中的m代表当前存储的长度  n代表小数的位数   m-n代表整数的位数  超出则报错

##### 整形后面的数字的意义：

1. 整形后面给定数字 并不是限定当前存储值的长度  并没有任何的意义 除非配合可选参数 zerofill 零填充 才有意义
2. 字符串类型后面给定的长度 则是限制当前存储数据的长度
3. 整形默认长度会比本身长度大1，因为是符号位

### 字符串类型：

| 类型            | 大小       | 用途                  |
| ------------- | -------- | ------------------- |
| char          | 0-255    | 存储定长字符串             |
| varchar       | 0-255    | 存储不定长度字符串           |
| text          | 0-65535  | 长文本数据               |
| blob          | 0-65535  | 存储二进制的长文本数据         |
| enum('w','m') | 65535个成员 | 枚举：只能选取括号中的某一个值进行存储 |
| set('w','m')  | 64个成员    | 集合：可以选择多个成员         |

##### char和varchar的相同和不同点 ：

1. char和varchar的存储长度都为0-255
2. char的执行效率高于varchar
3. varchar要比 char更节省存储空间
4. 当char存储的值达不到指定的长度时 则使用空来占位

##### enum和set区别

1. enum只能选择其中的一个值
2. set可以选择多个值 多个值使用逗号来隔开

### 日期类型：

| 类型       | 大小   | 范围                                       | 格式                  | 用途       |
| -------- | ---- | ---------------------------------------- | ------------------- | -------- |
| date     | 3    | 1000-01-01 - 9999-12-31                  | YYYY-MM-DD          | 日期       |
| time     | 3    | -838:59:59/838:59:59                     | HH:MM:SS            | 时间值或持续时间 |
| year     | 1    | 1901/2155                                | YYYY                | 年分值      |
| datetime | 8    | 1000-01-01 00:00:00 /9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS | 存储时间和日期  |



## MySQL命令：

### 进入数据库：

-h host 主机名

-u user 用户名

-p password 密码

`mysql -h<host> -u<user> -p`

### 对数据库的操作：

查看所有的数据库：`show databases;`

创建数据库：`create database [if not exists] <数据库名>;` 

查看创建数据库语句：`show create database <数据库名>;`

使用数据库：`use <数据库名>`

删除数据库：`drop database [if exists] ;`

创建数据库并设置字符集：`create database <数据库名> character set utf8;`

修改数据库字符集：`alter database <> character set utf8`

### 对数据表的操作

查看所有的表：`show tables`

创建表：

```mysql
create table if not exists <表名>(
  id int unsigned primary key auto_increment,
  username varchar(20),
  sex tinyint,
  info varchar(100)
);
```

查看表结构：`desc <表名>`

删除表中的某个字段：`alter table fs drop <字段名>`

删除表：`drop table [if exists] <表名>`

##### 字段约束

1. unsigned 无符号  正数

   只能用于数值类型 不允许出现负数  存储长度会扩大一倍

2. zerofill    零填充

   只能用于数值类型 当指定的位数不足的时候 零填充

3. default   默认值

   如果当前字段没有传值  则值为默认值  （不设定默认值  默认为null）

4. null 和 not null

   默认为null  当不插入值则插入的为null，当设置当前字段为 not null的时候  则该字段必须传值

5. comment   设置当前字段的说明

6. auto_increment  自增 

##### 注意

1. SQL 语句以分号作为结束

2. SQL命令 不区分大小写

3. 数据库的切换使用use

4. \c 撤销当前命令

5. \G 竖着查看

6. 当遇到在终端中 不管怎样输入命令都不执行 name查看一下左侧 是否为->

7. 在Windows下 不区分库,表名的大小写  但是在Ubuntu下区分

8. 退出数据库的几种方式

   \q exit  quit

### 索引

主键索引 primary key 

唯一索引 unique

常规索引 index

全文索引 fulltext

#### 主键索引

##### 注意

1. 每个表中 只能有一个主键索引
2. 每个表中最好有一个主键索引 并不是必须的
3. 主键索引可以有很多的候选项 （auto_increment,not null）
4. 当表中的数据 被删除以后 auto_increment 依然记录着下一个数据插入的行号值  
   - truncate 表名 清空表  并将自增归位
   - alter 表名 auto_increment=1

##### 实例

```mysql
create table myindex(
	id int primary key auto_increment not null
);
```

#### 唯一索引

唯一索引和主键索引 都一样 不能插入重复的值    不同的是 主键一个表只能存在一个 唯一索引可以存在多个

```mysql
create table myunique(
	username varchar(20) unique not null
);
```

#### 常规索引：

常规索引 只作为提高数据的查询效率来使用的 

缺点：

1. 提高了查询效率 但是降低了增删改的效率
2. 索引文件 占用磁盘空间

#### 全文索引：

`alter table <表名> add fulltext <索引名称>(<索引字段>)`

```mysql
mysql> create table test(    
    -> id int unsigned primary key auto_increment not null,
  	-> username varchar(50) not null,    
  	-> userpass varchar(50) not null,    
  	-> telno varchar(20) not null,    
  	-> sex enum('w','m') not null default 'm',    
  	-> birthday date not null default '0000-00-00',    
 	-> index(username),    
 	-> index(userpass),    
 	-> unique(telno)    
  	-> );
```

### 更改表结构

给表添加新字段 add：`alter table <表名> add sex enum('w','m') default 'm';`

添加新字段在字段的后面  after：`alter table <表名> add info varchar(100) default '个人说明' after id;`

添加新字段在第一位   first：`alter table user add infoop varchar(100) default '个人说明' first;`

更改字段名称并更改字段顺序   change:`alter table user change infoop money decimal(6,2);`

​						` alter table user change money money decimal(6,2) after username;`

更改字段类型  modify: `alter table user modify username char(11);`

添加字段索引:

```mysql
alter table user add index(username); #添加常规索引
alter table user add unique(username);
alter table user add index infoindex(info); 添加索引并添加索引名称
```

删除索引：`alter table user drop key username;`

创建一个表b和表a一样：`create table b like a;`

查看所有的索引：`show index from 表名;`

更改字段的字符集：`alter table user modify username varchar(20) character set utf8;`

## SQL语法增删改查：

### INSERT 数据的添加

1. ##### 指定字段名称添加数据

   insert into user(username,age,info) values('张三',18,'个人说明');

   需要注意：如果有字段不为空且没有默认值 那么必须插入值

2. ##### 不指定字段添加值（需要将所有字段都插入值）

   insert into user values(null,'李四',20,'个人说明李四')

3. ##### 指定字段添加多个值

   insert into user(username,age,info) values('王五',30,'王五的说明'),('赵六',33,'赵六的个人说明')...;

4. ##### 不指定字段添加多个值

   insert into user values('王五',30,'王五的说明'),('赵六',33,'赵六的个人说明')...;

### SELECT 数据的查询

1. 不指定字段查询（查询所有字段）

   select * from 表名;

2. 指定字段查询

   select 字段名1,字段名2...  from 表名;

3. 查询字段并运算

   select id+age from user;

4. 起别名

   select username as name from 表名;

   select username name from 表名;

### where 条件

#### (1) 比较运算符

1. \>
2. <
3. \>=
4. <=
5. !=/<>
6. =

#### (2) 逻辑运算符

1. ##### 逻辑与 and

   select * from user where username='苍老师' and age<=30;

   **俩侧为真才为真**

2. ##### 逻辑或 or

   select * from user where username='苍老师' or age>=30;

   **只要满足一个就为真**

3. ##### and 和 or 一起使用

   select * from user where id=1 or (age=18 and sex='w');

4. ##### 在...内 in

   select * from user where id in(1,3,5,6,7); #和下面的写法一个意思

   select * from user where id=1 or id=2 or id=6; 

5. ##### 不在...内 not  in

   select * from user where id not in(1,3,5,6,7); #和下面的写法一个意思

   select * from user where id!=1 and id!=2 and id!=6; 

6. ##### 在...范围内 between ... and ...

   select * from user where age between 20 and 33;和下面的写法一个意思

   select * from user where age>=20 and age<=33;

7. ##### 不在...范围内 not between ... and ...

   select * from user where age not between 20 and 33;和下面的写法一个意思

    select * from user where age not between 20 and 33;

#### (3) order by  排序

1. 默认升序  asc

    select * from user order by age;

    select * from user order by age asc;

2. 降序  desc

   select * from user order by age desc;

#### (4) is  is not

1. is not

    select * from user where username is not null;

2. is

    select * from user where username is null;

#### (5) limit

limit num   直接取出num条数据

select * from user order by age desc limit 1;  #取出年龄最大的一条数据

limit x,num  从x的位置取出 num条数据

**注意：**

如果是从头取出num条数据  limit num  ==  limit 0,num

##### 分页计算：

求分页取值的偏移量

（nowPage-1）*everyPage

#### (6) 模糊查询  like

1. 包含查询

   like ‘%字符%’

2. 以某个字符开头的查询

   like '字符%'

3. 以某个字符结尾的查询

   like  '%字符'

### 聚合函数

1. 最大值

   max()   

   select max(age) from user;ss

2. 最小值

   min()

   select min(age) from user;

3. 统计

   count()

   select count(*) from user

4. 平均值

   avg()

   select avg(age) from user;

5. 求和

   sum()

   select sum(age) from user;

### group by 分组

**查询男女分别多少人**

 select sex,count(*) from user group by sex;

**统计每个年龄段分别多少人**

select age,count(*) from user group by age;

**统计每个年龄段的男女分别有多少人**

 select sex,age,count(*) from user group by sex,age;

**groub by  having   其中的having相当于where**

**查询每个年龄段人数大于1人的数据**

 select age,count(*) as count from user group by age having count>1;

**查询每个年龄段人数大于1人的数据 并且年龄小于40**

select age,count(*) as count from user group by age having count>1 and age<40;

**查询每个年龄段人数大于1人的数据 并且年龄为10,20,30**

select age,count(*) as count from user group by age having age in(18,20,30);

### delete 删除

**主体结构**

delete from 表名 [where...]

**注意：**

where 应该加 如果没有where条件删除所有数据

**实例**

delete from user where username='xxx';

### update 修改

**主体结构**

update 表名 set 字段名=字段值,[字段名=字段值...[where.....]] 

**注意：**

不要忘记where条件  否则修改全部

**实例**

update user set sex='m' where age in(18,20);

### 多表联查

**关联条件**

外键

#### (1) 隐式内连接

select * from user,goods where `user`.id=goods.uid
select user.username,user.age,goods.goodsname from user,goods where `user`.id=goods.uid

**起别名**

select u.username,g.goodsname from user u,goods g where u.id=g.uid

#### (2) 显示内连接 inner join  on

select * from user INNER JOIN goods ON `user`.id=goods.uid and goods.uid=1

select u.username,g.goodsname from user u INNER JOIN goods g ON u.id=g.uid and g.uid=1

**注意：**

隐式内连接和显示内连接其实是同一个查询  会将**关联**的数据全部查询出来

#### (3) 左链接  left join  on

select u.username,g.goodsname from user u LEFT JOIN goods g ON u.id=g.uid

**注意：**

左链接会将左表作为主表 右表为辅表 会将主表所有数据查询出来 辅表没有关联的数据使用null来占位

## 用户管理

## 权限管理

## 远程连接

test数据库的所有表对任意IP地址的user用户开放所有权限，登陆密码是1234：

`grant all privileges on test.* to user@'%' identified by '1234';`

删除用户权限：`revoke all on [database.table] from [user];`

删除用户及权限：`drop user 用户名@权限;`

刷新权限：`flush privileges;`

改表法：

```mysql
mysql>update user set host = '%' where user = 'root';
mysql>select host, user from user;
```



## 存储过程

## 数据迁移

## SQL优化

## 数据恢复