# 注解测试XML
## 1. 什么是注解

所有的Annotation都是java.lang.annotation.Annotation接口的子接口，所以Annotation是一种特殊的接口（枚举是特殊的类）;

```java
@interface Override{} ---> interface Over extends java.lang.annotation.Annotation{}
```
    
所有的枚举类，都是java.lang.Enum类的子类。

```java
    enum Gender{} ---> class Gender extends java.lang.Enum{}
```

>*注解被用来为程序元素（类，方法成员变量等）设置元数据。*

>*注解，标签，Annotation都是一体。*

**使用注解需要三方面的参与**

>1.要有一个注解

>2.被贴的程序元素

>3.需要第三方程序参数，赋予注解特殊的功能

**注解：主要是用来描述程序元素,和xml的功能是一样的，都是用来实现配置信息**

---







