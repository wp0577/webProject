# 基于jdk1.7发布一个WebService服务

```text
3.1	服务端发布
第一步：创建一个Java项目
第二步：创建一个类，加入Webservice注解
第三步：提供一个方法sayHello
第四步：在main方法中调用jdk提供的发布服务的方法
第五步：访问服务的wsdl文档（服务的发布地址+?wsdl）http://192.168.115.87:8080/hello?wsdl
@WebService
public class HelloService {
	public String sayHello(String name,int i){
		System.out.println("服务端的sayHello方法被调用了。。。。");
		return "helle" + name;
	}
	
	public static void main(String[] args) {
		String address = "http://192.168.115.87:8080/hello";
		Object implementor = new HelloService();
		Endpoint.publish(address, implementor);
	}
}

3.2	客户端调用
3.2.1	jdk中wsimport命令使用
作用：解析wsdl文件，生成客户端本地代码

```

![](../../.gitbook/assets/image%20%2896%29.png)

```text
3.2.2	客户端调用
 1、使用wsimport命令解析wsdl文件生成本地代码
 2、通过本地代码创建一个代理对象
 3、通过代理对象实现远程调用

/**
 * 1、使用wsimport命令解析wsdl文件生成本地代码
 * 2、通过本地代码创建一个代理对象
 * 3、通过代理对象实现远程调用
 * @author zhaoqx
 *
 */
public class App {
	public static void main(String[] args) {
		HelloServiceService ss = new HelloServiceService();
		//创建客户端代理对象，用于远程调用
		HelloService proxy = ss.getHelloServicePort();
		String ret = proxy.sayHello("小明", 10);
		System.out.println(ret);
	}
}

```

