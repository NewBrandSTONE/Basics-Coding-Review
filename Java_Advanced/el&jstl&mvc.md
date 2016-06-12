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
      | ***requestScope*** | ***同请求作用域属性的名称和值有关的Map类***
      | ***sessionScope*** | ***同会话作用域属性的名称和值有关的Map类***
      | applicationScope | 同应用程序作用域属性的名称和值有关的Map类
