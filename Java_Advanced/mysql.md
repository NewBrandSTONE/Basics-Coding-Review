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







