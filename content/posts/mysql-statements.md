---
title: "MySQL 基础语句汇总"
date: 2018-08-29T23:17:29+08:00
---

MySQL 并不区分大小写，但是写 SQL
语句时建议将关键字大写，与表名列名等区分开来

## 基础检索

`SELECT`: 基础检索语句

基本示例：
```
SELECT prod_name FROM products;
SELECT prod_id, prod_name FROM products;
SELECT * FROM products;
```

只检索出值不同的行：
```
SELECT DISTINCT vend_id FROM products;
```

限制返回的行数：
```
SELECT prod_name FROM products LIMIT 5;
SELECT prod_name FROM products LIMIT 3, 4;
SELECT prod_name FROM products LIMIT 4 OFFSET 3;
```
其中 2, 3 条语句是等价的，意思是从第 3 行（从 0 开始算）开始，取 4 行

限定列名和表名：
```
SELECT products.prod_name FROM products;
SELECT products.prod_name FROM crashcourse.products;
```


## 排序
`ORDER BY`

升序（默认）：
```
SELECT prod_name FROM products ORDER BY prod_name;
SELECT prod_name FROM products ORDER BY prod_price, prod_name;
```

降序：
```
SELECT prod_name FROM products ORDER BY prod_price DESC;
```

LIMIT 要在 ORDER BY 之后：
```
SELECT prod_id, prod_price, prod_name FROM products ORDER BY prod_price DESC LIMIT 1;
```



## 过滤条件
```
SELECT prod_name, prod_price FROM products WHERE prod_price = 2.50;
```

`ORDER BY` 要在 `WHERE` 后面：
```
SELECT prod_name, prod_price FROM products WHERE prod_price = 2.50 ORDER BY prod_name DESC;
```
![WHERE 子句操作符](/images/where_ops.png)

IS NULL(空值检查)：
```
SELECT cust_id FROM customers WHERE cust_email IS NULL;
```

ps: 文章内的语句都来自 [MySQL 必知必会](https://book.douban.com/subject/3354490/)

pss: 此文将持续更新
