---
title: "MySQL 基础语句汇总"
date: 2018-08-29T23:17:29+08:00
---

MySQL 并不区分大小写，但是写 SQL
语句时建议将关键字大写，与表名列名等区分开来

### 基础检索

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


### 排序
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


### 过滤条件
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

AND 操作符
```
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 AND prod_price <= 10;
```

OR 操作符
```
SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 OR prod_price <= 10;
```

IN 操作符
```
SELECT prod_name, prod_price FROM products WHERE vend_id IN (1002, 1003) ORDER BY prod_name;
```

NOT 操作符
```
SELECT prod_name, prod_price FROM products WHERE vend_id NOT IN (1002, 1003) ORDER BY prod_name;
```

### 通配符过滤

LIKE 操作符

百分号（%）：
```
SELECT prod_id, prod_name FROM products WHERE prod_name LIKE 'jet%';
```

下划线（_）：
```
SELECT prod_id, prod_name FROM products WHERE prod_name LIKE '_ ton anvil';
```


### 正则表达式

```
SELECT prod_name FROM products WHERE prod_name REGEXP '.000' ORDER BY prod_name;
```

`|` 表示或（OR）

`.` 表示任意字符

![MySQL 字符类](/images/mysql_regexp_c.png)

![MySQL 重复元字符](/images/mysql_regexp_c2.png)

![MySQL 字符类](/images/mysql_regexp_c3.png)


### 计算字段

应用程序需要的数据不是数据库内的格式，有时候则需要计算字段来完成，比如人的全名由姓名和名字字段组成。

拼接字段
```
SELECT Concate(vend_name, ' (', vend_country, ')') FROM vendors ORDER BY vend_name;
```

别名（AS）
```
SELECT Concat(RTrim(vend_name), ' (', RTrim(vend_country), ')') AS vend_title FROM products ORDER BY vend_name;
```


### 汇总数据

![汇总函数](/images/mysql_aggregate_func.png)

```
SELECT AVG(prod_price) AS avg_price FROM products WHERE vend_id = 1003;
SELECT AVG(DISTINCT prod_price) AS avg_price FROM products WHERE vend_id = 1003;

```


### 分组数据
```
SELECT vend_id, COUNT(*) AS num_prods FROM products GROUP BY vend_id;
```

`GROUP BY` 要在 `WHERE` 之后，`ORDER BY` 之前。

过滤分组使用 `HAVING`，其用法与 `WHERE` 完全一直，只是它作用在分组上。
```
SELECT cust_id, COUNT(*) AS orders FROM order GROUP BY cust_id HAVING COUNT(*) >= 2;
```

使用 `GROUP BY` 的时候都要用 `ORDER BY` 获得正确的排序，而不依赖 `GROUP
BY` 的排序。


ps: 文章内的语句都来自 [MySQL 必知必会](https://book.douban.com/subject/3354490/)

pss: 此文将持续更新
