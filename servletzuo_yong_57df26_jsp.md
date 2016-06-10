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



