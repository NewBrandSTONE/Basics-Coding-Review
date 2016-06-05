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

>注意，记得关闭`Connection`,`Statement`,`ReusultSet`等类似的资源，下面的代码都省去了关闭资源的步骤

* IStudentDAO

```java
public interface IStudentDAO {
  /**
  * 保存学生信息
  * @param stu 需要保存的学生对象
  */
  void save(Student stu);
  
  /**
  * 根据id删除对应的学生
  * @param id 需要删除学生的di
  */
  void delete(Long id);
  
  /**
  * 修改学生信息
  * @param stu 封装需要修改的学生信息
  */
  void update(Student stu);
  
  /**
  * 根据id获取对应学生的信息
  * @param Long id
  */
  Student get(Long id);
  
  /**
  * 查询所有的学生信息
  * @return
  */
  List<Student> list();
}
```

 * StudentDAOImpl(DML)

```java
public void save(Student stu) {
  Connection conn = null;
  Statement stmt = null;
  try {
    // 加载驱动
    Class.forName("com.mysql.jdbc.Driver");
    // 获取连接对象
    conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
    // 创建语句对象
    stmt = conn.createStatement();
    // 执行SQL语句
    // 使用StringBuilder进行拼接的速度比较快
    StringBuilder sb = new StringBuilder("INSERT INTO t_student VALUES (NULL,)");
    sb.append("'").append(stu.getName()).append(",");
    sb.append(stu.getAge());
    sb.append(")");
    System.out.prinln(sb);
    st.executeUpdate(sb.toString());
  } catch (Exception e) {
    e.printStackTrace();
  }
}

public void delete(Long id) {
  Connection conn = null;
  Statement stmt = null;
  try {
    // 加载驱动
    Class.forName("com.mysql.jdbc.Driver");
    // 获取连接对象
    conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
    // 执行SQL语句
    String sql = "DELETE FROM t_student WHERE id = " + id;
    // 创建语句对象
    stmt = conn.createStatement();
    stmt.executeUpdate(sql);
  } catch (Exception e) {
    System.out.printStackTrace();
  }
}

public void update(Student stu) {
  Connection conn = null;
  Statement stmt = null;
  try {
    // 加载驱动
    Class.forName("com.mysql.jdbc.Driver");
    // 获取连接对象
    conn = DriverManageer.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
    // 创建语句对象
    stmt = conn.createStatement();
    // 执行SQL语句
    StringBuilder sb = new StringBuilder("UPDATE t_student SET name = ");
    sb.append("'").append(stu.getName()).append("' WHERE id = ").append(stu.getId());
    System.out.println(sb);
    stmt.executeUpdate(sb.toString);
  } catch (Exception e) {
    e.printStackTrace();
  }
}
```

* StudentDAOImpl(DQL)

```java
public Student get(Long id) {
  Connection conn = null;
  Statement stmt = null;
  ResultSet rs = null;
  try {
    // 加载注册驱动
    Class.forName("com.mysql.jdbc.Driver");
    // 获取连接对象
    conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
    // 创建语句对象
    stmt = conn.createStatmenet();
    // 执行SQL语句
    String sql = "SELECT * FROM WHERE id = " + id;
    rs = stmt.executeQuery(sql);
    // 处理结果集
    if (rs.next()) {
      Student stu = new Student();
      stu.setId(id);
      stu.setName(rs.getString("name")
      stu.setAge(rs.getInt("age"));
      return stu;
    }
  } catch(Exception e) {
    e.printStackTrace();
  }
}

public List<Student> list() {
  Connection conn = null;
  Statament stmt = null;
  ResultSert rs = null;
  try {
    // 加载驱动
    Class.forName("com.mysql.jdcb.Driver");
    // 获取连接对象
    conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
    // 创建语句对象
    stmt = conn.createStatement();
    // 执行sql语句
    String sql = "SELECT * FROM t_student";
    rs = st.executeQuery()
    while (rs.next()) {
      Student stu = new Student();
      stu.setId(rs.getLong("id"));
      stu.setName(rs.getString("name"));
      stu.setAge(rs.getInt("age"));
      // 将每个学生添加到List集合中
      list.add(stu);
    }
    return list;
  } catch(Exception e) {
    e.printStackTrace();
  }
}
```

