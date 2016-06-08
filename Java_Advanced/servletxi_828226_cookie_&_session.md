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

##`Servlet3.0`的新特性--`注解`配置
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
@WebServlet(value="/anno", 
loadOnStartup=0, 
initParams={@WebInitParam(name="encoding", value="UTF-8")})
public class AnnoServlet extends HttpServlet {
  // TODO...
}
```

##`Servlet`线程安全问题
* `Servlet`在整个应用中只有一个实例

>问题的根本原因：因为有多个线程并发访问（修改）当前的`Servlet`中的资源，又因为`Servlet`是单例的，所以所有的成员，都会去访问同一个对象


* 解决方法
  * 可以让当前的`Servlet`去实现一个接口`SingleThreadModel`，表示当前的`Servlet`只能被同一个线程访问***该方法不推荐，已经被淘汰了***
  * `Servlet`中使用局部变量代替成员变量

##`Http协议`无状态带来的问题
>`HTTP`是无状态协议，也就是没有记忆力，每个请求之间无法共享数据。这样就无法知道会话什么时候开始，什么时候结束，也无法确定发出请求的用户身份。
>
>`web`中的回话：在浏览器打开的时候，在这之钱应该要进行多次一问一答的交互，关闭浏览器的时候结束

* 解决方案：解决`HTTP`无协议状态的问题
  * 使用参数传递机制
  * `cookie`
  * `session`

>在请求路径后面跟上响应参数***如/param/list?username=ghyz***这种方式可以解决数据共享的问题，但是，参数显示在了地址栏中，***不安全(不推荐)***



##`Cookie`
>`Cookie`客户端技术，将共享数据保存在客户端中（浏览器）

* 原理

>让***浏览器***记住键值对，是向响应头中添加一下头即可`set-Cookie:name=tom`
>浏览器记住之后，向服务器发送键值对，是在请求头中添加下面的信息`Cookie:name=tom`


* `Cookie`的细节

```java
// 1.创建Cookie对象，并设置共享数据
Cookie c = new Cookie(String name, String value);
// 2.将Cookie响应给浏览器
response.addCookie(c);
// 3.获取Cookie中的共享数据
Cookie[] cookies = request.getCookies();
for (Cookie c : cookies) {
  if ("username".equals(c.getName())) {
    String value = c.getValue();
  }
}
// 4.修改Cookie中的共享数据
// 方式一：获取到要修改的Cookie对象，调用setValue(String newValue);
// 方式二：重新创建一个名字和要修改的Cookie一样的Cookie对象即可,修改之后需要将Cookie重新发送给浏览器
// 5.Cookie的生命周期(默认是在关闭浏览器就销毁)
void setMaxAge(int expiry)
// expiry > 0:设置生存的时间为expiry妙
// expiry = 0:立即删除当前的Cookie
// expiry < 0:缺省值，在关闭浏览器就销毁
// 6.删除Cookie
  c.setMaxAge(0);
// 7.Cookie中的name和value不支持中文
// 解决方案
// 将文中的字符先重新编码(编码成非中文的)
// 编码
URLEncoder.encode("大黄", "UTF-8");
// 解码
URLDecoder.decode("对应的编码","UTF-8");
```


