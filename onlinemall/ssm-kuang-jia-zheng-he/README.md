# SSM框架整合

### 1.1. 数据库

数据库使用mysql数据库，要求5.5以上版本。

1、在mysql数据库中创建数据库e3mall

2、将创建数据库的脚本导入到e3mall中。

{% file src="../../.gitbook/assets/e3mall \(1\).sql" caption="mall.sql" %}

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

### 1.2. Mybatis逆向工程

使用mybatis官方提供的mybatis-generator生成pojo、mapper接口及映射文件。

并且将pojo放到e3-manager-pojo工程中。

将mapper接口及映射文件放到e3-manager-dao工程中。

### 1.3. 整合思路

1、Dao层：

Mybatis的配置文件：SqlMapConfig.xml

不需要配置任何内容，需要有文件头。文件必须存在。

applicationContext-dao.xml：

mybatis整合spring，通过由spring创建数据库连接池，spring管理SqlSessionFactory、mapper代理对象。需要mybatis和spring的整合包。

2、Service层：

applicationContext-service.xml：

所有的service实现类都放到spring容器中管理。并由spring管理事务。

3、表现层：

Springmvc框架，由springmvc管理controller。

Springmvc的三大组件。

### 1.4. Dao整合

#### 1.4.1.                  创建SqlMapConfig.xml配置文件

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <!DOCTYPE configuration</p>
          <p>PUBLIC "-//mybatis.org//DTD Config 3.0//EN"</p>
          <p>"http://mybatis.org/dtd/mybatis-3-config.dtd"></p>
          <p>
            <configuration>
          </p>
          <p>
            </configuration>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.4.2.                  Spring整合mybatis

创建applicationContext-dao.xml

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
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <!-- 数据库连接池 -->
            </p>
            <p>
              <!-- 加载配置文件 -->
            </p>
            <p>
              <context:property-placeholder location="classpath:conf/db.properties"
              />
            </p>
            <p>
              <!-- 数据库连接池 -->
            </p>
            <p>
              <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" </p>
                <p>destroy-method="close"></p>
                <p>
                  <property name="url" value="${jdbc.url}" />
                </p>
                <p>
                  <property name="username" value="${jdbc.username}" />
                </p>
                <p>
                  <property name="password" value="${jdbc.password}" />
                </p>
                <p>
                  <property name="driverClassName" value="${jdbc.driver}" />
                </p>
                <p>
                  <property name="maxActive" value="10" />
                </p>
                <p>
                  <property name="minIdle" value="5" />
                </p>
                <p>
              </bean>
              </p>
              <p>
                <!-- 让spring管理sqlsessionfactory 使用mybatis和spring整合包中的 -->
              </p>
              <p>
                <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
              </p>
              <p>
                <!-- 数据库连接池 -->
              </p>
              <p>
                <property name="dataSource" ref="dataSource" />
              </p>
              <p>
                <!-- 加载mybatis的全局配置文件 -->
              </p>
              <p>
                <property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml"
                />
              </p>
              <p>
                </bean>
              </p>
              <p>
                <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
              </p>
              <p>
                <property name="basePackage" value="cn.e3mall.mapper" />
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
</table>**db.properties**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>jdbc.driver=com.mysql.jdbc.Driver</p>
        <p>jdbc.url=jdbc:mysql://localhost:3306/e3mall?characterEncoding=utf-8</p>
        <p>jdbc.username=root</p>
        <p>jdbc.password=root</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>备注：

Druid是目前最好的数据库连接池，在功能、性能、扩展性方面，都超过其他数据库连接池，包括DBCP、C3P0、BoneCP、Proxool、JBoss DataSource。

Druid已经在阿里巴巴部署了超过600个应用，经过多年多生产环境大规模部署的严苛考验。

### 1.5. Service整合

#### 1.5.1.                  管理Service

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
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <context:component-scan base-package="cn.e3mall.service" />
            </p>
            <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.5.2.                  事务管理

创建applicationContext-trans.xml

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"</p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <!-- 事务管理器 -->
            </p>
            <p>
              <bean id="transactionManager" </p>
                <p>class="org.springframework.jdbc.datasource.DataSourceTransactionManager"></p>
                <p>
                  <!-- 数据源 -->
                </p>
                <p>
                  <property name="dataSource" ref="dataSource" />
                </p>
                <p>
              </bean>
              </p>
              <p>
                <!-- 通知 -->
              </p>
              <p>
                <tx:advice id="txAdvice" transaction-manager="transactionManager">
              </p>
              <p>
                <tx:attributes>
              </p>
              <p>
                <!-- 传播行为 -->
              </p>
              <p>
                <tx:method name="save*" propagation="REQUIRED" />
              </p>
              <p>
                <tx:method name="insert*" propagation="REQUIRED" />
              </p>
              <p>
                <tx:method name="add*" propagation="REQUIRED" />
              </p>
              <p>
                <tx:method name="create*" propagation="REQUIRED" />
              </p>
              <p>
                <tx:method name="delete*" propagation="REQUIRED" />
              </p>
              <p>
                <tx:method name="update*" propagation="REQUIRED" />
              </p>
              <p>
                <tx:method name="find*" propagation="SUPPORTS" read-only="true" />
              </p>
              <p>
                <tx:method name="select*" propagation="SUPPORTS" read-only="true" />
              </p>
              <p>
                <tx:method name="get*" propagation="SUPPORTS" read-only="true" />
              </p>
              <p>
                </tx:attributes>
              </p>
              <p>
                </tx:advice>
              </p>
              <p>
                <!-- 切面 -->
              </p>
              <p>
                <aop:config>
              </p>
              <p>
                <aop:advisor advice-ref="txAdvice" </p>
                  <p>pointcut="execution(* cn.e3mall.service.*.*(..))" /></p>
                  <p>
                    </aop:config>
                  </p>
                  <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.6. 表现层整合

