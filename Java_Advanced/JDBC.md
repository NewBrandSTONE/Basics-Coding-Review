# JDBC
##持久化和持久化技术
* 持久化技术

  * 概念

>把数据保存到可掉电式的存储设备中，持久化的实现过程大多是通过`各种关系数据库`完成的
>
>

* Java访问数据库的两种方式
  * 直接使用`JDBC`的`API`去访问数据库(`mysql`/`Oracle`)
  * 间接地使用`JDBC`的`API`去访问数据库服务器，如`Hibernate`、`MyBatis`等


* JDBC的作用

>为访问不同的数据库提供了统一的途径，为开发者屏蔽了一些细节问题。


##获取数据库连接

* 获取连接对象的步骤

>记得导入相应的驱动包：`mysql-connecttor-java-5.1.26-bin.jar`

```java
步骤：
// 1.加载驱动
Class.forName("com.mysql.jdbc.Driver");
// 2.获取连接对象
Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");

```
##`JDBC`中常用的`API`

* JDBC操作步骤
  * 加载注册驱动
  * 获取连接对象
  * 创建语句对象
  * 执行sql
  * 释放资源(***一定要记得关闭资源***)

##`DML`操作
>对数据库的表进行`增加`、`删除`和`修改操作`

##`DQL`操作
>对数据库的表进行查询操作

##`DAO`的设计思想
* 概念

>`DAO(Data Access Object)`是一个数据访问接口，数据访问：与数据库打交道。夹在业务逻辑(Service)与数据库资源中间。

* `DAO`实现的规范

  * `DAO`的命名

```java
  com.ghyz.ppd.dao         -- 定义DAO的接口
  com.ghyz.ppd.dao.impl    -- 定义DAO的接口实现类
  com.ghyz.ppd.test        -- 定义测试类
  com.ghyz.ppd.util        -- 定义实体类
  
  // 接口和实现类的规范
  DAO的接口:IXxxDAO,IXxxDao
  DAO的接口实现类XxxDAOImpl,XxxDaoImpl
```

##示例代码
