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

>`JVM`中一份字节码

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

* 八大基本数据类型和关键字`Void`的`Class`实例

>在八大基本数据类型的包装类和`Void`类中都有一个常量：`TYPE`
>
>***所有数据类型都有class属性，表示对应的Class实例***

```java
  Integer.Type-->int.class
  Void.Type-->void.class
  
  Integer.Type == int.class; // true
  Integer.Type == Integer.class; // false
```

* 数据的`Class`实例
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
    // 常用方法
    public T newInstance(Object... initargs); // 如果调用带参数的构造器，只能使用该方法
    // 参数：initargs：表示调用构造器的实际参数
    // 返回：返回创建的实例，T表示Class所表示的类的类型
    // 如果：一个雷的构造器可以直接访问，同时没有参数，那么可以直接使用Class类中的newInstance方法创建对象.
    public Object newInstance(); // 相当于new 类名();
    // 注意不能调用私有的构造器(可以先将对象的)
    ```
    * 方式二
    
    ```java
    // 调用Class类中的newInstance()方法来创建
    Class clz = User.class;
    User user = clz.newInstance();
    ```

  * `API`中`AccessibleObject`的介绍
  
    > `AccessibleObject` 类是`File``Method`和`Constructor`对象的基类。在使用Field、Method或者Constructor对来来设置或者获取访问字段、调用方法或者创建和初始化类的实例的时候，会执行***访问检查***。
    >
    >在ACcessibleObject中提供了一个方法`setAccessible(boolean flag)`方法来设置是否忽略底层的访问检查`flag:true`表示忽略访问检查，`false表示要检查（缺省值）`

##获取类中的方法

  * 使用反射获取某一个勒种的方法：
    * 找到获取方法所在类的字节码对象
    * 摘到需要被获取的方法

  * `Class`类中常用方法：

```java
// 获取包括自身和继承过来的所有public方法
public Method[] getMethods();

// 获取自身的所有方法（不包括继承，和访问权限无关）
public Method[] getDeclaredMethods();

// 表示调用指定的一个公共方法（包括继承的）
// 参数：methodName(被调用方法的名字);parameterTypes(被调用方法的参数类型,如:String.class)
public Method getMethod(String methodName, Class<?>... parameterTypes);

// 调用指定的一个本类中的方法（不包括继承的）
// 参数：同上
public Method getDeclaredMethod(String name, Class<?>... parameterTypes);
```
  * 示例代码

```java
// 获取所有方法
private static void getAllMethod() {
  Class clz = User.class;
  Method[] ms = clz.getMethods(); // 获取所有方法(注意：包括继承的所有公共方法)
  for (Method m : ms) {
    System.out.println(m);
  }
  ms = clz.getDeclaredMethos();// 获取所有的方法(注意：不包括继承的)
}
for (Method m : ms) {
  System.out.println(m);
}

pubic void testMethods() {
  // 获取 public void sayHi();
  Class clz = User.class;
  // 只有通过方法签名才能找到唯一的方法
  // 方法签名 = 方法签名 + 参数列表(参数类型，参数个数，参数顺序)
  Method m = clz.getMethod("sayHi", String.class);
  System.out.println(m);
  
  // 调用private void sayGoodBye(String name, int age);
  m = clz.getDeclaredMethod("sayGoodBye", String.class, int.class);
  System.out.println(m);
}
```

##使用反射调用方法
* 使用反射调用方法：
  * 找到被调用方法所在的字节码
  * 获取到被调用的方法对象
  * 调用该方法

调用方法
```java
// 调用当前Method所表示的方法
// 参数:obj(被调用方法的对象--一般都是这个类的一个对象);args(传递的参数)
public Object invoke(Object obj,Object.. args);
```

* 示例代码

```java
public static void main(String[] args) {
  // 1.获取User类的Class实例
  Class<User> clz = User.class;
  // 2.获取User类中指定的方法:public String doWork3(String name, Integer.class);
  Method method3 = clz.getMethod("doWork3", String.class, Integer.class);
  // 3.调用对象方法，这里需要传入被调用方法所属的对象，和方法所需要的实际参数
  User u = clz.newInstance(); // 保证在User类中有公共无参数的构造器
  // 用变量ret来接收该方法的返回值
  String ret = (String) method3.invoke(u, "neld", 18);
  
  // 获取User类的一个指定的私有方法:private void doWork2(String name)
  Method method2 = clz.getMethod("doWork2", String.class);
  // 将方法设置为可访问的
  method2.setAccessible(true);
  // 调用方法
  method2.invoke(u, "neld");
}
```

##使用放射调用静态方法
* 方法
```java
public Object invoke(Object obj, Object.. args);
// 如果底层方法是静态的，那么可以忽略指定的obj参数。将obj参数设置为null即可。
``` 

##使用反射调用可变参数
* 方法

>对于***数组的引用类型***，底层会自动解包 ，为了解决该问题，我们使用Object的一个一维数组把实际参数包装起来。

>***无论是基本数据类型还是引用数据类型，或者是可变参数类型，方正就是一切实际参数都包装在 `new Object[] {}` 中，就没有`自动解包`的问题了***