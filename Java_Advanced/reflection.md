# Reflection
* 什么是反射

>在运行区间，动态地去获取类中的信息（类的信息，方法信息，构造器信息，字段信息）

* 反射中常用的API

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
Method getMethod(String name, Class<?> ...parameterTypes)
```