#### 1.6.1.                  Springmvc.xml

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
            <p>xmlns:mvc="http://www.springframework.org/schema/mvc"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd</p>
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
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.6.2.                  Web.xml

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" </p>
            <p>xmlns="http://java.sun.com/xml/ns/javaee"</p>
            <p>xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"</p>
            <p>id="WebApp_ID" version="2.5"></p>
            <p>
              <display-name>e3-manager</display-name>
            </p>
            <p>
              <welcome-file-list>
            </p>
            <p>
              <welcome-file>index.jsp</welcome-file>
            </p>
            <p>
              </welcome-file-list>
            </p>
            <p>
              <!-- 加载spring容器 -->
            </p>
            <p>
              <context-param>
            </p>
            <p>
              <param-name>contextConfigLocation</param-name>
            </p>
            <p>
              <param-value>classpath:spring/applicationContext*.xml</param-value>
            </p>
            <p>
              </context-param>
            </p>
            <p>
              <listener>
            </p>
            <p>
              <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
            </p>
            <p>
              </listener>
            </p>
            <p>
              <!-- 解决post乱码 -->
            </p>
            <p>
              <filter>
            </p>
            <p>
              <filter-name>CharacterEncodingFilter</filter-name>
            </p>
            <p>
              <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
            </p>
            <p>
              <init-param>
            </p>
            <p>
              <param-name>encoding</param-name>
            </p>
            <p>
              <param-value>utf-8</param-value>
            </p>
            <p>
              </init-param>
            </p>
            <p>
              </filter>
            </p>
            <p>
              <filter-mapping>
            </p>
            <p>
              <filter-name>CharacterEncodingFilter</filter-name>
            </p>
            <p>
              <url-pattern>/*</url-pattern>
            </p>
            <p>
              </filter-mapping>
            </p>
            <p>
              <!-- springmvc的前端控制器 -->
            </p>
            <p>
              <servlet>
            </p>
            <p>
              <servlet-name>e3-manager</servlet-name>
            </p>
            <p>
              <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            </p>
            <p>
              <!-- contextConfigLocation不是必须的， 如果不配置contextConfigLocation， springmvc的配置文件默认在：WEB-INF/servlet的name+"-servlet.xml"
              -->
            </p>
            <p>
              <init-param>
            </p>
            <p>
              <param-name>contextConfigLocation</param-name>
            </p>
            <p>
              <param-value>classpath:spring/springmvc.xml</param-value>
            </p>
            <p>
              </init-param>
            </p>
            <p>
              <load-on-startup>1</load-on-startup>
            </p>
            <p>
              </servlet>
            </p>
            <p>
              <servlet-mapping>
            </p>
            <p>
              <servlet-name>e3-manager</servlet-name>
            </p>
            <p>
              <url-pattern>/</url-pattern>
            </p>
            <p>
              </servlet-mapping>
            </p>
            <p>
          </web-app>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.7. 整合测试

#### 1.7.1.                  需求

根据商品id查询商品信息，返回json数据。

#### 1.7.2.                  Dao层

由于是单表查询可以使用逆向工程生成的代码。

#### 1.7.3.                  Service层

参数：商品id

返回值：TbItem

业务逻辑：根据商品id查询商品信息。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 商品管理Service</p>
        <p>*
          <p>Title: ItemServiceImpl</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>*
          <p>Company: www.itcast.cn</p>
        </p>
        <p>* <b>@version</b> 1.0</p>
        <p>*/</p>
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> ItemServiceImpl <b>implements</b> ItemService {</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbItemMapper itemMapper;</p>
        <p>@Override</p>
        <p> <b>public</b> TbItem getItemById(<b>long</b> id) {</p>
        <p>TbItem item = itemMapper.selectByPrimaryKey(id);</p>
        <p> <b>return</b> item;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.7.4.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 商品管理Controller</p>
        <p>*
          <p>Title: ItemController</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>*
          <p>Company: www.itcast.cn</p>
        </p>
        <p>* <b>@version</b> 1.0</p>
        <p>*/</p>
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> ItemController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> ItemService itemService;</p>
        <p>@RequestMapping("/item/{itemId}")</p>
        <p>@ResponseBody</p>
        <p> <b>private</b> TbItem getItemById(@PathVariable Long itemId) {</p>
        <p>TbItem tbItem = itemService.getItemById(itemId);</p>
        <p> <b>return</b> tbItem;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>