# Reflection
* 什么是反射

>在运行区间，动态地去获取类中的信息（类的信息，方法信息，构造器信息，字段信息）

* 反射中常用的API

```java
//Class:表示所有的类信息
// 获取对应类的Class实例
static Class<?> forName(String class className); // 获取对应类的Class实例

T newInstance(); // 创建对应类的对象（该类中必须有一个公共无参数的构造器）
```