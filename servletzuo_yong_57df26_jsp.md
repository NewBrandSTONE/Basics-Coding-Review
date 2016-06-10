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





