---
title: "MySQL 非基础概念"
date: 2018-09-05T21:06:07+08:00
draft: true
---

## 视图

视图是虚拟的表，包含了在使用时检索数据的查询。

```
CREATE VIEW productcustomers AS
SELECT cust_name, cust_contact, prod_id,
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num;
```

上面的语句创建了名为 `productcustomers` 的视图，视图的使用基本与实际的表相同，比如：
```
SELECT cust_name, cust_contact
FROM productcustomers
WHERE prod_id = 'TNT2';
```

视图也支持更新，更新存在许多限制，但是视图的主要用途还是查询。

视图提供了语句的封装，让某些查询可以直接使用。



## 存储过程

有些情况下对数据库进行操作，可能不止需要一条 SQL语句，而是多条语句才能完成，并且执行顺序和语句也可能会根据情况而变化，这时候可以建立存储过程来完成此类操作。

存储过程简化了复杂的操作，使用者只需要调用存储过程，便可以完成需要的一系列操作。

注意：如果是在 MySQL 命令行内写存储过程，需要把结束符重置成其他的字符，而不是`;`，可以使用 `DELIMITER //`，之后可以使用 `DELIMITER ;` 再恢复为分号。

执行存储过程（因为执行可能比编写更常用）

示例1：
```sql
CALL productpricing(@pricelow, @pricehigh, @priceaverage);
```

这里执行了名为 `productpricing` 的存储过程，`@` 开头的字符，在 MySQL 内是变量。

创建存储过程

```
CREATE PROCEDURE productpricing()
BEGIN
    SELECT AVG(prod_price) AS priceaverage
    FROM products;
END
```

调用这个存储过程只会输出平均价格。

那么如何使用参数呢？同名的存储过程不能被重复创建，所以先删除

```
DROP PROCEDURE productpricing;
```

再创建具有参数的

```
CREATE PROCEDURE productpricing(
    OUT pl DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
    SELECT MIN(prod_price)
    INTO pl
    FROM products;
    SELECT MAX(prod_price)
    INTO ph
    FROM products;
    SELECT AVG(prod_price)
    INTO pa
    FROM products;
END;
```

`BEGIN` 和 `END` 包裹了存储过程体，

`OUT` 为存储过程返回出来的值，MySQL 还支持 `IN`,`INOUT`，前者为传给存储过程的参数，后者即表示传入也表示传出。

这时候执行存储过程，并传入需要的参数，如示例1所示，相应的变量就会有相应的值，可以通过 `SELECT @pricelow` 等进行查看。存储过程内还可以做一些业务逻辑的处理，这里暂不详述了。


## 游标

暂无


## 触发器

触发器顾名思义，是当某些事件发生的时候执行的。MySQL 的触发器可以响应下列语句：

* DELETE
* INSERT
* UPDATE

### INSERT 触发器

```
CREATE TRIGGER neworder AFTER INSERT ON orders
FOR EACH ROW SELECT New.order_num;
```

这里创建了一个插入新订单到 order 表后的触发器，`NEW` 表示新插入的数据


### DELETE 触发器

```
CREATE TRIGGER deleteorder BEFORE DELETE ON orders
FOR EACH ROW
BEGIN
    INSERT INTO archive_orders(order_num, order_date, cust_id)
    VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
END;
```

当删除订单前，此触发器会把删除的订单存储到 `archive_orders` 表内。


### UPDATE 触发器

UPDATE 触发器内的 `NEW` 是可以被修改的，`OLD` 则是只读的。

保证州名为大写：
```
CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors
FOR EACH ROW SET NEW.vend_state = UPPER(NEW.vend_state);
```


### 事务处理

暂无
