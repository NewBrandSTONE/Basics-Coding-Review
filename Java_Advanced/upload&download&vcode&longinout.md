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

###Logout

* LogoutServlet.java

```java
 protected void service(HttpServletRequest req, HttpServletResponse resp) throws Exception {
   // 销毁session对象
   req.getSession().invalidate();
   resp.sendRedirect(req.getContextPath() + "/login.jsp");
 }
```

##文件上传
---

* 准备工作
  * 创建一个包含上传控件的表单
    * 表单的提交方式必须是`post`，因为`get`方式的只允许`1kb`的数据
    * 文件上传的`enctype`必须是`multipart/form-data`，不对表中的数据编码，按照二进制形式传输，此时表单的数据就不能通过`request.getParameter(String name)`来获取
    * 表单上必须有一个上传控件


* 上传表单示例

```jsp
<form action="/upload" method="post" enctype="multipart/form-data">
  姓名：<input name="name" />
  <!-- 上传控件 -->
  头像：<input type="file" name="headImg" />
  <input type="submit" value="提交">
</form>
```

* 如何实现文件上传
  * 导包`commons-fileupload.jar`和`commons-io.jar`


* 注意

> 表单中的控件对应着`API`中的`FileItem`

* 示例代码

```java
// 验证表单是否满足文件上传的条件（提交的方式为post，enctype是否以multipart/开头）
boolean isMultipart = ServletFileUpload.isMultipartContent(req);
if (!isMultipart) {
  // 不满足条件就什么都不做
  return;
}
// 创建一个FileItem的工厂类
FileItemFactory factory = new DiskFileItemFactory();
// 创建一个文件上传的处理器(装饰模式)
ServletFileUpload upload = new ServletFileUpload(factory);
// 解析请求，将页面中的表单控件解析成FileItem
// FileItem:对应着表单中的一个控件<input name="name" />
List<FileItem> items = upload.parseRequest(req); 
// 解析请求，将页面中的表单控件解析成FileItem
List<FileItem> items = upload.parseRequest(req);
for (FileItem fileItem : items) {
  // 获取控件的名称
  System.out.println(fileItem.getFileName());
  // 判断控件是普通控件还是上传控件
  if (fileItem.isFormField()) {
    // 普通控件
    // 获取控件的值
    System.out.println(fileItem.getString());
    // 解决乱码
    System.out.println(fileItem.getString("UTF-8"));
  } else {
    // 上传控件
    // 文件的MIME类型
    System.out.println(fileItem.getContentType());
    // 文件的大小
    System.out.println(fileItem.getSize());
    // 文件的名称
    System.out.println(fileItem.getName());
    File f = new File("C://", fileItem.getName());
    try {
      // 将文件写到服务端的磁盘
      fileItem.write(f);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }
}
```

##文件上传的细节处理

* 文件名处理

> 使用UUID来生成随机数作为文件的文件名

```java
// 生成文件名
String fileName = UUID.randomUUID().toString();
// 获取文件的后缀名
String extension = FilenameUtils.getExtension(fileItem.getName());
File f = new File("C:/", fileName + "," + extension);
```

* 注意

> 在实际开发中，将上传的文件保存到项目的一个目录下

```java
// 使用相对路径
String realPath = req.getServletContext().getRealPath("/upload/files");
```

###缓存大小和临时目录

* 缓存大小

> 文件上传的功能通常需要约束文件的大小，目的是防止用户随意上传文件。

```java
// 约束一个上传文件的大小
upload.setFileSizeMax(fileSizeMax);

// 约束一次完整请求中上传文件的总大小
upload.setSizeMax(siezeMax);
```

* 临时目录

> 将比较大的文件存储到一个临时目录中，小文件任然可以读取到内存中

```java
// 创建一个FileItem工厂类
DiskFileItemFactory factory = new DiskFileItemFactory();
// 设置上传文件缓冲大小，超过缓存大小就会被保存到临时目录中
factory.setSizeThreshold(50 * 1024);
// 设置临时目录的位置，默认在服务器中的/temp
factory.setRepository(new File("c:/temp"));
```

##文件下载
---

* 设置响应报头`contentType: application/x-msdownload`

> 如果不设置响应报头，有的浏览器IE6，可能会直接将需要下载的内容按照二进制的文件输出出来
 
* Content-Disposition:attachment

> 由用户选择将相应的实体内容保存到一个文件中
>
> response.setHeader("Content-Disposistion", "attachment;filename=建议保存名称")

* 下载文件乱码问题

```java
// IE
response.setHeader("Content-Disposition","attachment;filename=" + new String(filename.getByes("UTF-8"),"ISO8859-1"));
// 非IE
response.setHeader("Content-Disposition","attachement;filename="+URLEncoder.encode(filename,"utf-8"));
```

* 示例代码

```java
@WebServlet("/download")
public class DownloadServlet extends HttpServlet {
  private static final long serialVersionUID = 1L;
  
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws Exception {
    String filename = req.getParameter("filename");
    String download = req.getServletContext().getRealPath("/WEB-INF/download");
    String path = download + "\\" + filename;
    // 设置输出的MIME类型
    resp.setContentType("application/x-msdownload");
    // 获取字节输出流
    ServletOutputStream out = resp.getOutputStream();
    // 在非IE中设置文件名的方式
    String userAgent = req.getHeader("User-Agent");
    if (userAgent.contains("MSIE")) {
      // IE
      filename = URLEncoder.encode(filename, "UTF-8");
    } else {
      // 非IE
      filename = new String(filename.getBytes("UTF-8"), "ISO-8859-1");
    }
    // 设置响应头信息
    resp.setHeader("Content-Disposition", "attachment; filename=" + filename);
    Files.copy(Paths.get(path), out);
  }
}
```










