---
title: 数据库表数据相关操作
tags:
    - 学习记录
categories:
    - 表数据
description:
    - 增
    - 删
    - 改
    - 查
    - 联表查询
    - 分组查询
    - 排序查询
    - 表中值筛选
---

# 表数据

## 产品数据

### 添加
```
INSERT INTO product VALUE
  (	'1',	'E-ZIP',	'ZIP',		'EVERYDAY',		'GREEN',	'Y',	59.95	),
  (	'2',	'E-FLX',	'FLEX',		'EVERYDAY',		'BLACK',	'N',	99.95	),
  (	'3',	'A-BLZ',	'BLAZE',	'ACTIVE',		'PURPLE',	'Y',	199.95	),
  (	'4',	'P-SUG',	'SURGE',	'PERFORMANCE',	'BLACK',	'Y',	249.95	);
```

### 查看
```
SELECT * FROM product;
```
            
### 清空表数据
```
TRUNCATE TABLE shipping;
```
			
### 更新
```
UPDATE product SET msrp = 99.99 WHERE product_id = '2';
```

### 添加
```
INSERT INTO product VALUES (	'5',	'P-SUG',	'SURGEF',	'PERFORMANCE',	'BLACK',	'Y',	249.95	);
```

### 删除
```
DELETE FROM product WHERE product_id IN('5');
```
	
## 客户数据

### 添加
```
INSERT INTO CLIENT VALUE
  (	'1',	'FITBIT',	'ONLINE'	),
  (	'2',	'AMAZON',	'ONLINE'	),
  (	'3',	'BESTBUY',	'OFFLINE'	),
  (	'4',	'WALMART',	'OFFLINE'	);
```

### 查看
```
SELECT * FROM CLIENT;
```
### 添加
```
INSERT INTO CLIENT VALUES
  (	'5',	'ADDRESS',	'ONLINE'	),
  (	'6',	'NAMECON',	'OFFLINE'	);
```
            
## 订单数据

### 添加
```
INSERT INTO sales(sales_id,product_id,client_id,DATE,price,quantity ) VALUE
  (	1,	'1',	'1',	'2016-06-01',	40,		10	),
  (	2,	'1',	'2',	'2016-06-05',	30,		5	),
  (	3,	'2',	'3',	'2016-06-08',	80,		8	),
  (	4,	'3',	'2',	'2016-06-13',	160,		10	),
  (	5,	'4',	'3',	'2016-06-18',	40,		23	),
  (	6,	'2',	'4',	'2016-06-08',	70,		7	),
  (	7,	'3',	'1',	'2016-06-13',	150,		5	),
  (	8,	'4',	'4',	'2016-06-20',	40,		9	);
```

### 查看
```
SELECT * FROM sales ORDER BY DATE;
```
            
## 送货数据

### 添加
```
INSERT INTO shipping(shipping_id,sales_id,tracking_no,STATUS,arrive_date,eta ) VALUE
  (	1,	3,		103,	'ARRIVED',		'2016-06-02',	'2016-06-02'	),
  (	2,	4,		104,	'ARRIVED',		'2016-06-30',	'2016-06-25'	),
  (	3,	5,		105,	'SHIPPED',		NULL,			'2016-03-04'	),
  (	4,	6,		200,	'PREPARING',	NULL,			NULL			);
```

### 查看
```
SELECT * FROM shipping;
```

# 查询表数据

### 分页查询
```
SELECT id,title FROM sales limit (pageNum-1)*pageSize, pageSize
```

### 去重
```
SELECT DISTINCT status FROM shipping;
```

### 日期区间查找
```
SELECT * FROM sales WHERE date BETWEEN '2016-06-01' AND '2016-06-08';
```

### 在选择的值中
```
SELECT * FROM product WHERE msrp in (59.95,199.95,249.95);
```

### 模糊搜索
```
SELECT * FROM product WHERE code like '%Z%';
```

### 不包含查找
```
SELECT * FROM product WHERE name not like '%Z%';
```

