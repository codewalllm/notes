---
title: mysql数据库表相关操作
tags:
    - 学习记录
categories:
    - 表
description:
    - 增
    - 删
    - 改
    - 查
    - 外键
---
# 创建表 
```
# 产品
CREATE TABLE IF NOT EXISTS product(

-- 表内容
  product_id	VARCHAR(20)		NOT NULL,
  CODE		CHAR(5)			NOT NULL,
  NAME		VARCHAR(20)		NOT NULL,
  class		VARCHAR(30)		NOT NULL,
  color		VARCHAR(10)		NOT NULL,
  has_clock	ENUM('Y','N')	NOT NULL,
  msrp		DECIMAL(8,2)	NULL,

-- 主键约束
  PRIMARY KEY (product_id),

-- 唯一性的约束
  UNIQUE KEY (NAME)

);
```

# 删除表
```
-- DROP TABLE IF EXISTS product;
```

# 查看创建的表
```
DESCRIBE product;
```

# 给表中添加一列
```
ALTER TABLE product ADD COLUMN description VARCHAR(200) NOT NULL AFTER NAME;
```

# 修改列的名字
```
ALTER TABLE product CHANGE COLUMN description `desc` VARCHAR(200) NOT NULL;
```

# 删除某一列
```
ALTER TABLE product DROP `desc`;
```

# 创建表
```
/*
    客户
*/
CREATE TABLE IF NOT EXISTS CLIENT(

-- 表内容
  client_id	VARCHAR(20)		NOT NULL,
  NAME		VARCHAR(20)		NOT NULL,
  TYPE		VARCHAR(30)		NOT NULL,

-- 主键约束
  PRIMARY KEY (client_id),

-- 唯一性的约束
  UNIQUE KEY (NAME)

);
```

# 查看创建的表
```
DESCRIBE CLIENT;
```
    

# 创建表
```
/*
    订单
        AUTO_INCREMENT	-	表内容自动生成(INT)
*/
CREATE TABLE IF NOT EXISTS sales(

-- 表内容
  sales_id	INT				NOT NULL	AUTO_INCREMENT,
  product_id	VARCHAR(20)		NOT NULL,
  client_id	VARCHAR(20)		NOT NULL,
  DATE		DATE			NOT NULL,
  price		DECIMAL(8,2)	NOT NULL,
  quantity	INT				NULL DEFAULT 1,

-- 主键约束
  PRIMARY KEY (sales_id),

-- 外键约束
  CONSTRAINT fk_product_id
    -- 外键的名字
    FOREIGN KEY (product_id)
    -- 从哪里来的外键
    REFERENCES product(product_id)
    -- 级联更新
    ON UPDATE CASCADE,
-- 外键约束
  CONSTRAINT fk_client_id
    -- 外键的名字
    FOREIGN KEY (client_id)
    -- 从哪里来的外键
    REFERENCES CLIENT(client_id)
    -- 级联更新
    ON UPDATE CASCADE,
-- 约束 - 表价格
  CONSTRAINT money_price CHECK(price>=0)

);
```

# 添加一个外键约束
```
ALTER TABLE sales ADD
-- 外键约束
CONSTRAINT fk_client_id_ADD
  -- 外键的名字
  FOREIGN KEY (client_id)
  -- 从哪里来的外键
  REFERENCES CLIENT(client_id)
  -- 级联更新
  ON UPDATE CASCADE;
```

# 删除外键约束
```
ALTER TABLE sales DROP FOREIGN KEY fk_client_id_ADD;
```

# 查看创建的表
```
DESCRIBE sales;
```
    
# 创建表
```
/*
    送货
*/
CREATE TABLE IF NOT EXISTS shipping(

-- 表内容
  shipping_id	INT				NOT NULL	AUTO_INCREMENT,
  sales_id	INT				NOT NULL,
  tracking_no INT,
  STATUS		ENUM('preparing','shipped','arrived'),
  arrive_date DATE,
  eta			DATE,

-- 主键约束
  PRIMARY KEY (shipping_id),

-- 外键约束
  CONSTRAINT fk_sales_id
    -- 外键的名字
    FOREIGN KEY (sales_id)
    -- 从哪里来的外键
    REFERENCES sales(sales_id)
    -- 级联更新
    ON UPDATE CASCADE

);
```

# 查看创建的表
```
DESCRIBE shipping;
```
