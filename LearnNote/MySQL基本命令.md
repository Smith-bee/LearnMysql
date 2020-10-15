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

