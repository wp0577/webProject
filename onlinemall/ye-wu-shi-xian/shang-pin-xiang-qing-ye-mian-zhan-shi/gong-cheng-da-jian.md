# 工程搭建

创建一个商品详情页面展示的工程。是一个表现层工程。

### 1.1. 工程搭建

e3-item-web。打包方式war。可以参考e3-portal-web

#### 1.1.1.                  Pom文件

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          </p>
            <p>xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"></p>
            <p>
              <modelVersion>4.0.0</modelVersion>
            </p>
            <p>
              <parent>
            </p>
            <p>
              <groupId>cn.e3mall</groupId>
            </p>
            <p>
              <artifactId>e3-parent</artifactId>
            </p>
            <p>
              <version>0.0.1-SNAPSHOT</version>
            </p>
            <p>
              </parent>
            </p>
            <p>
              <groupId>cn.e3mall</groupId>
            </p>
            <p>
              <artifactId>e3-item-web</artifactId>
            </p>
            <p>
              <version>0.0.1-SNAPSHOT</version>
            </p>
            <p>
              <packaging>war</packaging>
            </p>
            <p>
              <dependencies>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>cn.e3mall</groupId>
            </p>
            <p>
              <artifactId>e3-manager-interface</artifactId>
            </p>
            <p>
              <version>0.0.1-SNAPSHOT</version>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <!-- Spring -->
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-context</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-beans</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-webmvc</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-jdbc</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-aspects</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-jms</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>org.springframework</groupId>
            </p>
            <p>
              <artifactId>spring-context-support</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <!-- JSP相关 -->
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>jstl</groupId>
            </p>
            <p>
              <artifactId>jstl</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>javax.servlet</groupId>
            </p>
            <p>
              <artifactId>servlet-api</artifactId>
            </p>
            <p>
              <scope>provided</scope>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              <dependency>
            </p>
            <p>
              <groupId>javax.servlet</groupId>
            </p>
            <p>
              <artifactId>jsp-api</artifactId>
            </p>
            <p>
              <scope>provided</scope>
            </p>
            <p>
              </dependency>
            </p>
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
              <!-- 排除依赖 -->
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
            <p>
              <dependency>
            </p>
            <p>
              <groupId>junit</groupId>
            </p>
            <p>
              <artifactId>junit</artifactId>
            </p>
            <p>
              </dependency>
            </p>
            <p>
              </dependencies>
            </p>
            <p>
              <!-- 配置tomcat插件 -->
            </p>
            <p>
              <build>
            </p>
            <p>
              <plugins>
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
              <port>8086</port>
            </p>
            <p>
              <path>/</path>
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
            <p>
          </project>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 功能分析

在搜索结果页面点击商品图片或者商品标题，展示商品详情页面。

![](../../../.gitbook/assets/image%20%28181%29.png)

请求的url：/item/{itemId}

参数：商品id

返回值：String 逻辑视图

业务逻辑：

1、从url中取参数，商品id

2、根据商品id查询商品信息\(tb\_item\)得到一个TbItem对象，缺少images属性，可以创建一个pojo继承TbItem，添加一个getImages方法。在e3-item-web工程中。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> Item <b>extends</b> TbItem {</p>
        <p> <b>public</b> String[] getImages() {</p>
        <p>String image2 = <b>this</b>.getImage();</p>
        <p> <b>if</b> (image2 != <b>null</b> && !"".equals(image2)) {</p>
        <p>String[] strings = image2.split(",");</p>
        <p> <b>return</b> strings;</p>
        <p>}</p>
        <p> <b>return</b>  <b>null</b>;</p>
        <p>}</p>
        <p> <b>public</b> Item() {</p>
        <p>}</p>
        <p> <b>public</b> Item(TbItem tbItem) {</p>
        <p> <b>this</b>.setBarcode(tbItem.getBarcode());</p>
        <p> <b>this</b>.setCid(tbItem.getCid());</p>
        <p> <b>this</b>.setCreated(tbItem.getCreated());</p>
        <p> <b>this</b>.setId(tbItem.getId());</p>
        <p> <b>this</b>.setImage(tbItem.getImage());</p>
        <p> <b>this</b>.setNum(tbItem.getNum());</p>
        <p> <b>this</b>.setPrice(tbItem.getPrice());</p>
        <p> <b>this</b>.setSellPoint(tbItem.getSellPoint());</p>
        <p> <b>this</b>.setStatus(tbItem.getStatus());</p>
        <p> <b>this</b>.setTitle(tbItem.getTitle());</p>
        <p> <b>this</b>.setUpdated(tbItem.getUpdated());</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>3、根据商品id查询商品描述。

4、展示到页面。

