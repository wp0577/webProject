# Solrj管理集群

### 1.1. 添加文档

使用步骤：

第一步：把solrJ相关的jar包添加到工程中。

第二步：创建一个SolrServer对象，需要使用CloudSolrServer子类。构造方法的参数是zookeeper的地址列表。

第三步：需要设置DefaultCollection属性。

第四步：创建一SolrInputDocument对象。

第五步：向文档对象中添加域

第六步：把文档对象写入索引库。

第七步：提交。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testSolrCloudAddDocument() <b>throws</b> Exception
          {</p>
        <p>// 第一步：把solrJ相关的jar包添加到工程中。</p>
        <p>// 第二步：创建一个SolrServer对象，需要使用CloudSolrServer子类。构造方法的参数是zookeeper的地址列表。</p>
        <p>//参数是zookeeper的地址列表，使用逗号分隔</p>
        <p>CloudSolrServer solrServer = <b>new</b> CloudSolrServer("192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183");</p>
        <p>// 第三步：需要设置DefaultCollection属性。</p>
        <p>solrServer.setDefaultCollection("collection2");</p>
        <p>// 第四步：创建一SolrInputDocument对象。</p>
        <p>SolrInputDocument document = <b>new</b> SolrInputDocument();</p>
        <p>// 第五步：向文档对象中添加域</p>
        <p>document.addField("item_title", "测试商品");</p>
        <p>document.addField("item_price", "100");</p>
        <p>document.addField("id", "test001");</p>
        <p>// 第六步：把文档对象写入索引库。</p>
        <p>solrServer.add(document);</p>
        <p>// 第七步：提交。</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 查询文档

创建一个CloudSolrServer对象，其他处理和单机版一致。

## 2.  把搜索功能切换到集群版

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
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util4.2.xsd"></p>
            <p>
              <!-- 单机版solr服务配置 -->
            </p>
            <p>
              <!-- <bean id="httpSolrServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer"></p><p>                  <constructor-arg name="baseURL" value="http://192.168.25.154:8080/solr"></constructor-arg></p><p>         </bean> -->
            </p>
            <p>
              <!-- 集群版solr服务 -->
            </p>
            <p>
              <bean id="cloudSolrServer" class="org.apache.solr.client.solrj.impl.CloudSolrServer">
            </p>
            <p>
              <constructor-arg name="zkHost" value="192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183"></constructor-arg>
            </p>
            <p>
              <property name="defaultCollection" value="collection2"></property>
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
</table>