##预编译语句对象`PreparedStatement`
* 概念

>创建一个预编译语句对象，将带有占位符的`SQL`发送到数据库中进行编译

* 注意事项

>发送参数到数据库中执行sql，***千万不要传递参数***如果传递了参数，则会调用Statement的方法了

```java
boolean execute();
ResultSet executeQuery();
int executeUpdate();
```

* 示例代码

```java
@Test
public void testStatement() throws Exception {
    Class.forName("com.mysql.jdbc.Driver");
    Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
    Statement stmt = conn.createStatement();
    stmt.executeUpdate("INSERT INTO t_student(name, age) VALUES('张三', 19)");
}

@Test
public void testPreparedStatement() throws Exception {
  Class.forName("com.mysql.jdbc.Driver");
  Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/mytest", "root", "root");
  String sql = "INSERT INTO t_student(name, age) VALUES(?,?)";
  PreparedStatement pstmt = conn.prepareStatement(sql);
  // 为占位符设置参数
  ps.setString(1, "李四");
  ps.setInt(2, 200);
  ps.executeUpdate(); // 不要传递参数
  ps.close();
  conn.close();
}
```

##实现`DAO`和`CRUD`的操作

```java
public vod save(Student stu) {
  Connection conn = null;
  PreparedStatement pstmt = null;
  try {
    conn = JdbcUtil.getConnection();
    // 创建语句对象
    String sql = "INSERT INTO t_student VALUES(null, ?, ?)";
    ps = conn.prepareStatement(sql);
    // 为占位符设置
    ps.setString(1, stu.getName());
    ps.setInt(2, stu.getAge());
    ps.executeUpdate();
  } catch(Exception e) {
    e.printStackTrace();
  } finally {
    // 释放资源
    JdbcUtil.close(conn, pstmt, null);
  }
}

public Student get(Long id) {
  Connection conn = null;
  PreparedStatement pstmt = null;
  ResultSet rs = null;
  try {
    conn = JdbcUtil.getConnection();
    // 创建语句对象
    String sql = "SELECT * FROM t_student WHERE id=?";
    pstmt = conn.prepareStatement(sql);
    pstmt.setLong(1, id);
    // 执行SQL语句
    rs = pstmt.excecuteQuery();
    while (rs.next()) {
      Student stu = new Student();
      stu.setId(rs.getLong("id"));
      stu.setName(rs.getString("name"));
      stu.setAge(rs.getAge("age"));
      
      return stu;
    }
  } cathch(Exception e) {
    e.printStackTrace();
  } finally {
    JdbcUtil.close(conn, pstmt, rs);
  }
  return null;
}
```

##`Statement`与`preparedStatement`的对比
* PreparedStatement的优势
  * 语法简单，便于维护
  * 执行的效率更高(MySQL不支持)
  * 安全性更高(防止SQL注入)

##数据库事务的概述
* 事务(Transaction,简写为tx)

>指一组逻辑操作单元，使数据从一种状态变换到另一种状态。
>
>为确保数据库中数据的一致性，数据的操作应该是离散的成组的逻辑单元：
>
>当每个逻辑操作单元全部完成时，数据的一致性可以保持。
>
>而当这个单元中的一部分操作失败整个事物应全部视为错误，所有从起点以后的操作应全部回退到开始状态。

* 事务的操作

> 给事务定义一个起点，然后对数据做修改操作，这时如果提交(commite)，这些修改就永久地保存下来，如果回退(rollback),数据库管理系统将放弃所有的修改而回到开始事务时的状态。

* 事务的`ACID`属性：(面试题)

`1.原子性(Atomicity)`

>原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。(一损俱损)
 
`2.一致性(Consistency)`
 
