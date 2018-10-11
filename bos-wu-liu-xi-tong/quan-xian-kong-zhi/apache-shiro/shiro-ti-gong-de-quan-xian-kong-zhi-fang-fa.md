# shiro提供的权限控制方法

## 1      使用shiro的方法注解方式权限控制

第一步：在spring配置文件中开启shiro注解支持

&lt;!-- 开启shiro框架注解支持 --&gt;

                  &lt;bean id="defaultAdvisorAutoProxyCreator"

                                    class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"&gt;

                                                      &lt;!-- 必须使用cglib方式为Action对象创建代理对象 --&gt;

                                    &lt;property name="proxyTargetClass" value="true"/&gt;

                  &lt;/bean&gt;

                  &lt;!-- 配置shiro框架提供的切面类，用于创建代理对象 --&gt;

                  &lt;bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor"/&gt;

第二步：在Action的方法上使用shiro注解

![](../../../.gitbook/assets/image%20%2867%29.png)

![](../../../.gitbook/assets/image%20%28115%29.png)

第三步：在struts.xml中配置全局异常捕获，当shiro框架抛出权限不足异常时，跳转到权限不足提示页面

![](../../../.gitbook/assets/image%20%2894%29.png)

## 2      使用shiro提供的页面标签方式权限控制

第一步：在jsp页面中引入shiro的标签库

&lt;%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %&gt;

第二步：使用shiro的标签控制页面元素展示

![](../../../.gitbook/assets/image%20%28112%29.png)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

5 总结shiro框架提供的权限控制方式 

 URL拦截权限控制（基于过滤器实现）

![](../../../.gitbook/assets/image%20%2872%29.png)

 方法注解权限控制（基于代理技术实现）

![](../../../.gitbook/assets/image%20%28155%29.png)

 页面标签权限控制（标签技术实现）

![](../../../.gitbook/assets/image%20%2818%29.png)

 代码级别权限控制（基于代理技术实现）

![](../../../.gitbook/assets/image%20%2823%29.png)

