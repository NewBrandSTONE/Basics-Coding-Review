# Upload&Download&VCode&Longin/out

##`Login` & `Logout`

###`Login`

>简单的写了个登录页面


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
		<input type="text" name="username" placeholder="用户名" /> <input
			type="password" name="password" placeholder="密码" /> <input
			type="submit" value="登录" /><br /> 验证码<input type="text"
			name="userRandomCode" /><img id="code" alt="验证码"
			src="${pageContext.request.contextPath }/randomCode"
			onclick="random();" />
	</form>

</body>

</html>
```

