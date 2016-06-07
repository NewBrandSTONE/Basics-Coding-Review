# Servlet细节&Cookie&Session
##Servlet的细节问题
* 一个`Servlet`可以有多个映射`<url-pattern>`。
* `Servlet`的`<url-pattern>`可以使用通配符`*`进行配置
  * `/*`表示可以使用任意字符访问当前的`Servlet`
  * `/system/*`可以使用以`/system`为前缀，后面是任意字符即可访问当前的`Servlet`
  * `*.xxx`任何字符，加上指定的后缀名即可访问当前的`Servlet`
* `Servlet`的`<servlet-name>`不能为`default`，如果为`default`那么当前项目中的静态资源将无法被访问到。

