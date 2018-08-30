# 用户登陆拦截校验

## 编写拦截器类

```text
public class BOSLoginInterceptor extends MethodFilterInterceptor{
	//拦截方法
	protected String doIntercept(ActionInvocation invocation) throws Exception {
		//从session中获取用户对象
		User user = BOSUtils.getLoginUser();
		if(user == null){
			//没有登录，跳转到登录页面
			return "login";
		}
		//放行
		return invocation.invoke();
	}
}

```

## 注册拦截器

{% code-tabs %}
{% code-tabs-item title="struts.xml" %}
```text
<interceptors>
			<!-- 注册自定义拦截器 -->
			<interceptor name="bosLoginInterceptor" class="com.itheima.bos.web.interceptor.BOSLoginInterceptor">
				<!-- 指定哪些方法不需要拦截 -->
				<param name="excludeMethods">login</param>
			</interceptor>
			<!-- 定义拦截器栈 -->
			<interceptor-stack name="myStack">
				<interceptor-ref name="bosLoginInterceptor"></interceptor-ref>
				<interceptor-ref name="defaultStack"></interceptor-ref>
			</interceptor-stack>
		</interceptors>
		<default-interceptor-ref name="myStack"/>
		
		<!-- 全局结果集定义 -->
		<global-results>
			<result name="login">/login.jsp</result>
		</global-results>


```
{% endcode-tabs-item %}
{% endcode-tabs %}

