# MySQL
##数据库对象
---
###表

* 增加字段

```sql
ALERT TABLE table ADD (column datatype [DEFAULT expr][, column datatype]...);
#使用ADD子句增加字段，新的字段只能被加到整个表的最后，并且不能与表中原有的字段重名
```

* 修改表字段

```sql
ALERT TABLE table
MODIFY (column datatype [DEAULT expr] [,column datatype...])
#可修改列的数据类型，大小，不是任何情况都可以修改的
#当字段只包含空值的时候，类型、大小都可以修改，否则修改可能不成功
```

* 删除表字段

```sql
ALERT TABLE table
DROP column (columns)
#从每行中删除掉字段占据的长度和数据
#释放在数据块中占用的空间。
#删除大表中的字段将需要比较长的时间
```

* 删除表

```sql
DROP table #删除表，但是并不释放表所占空间
TRUNCATE TABLE tablename
#清除表中所有的记录
#DDL语句，不可以回滚
#释放表的存储空间
```

* 约束
  * 非空约束(NK):NOT NULL,不允许某列的内容为空。
  * 设置列的默认值：DEFAULT。
  * 唯一约束(UK):UNIQUE，在该表中，该列的内容必须唯一。
  * 主键约束:PRIMARY KEY,非空并且唯一。
  * 主键自增长:AUTO_INCREMENT，从1开始，步长为1.
  * 外键约束：FOREIGN KEY


* 索引
  * 一个数据库对象
  * ***用来加速对表的查询***
  * 通过使用快速路径访问方法快速定位数据，减少磁盘I/O
  * 与表独立存放
  * 由数据库自动维护
  * 创建索引`CREATE INDEX index ON table (column[, column]...)`
  * 哪些值可以创建索引(索引的使用问题)
    * 外键一般需要创建索引
    * 经常使用的查询条件要创建索引。如果使用`like`或者`%`操作，不会使用索引
    * 索引不是越多越好(表中数据的修改会影响索引库)
    * 不要再可选值很少的属性上面创建索引
    * MySQL索引的使用，并不是所有情况下都会使用索引，只有当MySQL认为索引足够能提升查询性能的时候，才使用


### 视图

* 概念

> 视图就是虚表，实际上视图就是一个命名的查询，用于改变表的数据显示。

* 作用

> 可以显示对数据的访问

> 可以是复杂的查询变简单

> 提供了数据的独立性

> 提供了对相同数据的不同显示

* 语法

```sql
在CREATE VIEW语句后加入子查询
CREATE [OR REPLACE] VIEW view
  [(alias[, alias]...)]
  AS subquery
  [WITH READ ONLY];a

创建视图
CREATE [OR REPLACE] VIEW emp_v_10
AS SELECT employee_id, last_name, salary
FROM employees
WHERE department_id = 10;
```

* 注意

> 默认情况下，可以直接通过对视图的`DML`操作去修改视图对应表中的内容

> ***前提是视图中没有通过公式导出的列***

> 删除一个视图 `DROP VIEW viewname`

##MySQL查询函数
---

* 函数的分类
  * 单行函数：对每一行输入值进行计算，得到相应的计算结果。
  * 多行函数：对多行输入进行计算，得到多行对应的单个结果（多进单出）。

* CONCAT(str1, str2...)
  * 返回结果为连接参数产生的字符串。
  * 如有任何一个参数为NULL，则返回NULL
  * 允许有一个或多个参数

* 示例代码

```sql
SELECT CONCAT(productName, '商品的零售价为：', salePrice) AS productSalePrice FROM product;
```

* AVG and SUM






















