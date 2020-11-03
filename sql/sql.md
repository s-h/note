sql必知必会阅读笔记, 主要以mysql为主
## 检索数据
### 检索不同的值
**DISTINCT**关键字，数据库只返回不同的值

    SELECT DISTINCT vend_id
    FROM Products;

### 限制结果
mysql使用**LIMIT**

    SELECT foo FROM bar LIMIT 5 OFFSET 10;
    返回第5行起的10行数据。
    
### 排序
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

### 条件
检查某值的范围使用**BETWEEN**

    SELECT prod_name, prod_price
    FROM Products
    WHERE prod_price BETWEEN 5 AND 10;

空值检查**NUll**

    SELECT prod_name 
    FROM Products
    WHERE prod_price IS NULL;

## 高级数据过滤查询
#### AND OR
优先处理**AND**操作符，使用()改变优先级

    SELECT prod_name, prod_price
    FROM Products
    WHERE (vend_id = 'DELL01' OR vend_id = 'BRS01')
	AND prod_price >= 10;

#### IN操作符
**IN**操作符用来指定条件范围，范围中的每个条件可以进行匹配。IN取一组由(foo, bar)组成

    SELECT prod_name, prod_price
    FROM Products
    WHERE vend_id IN ('DELL01' OR 'BRS01')
    ORDER BY prod_name;

#### NOT操作符
**NOT**否定其后所跟的任何条件

    SELECT prod_name
    FROM Products
    WHERE NOT vend_id = 'DLL01'
    ORDER BY prod_name;

### 用通配符进行过滤
**%**匹配任意字符，通配符**不会**匹配NULL

    SELECT  prod_id, prod_name
    FROM Products
    WHERE prod_name LIKE 'Fish%';

**_**下划线匹配单个字符

## 创建计算字段
### 连接字符
**CONCAT**连接字符, **AS**创建别名

    SELECT CONCAT(vend_name, '(', vend_country, ')') 
        AS new_field
    FROM Vendors
    ORDER BY vend_name;

### 执行算数计算

    SELECT prod_id,
	   quantity,
	   item_price,
       quantity*item_price AS expanded_price
    FROM OrderItems
    WHERE order_num = 20008;

## 使用函数处理数据
只有少数几个函数被所有主要的DBMS等同地支持，常用：
PRRIM() 去除列值右边的空格
UPPER() 将文本转换为大写
ABS() 绝对值
EXP() 指数值

## 汇总数据
### 汇总函数
针对某列的聚合函数
+ AVG() 平均值
+ COUNT() 行数
+ MAX() 最大值
+ MIN() 最小值
+ SUM() 和

    SELECT AVG(prod_price) AS avg_price
    FROM Products;

    SELECT COUNT(*) AS num_cust
    FROM Customers;
    #COUNT()函数会忽略指定列的值为空的行，但如果使用*则不忽略

    SELECT SUM(quantity)
    FROM OrderItems
    WHERE order_num = 20005;

### 聚集不同的值
使用**DISTINCT**只聚集不同的值

    SELECT AVG(DISTINCT prod_price) AS avg_price
    FROM Products
    WHERE vend_id = 'DLL01';

## 分组数据
### 创建分组
使用**GROUP BY**创建分组

    # GROUP BY 语句指示按vend_id排序
    SELECT vend_id, COUNT(*) AS num_prods
    FROM products
    GROUP BY vend_id;

### 过滤分组
使用**HAVING**过滤分组，WHERE过滤行，HAVING过滤行

    SELECT cust_id, COUNT(*) AS orders
    FROM Orders
    GROUP BY cust_id
    HAVING COUNT(*) >=2;

    #WHERE和HAVING同时使用
    SELECT vend_id, COUNT(*) AS num_prods
    FROM Products
    WHERE prod_price >=4
    GROUP BY vend_id
    HAVING COUNT(*) >=2;

### 分组与排序
一般在使用 GROUP BY 子句时，应该也给出 ORDER BY 子句。这是保证数据正确排序的唯一方法。千万不要仅依赖 GROUP BY 排序数据。
ORDER BY                    |                GROUP BY
对产生的输出排序              |          对行分组，但输出可能不是分组的顺序
任意列都可以使用              |          只能使用选择列或表达式列
不一定需要                   |           如果与聚集函数一起使用，则必须使用

    SELECT order_num, COUNT(*) AS items
    FROM OrderItems
    GROUP BY order_num
    HAVING COUNT(*) >= 3
    ORDER BY order_num DESC;

### SELECT 子句顺序
SELECT语句必须遵循以下次序:
+ SELECT
+ FROM
+ WHERE
+ GROUP BY
+ HAVING
+ ORDER BY

## 子查询
### 利用子查询过滤
作为子查询的SELECT只能是查询单个列

    SELECT cust_name
    FROM Customers
    WHERE cust_id IN (SELECT cust_id 
                    FROM orders
                    WHERE order_num IN (SELECT order_num
                                        FROM OrderItems
                                        WHERE prod_id LIKE 'ANV%')
                    );

### 作为计算字段使用子查询

    -- 如果在SELECT语句中操作多个表，应使用完全限定列名
    SELECT cust_name,
       cust_state,
	   (SELECT COUNT(*) 
       FROM orders 
       WHERE orders.cust_id = customers.cust_id) AS orders
    FROM customers
    ORDER BY cust_name;

## 联结表
### 创建联结
创建联结非常简单，指定要联结的所有表以及关联它们的方式即可。

    SELECT vend_name, prod_name, prod_price
    FROM vendors, products
    WHERE vendors.vend_id = products.vend_id;

### 内联结
基于两个表之间的相等测试，可以使用不同的语法。两个表的关系是以**INNER JOIN**指定的部分FROM语句。联结条件使用**ON**

    SELECT vend_name, prod_name, prod_price
    FROM vendors INNER JOIN products
    ON vendors.vend_id = products.vend_id;

### 联结多个表

    SELECT prod_name, vend_name, prod_price, quantity
    FROM orderitems, products, vendors
    WHERE products.vend_id = vendors.vend_id
    AND orderitems.prod_id = products.prod_id
    AND order_num = 20007;

## 高级联结
### 表别名

    SELECT cust_name, cust_contact
    FROM customers AS c, orders AS o, orderitems AS oi
    WHERE c.cust_id = o.cust_id
    AND oi.order_num = o.order_num
    AND prod_id = 'RGAN01';

### 自联结
self-join SELECT不止一次联结相同的表