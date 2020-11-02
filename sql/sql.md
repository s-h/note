sql必知必会阅读笔记
## 检索数据
### 检索不同的值
DISTINCT关键字，数据库只返回不同的值

    SELECT DISTINCT vend_id
    FROM Products;

### 限制结果
mysql 使用 LIMIT

    SELECT foo FROM bar LIMIT 5 OFFSET 10;
    返回第5行起的10行数据。
    
### 排序

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
检查某值的范围使用BETWEEN

    SELECT prod_name, prod_price
    FROM Products
    WHERE prod_price BETWEEN 5 AND 10;

空值检查NUll

    SELECT prod_name 
    FROM Products
    WHERE prod_price IS NULL;

## 高级数据过滤查询
#### AND OR
优先处理AND操作符，使用()改变优先级

    SELECT prod_name, prod_price
    FROM Products
    WHERE (vend_id = 'DELL01' OR vend_id = 'BRS01')
	AND prod_price >= 10;

#### IN操作符
IN操作符用来指定条件范围，范围中的每个条件可以进行匹配。IN取一组由(foo, bar)组成

    SELECT prod_name, prod_price
    FROM Products
    WHERE vend_id IN ('DELL01' OR 'BRS01')
    ORDER BY prod_name;

#### NOT操作符
否定其后所跟的任何条件

    SELECT prod_name
    FROM Products
    WHERE NOT vend_id = 'DLL01'
    ORDER BY prod_name;

### 用通配符进行过滤