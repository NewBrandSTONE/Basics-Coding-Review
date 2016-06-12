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
```
