# 注解测试XML
## 什么是注解
>所有的Annotation都是java.lang.annotation.Annotation接口的子接口，所以Annotation是一种特殊的接口（枚举是特殊的类）;

    @interface Override{} ---> interface Over extends java.lang.annotation.Annotation{}
>所有的枚举类，都是java.lang.Enum类的子类。

    enum Gender{} ---> class Gender extends java.lang.Enum{}
