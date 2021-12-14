sql必知必会阅读笔记, 主要以mysql为主
# 1. sql
<!-- TOC -->

- [1. sql](#1-sql)
    - [1.1. 检索数据](#11-检索数据)
        - [1.1.1. 检索不同的值](#111-检索不同的值)
        - [1.1.2. 限制结果](#112-限制结果)
        - [1.1.3. 排序](#113-排序)
        - [1.1.4. 条件](#114-条件)
    - [1.2. 高级数据过滤查询](#12-高级数据过滤查询)
            - [1.2.0.1. AND OR](#1201-and-or)
            - [1.2.0.2. IN操作符](#1202-in操作符)
            - [1.2.0.3. NOT操作符](#1203-not操作符)
        - [1.2.1. 用通配符进行过滤](#121-用通配符进行过滤)
    - [1.3. 创建计算字段](#13-创建计算字段)
        - [1.3.1. 连接字符](#131-连接字符)
        - [1.3.2. 执行算数计算](#132-执行算数计算)
    - [1.4. 使用函数处理数据](#14-使用函数处理数据)
    - [1.5. 汇总数据](#15-汇总数据)
        - [1.5.1. 汇总函数](#151-汇总函数)
        - [1.5.2. 聚集不同的值](#152-聚集不同的值)
    - [1.6. 分组数据](#16-分组数据)
        - [1.6.1. 创建分组](#161-创建分组)
        - [1.6.2. 过滤分组](#162-过滤分组)
        - [1.6.3. 分组与排序](#163-分组与排序)
        - [1.6.4. SELECT 子句顺序](#164-select-子句顺序)
    - [1.7. 子查询](#17-子查询)
        - [1.7.1. 利用子查询过滤](#171-利用子查询过滤)
        - [1.7.2. 作为计算字段使用子查询](#172-作为计算字段使用子查询)
    - [1.8. 联结表](#18-联结表)
        - [1.8.1. 创建联结](#181-创建联结)
        - [1.8.2. 内联结](#182-内联结)
        - [1.8.3. 联结多个表](#183-联结多个表)
    - [1.9. 高级联结](#19-高级联结)
        - [1.9.1. 表别名](#191-表别名)
        - [1.9.2. 自联结](#192-自联结)
        - [1.9.3. 外联结](#193-外联结)
        - [1.9.4. 带聚集函数的联结](#194-带聚集函数的联结)
    - [1.10. 组合查询](#110-组合查询)
        - [1.10.1. 创建组合查询](#1101-创建组合查询)
    - [1.11. 插入数据](#111-插入数据)
        - [1.11.1. 数据插入](#1111-数据插入)
        - [1.11.2. 插入检索出的数据](#1112-插入检索出的数据)
        - [1.11.3. 从一个表复制到另一个表](#1113-从一个表复制到另一个表)
    - [1.12. 更新和删除数据](#112-更新和删除数据)
        - [1.12.1. 更新数据](#1121-更新数据)
        - [1.12.2. 删除数据](#1122-删除数据)
    - [1.13. 创建和操作表](#113-创建和操作表)
        - [1.13.1. 创建表](#1131-创建表)
        - [1.13.2. 更新表](#1132-更新表)
        - [1.13.3. 删除表](#1133-删除表)
    - [1.14. 视图](#114-视图)

<!-- /TOC -->
## 1.1. 检索数据
### 1.1.1. 检索不同的值
**DISTINCT**关键字，数据库只返回不同的值

    SELECT DISTINCT vend_id
    FROM Products;

### 1.1.2. 限制结果
mysql使用**LIMIT**

    SELECT foo FROM bar LIMIT 5 OFFSET 10;
    返回第5行起的10行数据。
    
### 1.1.3. 排序
使用**ORDER BY**进行排序，**DESC**倒序

    SELECT prod_id, prod_price, prod_name 
    FROM Products 
    ORDER BY prod_price, prod_name;

    SELECT prod_id, prod_price, prod_name 
    FROM Products 
    ORDER BY 2, 3;

    SELECT prod_id, prod_price, prod_name 
    FROM Products 
    ORDER BY prod_price DESC;

### 1.1.4. 条件
检查某值的范围使用**BETWEEN**

    SELECT prod_name, prod_price
    FROM Products
    WHERE prod_price BETWEEN 5 AND 10;

空值检查**NUll**

    SELECT prod_name 
    FROM Products
    WHERE prod_price IS NULL;

## 1.2. 高级数据过滤查询
#### 1.2.0.1. AND OR
优先处理**AND**操作符，使用()改变优先级

    SELECT prod_name, prod_price
    FROM Products
    WHERE (vend_id = 'DELL01' OR vend_id = 'BRS01')
	AND prod_price >= 10;

#### 1.2.0.2. IN操作符
**IN**操作符用来指定条件范围，范围中的每个条件可以进行匹配。IN取一组由(foo, bar)组成

    SELECT prod_name, prod_price
    FROM Products
    WHERE vend_id IN ('DELL01' OR 'BRS01')
    ORDER BY prod_name;

#### 1.2.0.3. NOT操作符
**NOT**否定其后所跟的任何条件

    SELECT prod_name
    FROM Products
    WHERE NOT vend_id = 'DLL01'
    ORDER BY prod_name;

### 1.2.1. 用通配符进行过滤
%匹配任意字符，通配符**不会**匹配NULL

    SELECT  prod_id, prod_name
    FROM Products
    WHERE prod_name LIKE 'Fish%';

_下划线匹配单个字符

## 1.3. 创建计算字段
### 1.3.1. 连接字符
**CONCAT**连接字符, **AS**创建别名

    SELECT CONCAT(vend_name, '(', vend_country, ')') 
        AS new_field
    FROM Vendors
    ORDER BY vend_name;

### 1.3.2. 执行算数计算

    SELECT prod_id,
	   quantity,
	   item_price,
       quantity*item_price AS expanded_price
    FROM OrderItems
    WHERE order_num = 20008;

## 1.4. 使用函数处理数据
只有少数几个函数被所有主要的DBMS等同地支持，常用：
+ PRRIM() 去除列值右边的空格
+ UPPER() 将文本转换为大写
+ ABS() 绝对值
+ EXP() 指数值

## 1.5. 汇总数据
### 1.5.1. 汇总函数
针对某列的聚合函数
+ AVG() 平均值
+ COUNT() 行数
+ MAX() 最大值
+ MIN() 最小值
+ SUM() 和

一些例子：

    SELECT AVG(prod_price) AS avg_price
    FROM Products;

    SELECT COUNT(*) AS num_cust
    FROM Customers;
    -- COUNT()函数会忽略指定列的值为空的行，但如果使用*则不忽略

    SELECT SUM(quantity)
    FROM OrderItems
    WHERE order_num = 20005;

### 1.5.2. 聚集不同的值
使用**DISTINCT**只聚集不同的值

    SELECT AVG(DISTINCT prod_price) AS avg_price
    FROM Products
    WHERE vend_id = 'DLL01';

## 1.6. 分组数据
### 1.6.1. 创建分组
使用**GROUP BY**创建分组

    -- GROUP BY 语句指示按vend_id排序
    SELECT vend_id, COUNT(*) AS num_prods
    FROM products
    GROUP BY vend_id;

### 1.6.2. 过滤分组
使用**HAVING**过滤分组，WHERE过滤行，HAVING过滤行

    SELECT cust_id, COUNT(*) AS orders
    FROM Orders
    GROUP BY cust_id
    HAVING COUNT(*) >=2;

    -- WHERE和HAVING同时使用
    SELECT vend_id, COUNT(*) AS num_prods
    FROM Products
    WHERE prod_price >=4
    GROUP BY vend_id
    HAVING COUNT(*) >=2;

### 1.6.3. 分组与排序
一般在使用 GROUP BY 子句时，应该也给出 ORDER BY 子句。这是保证数据正确排序的唯一方法。千万不要仅依赖 GROUP BY 排序数据。

|ORDER BY                    |                GROUP BY                   |
|-----------------------------|-------------------------------------------|
|对产生的输出排序             |          对行分组，但输出可能不是分组的顺序|
|任意列都可以使用             |          只能使用选择列或表达式列         |
|不一定需要                   |           如果与聚集函数一起使用，则必须使用|

    SELECT order_num, COUNT(*) AS items
    FROM OrderItems
    GROUP BY order_num
    HAVING COUNT(*) >= 3
    ORDER BY order_num DESC;

### 1.6.4. SELECT 子句顺序
SELECT语句必须遵循以下次序:
+ SELECT
+ FROM
+ WHERE
+ GROUP BY
+ HAVING
+ ORDER BY

## 1.7. 子查询
### 1.7.1. 利用子查询过滤
作为子查询的SELECT只能是查询单个列

    SELECT cust_name
    FROM Customers
    WHERE cust_id IN (SELECT cust_id 
                    FROM orders
                    WHERE order_num IN (SELECT order_num
                                        FROM OrderItems
                                        WHERE prod_id LIKE 'ANV%')
                    );

### 1.7.2. 作为计算字段使用子查询

    -- 如果在SELECT语句中操作多个表，应使用完全限定列名
    SELECT cust_name,
       cust_state,
	   (SELECT COUNT(*) 
       FROM orders 
       WHERE orders.cust_id = customers.cust_id) AS orders
    FROM customers
    ORDER BY cust_name;

## 1.8. 联结表
### 1.8.1. 创建联结
创建联结非常简单，指定要联结的所有表以及关联它们的方式即可。

    SELECT vend_name, prod_name, prod_price
    FROM vendors, products
    WHERE vendors.vend_id = products.vend_id;

### 1.8.2. 内联结
基于两个表之间的相等测试，可以使用不同的语法。两个表的关系是以**INNER JOIN**指定的部分FROM语句。联结条件使用**ON**

    SELECT vend_name, prod_name, prod_price
    FROM vendors INNER JOIN products
    ON vendors.vend_id = products.vend_id;

### 1.8.3. 联结多个表

    SELECT prod_name, vend_name, prod_price, quantity
    FROM orderitems, products, vendors
    WHERE products.vend_id = vendors.vend_id
    AND orderitems.prod_id = products.prod_id
    AND order_num = 20007;

## 1.9. 高级联结
### 1.9.1. 表别名

    SELECT cust_name, cust_contact
    FROM customers AS c, orders AS o, orderitems AS oi
    WHERE c.cust_id = o.cust_id
    AND oi.order_num = o.order_num
    AND prod_id = 'RGAN01';

### 1.9.2. 自联结
self-join SELECT不止一次联结相同的表

    SELECT cust_id, cust_name, cust_contact
    FROM customers
    WHERE cust_name=(SELECT cust_name 
                    FROM  customers 
                    WHERE cust_contact = 'Jim Jones');

    -- 使用联结相同查询：
    SELECT c1.cust_id, c1.cust_name, c1.cust_contact
    FROM customers AS c1, customers AS c2
    WHERE c1.cust_name = c2.cust_name
    AND C2.cust_contact = 'Jim Jones';

### 1.9.3. 外联结
使用**OUTER JOIN**创建外联结，外联结包括没有关联的行，**LEFT**包含左边表所有的行，**RIGHT**包含右边表所有的行

    SELECT customers.cust_id, orders.order_num
    FROM customers LEFT OUTER JOIN orders
    ON customers.cust_id = orders.cust_id;

### 1.9.4. 带聚集函数的联结
聚集函数可以与联结一起使用

    SELECT customers.cust_id,
           COUNT(orders.order_num) AS num_ord
    FROM customers INNER JOIN orders
    ON customers.cust_id = orders.cust_id
    GROUP BY customers.cust_id;

## 1.10. 组合查询
SQL允许执行多个查询（多条SELECT语句），并将结果作为一个查询集返回。这些组合查询通常称为并（union）或符合查询（compound query）。
主要有两种情况使用组合查询：
+ 在一个查询中从不同的表返回结构数据
+ 对一个表执行多个查询，安一个查询返回数据
### 1.10.1. 创建组合查询
使用**UNION**组合数条SQL查询，必须包含两条或两条以上SELECT；每个查询包含相同的列、列的数据类型相同。

    SELECT cust_name, cust_contact, cust_email
    FROM customers
    WHERE cust_state IN ('TL', 'IN', 'MI')
    UNION
    SELECT cust_name, cust_contact, cust_email
    FROM customers
    WHERE cust_name = 'E Fudd';

## 1.11. 插入数据
### 1.11.1. 数据插入
使用**INSERT**进行数据插入

    -- 插入完整的行
    INSERT INTO customers
    VALUES('100000006',
        'zhangsan',
        '123 any street',
        'new york',
        'ny',
        '111',
        'usa',
        NULL,
        NULL);

    INSERT INTO customers(cust_id,
                        cust_name,
                        cust_address,
                        cust_city,
                        cust_state,
                        cust_zip,
                        cust_country,
                        cust_contact,
                        cust_email)
    VALUES('100000007',
        'lisi',
        '123 any street',
        'new york',
        'ny',
        '111',
        'usa',
        NULL,
        NULL);

### 1.11.2. 插入检索出的数据
使用**INSERT SELECT**插入检索出的数据

    INSERT INTO customers(cust_id,
                        cust_name,
                        cust_address,
                        cust_city,
                        cust_state,
                        cust_zip,
                        cust_country,
                        cust_contact,
                        cust_email)
    SELECT cust_id,
        cust_name,
        cust_address,
        cust_city,
        cust_state,
        cust_zip,
        cust_country,
        cust_contact,
        cust_email
    FROM custnew;

### 1.11.3. 从一个表复制到另一个表

    CREATE TABLE custcopy AS
    SELECT * FROM customers;

## 1.12. 更新和删除数据
### 1.12.1. 更新数据

    UPDATE customers
    SET cust_email='a@mail.com'
    WHERE cust_id='10005';

### 1.12.2. 删除数据

    DELETE FROM customers
    WHERE cust_id = '10001';

    -- 清空表 不记录
    TRUNCATE tablename;

## 1.13. 创建和操作表
### 1.13.1. 创建表
允许NULL的列允许插入时不给定值，**default**默认值

    CREATE TABLE newtable
    (id CHAR(10) NOT NULL,
    price DECIMAL(8,2) NULL,
    P_desc VARCHAR(1000) NOT NULL DEFAULT 'hello'
    );

### 1.13.2. 更新表
**ALTER**对表进行更新

    ALTER TABLE newtable
    ADD p_field2 CHAR(20) NOT NULL;

复杂的表结构修改一般需要手动删除过程：
+ 用新的列布局创建一个新表
+ 使用INSERT SELECT语句复制数据
+ 检验新表
+ 重命名旧表、新表
+ 重新创建触发器、存储过程、索引和外键

### 1.13.3. 删除表

    DROP TABLE tablename;

## 1.14. 视图
使用**CREATE VIEW**创建视图，隐藏复杂的SQL

    CREATE VIEW myview AS 
    SELECT cust_name, cust_contact, prod_id 
    FROM Customers, Orders, OrderItems 
    WHERE Customers.cust_id = Orders.cust_id 
    AND OrderItems.order_num = Orders.order_num;
