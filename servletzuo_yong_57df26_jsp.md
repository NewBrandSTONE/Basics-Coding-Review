# Servlet作用域&JSP
##为什么Servlet之间需要交互
* `DeleteServlet`删除列表中某一项后要返回到`ListServlet`中

##Web组件之间的跳转

* 三种方式
  * 请求转发(`forward`)
  * `URL`重定向(`redirect`)
  * 请求包含(`include`)

* 请求转发（`forward`）
  * 语法
>  request对象.getRequestDispatcher(String path).forward(request, response);
>  
>  // 参数  
>  // path表明目标资源名称(需要跳转到哪里)

  * 特点
    * 浏览器地址栏路径没变，依然是`Servlet1`的资源名称
    * 只发送了一个请求
    * ***共享一个请求，在请求中共享数据***
    * 最终的响应输出由`Servlet2`来界定
    * 只能访问当前应用中的资源，不能跨域跳转
    * 可以访问`WEB-INF`中的资源


* `URL`重定向(`redirect`)
  * 语法
>response对象.sendRedirect(String path);
>
>// 参数
>path表示目标资源名称

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
  * 若需要跨域访问，只能使用`URL`重定向
  * ***请求转发可能造成表单的重复提交问题***

##数据共享

名称 | 类型 | 描述
-|-|-
pageContext(page) | PageContext | 表示当前JSP页面的范围
request | HttpServletRequest | 表示当前请求的范围，知识共享一个请求中的数据。
session | HttpSession | 表示当前回话的范围，只要浏览器不关闭，session就是同一个对象
application | ServletContext | Tomcat关闭applicaiton对象才销毁

##ServletContext接口和常用方法

* 概念
>`ServletContext`接口，表示的应用，`Tomcat`启动的时候回创建一个对象，`Tomcat`关闭的时候对象销毁
>
>在整个`Web`的生命中期中，只有一个对象，表示的就是当前应用

