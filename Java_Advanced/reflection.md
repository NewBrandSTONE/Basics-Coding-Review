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
>***所有具有相同元素类型和维数的数组才共享同一份字节码对象（Class对象）***
 








