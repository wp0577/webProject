# Webservice概述

## 什么是WebService 

      Web service是一个平台独立的，低耦合的，自包含的、基于可编程的web的应用程序，可使用开放的XML（标准通用标记语言下的一个子集）标准来描述、发布、发现、协调和配置这些应用程序，用于开发分布式的互操作的应用程序。  
      Web Service技术， 能使得运行在不同机器上的不同应用无须借助附加的、专门的第三方软件或硬件， 就可相互交换数据或集成。依据Web Service规范实施的应用之间， 无论它们所使用的语言、 平台或内部协议是什么， 都可以相互交换数据。Web Service是自描述、 自包含的可用网络模块， 可以执行具体的业务功能。Web Service也很容易部署， 因为它们基于一些常规的产业标准以及已有的一些技术，诸如标准通用标记语言下的子集XML、HTTP。Web Service减少了应用接口的花费。Web Service为整个企业甚至多个组织之间的业务流程的集成提供了一个通用机制。

![](../../.gitbook/assets/image%20%2858%29.png)

l  **WebService的特点**

•    WebService通过HTTP POST方式接受客户的请求

•    WebService与客户端之间一般使用SOAP协议传输XML数据 它本身就是为了跨平台或跨语言而设计的



## SOAP和WSDL概念

### 1.1.1  SOAP（Simple Object Access Protocol）：简单对象访问协议

n  SOAP作为一个基于XML语言的协议用于在网上传输数据。

n  SOAP = 在HTTP的基础上+XML数据。

n  SOAP是基于HTTP的。

u  SOAP的组成如下：

l  Envelope – 必须的部分。以XML的根元素出现。

l  Headers – 可选的。

l  Body – 必须的。在body部分，包含要执行的服务器的方法。和发送到服务器的数据。

示例：

POST /WebServices/IpAddressSearchWebService.asmx HTTP/1.1

Host: ws.webxml.com.cn

Content-Type: text/xml; charset=utf-8

Content-Length: length

SOAPAction: "http://WebXml.com.cn/getCountryCityByIp"

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"&gt;

  &lt;soap:Body&gt;

    &lt;getCountryCityByIp xmlns="http://WebXml.com.cn/"&gt;

      &lt;theIpAddress&gt;string&lt;/theIpAddress&gt;

    &lt;/getCountryCityByIp&gt;

  &lt;/soap:Body&gt;

&lt;/soap:Envelope&gt;

### 1.1.2  WSDL Web服务描述语言

WSDL\(WebService Description Language\):web 服务描述语言

就是一个xml文档，用于描述当前服务的一些信息（服务名称、服务的发布地址、服务提供的方法、方法的参数类型、方法的返回值类型等）





## 基于jdk1.7发布一个WebService服务

### 1.1   服务端发布

第一步：创建一个Java项目

第二步：创建一个类，加入Webservice注解

第三步：提供一个方法sayHello

第四步：在main方法中调用jdk提供的发布服务的方法

第五步：访问服务的wsdl文档（服务的发布地址+?wsdl）http://192.168.115.87:8080/hello?wsdl

@WebService

**public** **class** HelloService {

                  **public** String sayHello\(String name,**int** i\){

                                    System.**out**.println\("服务端的sayHello方法被调用了。。。。"\);

                                    **return** "helle" + name;

                  }

                  **public** **static** **void** main\(String\[\] args\) {

                                    String address = "http://192.168.115.87:8080/hello";

                                    Object implementor = **new** HelloService\(\);

                                    Endpoint.publish\(address, implementor\);

                  }

}

### 1.2   客户端调用

#### 1.2.1  jdk中wsimport命令使用

作用：解析wsdl文件，生成客户端本地代码

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

#### 1.2.2  客户端调用

 1、使用wsimport命令解析wsdl文件生成本地代码

 2、通过本地代码创建一个代理对象

 3、通过代理对象实现远程调用

/\*\*

 \* 1、使用wsimport命令解析wsdl文件生成本地代码

 \* 2、通过本地代码创建一个代理对象

 \* 3、通过代理对象实现远程调用

 \* **@author** zhaoqx

 \*

 \*/

**public** **class** App {

                  **public** **static** **void** main\(String\[\] args\) {

                                    HelloServiceService ss = **new** HelloServiceService\(\);

                                    //创建客户端代理对象，用于远程调用

                                    HelloService proxy = ss.getHelloServicePort\(\);

                                    String ret = proxy.sayHello\("小明", 10\);

                                    System.**out**.println\(ret\);

                  }

}

