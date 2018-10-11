# Maven搭建

## json-lib-2.4-jdk15.jar can't be find?

1. 发现json-lib-2.4后面多了个“jdk15”，搜索后发现，这个json-lib包对于不同的jdk版本有不同的实现，在引入pom.xml的时候，需要通过增加classifier来指定的jdk版本：

```text
<!-- https://mvnrepository.com/artifact/net.sf.json-lib/json-lib -->
<dependency>
	<groupId>net.sf.json-lib</groupId>
	<artifactId>json-lib</artifactId>
	<version>2.4</version>
	<classifier>jdk15</classifier>
</dependency>
```

   2. 上述方面并未成功，后来直接下载json-lib-2.4-jdk15.jar包到对应的maven仓库中，成功。

## 使用maven

maven是依赖管理和项目构建的工具，下图是分层结构

![](../../../.gitbook/assets/image%20%28260%29.png)

## IDEA创建父子模块方法：

本文通过一个例子来介绍利用maven来构建一个多模块的jave项目。开发工具：intellij idea。

## 一、项目结构

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021214045958-176796341.png)

multi-module-project是主工程，里面包含两个模块（Module）：

1. web-app是应用层，用于界面展示，依赖于web-service参的服务。
2. web-service层是服务层，用于给app层提供服务。

## 二、构建项目

### 2.1 Parent Project

新建一个空白标准maven project（不要选择Create from archetype选项）

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215418411-426885337.png)

填写项目坐标

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215616708-301786239.png)

得到一个标准的maven项目，因为该项目是作为一个Parent project存在的，可以直接删除src文件夹。

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215625692-910961834.png)

### 2.2 增加web-app模块（Module）

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215840989-371606679.png)

选择从archetype创建（选择webapp选项）

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215637161-821333201.png)

groupId和version继承自Parent project，这里只需要填写artifactId即可。

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215648255-60499131.png)

### 2.3增加web-service模块

用同样的方法创建web-service模块（不过该模块是一个空白maven标准项目，不要从archetype创建）

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021215704395-178501147.png)

### 2.4 得最终项目结构

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021220937817-1710662397.png)

### 2.5 关键几点

1. Parent project和各个Module拥有独立pom文件，他们之间的关系后续会专门写文章介绍。
2. Parent project用于组织不同的Module，不实现逻辑
3. Module集成Parent project的groupId和version，Module只需要指定自己的artifactId即可。

## 三、添加项目依赖

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021223626911-880704253.png)

上面的操作是添加web-app对web-service模块的依赖，完成上述操作后web-Service中public的类已经在web-app模块中可见了。但是这个时候在app模块使用了service模块中的类，通过maven进行编译（compile）的时候还是会报错XX找不到（XX为service模块的类），要解决这个问题需要在app的pom中增加对service的依赖：

```text
   <dependency>
            <groupId>com.cnblogs.kmpp</groupId>
            <artifactId>web-service</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
```

以上，项目依赖的添加已经完成

### 四、开始编程

### 4.1 web-service模块编程

在web-service模块中增加一个Service类（SimpleService.java\)

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021222011395-1985582106.png)

### 4.2 web-app模块编程

 在web-app模块增加一个Servlet，并且调用web-service模块的SimpleService类的getServiceDescription方法。

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021224515364-1631467553.png)

### 4.3 配置Servlet

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>

    <servlet>
        <servlet-name>Simple</servlet-name>
        <servlet-class>com.cnblogs.kmpp.SimpleServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Simple</servlet-name>
        <url-pattern>/simple-servlet</url-pattern>
    </servlet-mapping>
</web-app>
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

## 五、运行

在web-app的pom中增加j2ee的依赖，以及jetty插件的依赖。运行jetty。[详情](http://www.cnblogs.com/kmpp/p/create_maven_web_app_via_intellij_idea.html)

在浏览器中输入：http://localhost:8080/web-app/simple-servlet

得到运行结果：

![](https://images2015.cnblogs.com/blog/737467/201510/737467-20151021224800208-875244583.png)

## 六、结束 

至此，本文完整演示了使用maven构建多模块项目。

