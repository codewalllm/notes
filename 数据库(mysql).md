---
title: mysql数据库相关操作
tags:
    - 学习记录
categories:
    - 数据库
description:
    - 增
    - 删
    - 改
    - 查
    - 创建用户
    - 用户密码
    - 权限
---

# 创建数据库
```
CREATE DATABASE fitbit_new;
```

# 删除数据库
```
DROP DATABASE IF EXISTS fitbit_new;
```

# 使用数据库
```
USE fitbit_new;
```
    
# 查找所有的表
```
SHOW TABLES;
```

# mysql 数据库下
	SELECT * FROM USER;

# 创建用户
	CREATE USER 'zhaofengyan'@'%' IDENTIFIED BY '123';
	CREATE USER 'liuming'@'%' IDENTIFIED BY '123';
# 修改密码
```
# 2021-03-06 可用
SET PASSWORD FOR 'liuming'@'%' = PASSWORD('456');
# 2021-07-01 可用
alter user 'root'@'localhost' identified by '123456';
```

# 查看权限
```
SHOW GRANTS FOR 'zhaofengyan'@'%';
```

# 设置权限
```
# 单独设置
GRANT SELECT ON zhaofengyan.* TO 'zhaofengyan'@'%';
# 放开所有权限
GRANT ALL ON zhaofengyan.* TO 'zhaofengyan'@'%';
```

# 撤销权限
```
# 撤销所有权限
REVOKE ALL ON *.* FROM 'zhaofengyan'@'%';
```