### 指定长度查找
```
SELECT * FROM product WHERE length(name) = 4;
```

### 将产品加个打9折
```
SELECT *, msrp*0.9 as adjusted_msrp FROM product;
```
        
# 查询表数据 - join

### sales 表中中的 FLEX 和 BLAZE
```
SELECT * FROM product p, sales s 
  WHERE p.product_id = s.product_id AND lower(p.name) in ("flex","blaze");
```

### 订单中发货的 AnB
```
SELECT * 
  FROM sales s 
  INNER JOIN shipping sh
  WHERE s.sales_id = sh.sales_id;
```

### 订单的发货状态 Au(AnB)
```
SELECT *
  FROM sales s 
  LEFT JOIN shipping sh
  ON s.sales_id = sh.sales_id;
```

### 发货的订单 (AnB)uA
```
SELECT *
  FROM shipping sh 
  RIGHT JOIN sales s
  ON s.sales_id = sh.sales_id;
```

# 同时查询 - mysql

### 去重
```
SELECT * FROM sales WHERE sales_id>1
UNION
SELECT * FROM sales WHERE sales_id>5;
```

### 全部
```
SELECT * FROM sales WHERE sales_id>1 
UNION 
All SELECT * FROM sales WHERE sales_id>5;
```
        
# 筛选值

### 产品表中最小值 - 查询的当前数据，不带其他参数
```
SELECT min(msrp) as min_msrp FROM product;
```

### 产品表中最大值 - 查询的当前数据，不带其他参数
```
SELECT max(msrp) as max_msrp FROM product;
```

### 产品表中平均值
```
SELECT avg(msrp) as avg_msrp FROM product;
```

### 产品表中价格总数
```
SELECT sum(msrp) as sum_msrp FROM product;
```

### 订单表中销售额
```
SELECT sum(price*quantity) as sum_msrp FROM sales;
```
		
### 产品表中多少个产品
```
SELECT count(product_id) as num_product FROM product;
```
        
# 排序查询 - ORDER BY (order by)

### 按时间顺序查询 - DESC - 倒序
```
SELECT * FROM sales ORDER BY date;
```

### 价格从大到小，取前3
```
SELECT * FROM product ORDER BY msrp DESC LIMIT 3;
```
        
# 分组汇总 - GROUP BY (group by)

### 查询类别的总量
```
SELECT status,sum(tracking_no) as sum FROM shipping GROUP BY status;
```

### 订单分产品汇总
```
SELECT product_id,sum(price*quantity) as sales_amount
  FROM sales
  GROUP BY product_id
  ORDER BY sales_amount DESC;
```

### 交易的产品有哪些
```
SELECT p.product_id, p.name, count(1) as sales_count
  FROM sales s, product p
  WHERE s.product_id = p.product_id
  GROUP BY p.product_id;
```

### 产品被客户购买的数量与总价,按产品排序
```
SELECT p.name as product_name, c.name as client_name, s.price ,s.quantity, sum(s.price*s.quantity) as amount
  FROM product p, client c, sales s
  WHERE p.product_id = s.product_id AND c.client_id = s.client_id
  GROUP BY s.sales_id
  ORDER BY p.name;
```

### 发生过交易的产品有几种
```
SELECT count(DISTINCT product_id) as dist_product FROM sales s;
```

### 客户买过多少个产品
```
SELECT c.name, count(distinct s.product_id) as dist_product
  FROM client c, sales s
  WHERE c.client_id = s.client_id
  GROUP BY c.client_id;
```

# 对分类汇总的限制 - HAVING (having)

### 客户买过多少个产品
```
SELECT c.name, count(distinct s.product_id) as dist_product
  FROM client c, sales s
  WHERE c.client_id = s.client_id
  GROUP BY c.client_id
  HAVING dist_product > 1;
```

### 哪些产品卖20个以下
```
SELECT p.name, sum(s.quantity) as total_quantity
  FROM product p, sales s
  WHERE p.product_id = s.product_id
  GROUP BY s.product_id
  HAVING total_quantity < 20;
```
