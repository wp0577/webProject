# 语法

#### 1.1.1.                  访问map中的key

${key}

#### 1.1.2.                  访问pojo中的属性

Student对象。学号、姓名、年龄

${key.property}

![](../../../.gitbook/assets/image%20%28136%29.png)

#### 1.1.3.                  取集合中的数据

&lt;\#list studentList as student&gt;

${student.id}/${studnet.name}

&lt;/\#list&gt;

![](../../../.gitbook/assets/image%20%28207%29.png)

![](../../../.gitbook/assets/image%20%28210%29.png)

#### 1.1.4.                  取循环中的下标

&lt;\#list studentList as student&gt;

            ${student\_index}

&lt;/\#list&gt;

![](../../../.gitbook/assets/image%20%284%29.png)

#### 1.1.5.                  判断

&lt;\#if student\_index % 2 == 0&gt;

&lt;\#else&gt;

&lt;/\#if&gt;

![](../../../.gitbook/assets/image%20%28228%29.png)

#### 1.1.6.                  日期类型格式化

![](../../../.gitbook/assets/image%20%28260%29.png)

#### 1.1.7.                  Null值的处理

![](../../../.gitbook/assets/image%20%2881%29.png)

#### 1.1.8.                  Include标签

&lt;\#include “模板名称”&gt;

![](../../../.gitbook/assets/image%20%28250%29.png)

### 1.2. Freemarker整合spring

引入jar包：

Freemarker的jar包

![](../../../.gitbook/assets/image%20%2899%29.png)

#### 1.2.1.                  创建整合spring的配置文件

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:context="http://www.springframework.org/schema/context"</p>
            <p>xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xmlns:mvc="http://www.springframework.org/schema/mvc"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd</p>
            <p>http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd</p>
            <p>http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"></p>
            <p>
              <bean id="freemarkerConfig" </p>
                <p>class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer"></p>
                <p>
                  <property name="templateLoaderPath" value="/WEB-INF/ftl/" />
                </p>
                <p>
                  <property name="defaultEncoding" value="UTF-8" />
                </p>
                <p>
              </bean>
              </p>
              <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>需要编写一Controller进行测试

#### 1.2.2.                  Controller

请求的url：/genhtml

参数：无

返回值：ok （String， 需要使用@ResponseBody）

业务逻辑：

1、从spring容器中获得FreeMarkerConfigurer对象。

2、从FreeMarkerConfigurer对象中获得Configuration对象。

3、使用Configuration对象获得Template对象。

4、创建数据集

5、创建输出文件的Writer对象。

6、调用模板对象的process方法，生成文件。

7、关闭流。

加载配置文件：

![](../../../.gitbook/assets/image%20%28219%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> HtmlGenController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> FreeMarkerConfigurer freeMarkerConfigurer;</p>
        <p>@RequestMapping("/genhtml")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> String genHtml()<b>throws</b> Exception {</p>
        <p>// 1、从spring容器中获得FreeMarkerConfigurer对象。</p>
        <p>// 2、从FreeMarkerConfigurer对象中获得Configuration对象。</p>
        <p>Configuration configuration = freeMarkerConfigurer.getConfiguration();</p>
        <p>// 3、使用Configuration对象获得Template对象。</p>
        <p>Template template = configuration.getTemplate("hello.ftl");</p>
        <p>// 4、创建数据集</p>
        <p>Map dataModel = <b>new</b> HashMap
          <>();</p>
        <p>dataModel.put("hello", "1000");</p>
        <p>// 5、创建输出文件的Writer对象。</p>
        <p>Writer out = <b>new</b> FileWriter(<b>new</b> File("D:/temp/term197/out/spring-freemarker.html"));</p>
        <p>// 6、调用模板对象的process方法，生成文件。</p>
        <p>template.process(dataModel, out);</p>
        <p>// 7、关闭流。</p>
        <p>out.close();</p>
        <p> <b>return</b> "OK";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 商品详情页面静态化

#### 1.3.1.                  网页的静态化方案

输出文件的名称：商品id+“.html”

输出文件的路径：工程外部的任意目录。

网页访问：使用nginx访问网页。在此方案下tomcat只有一个作用就是生成静态页面。

工程部署：可以把e3-item-web部署到多个服务器上。

生成静态页面的时机：商品添加后，生成静态页面。可以使用Activemq，订阅topic（商品添加）

![](../../../.gitbook/assets/image%20%2892%29.png)

