---
title: MySQL基本命令(一)
date: 2020.10.15
tags: [MySQL]
categories: [MySQL]
---

## 登录

- 登录MySQL

```bash
mysql -u root -p
```

其中**root**为用户名(视自己的为准)，输入密码即可登录

如果安装时没有初始化密码或密码忘记了，解决👉[MySQL安装及密码设置](https://smith-bee.github.io/2020/09/28/MySQL/%E5%AE%89%E8%A3%85MySQL/)

## 数据库

- 列出所有数据库

```mysql
SHOW DATABASES;
```

- 创建数据库

```mysql
CREATE DATABASE test;
```

其中**test**为数据库名

- 删除数据库

```mysql
DROP DATABASE test;
```

- 使用数据可

```mysql
USE test;
```

## 创建用户与授权

- 创建用户

```mysql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

说明：

**username** : 创建的用户名

**host** : 指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以**从任意远程主机登陆**，可以使用通配符**%**    

**password** : 该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

例子：

```mysql
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '';
CREATE USER 'pig'@'%';
```

- 授权

```mysql
GRANT PRIVILEGES ON databasename.tablename TO 'username'@'host';
```

说明：

**privileges**：用户的操作权限，如**SELECT**，**INSERT**，**UPDATE**等，如果要授予所的权限则使用**ALL**

databasename : 数据库名

tablename : 表名，如果想授权该用户所有数据库和表的相应操作权限可以用**`*`**表示，**`*.*`**

例子：

```mysql
GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
GRANT ALL ON *.* TO 'pig'@'%';
GRANT ALL ON maindataplus.* TO 'pig'@'%';
```

注意：

用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

```mysql
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
```

小提示：还是不要轻易给其他用户这种授权

- 查看所有用户

```mysql 
SELECT user,localhost FROM mysql.user;
```

![image-20201015173205206](https://i.loli.net/2020/10/15/3rcwNs2mK7bod48.png)

- 设置与更改用户密码

```mysql
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

如果是当前登录用户：

```mysql
SET PASSWORD = PASSWORD("newpassword");
```

例子：

```mysql
SET PASSWORD FOR 'pig'@'%' = PASSWORD("123456");
```

![image-20201015233332536](https://i.loli.net/2020/10/15/u6IbZMms8hqvL2C.png)

- 撤销用户权限

```mysql
REVOKE PRIVILEGES ON databasename.tablename FROM 'username'@'host';
```

说明：

**privileges**：用户的操作权限，如**SELECT**，**INSERT**，**UPDATE**等，如果要授予所的权限则使用**ALL**

databasename : 数据库名

tablename : 表名，如果想授权该用户所有数据库和表的相应操作权限可以用**`*`**表示，**`*.*`**

例子：

```mysql
REVOKE SELECT ON *.* FROM 'pig'@'%';
```

注意：

1. 要取消某些权限，可使用比要撤回的权限更大范围的权限。例如，当用户有 `SELECT (x,y)`权限时，管理员可执行 `REVOKE SELECT(x,y) ...`, 或 `REVOKE SELECT * ...`, 甚至是 `REVOKE ALL PRIVILEGES ...`来取消原有权限。

2. 取消部分权限，可以取消部分权限。例如，当用户有 `SELECT *.*` 权限时，可以通过授予对部分库或表的读取权限来撤回原有权限。

例子：

1. 授权 `john`账号能查询所有库的所有表，除了 `account`库。

```mysql
GRANT SELECT ON *.* TO john;
REVOKE SELECT ON accounts.* FROM john;
```

2. 授权 `mira`账号能查询 `accounts.staff`表的所有列，除了 `wage`这一列。

```mysql
GRANT SELECT ON accounts.staff TO mira;
REVOKE SELECT(wage) ON accounts.staff FROM mira;
```

- 查看用户的权限

```mysql
SHOW GRANTS FOR 'username'@'localhost';
```

- 删除用户

```mysql
DROP USER 'username'@'host';
```

