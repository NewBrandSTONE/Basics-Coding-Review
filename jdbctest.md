# JDBCTest

期中测试代码

* 需求

```xml
1，在数据库jdbcdemo中创建小码哥班级表  t_class 字段和实体类的变量一一对应
	备注：必须手写，不能用工具创建（建表语句放在t_class.sql文件里面）。
2，用jdbc完成t_class 数据的 增（save），删(delete)，改(update)，查(get,list)（查询一条AND多条数据）用dao层进行数据交互。
4，需要抽出DBUtil.java 工具类封装jdbc，数据库的配置参数要从db.properties加载
5，创建测试类，分别测试增删该查各个方法 
6，抽出DQL语句和DML语句模板，取名 JdbcTemplate.java。(可选)
代码规范性（10分）
备注：
数据库一定要用jdbcdemo，用户名 root 密码是 admin  如果不是的，请在提交前改成 admin
```

* 项目结构
  * pps.dao
  * pps.dao.impl
  * pps.domain
  * pps.test
  * pps.util
  * pps.template

* db.properties

```xml
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/jdbcdemo?useUnicode=true&characterEncoding=utf-8
username=root
password=admin
```

* JDBCUtil.java

```java
public class JDBCUtil {
  private static Properties p = new Properties();
  
  static {
    ClassLoader loader = Thread().currentThread().getClassLoader();
    InputStream ins = loader.getResourceAsStream("db.properties");
    
  }
  
}
```



* Classes.java

```java
public class Classes {
  private Long id;
  private String classname;
  private String teacher;
  private BigDecimal fee;
  // 省略了构造方法,Setter，Getter
}
```

* IClassesDAO.java

```java
public interface IClassesDAO {
  void save(Classes c);
  void delete(Long id);
  void update(Classes c);
  Classes get(Classes c) throws Exception;
  List<Classes> list() throws Exception;
}
```

* IClassesDAOBasicImpl.java

```java
public class IClassesDAOBasicImpl implements IClassesDAO {
  void save(Classes c) {
    
  } 
}
```