>事务必须使数据库从一个一致性状态变换到另外一个一致性状态。(数据不被破坏)

`3.隔离性(Isolation)`

>事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作以及使用的数据对并发的其他事务是隔离的，各个事务之间不能互相干扰。

`4.持久性(Durability)`

>持久性是指一个事务一旦被提交，它对数据库中数据的改变是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响

* DML的操作默认是自动提交的

```java
try {
  // 1.将事务设置为手动提交(设置事务的起点)
  Connection对象.setAutoCommit(false);
  
  // 3.提交事务
  Connection对象.commit();
} catch (Exception e) {
  e.printStackTrace();
  // 2.执行事务回滚操作
  Connection对象.rollback();
}
```

* 注意事项

> 1.在`JDBC`中，事务是默认提交的，必须下线设置事务为手动提交
> 
> Connecton对象.setAutoCommit(false);
>
> 2.手动的提交事务.
>
> connection对象.commit();
>
> 3.若出现异常则必须回滚事务:
>
> connection对象.rollback();
>
> `MySQL`中，`InnoDB`支持外键,`MyISAM`不支持外键，不支持事务.


##大数据类型操作
* 示例代码

`将图片插入到数据库中`

```java
@Test
public void testInsert() throws Exception {
  Connection conn = JdbcUtil.getConnection();
  String sql = "INSERT INTO t_user(headImg) VALUES (?)";
  PreparedStatement pstmt = conn.prepareStatement(sql);
  InputStream in = new FileInputStream("asd.png");
  pstmt.setBlob(1, in);
  pstmt.executeUpdate();
  JdbcUtil.close(conn, pstmt, null);
  in.close();
}

@Test
public void testGet() throws Exception {
  Connection conn = JdbcUtil.getConnection();
  String sql = "SELET headImg FROM t_user WHERE id=?"
  PreparedStatement pstmt = conn.prepareStatement();
  ps.setLong(1, 2L);
  ResultSet rs = ps.executeQuery();
  if (rs.next()) {
    Blob blob = rs.getBlob("headImg");
    // 将二进制数据保存到磁盘中
    InputStream in = blob.getBinaryStream();
    // Java7的新特性，复制文件
    Files.copy(in, Paths.get("head.png"));
    in.close();
  }
  JdbcUtil.close(conn, pstmt, rs);
}
```

##连接池的思想
* 作用

>合理利用连接资源

* 注意

>释放资源`Connection对象.close()`的时候是是把`Connection对象`放回连接池，而不是和数据库断开

##Druid的使用

* 使用的步骤
  * 导入jar包(`druid-1.0.9.jar`)
  * 操作的方式和DBCP大致一样

* 示例代码

```java
// 获取连接池对象
public static DataSource getDataSource() {
  BasicDataSource ds = new BasicDataSource();
  ds.setDriverClassName("com.mysql.jdbc.Driver");
  ds.setUserName("root");
  ds.setPassword("root");
  ds.setUrl("jdbc:mysql:///mytest");
  retru ds;
}

// 获取连接对象
public static Connection getConnection() throws Exception {
  //使用工厂类创建连接池
  DataSource ds = BasicDataSourceFactory.createDataSource(p);
  // 从连接池中获取对象
  return ds.getConnection();
}
```



##代码重构

* JdbcUtil

```java
public class JdbcUtil {
  private static Properties p = new Properties();
  static {
    try {
       // 加载资源文件
       ClassLoader loader = Thread.currentThread().getContextClassLoader();
       InputStream ins = loader.getResourceAsStream("db.properties");
       p.load(in);
       // 加载注册驱动
       Class.forName(p.getProperty("driverClassName"));
     )
    } catch(Exception e) {
      e.printStackTrace();
    }
  }
  
  // 获取连接对象
  public static Connection getConnection() throws Exception {
     return DriverManager.getConnection(p.getProperty("url"), p.getProperty("usernmae"), p.getProperty("password"));
  }
}
```

* `DML`和`DQL`操作使用`PreparedStatement`代替Statement


