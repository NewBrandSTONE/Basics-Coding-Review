# EL&JSTL&MVC

##`EL`表达式

* 概念
>表达式语言

* 目的
>获取作用域中的共享数据

* 语法
>`${属性名称}` ---> ${msg}

```jsp
<%
  // 设置不同作用于的共享数据
  pageContext.setAttribute("msg", "pageContextValue");
  request.setAttribute("msg", "requestValue");
  session.setAttribute("msg", "sessionValue");
  application.setAttribute("msg", "applicationValue");
%>

  // 获取不同作用域中，同属性名的属性值
  pageContext:${pageScope.msg}
  request:${requestScope.msg}
  session:${sessionScope.msg}
  application:${applicationScope.msg}
```

* `EL`隐藏对象

类别 | 标识符 | 描述
--| -- | --
JSP | ***pageContext*** | ***PageContext处理当前页面***
作用域 | pageScope | 同页面作用域属性名称和值有关的Map类
作用域 | ***requestScope*** | ***同请求作用域属性的名称和值有关的Map类***
作用域 | ***sessionScope*** | ***同会话作用域属性的名称和值有关的Map类***
作用域 | applicationScope | 同应用程序作用域属性的名称和值有关的Map类

* 在`EL`中访问`JavaBean`属性方式
  * 使用`.`来访问
  * 使用`[]`来访问

```jsp
${对象.属性名}--->对象.getXxx,注意必须提供getter方法
若操作的是Map:${对象.key}
比如：${u.userName} 等价于 ${u["username"]}
```

* `EL`表达式的细节
  * 获取上下文路径

>表单中的`action`属性组成部分：`上下文路径` + `资源名称`如果我们在页面写死的话，上下文路径改动之后，所有的表单都要修改

`EL表达式`获取上下文路径的方式

```jsp
// 方式一
${pageContext.request.contenPath}
// 方式二Servlet3.0以后，EL表达式支持直接访问
${pageContext.getRequest().getContextPath}
```



