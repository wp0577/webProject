# 将商品数据导入索引库中



### 1.1. Dao层

#### 1.1.1.                  Sql语句

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>SELECT</p>
        <p>a.id,</p>
        <p>a.title,</p>
        <p>a.sell_point,</p>
        <p>a.price,</p>
        <p>a.image,</p>
        <p>b.`name` category_name</p>
        <p>FROM</p>
        <p>`tb_item` a</p>
        <p>LEFT JOIN tb_item_cat b ON a.cid = b.id</p>
        <p>WHERE a.`status`=1</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>需要自己创建Mapper文件。

#### 1.1.2.                  创建对应数据集的pojo

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> SearchItem <b>implements</b> Serializable{</p>
        <p> <b>private</b> String id;</p>
        <p> <b>private</b> String title;</p>
        <p> <b>private</b> String sell_point;</p>
        <p> <b>private</b>  <b>long</b> price;</p>
        <p> <b>private</b> String image;</p>
        <p> <b>private</b> String category_name;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.3.                  接口定义

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>interface</b> ItemMapper {</p>
        <p>List
          <SearchItem>getItemList();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.4.                  Mapper映射文件

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        </p>
        <p>
          <mapper namespace="cn.e3mall.search.mapper.ItemMapper">
        </p>
        <p>
          <select id="getItemList" resultType="cn.e3mall.common.pojo.SearchItem">
        </p>
        <p>SELECT</p>
        <p>a.id,</p>
        <p>a.title,</p>
        <p>a.sell_point,</p>
        <p>a.price,</p>
        <p>a.image,</p>
        <p>b.`name` category_name</p>
        <p>FROM</p>
        <p>`tb_item` a</p>
        <p>LEFT JOIN tb_item_cat b ON a.cid = b.id</p>
        <p>WHERE a.`status`=1</p>
        <p>
          </select>
        </p>
        <p>
          </mapper>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. Service层

#### 1.2.1.                  功能分析

1、查询所有商品数据。

2、循环把商品数据添加到索引库。使用solrJ实现。

3、返回成功。返回E3Result

参数：无

返回值：E3Result

#### 1.2.2.                  solrJ添加索引库

1、把solrJ的jar包添加到工程。

2、创建一个SolrServer对象。创建一个和sorl服务的连接。HttpSolrServer。

3、创建一个文档对象。SolrInputDocument。

4、向文档对象中添加域。必须有一个id域。而且文档中使用的域必须在schema.xml中定义。

5、把文档添加到索引库

6、Commit。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> addDocument() <b>throws</b> Exception {</p>
        <p>// 1、把solrJ的jar包添加到工程。</p>
        <p>// 2、创建一个SolrServer对象。创建一个和sorl服务的连接。HttpSolrServer。</p>
        <p>//如果不带Collection默认连接Collection1</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>// 3、创建一个文档对象。SolrInputDocument。</p>
        <p>SolrInputDocument document = <b>new</b> SolrInputDocument();</p>
        <p>// 4、向文档对象中添加域。必须有一个id域。而且文档中使用的域必须在schema.xml中定义。</p>
        <p>document.addField("id", "test001");</p>
        <p>document.addField("item_title", "测试商品");</p>
        <p>// 5、把文档添加到索引库</p>
        <p>solrServer.add(document);</p>
        <p>// 6、Commit。</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.3.                  代码实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 将商品数据导入索引库</p>
        <p>*
          <p>Title: SearchItemServiceImpl</p>
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
        <p><b>public</b>  <b>class</b> SearchItemServiceImpl <b>implements</b> SearchItemService
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> ItemMapper itemMapper;</p>
        <p>@Autowired</p>
        <p> <b>private</b> SolrServer solrServer;</p>
        <p>@Override</p>
        <p> <b>public</b> E3Result importItmes() {</p>
        <p> <b>try</b> {</p>
        <p>//查询商品列表</p>
        <p>List
          <SearchItem>itemList = itemMapper.getItemList();</p>
        <p>//导入索引库</p>
        <p> <b>for</b> (SearchItem searchItem : itemList) {</p>
        <p>//创建文档对象</p>
        <p>SolrInputDocument document = <b>new</b> SolrInputDocument();</p>
        <p>//向文档中添加域</p>
        <p>document.addField("id", searchItem.getId());</p>
        <p>document.addField("item_title", searchItem.getTitle());</p>
        <p>document.addField("item_sell_point", searchItem.getSell_point());</p>
        <p>document.addField("item_price", searchItem.getPrice());</p>
        <p>document.addField("item_image", searchItem.getImage());</p>
        <p>document.addField("item_category_name", searchItem.getCategory_name());</p>
        <p>//写入索引库</p>
        <p>solrServer.add(document);</p>
        <p>}</p>
        <p>//提交</p>
        <p>solrServer.commit();</p>
        <p>//返回成功</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p> <b>return</b> E3Result.build(500, "商品导入失败");</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.4.                  SolrServer的配置

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
              <bean id="httpSolrServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer">
            </p>
            <p>
              <constructor-arg index="0" value="http://192.168.25.154:8080/solr" />
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
</table>#### 1.2.5.                  发布服务

![](../../.gitbook/assets/image%20%28142%29.png)

### 1.3. 表现层

后台管理工程中调用商品导入服务。

![](../../.gitbook/assets/image%20%28220%29.png)

#### 1.3.1.                  功能分析

![](../../.gitbook/assets/image%20%2883%29.png)

请求的url：/index/item/import

响应的结果：json数据。可以使用E3Result

#### 1.3.2.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> SearchItemController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> SearchItemService searchItemService;</p>
        <p>@RequestMapping("/index/item/import")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> E3Result impotItemIndex() {</p>
        <p>E3Result result = searchItemService.importItmes();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. 解决Mapper映射文件不存在异常

在e3-search-service的pom文件中需要添加资源配置。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 如果不添加此节点mybatis的mapper.xml文件都会被漏掉。 -->
        </p>
        <p>
          <build>
        </p>
        <p>
          <resources>
        </p>
        <p>
          <resource>
        </p>
        <p>
          <directory>src/main/java</directory>
        </p>
        <p>
          <includes>
        </p>
        <p>
          <include>**/*.properties</include>
        </p>
        <p>
          <include>**/*.xml</include>
        </p>
        <p>
          </includes>
        </p>
        <p>
          <filtering>false</filtering>
        </p>
        <p>
          </resource>
        </p>
        <p>
          <resource>
        </p>
        <p>
          <directory>src/main/resources</directory>
        </p>
        <p>
          <includes>
        </p>
        <p>
          <include>**/*.properties</include>
        </p>
        <p>
          <include>**/*.xml</include>
        </p>
        <p>
          </includes>
        </p>
        <p>
          <filtering>false</filtering>
        </p>
        <p>
          </resource>
        </p>
        <p>
          </resources>
        </p>
        <p>
          </build>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>