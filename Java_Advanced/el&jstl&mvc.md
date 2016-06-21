# EL&JSTL&MVC

##EL表达式
---

* 概念
> 表达式语言

* 目的
> 获取作用域中的共享数据

* 语法
> `${属性名称}` ---> ${msg}

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
// 使用EL表达式的内置对象pageContext可以动态获取到上下文路径
${pageContext.request.contenPath}
// 方式二Servlet3.0以后，EL表达式支持直接访问
${pageContext.getRequest().getContextPath}
```

##`JSTL`

使用`JSTL`来解决`JSP`中的`Java`代码

* 使用步骤
  * 导入`jstl.jar`和`standard.jar`
  * 引入标签库

>`<%@taglib uri="http://java.sun.com/jsp/jstl/cor" prefix="c"%>`

* `JavaWeb`开发需要的最少的`jar包`

```xml
el-api.jar
jsp-api.jar
servlet-api.jar
jstl.jar
standard.jar
```

### `JSTL`的逻辑判断标签
* 单条件的判断

  * `<c:if>`不包含内容
  * `<c: if>`包含内容
  
```jsp
// 包含内容
<c:if test="checkCondiction" var="varName" scope="page|request|session|application">
    body content
</c :if>

// 不包含内容
<c:if test="checkCondiction" var="varName" scope="page|request|session| application />
```

* 示例代码

```jsp
<%
  Integer age = 18;
  request.setAttribute("age", age);
%>

<!--
  var : 存储条件表达式的结果，在applicationScope作用域中
  scope : 指定将结果存储到哪个作用域中
-->

<c:if test="${age>=18}" var="ret" scope="application">
  成年人-->${applicationScope.ret}
</c:if>
```

* 多条件判断:`(choose-when-other)`相当于`Java`中的`if-elseif-elseif-else`

  * 示例代码

```jsp
<%
  Integer age = 18;
  request.setAttribute("age", age);
%>

<c:choose>
  <c:when test="${age>=18}">
    成年人
  </c:when>
  <c:otherwise>
    未成年人
  </c:otherwise>
</c:choose>
```

* 循环遍历标签(`foreach`)
  * 作用
>用来对一个collection集合中的一系列对象进行迭代输出，并且可以指定迭代次数
  
  * 使用方法

```jsp
<!--
  items:要迭代的集合，通常是保存在作用域中，所以需要使用EL表达式取值
  var:迭代出的每个元素存储的变量，每个迭代出来的元素都保存在pageScope中
-->
<c:forEach items="collection" var="varName">
    Body content
</c:forEach>

<!--
  需求：在页面中输出一串数字 1,2,3,4,5,6,7,8,9,10
  begin : 开始的数字
  end : 结束的数字
  step : 步长，默认为1
-->
<c:forEach var="varName" [begin="begin"] [end="end"] [step="step"]>
  ${varName}
</c:forEach>
```

* `<c:forEach>`标签的属性

属性 | 类型 | 意义
--| -- | --
index | number | 现在指到成员的索引
count | number | 总共指到成员的总和
first | boolean | 现在指到的成员是否为第一个
last | boolean | 现在指到成员是否为最后一个

* 示例代码

```jsp
<%
  List<String> list = new ArrayList<String>();
  list.add("O.z");
  list.add("Will");
  list.add("Tim")
%>

<c:forEach items="${list}" var="item" varStatus="vs">
  ${vs.count} --> ${pageScope.item}<br />
</c:forEach>

<c:forEach begin="1" end="10" var="num" step="2">
  ${num}
</c:forEach>
```

* 日期格式化标签

```jsp
<%@taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>

<%
  request.setAttribute("date", new Date());
%>

<fmt:formateDate value="${date}" pattern="yyyy/MM/dd HH:mm:ss" />
```






