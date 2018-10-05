# Solr服务器搭建

### 1.1. Solr的环境

Solr是java开发。

需要安装jdk。

安装环境Linux。

需要安装Tomcat。

### 1.2. 搭建步骤

第一步：把solr 的压缩包上传到Linux系统

第二步：解压solr。

第三步：安装Tomcat，解压缩即可。

第四步：把solr部署到Tomcat下。

第五步：解压缩war包。启动Tomcat解压。

第六步：把/root/solr-4.10.3/example/lib/ext目录下的所有的jar包，添加到solr工程中。

\[root@localhost ext\]\# pwd

/root/solr-4.10.3/example/lib/ext

\[root@localhost ext\]\# cp \* /usr/local/solr/tomcat/webapps/solr/WEB-INF/lib/

第七步：创建一个solrhome。/example/solr目录就是一个solrhome。复制此目录到/usr/local/solr/solrhome

\[root@localhost example\]\# pwd

/root/solr-4.10.3/example

\[root@localhost example\]\# cp -r solr /usr/local/solr/solrhome

\[root@localhost example\]\#

第八步：关联solr及solrhome。需要修改solr工程的web.xml文件。  
 ![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

![](../../.gitbook/assets/image%20%28184%29.png)

第九步：启动Tomcat

[http://192.168.25.154:8080/solr/](http://192.168.25.154:8080/solr/)

和windows下的配置完全一样。

### 1.3. 配置业务域

schema.xml中定义

1、商品Id

2、商品标题

3、商品卖点

4、商品价格

5、商品图片

6、分类名称

创建对应的业务域。需要制定中文分析器。

创建步骤：

第一步：把中文分析器添加到工程中。

1、把IKAnalyzer2012FF\_u1.jar添加到solr工程的lib目录下

2、把扩展词典、配置文件放到solr工程的WEB-INF/classes目录下。

第二步：配置一个FieldType，制定使用IKAnalyzer

修改schema.xml文件

修改Solr的schema.xml文件，添加FieldType：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <fieldType name="text_ik" class="solr.TextField">
        </p>
        <p>
          <analyzer class="org.wltea.analyzer.lucene.IKAnalyzer" />
        </p>
        <p>
          </fieldType>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第三步：配置业务域，type制定使用自定义的FieldType。

设置业务系统Field

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <field name="item_title" type="text_ik" indexed="true" stored="true" />
        </p>
        <p>
          <field name="item_sell_point" type="text_ik" indexed="true" stored="true"
          />
        </p>
        <p>
          <field name="item_price" type="long" indexed="true" stored="true" />
        </p>
        <p>
          <field name="item_image" type="string" indexed="false" stored="true" />
        </p>
        <p>
          <field name="item_category_name" type="string" indexed="true" stored="true"
          />
        </p>
        <p>
          <field name="item_keywords" type="text_ik" indexed="true" stored="false"
          multiValued="true" />
        </p>
        <p>
          <copyField source="item_title" dest="item_keywords" />
        </p>
        <p>
          <copyField source="item_sell_point" dest="item_keywords" />
        </p>
        <p>
          <copyField source="item_category_name" dest="item_keywords" />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：重启tomcat

