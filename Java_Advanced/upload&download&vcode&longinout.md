# Upload&Download&VCode&Longin/out

##Login & Logout
---
###Login

* login.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function random() {
		var obj = document.getElementById("code");
		obj.src = "${pageContext.request.contextPath}/randomCode?" + new Date().toString();
	}
</script>
</head>
<body>
	<h2>用户登录</h2>
	<span style="color: red;">${errorMsg}</span>
	<form action="${pageContext.request.contextPath}/login" method="post">
		<!-- placeholder直接在输入框中给用户看到提示 -->
		<input type="text" name="username" placeholder="用户名" /> 
		<input type="password" name="password" placeholder="密码" />
		<input type="submit" value="登录" /><br /> 
		验证码<input type="text"	name="userRandomCode" />
		<!-- src是生成验证码的Servlet对应的路径 -->
		<img id="code" alt="验证码"	src="${pageContext.request.contextPath }/randomCode" onclick="random();" />
	</form>
</body>
</html>
```

* LoginServlet.java

```java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {

  private IUserDAO dao;
  
  @Override
  public void init() throws ServletException {
    dao = new UserDAOImpl();
  }
  
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws Exception {
    // 设置请求参数编码只对post方法有用,一般的表单都是post一般的url请求都是get
    req.setCharacterEncoding("utf-8");
    // 定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件
    resp.setContentType("text/html;charset=utf-8");
    // 验证用户名密码
    String username = req.getParameter("username");
    String password = req.getParameter("password");
    // 调用业务方法处理请求
    User user = dao.checkLogin(username, password);
    if (user == null) {
      // 返回重新登录的页面
      // 将错误信息放到请求作用域中
      req.setAttribute("errorMsg", "用户名密码错误");
      req.getRequestDispatcher("/login.jsp").forward(req, resp);
      //------验证码的验证
      // 获取用户填写的验证码
      String userRandomCode = req.getParameter("userRandomCode");
      // 获取生成的二维码
      Object randomCode = req.getSession().getAttribute("RANDOMCODE_IN_SESSSION");
      // 对比用户输入的验证码和生成的验证码是否匹配
      if (!StringUtil.hasLength(userRandomCode) || !userRandomCode.equals(randomCode)) {
        // 返回登录界面
        req.setAttribute("errorMsg", "验证码不能为空或者验证码错误或者验证码无效");
        resp.getRequestDispatcher("/login.jsp").forward(req, resp);
        return;
      }
      //-----
      // 跳转到登录后的页面
      // 登录成功以后将用户信息放入到session中
      req.getSession().setAttribute("USER_IN_SESSION", user);
      resp.sendRedirect(req.getContextPath() + "/product");
    }
  }
}
```


