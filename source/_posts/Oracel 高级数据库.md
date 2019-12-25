---
title: Oracel 高级数据库
categories:
- Oracel
tags: 
- Oracel
---

![](/images/oracel.jpg)

小鹿带你学习 Oracel 高级数据库（企业级）。

<!-- more -->

#  Oracel 高级数据库

## 入门命令

### 01|基本命令

 #### 使用超级管理员登录 :

```
$ sys/密码  as sysdba  
```

#### 用户切换到超级管理员

```
$ conn sys/密码  as sysdba  
```

#### 新建用户并设置密码：

```
$ create user 用户名 identified by 口令[即密码]；
```

#### 删除用户：

```
$ drop user 用户名;
```

#### 锁定用户：

```
$ alter user 用户名 account lock;
```

#### 解锁用户：

```
$  alter user 用户名 account unlock;
```

#### 授予用户登录的权限：

```
grant create session to 用户名；
```

#### 授予用户操作数据库的权限：

```
grant resource to 用户名；
```

#### 登录被创建的用户：

```
connect 用户名/密码
```



### 02|查询操作

#### 查询表中的年龄并分组显示：

```
select Tname,to_char(sysdate,'yyyy')- to_char(生日日期字段，'yyyy')as age
from teacher;
```

#### 查询某一字段不为空的信息：

```
select *
from 表名
where 字段名 is null;
```

#### 查询日期在一个范围之内：

```
select *
from 表名
where to_char(生日日期字段，'yyyy') between '年月日' and '年月日'
```

#### 授予用户创建表空间的权限

```
grant create tablespace to  user10;
```

#### 授予用户修改表空间的权限、

```
grant unlimited tablespace to user10; 
```

#### 修改表空间的名字

```
alter tablespace tbs10 rename to tbs20;
```

#### 将表空间设置为脱机装状态

```
alter tablespaace tbs20 offline;
```

#### 先将表表空间设置为联机再设置为只读状态

```
alter tablespaace tbs20 online;
alter tablespace tbs20 read only;
```

#### 删除表空间,并删除数据文件

```
drop tablespace tbs20 including contents;
```


## 表的创建和管理

### 基本步骤（整个操作在超级管理员的环境下运行的）

> 所有用到的表名之前要加上用户名。

- 登录超级管理员系统
- 创建用户
- 授权用户
- 创建一个表空间
- 创建表并添加到已创建的表空间中

### 基本命令使用

#### 创建表空间

```
SQL> create tablespace 表空间名
  2  datafile 'C:\app\Administrator\admin\orcl\tbs02.dbf' size 10m;
```

#### 创建表并添加到已创建的表空间中

```
SQL> create table 用户名.表名(
  2  BT_ID char(2) primary key,
  3  BT_Name varchar2(20),
  4  BT_Info VARCHAR2(50)
  5  ) tablespace 表空间名;
```

#### 将一个表从一个表空间添加到另一个表空间中

```
SQL> alter table 用户名.表名 move tablespace 表空间名;
```

#### 创建表并设置外键

```
create table 用户名.表名(
b_id char(10) primary key,
b_name varchar2(40),
author varchar2(20),
bt_id char(2),
p_id char(4),
pubdate date,
price number(5,2),
constraint 外键新名字 foreign key(外键) references 用户名.参照的另一个表名(主键)
)tablespace 表空间名;
```

#### 主键约束

```
Alter table 设置主键的表
add constraint(约束) 主键字段别名 primary key(主键字段)
```

#### 外键约束

```
Alter table 设置外键表名
add constraint(约束) 外键别名 foreign key(外键) references 用户名.参照的另一个表名(主键)
on delete cascade);//级联删除
```

#### 非空约束

```
Alter table 表名
modify 字段名 not null;
```

#### 删除表中的字段

```
Alter table 表名
drop column 字段名；
```

#### 添加表中的字段

```
Alter table 表名 
add 字段名 数据类型;
```













