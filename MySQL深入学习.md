## MySQL深入学习

### 一、distinct（去重）

语法：

```sql
SELECT DISTINCT 列名称 FROM 表名称
```

例：

```sql
SELECT distinct a.pwd from user_table a
```

### 二、order by（排序）

例 1
以字母顺序显示公司名称：

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company
```

例2

以逆字母顺序显示公司名称，并以数字顺序显示顺序号：

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC
```

### 三、limit（返回查询指定数目）

例

```sql
SELECT *
FROM user_table
LIMIT 5
```

### 四、like（模糊查询）

```sql
SELECT *
FROM user_table
where username like '%l'
```

### 五、in(条件匹配多个内容)

```sql
select * from user_table where username in ("lijun","wang")
```

### 六、between and（二个数值的范围）

```sql
select * from user_table where id bewteen 1 and 2
```

### 七、join（多表查询）

#### 1、join on内连接

SELECT * FROM 表名 inner join 表名 on 关联语句

inner可省略

```sql
SELECT * FROM user_ids a INNER JOIN user_table b on a.user_id = b.user_id
SELECT * FROM user_ids a JOIN user_table b ON a.user_id = b.user_id
```

#### 2、left JOIN左连接

SELECT * FROM 表名 LEFT JOIN名  ON 关联语句

```sql
SELECT * FROM user_ids a LEFT JOIN user_table b ON a.user_id = b.user_id
```

#### 3、right join

```sql
SELECT * FROM user_ids a RIGHT JOIN user_table b ON a.user_id = b.user_id
```

### 八、into  in（拷贝表）

下面的例子会制作 "Persons" 表的备份复件：

```sql
SELECT *
INTO Persons_backup
FROM Persons
```

IN 子句可用于向另一个数据库中拷贝表：

```sql
SELECT *
INTO Persons IN 'Backup.mdb'
FROM Persons
```

如果我们希望拷贝某些域，可以在 SELECT 语句后列出这些域：

```sql
SELECT LastName,FirstName
INTO Persons_backup
FROM Persons
```

### 九、sql约束

可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）。

我们将主要探讨以下几种约束：

- NOT NULL
- UNIQUE
- PRIMARY KEY
- FOREIGN KEY
- CHECK
- DEFAULT

### 十、索引

**注释：**更新一个包含索引的表需要比更新一个没有索引的表更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

#### SQL CREATE INDEX 语法

在表上创建一个简单的索引。允许使用重复的值：

```sql
CREATE INDEX 索引名
ON table_name (column_name)
```

**注释：**"column_name" 规定需要索引的列。

#### SQL CREATE UNIQUE INDEX 语法

在表上创建一个唯一的索引。唯一的索引意味着两个行不能拥有相同的索引值。

```sql
CREATE UNIQUE INDEX 索引名
ON table_name (column_name)
```

#### CREATE INDEX 实例

本例会创建一个简单的索引，名为 "PersonIndex"，在 Person 表的 LastName 列：

```sql
CREATE INDEX PersonIndex
ON Person (LastName) 
```

如果您希望以*降序*索引某个列中的值，您可以在列名称之后添加保留字 *DESC*：

```sql
CREATE INDEX PersonIndex
ON Person (LastName DESC) 
```

假如您希望索引不止一个列，您可以在括号中列出这些列的名称，用逗号隔开：

```sql
CREATE INDEX PersonIndex
ON Person (LastName, FirstName)
```

### 十一、DROP （删除，撤销）

####  DROP INDEX 命令删除表格中的索引

```sql
DROP INDEX index_name ON table_name
```

#### SQL DROP TABLE 语句

```sql
DROP TABLE 表名称
```

#### DROP DATABASE 语句用于删除数据库：

```sql
DROP DATABASE 数据库名称
```

#### TRUNCATE TABLE 命令（仅仅删除表格中的数据）：

```sql
TRUNCATE TABLE 表名称
```

### 十二、ALTER TABLE （修改）

#### 表中添加列，请使用下列语法:

语法：

ALTER TABLE 表名 ADD 字段名  字段类型

```sql
ALTER TABLE table_name
ADD column_name datatype
```

#### 改变 "Persons" 表中 "Birthday" 列的数据类型。

