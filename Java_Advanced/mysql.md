# MySQL
##数据库对象
---
表

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












