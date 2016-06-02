# Reflection
* 什么是`反射`

>在运行区间，动态地去获取类中的信息（类的信息，方法信息，构造器信息，字段信息）

* 反射中常用的`API`

```java
//Class:表示所有的类信息
// 获取对应类的Class实例
static Class<?> forName(String class className); // 获取对应类的Class实例
T newInstance(); // 创建对应类的对象（该类中必须有一个公共无参数的构造器）
String getName(); //获取类的权限定名
String getSimpleName(); //获取类的简单名称
Constructor<?>[] getConstructors(); // 获取当前类中所有的公共构造器
COnstructor<T> getConstructor(Class<?> ...parameterTypes); // 获取当前类中指定的构造方法
Method[] getMethods(); // 获取当前类中所有的公共方法（包括从父类中继承过来的方法）
Method getMethod(String name, Class<?> ...parameterTypes); // 获取指定的类方法
Method[] getDeclaredMethods(); //获取当前类中所有方法，和访问权限无关
Method getDeclaredMethod(String name, Class<?> ...parameterTypes);
// 获取指定的类方法，和访问权限无关

Object invoke(Object obj, Object ...args); //调用指定的方法
//参数:obj,该方法所属的对象，如果是静态方法则传入null;args,调用方法所需的实际参数
```

#`Class`类和`Class`的实例
* `Class`类：

>用于描述一切`类`和`接口`。`枚举`是一种`类`，`注解`是一种`接口`。
>
>为了明确区分出`Class`实例表示的是谁的字节码，`Class`提供了`泛型`

```java
Class<Date> clzl = Date.class; //clzl表示的是Date的字节码
Class<String> clz2 = String.class; // clz2表示的是String的字节码
```


* `Class`实例：

>JVM中一份字节码

* 获取到Class实例的三种方式
  * `类型.class`（就是一份字节码）
  * `Class.forName(String className);`根据一个类的全限定名来构建Class对象
  * 每一个对象都有`getClass()的方法`

>***反射很强大，但是非常消耗性能，主要是为了做工具和框架是用的***。

```java
// 同一个类在JVM中只有一份字节码;
//第一种方式：数据类型.class
Class<User> clz1 = User.class;
// 第二种方式:Class.forName(String className);
Class<?> clz2 = Class.forName("cn.itsource.User");
// 第三种方式:对象.getClass();得到对象的真实类型
User u = new User();
Class clz3 = u.getClass();
//clz1 == clz2 == clz3; 因为表示都是JVM中共同的一份字节码(User.class)
```

* 八大基本数据类型和关键字Void的Class实例

>在八大基本数据类型的包装类和Void类中都有一个常量：TYPE
>
>***所有数据类型都有class属性，表示对应的Class实例***

```java
  Integer.Type-->int.class
  Void.Type-->void.class
  
  Integer.Type == int.class; // true
  Integer.Type == Integer.class; // false
```

* 数据的Class实例
  * 特点
 >***所有具有相同元素类型和维数的数组才共享同一份字节码对象（Class对象）***

    ```java
 // 表示数组的Class实例：
String[] sArr = {"A", "B", "C"};
Class clz = String[].class; // 此时clz表示就是一个String类的一位数组类型;
```

  * 示例代码
    ```java
    public class ArrayClassInstanceDemo {
           String[] arr1 = {};
           String[] arr2 = {"A", "B"};
           Class clz1 = String[].class
           Class clz2 = arr2.getClass();
           Class clz3 = getClass();
           System.out.println(cl2 == c3);
           String[][] arr = {}
           System.out.println(clz1 == String[][].class); //false
           int[] iArr = {};
           System.out.prinln(iArr.getClass == clz1); // false
    }
  ```
* 获取类中的构造器

  * 常用方法
 
  ```java
 // Constructor<T>类：表示父类中构造器的类型，Constructor的实例就是某一个类中的某一个构造器
 // 获取某一个类中的所有构造器：
 // 1.明确操作的是哪一份字节码对象。
 // 2.获取构造器。
 
 // Class类获取构造器方法：
 // Constructor类：表示类中构造器的类型，Constructor的实例就是某一个类中中的某一个构造器
  
  public Constructor<?>[] getConstructors(); // 该方法只能获取当前Class所表示的类的public修饰的构造器
  public Constructor<?>[] getDeclaredConstructors(); // 获取当前Class所表示类的所有构造器，和访问权限无关
  public Constructor<T> getConstructor(Class<?>... parameterTypes); // 获取当前Class所表示类中指定的一个public的构造器
  // 参数：parameterTypes 表示：构造器参数的Class类型
  // 例如:
  public User(String username);
  Constructor c = clz.getConstructor(String.class);
  public Constructor<T> getDeclaredConstructor(Class<?>.. parameterTypes); // 获取当前Class所表示类中指定的一个构造器
  ```
  * 实例代码

  ```java
  public class User {
    private String name;
    private Integer age;
    private User() {}
    public User(String name) {}
    
    public static void main(String[] args) {
      Class<User> clz = User.class;
      Constructor<User> conn = clz.getConstructor(in.class);
      System.out.println(conn);
      // 由于在User类中没有带有int类型参数的构造器，所以会抛出异常
      // NoSuchMethodException
    }
  }
  ```

  * 创建对象
    * 方式一

    ```java
    // 在反射中，Constructor最主要的作用就是调用构造器，创建构造对象
    ```