```sql
ALTER TABLE Persons
ALTER COLUMN Birthday year
```

#### 删除 "Person" 表中的 "Birthday" 列：

```sql
ALTER TABLE Person
DROP COLUMN Birthday
```

### 十三、日期函数

#### MySQL Date 函数

下面的表格列出了 MySQL 中最重要的内建日期函数：

| 函数                                                         | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [NOW()](https://www.w3school.com.cn/sql/func_now.asp)        | 返回当前的日期和时间                |
| [CURDATE()](https://www.w3school.com.cn/sql/func_curdate.asp) | 返回当前的日期                      |
| [CURTIME()](https://www.w3school.com.cn/sql/func_curtime.asp) | 返回当前的时间                      |
| [DATE()](https://www.w3school.com.cn/sql/func_date.asp)      | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.w3school.com.cn/sql/func_extract.asp) | 返回日期/时间按的单独部分           |
| [DATE_ADD()](https://www.w3school.com.cn/sql/func_date_add.asp) | 给日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.w3school.com.cn/sql/func_date_sub.asp) | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.w3school.com.cn/sql/func_datediff_mysql.asp) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.w3school.com.cn/sql/func_date_format.asp) | 用不同的格式显示日期/时间           |

### 十四、内置函数

| [AVG(column)](https://www.w3school.com.cn/sql/sql_func_avg.asp) | 返回某列的平均值                 |
| ------------------------------------------------------------ | -------------------------------- |
| [COUNT(column)](https://www.w3school.com.cn/sql/sql_func_count.asp) | 返回某列的行数（不包括 NULL 值） |
| [COUNT(*)](https://www.w3school.com.cn/sql/sql_func_count_ast.asp) | 返回被选行数                     |
| FIRST(column)                                                | 返回在指定的域中第一个记录的值   |
| LAST(column)                                                 | 返回在指定的域中最后一个记录的值 |
| [MAX(column)](https://www.w3school.com.cn/sql/sql_func_max.asp) | 返回某列的最高值                 |
| [MIN(column)](https://www.w3school.com.cn/sql/sql_func_min.asp) | 返回某列的最低值                 |
| [SUM(column)](https://www.w3school.com.cn/sql/sql_func_sum.asp) | 返回某列的总和                   |

### 十五、group by（对查询结果分组）

例

```
某个员工信息表结构和数据如下：
  id  name  dept  salary  edlevel  hiredate 
      1 张三 开发部 2000 3 2009-10-11
      2 李四 开发部 2500 3 2009-10-01
      3 王五 设计部 2600 5 2010-10-02
      4 王六 设计部 2300 4 2010-10-03
      5 马七 设计部 2100 4 2010-10-06
      6 赵八 销售部 3000 5 2010-10-05
      7 钱九 销售部 3100 7 2010-10-07
      8 孙十 销售部 3500 7 2010-10-06 
例如，我想列出每个部门最高薪水的结果，sql语句如下：
SELECT DEPT, MAX(SALARY) AS MAXIMUM
FROM STAFF
GROUP BY DEPT
查询结果如下：
      DEPT  MAXIMUM 
      开发部 2500
      设计部 2600
      销售部 3500
```

### 十六、FORMAT() 函数

FORMAT 函数用于对字段的显示进行格式化。

### SQL FORMAT() 语法

```
SELECT FORMAT(column_name,format) FROM table_name
```

| 参数        | 描述                   |
| :---------- | :--------------------- |
| column_name | 必需。要格式化的字段。 |
| format      | 必需。规定格式。       |

例：

我们希望显示每天日期所对应的名称和价格（日期的显示格式是 "YYYY-MM-DD"）

```sql
SELECT ProductName, UnitPrice, FORMAT(Now(),'YYYY-MM-DD') as PerDate
FROM Products
```

### 十七、索引进阶

#### 1、什么是索引

索引其实是一种数据结构，其功能是帮助我们快速匹配查找到需要的数据行，是数据库性能优化最常用的工具之一。其作用相当于超市里的导购员、书本里的目录。

#### 2、索引类型

column（列名）

可以使用SHOW INDEX FROM 表名;查看索引详情：

#### 3、主键索引 PRIMARY KEY：

它是一种特殊的唯一索引，不允许有空值。一般是在建表的时候同时创建主键索引。注意：一个表只能有一个主键。

#### 4、唯一索引 UNIQUE：

唯一索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。可以通过ALTER TABLE table_name ADD UNIQUE (column);创建唯一索引：

#### 5、创建唯一组合索引

可以通过ALTER TABLE table_name ADD UNIQUE (column1,column2)

#### 6、普通索引 INDEX：

这是最基本的索引，它没有任何限制。可以通过ALTER TABLE table_name ADD INDEX index_name (column);创建普通索引

#### 7、组合索引 INDEX：

即一个索引包含多个列，多用于避免回表查询。可以通过ALTER TABLE table_name ADD INDEX index_name(column1,column2, column3);

#### 8、全文索引 FULLTEXT：

也称全文检索，是目前搜索引擎使用的一种关键技术。可以通过ALTER TABLE table_name ADD FULLTEXT (column);

#### 9、索引设计的原则

1）适合索引的列是出现在where子句中的列，或者连接子句中指定的列；

2）基数较小的类，索引效果较差，没有必要在此列建立索引；

3）使用短索引，如果对长字符串列进行索引，应该指定一个前缀长度，这样能够节省大量索引空间；

4）不要过度索引。索引需要额外的磁盘空间，并降低写操作的性能。在修改表内容的时候，索引会进行更新甚至重构，索引列越多，这个时间就会越长。所以只保持需要的索引有利于查询即可。

#### 10、索引优化规则：

1）如果MySQL估计使用索引比全表扫描还慢，则不会使用索引。

​	前导模糊查询不能命中索引：

​	EXPLAIN SELECT * FROM user WHERE name LIKE '%s%';

2）前导模糊查询不能命中索引。

​	非前导模糊查询则可以使用索引，可优化为使用非前导模糊查询：

​	EXPLAIN SELECT * FROM user WHERE name LIKE 's%';

3）数据类型出现隐式转换的时候不会命中索引，特别是当列类型是字符串，一定要将字符常量值用引号引起来。

4）复合索引的情况下，查询条件不包含索引列最左边部分（不满足最左原则），不会命中符合索引。

​	注意，最左原则并不是说是查询条件的顺序：而是查询条件中是否包含索引最左列字段：

5）union、in、or都能够命中索引，建议使用in。

6）用or分割开的条件，如果or前的条件中列有索引，而后面的列中没有索引，那么涉及到的索引都不会被用到。

7）负向条件查询不能使用索引，可以优化为in查询。

​	负向条件有：!=、<>、not in、not exists、not like等。

8）范围条件查询可以命中索引。范围条件有：<、<=、>、>=、between等。

9）数据库执行计算不会命中索引。

10）利用覆盖索引进行查询，避免回表。

​	被查询的列，数据能从索引中取得，而不用通过行定位符row-locator再到row上获取，即“被查询列要被所建的索引覆盖”，这能够加速查询速度。

11）建立索引的列，不允许为null。

​	单列索引不存null值，复合索引不存全为null的值，如果列允许为null，可能会得到“不符合预期”的结果集，所以，请使用not null约束以及默认值。

#### 11、总结

a. 更新十分频繁的字段上不宜建立索引：因为更新操作会变更B+树，重建索引。这个过程是十分消耗数据库性能的。

b. 区分度不大的字段上不宜建立索引：类似于性别这种区分度不大的字段，建立索引的意义不大。因为不能有效过滤数据，性能和全表扫描相当。另外返回数据的比例在30%以外的情况下，优化器不会选择使用索引。

c. 业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引。虽然唯一索引会影响insert速度，但是对于查询的速度提升是非常明显的。另外，即使在应用层做了非常完善的校验控制，只要没有唯一索引，在并发的情况下，依然有脏数据产生。

d. 多表关联时，要保证关联字段上一定有索引。

e. 创建索引时避免以下错误观念：索引越多越好，认为一个查询就需要建一个索引；宁缺勿滥，认为索引会消耗空间、严重拖慢更新和新增速度；抵制唯一索引，认为业务的唯一性一律需要在应用层通过“先查后插”方式解决；过早优化，在不了解系统的情况下就开始优化。