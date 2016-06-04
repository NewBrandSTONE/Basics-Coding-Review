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
static Connection getConnection(String url, String username, String password);

```