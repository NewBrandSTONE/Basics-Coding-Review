# Servlet细节&Cookie&Session
##Servlet的细节问题
* 一个`Servlet`可以有多个映射`<url-pattern>`

```xml
<servlet>
  <servlet-name>MappingServlet</servlet-name>
  <servlet-class>com.ghyz.mapping.MappingServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>MappingServlet</servlet-name>
  <url-pattern>/mapping</url-pattern>
  <url-pattern>/mapping2</url-pattern>
  <url-pattern>/mapping3</url-pattern>
</servlet-mapping>
```

* `Servlet`的`<url-pattern>`可以使用通配符`*`进行配置
  * `/*`表示可以使用任意字符访问当前的`Servlet`
  * `/system/*`可以使用以`/system`为前缀，后面是任意字符即可访问当前的`Servlet`
  * `*.xxx`任何字符，加上指定的后缀名即可访问当前的`Servlet`


* `Servlet`的`<servlet-name>`不能为`default`如果为`default`那么当前项目中的静态资源将无法被访问
  * 因为在`Tomcat`中有一个默认的`Servlet`配置其`<servlet-name>`就是`default`专门用来访问项目中的静态资源。


* 当`Servlet`中初始化操作非常复杂的时候，那么这种操作就非常好使，给第一个访问用户的体验就很差
  * 问题的根本：初始化操作放在了第一次访问的时候执行
  * 解决方案：应该将耗时的初始化操作放在第一次访问执行之前

```xml
<servlet>
  <servlet-name>MappingServlet</servlet-name>
  <servlet-class>com.ghyz.mapping.MappingServlet</servlet-class>
  <!-- 设置Servlet在Tomcat中启动的时候执行初始化，数字越小越先执行 -->
  <load-on-startup>0</load-on-startup>
</servlet>
```

##`Servlet3.0`的新特性--注解配置
>从`Servlet3.0`开始`Servlet`就支持使用注解进行配置，可以使用注解来***替代部分***的`web.xml`文件

* 使用`注解`的准备工作(`web.xml`的配置)
  * `metadata-complete="true"`:指定服务器忽略`Servlet`上面的注解
  * `metadata-complete="false"`:指定服务器编译`Servlet`上的注解***(缺省值)***


* 选择`注解`还是`web.xml`配置`Servlet`?
  * `web.xml`维护难度大
  * `注解`：将硬编码拉回到程序中，不好维护
  * 如果当前配置是针对某一个`Servlet`的话，可以使用`注解`
  * 如果是通用的配置，应该使用`web.xml`例如给多个`Servlet`共享数据

>使用`注解`配置`Servlet`示例代码
```java
@WebServlet(value="/anno", loadOnStartup=0, initParams={@WebInitParam(name="encoding", value="UTF-8")})
public class AnnoServlet extends HttpServlet {
  // TODO...
}
```

##`Servlet`线程安全问题
* `Servlet`在整个应用中只有一个实例
>问题的根本原因：因为有多个线程并发访问（修改）当前的`Servlet`中的资源，又因为`Servlet`是单例的，所以所有的成员，都会去访问同一个对象

