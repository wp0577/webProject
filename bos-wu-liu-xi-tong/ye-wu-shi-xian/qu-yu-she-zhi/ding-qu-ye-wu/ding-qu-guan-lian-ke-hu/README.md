# 定区关联客户

WebService详解

{% page-ref page="../../../../webservice/" %}

## 在BOS项目中配置代理对象远程调用crm

### 第一步：在BOS项目的pom.xml中引入CXF的依赖

```text
<dependency>
			<groupId>org.apache.cxf</groupId>
			<artifactId>cxf-rt-frontend-jaxws</artifactId>
			<version>3.0.1</version>
		</dependency>
		<dependency>
			<groupId>org.apache.cxf</groupId>
			<artifactId>cxf-rt-transports-http</artifactId>
			<version>3.0.1</version>
		</dependency>

```

### 第二步：使用wsimport命令解析wsdl文件生成本地代码，只需要接口文件和实体类

![](../../../../../.gitbook/assets/image%20%28104%29.png)

{% hint style="info" %}
并在ICustomerService中加上@Service注解
{% endhint %}

### 第三步：在spring配置文件中注册crm客户端代理对象

```text
<!-- 注册crm客户端代理对象 -->
	<jaxws:client id="crmClient" 
		serviceClass="com.itheima.crm.ICustomerService" 
		address="http://192.168.115.89:8080/crm_heima32/service/customer"/>

```

### 第四步：通过注解方式将代理对象注入给Action

![](../../../../../.gitbook/assets/image%20%28224%29.png)

