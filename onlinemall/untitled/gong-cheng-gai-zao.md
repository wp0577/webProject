# 工程改造

## 1.1.1.   拆分工程

1）将表现层工程独立出来：

e3-manager-web

2）将原来的e3-manager改为如下结构

e3-manager

   \|--e3-manager-dao

   \|--e3-manager-interface

   \|--e3-manager-pojo

   \|--e3-manager-service（打包方式改为war）

## 1.1.2.  服务层工程

第一步：把e3-manager的pom文件中删除e3-manager-web模块。

第二步：把e3-manager-web文件夹移动到e3-manager同一级目录。

第三步：e3-manager-service的pom文件修改打包方式

&lt;packaging&gt;war&lt;/packaging&gt;

第四步：在e3-manager-service工程中添加web.xml文件

第五步：把e3-manager-web的配置文件复制到e3-manager-service中。

删除springmvc.xml

第六步：web.xml 中只配置spring容器。删除前端控制器

第七步：发布服务

1、在e3-manager-Service工程中添加dubbo依赖的jar包。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- dubbo相关 -->
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>com.alibaba</groupId>
        </p>
        <p>
          <artifactId>dubbo</artifactId>
        </p>
        <p>
          <exclusions>
        </p>
        <p>
          <exclusion>
        </p>
        <p>
          <groupId>org.springframework</groupId>
        </p>
        <p>
          <artifactId>spring</artifactId>
        </p>
        <p>
          </exclusion>
        </p>
        <p>
          <exclusion>
        </p>
        <p>
          <groupId>org.jboss.netty</groupId>
        </p>
        <p>
          <artifactId>netty</artifactId>
        </p>
        <p>
          </exclusion>
        </p>
        <p>
          </exclusions>
        </p>
        <p>
          </dependency>
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>org.apache.zookeeper</groupId>
        </p>
        <p>
          <artifactId>zookeeper</artifactId>
        </p>
        <p>
          </dependency>
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>com.github.sgroschupf</groupId>
        </p>
        <p>
          <artifactId>zkclient</artifactId>
        </p>
        <p>
          </dependency>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>2、在spring的配置文件中添加dubbo的约束，然后使用dubbo:service发布服务。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"</p>
            <p>xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <context:component-scan base-package="cn.e3mall.service"></context:component-scan>
            </p>
            <p>
              <!-- 使用dubbo发布服务 -->
            </p>
            <p>
              <!-- 提供方应用信息，用于计算依赖关系 -->
            </p>
            <p>
              <dubbo:application name="e3-manager" />
            </p>
            <p>
              <dubbo:registry protocol="zookeeper" </p>
                <p>address="192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183"
                  /></p>
                <p>
                  <!-- 用dubbo协议在20880端口暴露服务 -->
                </p>
                <p>
                  <dubbo:protocol name="dubbo" port="20880" />
                </p>
                <p>
                  <!-- 声明需要暴露的服务接口 -->
                </p>
                <p>
                  <dubbo:service interface="cn.e3mall.service.ItemService" ref="itemServiceImpl"
                  />
                </p>
                <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>## 1.1.3.   表现层工程

改造e3-manager-web工程。

第一步：删除mybatis、和spring的配置文件。只保留springmvc.xml

第二步：修改e3-manager-web的pom文件，

1、修改parent为e3-parent

2、添加spring和springmvc的jar包的依赖

3、删除e3-mangager-service的依赖

4、添加dubbo的依赖

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- dubbo相关 -->
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>com.alibaba</groupId>
        </p>
        <p>
          <artifactId>dubbo</artifactId>
        </p>
        <p>
          <exclusions>
        </p>
        <p>
          <exclusion>
        </p>
        <p>
          <groupId>org.springframework</groupId>
        </p>
        <p>
          <artifactId>spring</artifactId>
        </p>
        <p>
          </exclusion>
        </p>
        <p>
          <exclusion>
        </p>
        <p>
          <groupId>org.jboss.netty</groupId>
        </p>
        <p>
          <artifactId>netty</artifactId>
        </p>
        <p>
          </exclusion>
        </p>
        <p>
          </exclusions>
        </p>
        <p>
          </dependency>
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>org.apache.zookeeper</groupId>
        </p>
        <p>
          <artifactId>zookeeper</artifactId>
        </p>
        <p>
          </dependency>
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>com.github.sgroschupf</groupId>
        </p>
        <p>
          <artifactId>zkclient</artifactId>
        </p>
        <p>
          </dependency>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>5、e3-mangager-web添加对e3-manager-Interface的依赖。

第三步：修改springmvc.xml，在springmvc的配置文件中添加服务的引用。

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
            <p>xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"</p>
            <p>xmlns:mvc="http://www.springframework.org/schema/mvc"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd</p>
            <p>http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd"></p>
            <p>
              <context:component-scan base-package="cn.e3mall.controller" />
            </p>
            <p>
              <mvc:annotation-driven />
            </p>
            <p>
              <bean</p>
                <p>class="org.springframework.web.servlet.view.InternalResourceViewResolver"></p>
                <p>
                  <property name="prefix" value="/WEB-INF/jsp/" />
                </p>
                <p>
                  <property name="suffix" value=".jsp" />
                </p>
                <p>
                  </bean>
                </p>
                <p>
                  <!-- 引用dubbo服务 -->
                </p>
                <p>
                  <dubbo:application name="e3-manager-web" />
                </p>
                <p>
                  <dubbo:registry protocol="zookeeper" address="192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183"
                  />
                </p>
                <p>
                  <dubbo:reference interface="cn.e3mall.service.ItemService" id="itemService"
                  />
                </p>
                <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：在e3-manager-web工程中添加tomcat插件配置。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <build>
        </p>
        <p>
          <plugins>
        </p>
        <p>
          <!-- 配置Tomcat插件 -->
        </p>
        <p>
          <plugin>
        </p>
        <p>
          <groupId>org.apache.tomcat.maven</groupId>
        </p>
        <p>
          <artifactId>tomcat7-maven-plugin</artifactId>
        </p>
        <p>
          <configuration>
        </p>
        <p>
          <path>/</path>
        </p>
        <p>
          <port>8081</port>
        </p>
        <p>
          </configuration>
        </p>
        <p>
          </plugin>
        </p>
        <p>
          </plugins>
        </p>
        <p>
          </build>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>