# Servlet作用域&JSP
##为什么Servlet之间需要交互
---

> `DeleteServlet`删除列表中某一项后要返回到`ListServlet`中

##`Web组件`之间的跳转

* 三种方式
  * 请求转发(`forward`)
  * `URL`重定向(`redirect`)
  * 请求包含(`include`)


* 请求转发（`forward`）
  * 语法
 
```java
request.getRequestDispatcher(String path).forward(request, response);
// 参数
// path表明目标资源名称(需要跳转到哪里)
```

* 特点
  * 浏览器地址栏路径没变，依然是`Servlet1`的资源名称
  * 只发送了一个请求
  * ***共享一个请求，在请求中共享数据***
  * 最终的响应输出由`Servlet2`来界定
  * 只能访问当前应用中的资源，不能跨域跳转
  * 可以访问`WEB-INF`中的资源


* `URL`重定向(`redirect`)
  * 语法

```java
response.sendRedirect(String path);
// 参数
// path表示目标资源名称
```

* 特点
  * 浏览器地址栏路径发生变化，变成`Servlet2`的资源名称
  * 发送了两个请求
  * ***因为是不同的请求，所以不能共享数据***
  * 最终的响应输出由`Servlet2`来决定
  * 可以跨域访问资源
  * ***不可以访问`WEB-INF`中的资源***


* 请求转发和`URL`重定向的区别
  * 分别解释各自的定义
  * 罗列各自的特点
  * 各自的应用场景


* 请求转发和`URL`重定向的选择
  * ***若需要共享请求中的数据***只能使用请求转发
  * ***若需要访问WEB-INF中的资源，只能使用请求转发***
  * 若需要跨域访问，只能使用`URL重定向`
  * ***请求转发可能造成表单的重复提交问题***

##Servlet与JSP的数据共享作用域
---

名称 | 类型 | 描述
-|-|-
pageContext(page) | PageContext | 表示当前JSP页面的范围
request | HttpServletRequest | 表示当前请求的范围，只是共享一个请求中的数据。
session | HttpSession | 表示当前回话的范围，只要浏览器不关闭，session就是同一个对象
application | ServletContext | Tomcat关闭applicaiton对象才销毁

##ServletContext接口和常用方法
---

* 概念

>`ServletContext`接口，表示的是当前应用对象，`Tomcat`启动的时候会创建一个对象，`Tomcat`关闭的时候对象销毁
>
>在整个`Web`的生命中期中，只有一个对象，表示的就是当前应用

* 如何获取该对象

```java
// 1.在Servlet中
ServletContext ctx = super.getServletContext();
// GenericServlet实现了Servlet和ServletConfig接口，而getServletContext是GenericServlet的一个方法

// 2.通过请求对象获取
// Servlet3.0之后
ServletContext ctx2 = req.getServletContext();
// Servlet3.0之前
ServletContext ctx3 = req.getSession.getServletContext();
```

* `ServletContext`的常用方法

```java
// 获取上下文路径
String getContextPath();
// 根据指定的相对路径获取到绝对路径
String getRealPath(String path);
```

* 问题
  * 如果在`Servlet`中，出现了硬编码，应该将硬编码配置到`web.xml`中，然后使用`ServletConfig`中的`getinitParameter(String name)`获取，但是如果在多个`Servlet`中有相同的配置，那就要在多个`Servlet`中写相同的配置，不便于维护
  * 解决方案

```xml
<!-- 使用全局初始化参数 -->
<context-param>
  <param-name>name</param-name>
  <param-value>neld</param-value>
</context-param>
```

```java
// 根据指定的名称获取全局初始化参数
String getInitParameter(String name);
// 获取所有全局初始化参数的名称
Enumeration getInitParameterNames();
```

