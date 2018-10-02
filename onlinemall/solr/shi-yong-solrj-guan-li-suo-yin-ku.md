# 使用solrJ管理索引库

使用SolrJ可以实现索引库的增删改查操作。

### 1.1. 添加文档

第一步：把solrJ的jar包添加到工程中。

第二步：创建一个SolrServer，使用HttpSolrServer创建对象。

第三步：创建一个文档对象SolrInputDocument对象。

第四步：向文档中添加域。必须有id域，域的名称必须在schema.xml中定义。

第五步：把文档添加到索引库中。

第六步：提交。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> addDocument() <b>throws</b> Exception {</p>
        <p>// 第一步：把solrJ的jar包添加到工程中。</p>
        <p>// 第二步：创建一个SolrServer，使用HttpSolrServer创建对象。</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>// 第三步：创建一个文档对象SolrInputDocument对象。</p>
        <p>SolrInputDocument document = <b>new</b> SolrInputDocument();</p>
        <p>// 第四步：向文档中添加域。必须有id域，域的名称必须在schema.xml中定义。</p>
        <p>document.addField("id", "test001");</p>
        <p>document.addField("item_title", "测试商品");</p>
        <p>document.addField("item_price", "199");</p>
        <p>// 第五步：把文档添加到索引库中。</p>
        <p>solrServer.add(document);</p>
        <p>// 第六步：提交。</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 删除文档

#### 1.2.1.                  根据id删除

第一步：创建一个SolrServer对象。

第二步：调用SolrServer对象的根据id删除的方法。

第三步：提交。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> deleteDocumentById() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个SolrServer对象。</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>// 第二步：调用SolrServer对象的根据id删除的方法。</p>
        <p>solrServer.deleteById("1");</p>
        <p>// 第三步：提交。</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.2.                  根据查询删除

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> deleteDocumentByQuery() <b>throws</b> Exception
          {</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>solrServer.deleteByQuery("title:change.me");</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 查询索引库

查询步骤：

第一步：创建一个SolrServer对象

第二步：创建一个SolrQuery对象。

第三步：向SolrQuery中添加查询条件、过滤条件。。。

第四步：执行查询。得到一个Response对象。

第五步：取查询结果。

第六步：遍历结果并打印。

#### 1.3.1.                  简单查询

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> queryDocument() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个SolrServer对象</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>// 第二步：创建一个SolrQuery对象。</p>
        <p>SolrQuery query = <b>new</b> SolrQuery();</p>
        <p>// 第三步：向SolrQuery中添加查询条件、过滤条件。。。</p>
        <p>query.setQuery("*:*");</p>
        <p>// 第四步：执行查询。得到一个Response对象。</p>
        <p>QueryResponse response = solrServer.query(query);</p>
        <p>// 第五步：取查询结果。</p>
        <p>SolrDocumentList solrDocumentList = response.getResults();</p>
        <p>System.<b>out</b>.println("查询结果的总记录数：" + solrDocumentList.getNumFound());</p>
        <p>// 第六步：遍历结果并打印。</p>
        <p> <b>for</b> (SolrDocument solrDocument : solrDocumentList) {</p>
        <p>System.<b>out</b>.println(solrDocument.get("id"));</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_title"));</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_price"));</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.3.2.                  带高亮显示

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> queryDocumentWithHighLighting() <b>throws</b> Exception
          {</p>
        <p>// 第一步：创建一个SolrServer对象</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>// 第二步：创建一个SolrQuery对象。</p>
        <p>SolrQuery query = <b>new</b> SolrQuery();</p>
        <p>// 第三步：向SolrQuery中添加查询条件、过滤条件。。。</p>
        <p>query.setQuery("测试");</p>
        <p>//指定默认搜索域</p>
        <p>query.set("df", "item_keywords");</p>
        <p>//开启高亮显示</p>
        <p>query.setHighlight(<b>true</b>);</p>
        <p>//高亮显示的域</p>
        <p>query.addHighlightField("item_title");</p>
        <p>query.setHighlightSimplePre("<em>");</p><p>                  query.setHighlightSimplePost("</em>");</p>
        <p>// 第四步：执行查询。得到一个Response对象。</p>
        <p>QueryResponse response = solrServer.query(query);</p>
        <p>// 第五步：取查询结果。</p>
        <p>SolrDocumentList solrDocumentList = response.getResults();</p>
        <p>System.<b>out</b>.println("查询结果的总记录数：" + solrDocumentList.getNumFound());</p>
        <p>// 第六步：遍历结果并打印。</p>
        <p> <b>for</b> (SolrDocument solrDocument : solrDocumentList) {</p>
        <p>System.<b>out</b>.println(solrDocument.get("id"));</p>
        <p>//取高亮显示</p>
        <p>Map
          <String, Map<String, List<String>>> highlighting = response.getHighlighting();</p>
        <p>List
          <String>list = highlighting.get(solrDocument.get("id")).get("item_title");</p>
        <p>String itemTitle = <b>null</b>;</p>
        <p> <b>if</b> (list != <b>null</b> && list.size() > 0) {</p>
        <p>itemTitle = list.get(0);</p>
        <p>} <b>else</b> {</p>
        <p>itemTitle = (String) solrDocument.get("item_title");</p>
        <p>}</p>
        <p>System.<b>out</b>.println(itemTitle);</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_price"));</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>