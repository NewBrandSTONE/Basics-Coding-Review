# Log4j&Filter&Listener

##日志管理

* 背景

>日志记录是应用程序运行中必不可少的一部分。具有良好格式和完备信息的日志记录可以在程序出现问题时帮助开发人员迅速地定位错误的根源

* `Log4J`的使用方法
  * 导入包`log4j-1.2.17.jar`
  * 在资源文件下面创建一个`log4j.properties`
  * 使用`Java`代码实现日志的输出


* 日志的级别

>`Log4J`建议只使用四个级别，优先级从高到低分别是`ERROR`、`WARN`、`INFO`、`DEBUG`

* 示例代码

```java
private static Logger logger = Logger.getLogger(ProductServlet.class)

protected void delete(HttpServletRequest req, HttpServletResponse resp) throws Exception {
  logger.debug("This is debug message.");
  logger.error("This is error message.");
  logger.info("This is info message.");
}
```

* log4j.properties(必须要确保文件被放到classes目录下)

```properties
### \u8BBE\u7F6E###
log4j.rootLogger = debug,stdout,D,E

### \u8F93\u51FA\u4FE1\u606F\u5230\u63A7\u5236\u62AC ###
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n

### \u8F93\u51FADEBUG \u7EA7\u522B\u4EE5\u4E0A\u7684\u65E5\u5FD7\u5230=E://logs/error.log ###
log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
#log4j.appender.D.File = /logs/log.log
log4j.appender.D.Append = false
log4j.appender.D.Threshold = INFO 
log4j.appender.D.layout = org.apache.log4j.HTMLLayout
log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n

### \u8F93\u51FAERROR \u7EA7\u522B\u4EE5\u4E0A\u7684\u65E5\u5FD7\u5230=E://logs/error.log ###
log4j.appender.E = org.apache.log4j.DailyRollingFileAppender
#log4j.appender.E.File =/logs/error.log 
log4j.appender.E.Append = false
log4j.appender.E.Threshold = ERROR 
log4j.appender.E.layout = org.apache.log4j.PatternLayout
log4j.appender.E.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]
```






##过滤器概述

* `WEB中的过滤器`

>WEB组件，在Java中最小的组件就是一个类（双向）

* 特点

>开发人员可以实现用户在访问某个目标资源之前，对访问的***请求和响应***进行拦截。
>
>简单说，就是可以实现`Web容器`对某个资源的访问前截获进行相关的处理，
>
>还可以在某资源向`Web容器`返回响应前进行截获处理

* `Web`中过滤器存在的意义
  * `DRY`不要重复自己
  * 责任分类:各自做自己擅长的事儿

##过滤器操作和应用场景

>实现`Web`容器对某资源的访问前截获进行相关处理，还可以在某资源向`Web`容器返回响应前进行截获处理

* 过滤器能够实现的功能
  * 以常规的方式调用资源（访问`servlet`或`JSP`页面）
  * 利用修改过的请求信息调用资源
  * 利用资源，但在发送响应到浏览器之前对其进行修改
  * `阻止该资源调用`，取而代之的是跳转到其他资源，返回一个特定的状态码


* 应用场景
  * 可以对客户提交的数据进行重新编码`req.setCharacterEndocing("UTF-8");`
  * 使用浏览器不缓存页面的过滤器
  * 可以过滤掉客户的非法文字
  * 验证客户是否已经登录
  * 请求分发器(类似`structs2`)
  * 页面伪静态化处理(.jsp结尾的变成.html结尾)

### `Filter`开发和使用

* `Filter`的开发步骤
  * 创建一个类，实现`Filter`接口
  * 配置`Filter`(告诉`Tomcat`来管理`Filter`)

```xml
<filter>
  <filter-name>HelloFilter</filter-name>
  <filter-class>com.filter.HelloFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>HelloFilter</filter-name>
  <!-- 资源名称，指定要对那些资源进行过滤 -->
  <url-pattern>/hello</url-pattern>
</filter-mapping>
```

### `Filter`的细节处理
* 可以使用注解来配置`Filter`


>但是使用注解来配置过滤器，就无法控制哪个过滤器先执行

* `FilterChain`和`FilterConfig`

```java
String getFilterName() // 获取<filter-name>中的内容
String getInitParameter(String name) // 获取Filter中的初始化参数,解决硬编码
Enumeration getInitParameterNames() 
ServletContext getServletContext() // 获取当前应用对象
```

* `FilterCHain:过滤器链`

>注意:
>
>对***请求***的拦截操作应该在***放行之前***来实现
>
>对***响应***的拦截操作应该在***放行之后***来实现

###请求编码过滤器

* 示例代码

```java
// 对请求参数的编码进行设置
public class CharacterEncodeingFilter implements Filter {
  // 编码
  private String encoding;
  // 判断是否要强制使用web.xml中的编码配置
  private boolean force;
  
  public void init(FilterConfig filterConfig) throws ServletException {
    encoding = filterConfig.getInitParameter("encoding");
    force = Boolean.valueOf(filterConfig.getInitParameter("force"));
  }
  
  protected void doFilter(ServletRequest request, ServletResponse response) throws IOException, ServletException {
    // 设置请求参数的编码方式
    // 通常在程序中需要给编码一个默认值，在绝大多数情况下，我们不对其进行配置
    if (request.getCharacterEncoding() == null || force && StringUtils.hasLength(encoding)) {
      request.setCharacterEncoding(encoding);
    } else {
      // 设置默认码
      request.setCharacterEncoding("UTF-8");
    }
    chain.doFilter(request, response);
  }
}
```

###登录验证过滤器`CheckLoginFilter`

* 示例代码

```java
public class LoginCheckFilter implements Filter {
  private String[] uris;
  private String contextPath;

  /**
   * Default constructor.
   */
  public LoginCheckFilter() {
      // TODO Auto-generated constructor stub
  }

  /**
   * @see Filter#destroy()
   */
  public void destroy() {
      // TODO Auto-generated method stub
  }

  /**
   * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
   */
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
          throws IOException, ServletException {
      HttpServletRequest req = (HttpServletRequest) request;
      HttpServletResponse resp = (HttpServletResponse) response;

      // String uri = req.getRequestURI().substring(1);
      String uri = req.getRequestURI();

      if (Arrays.asList(uris).contains(uri)) {
          chain.doFilter(req, resp);
      } else {
          Object user = req.getSession().getAttribute("USER_IN_SESSION");
          if (user == null) {
              // 说明没有登录
              resp.sendRedirect(req.getContextPath() + "/login.jsp");
              return;
          }
          chain.doFilter(req, resp);
      }
  }

  /**
   * @see Filter#init(FilterConfig)
   */
  public void init(FilterConfig fConfig) throws ServletException {
      // 获取不需要验证的资源名称
      // login.jsp,login,randomCode
      String notCheck = fConfig.getInitParameter("NOTCHECK");
      contextPath = fConfig.getInitParameter("contextPath");
      uris = notCheck.split(",");
      for (int i = 0; i < uris.length; i++) {
          uris[i] = contextPath + "/" + uris[i];
      }
  }
}
```