* 获取初始化参数的区别
  * `HttpServletRequest`中的`getParameter(String name)`获取用户提交的数据
  * `ServletConfig`中的`getInitParameter(String name)获取`Servlet`中的初始化参数
  * `ServletContext`中的`getInitParmeter(String name)`获取`web.xml`中的全局初始化参数作用域中的`getAttribute(String name)`获取对应作用于中的共享数据


##JSP
---
* 功能
  * 与`Servlet`一样，都是用来实现动态页面输出

* `Servlet`的缺陷
  * 输出页面的代码非常恶心
  * 在`Servlet`中，没有体现`责任分离`的原则

* `Servlet`擅长做的事情
  * 获取请求参数
  * 调用业务处理请求
  * 控制页面跳转

* `JSP`擅长的事情
  * 页面输出

* `Servlet`动态页面输出
  * `Java`代码（主)+`Html`代码（辅） --> 动态页面

* `JSP`动态页面输出
  * `Java`代码（辅） + `Html`代码(主) --> 动态页面

##JSP底层原理分析
---
* 在`web.xml`中配置有`<servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>`
  * 只要是后缀名为jsp/jspx的资源都会访问到该`servlet`
  * 该`Servlet`的职责就是负责将`JSP`文件翻译成`Servlet

* 查看编译后`JSP`文件，发现有`.class/.java`的文件

* `HttpJspBase`继承了`HttpServlet`并且实现了`HttpJspPage`

* 最终在页面上输出的还是使用`Servlet`代码来实现的



##JSP三大指令
---

标准指令：设定`JSP`网页的个整体配置信息

* 特点
  * 不向客户端产生任何输出
  * 指令在`JSP`整个文件范围内有效

* 指定的使用语法

```jsp
<%@ 指令名称 属性名=属性值 属性名=属性值 %>
```

###page
---
* 作用

>定义`JSP`页面的各种属性

* 属性

>`import`:导入`JSP`中的`Java`脚本使用到的类或者包，一个`import`可以导入多个包，需要用逗号分隔。
>
>`session`:指示`JSP`页面是否创建`HttpSession`对象，默认值是`true`
>
>`buffer`:指示`JSP`用的出书流的缓冲大小，默认是`8KB`
>
>`errorPage`:指当前页面出错以后转向的页面，需要在`web.xml`中配置

```xml
<error-page>
  <excepiton-type>java.lang.Exception</exception-type>
  <location>/error.jsp</location>
</error-page>
```
>`isErrorPage`:指示当前页面是否产生`Exception`对象
>
>`contenType`:指定当前页面的`MIME`类型，作用与`Servlet`中的`response.setContentType`一致
>
>`pageEncoding`:通知引擎读取`JSP`的时候采用的编码
>
>`isELIgnored`:是否忽略`EL`表达式,默认是`false`

###include
---
静态包含，在开发的时候，如果能使用静态的则使用静态的，而不使用动态的

* 作用

>包含其他组件

* 语法

>`<%@include file=""%>`其中`file`为指定要包含的目标组件。路径如果以`"/"`(当前应用)，就是绝对路径。

* 原理

>把目标组件的内容加到源组件中，输出结果

* 动态包含

>采用动作元素：`<jsp:include page="" />` 路径如果以`"/"`(当前应用)就是绝对路径。

###taglib
---

* 作用

>引入外部的标签

* 语法

>`<%@taglib uri="标签名称空间" prefix="前缀"%>`

```xml
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```


###JSP九大内置对象和四大作用域
---
* `JSP`内置对象
  * `JSP`中事先创建好的对象，可以直接拿来使用

名称 | 类型 | 描述
--|--|--
request | HttpServletRequest | 当前的请求对象
response | HttpServletResponse | 当前的响应对象
pageContext | PageContext | 表示当前的JSP页面对象
session | HttpSession | 当前会话对象，在JSP中必须设置`session="true"`
exception | Throwable | 表示异常对象，必须在JSP中设置`isErrorPage="true"`
application | ServletContext | 表示当前应用
config | ServletConfig | 表示JSP的配置对象
out | JspWriter | 表示输出流对象
page | Object | 表示当前页面对象

* `JSP`四大作用域对象

名称 | 类型 | 描述
--|--|--
pageContext | PageContext | 表示当前的JSP页面对象
request | HttpServletRequest | 当前的请求对象
session | HttpSession | 当前会话对象,在JSP中必须设置`session="true"`
application | ServletContext | 表示当前应用


##JSP中静态包含和动态包含的区别
---
在JSP中使用静态包含
>`<%@include file="被包含的页面">`

* 特点
>在翻译阶段就已经合并在一起了，只有一个`Java`文件

在JSP中使用动态包含
>`<jsp:include page="被包含的页面">`

* 特点

>在运行阶段合并在一起，有两个`Java`文件
















 



