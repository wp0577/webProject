# Solrj查询索引库



### 1.1. 使用sorlJ 查询索引库

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//使用solrJ实现查询</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> queryDocument() <b>throws</b> Exception {</p>
        <p>//创建一个SolrServer对象</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://192.168.25.154:8080/solr");</p>
        <p>//创建一个查询对象，可以参考solr的后台的查询功能设置条件</p>
        <p>SolrQuery query = <b>new</b> SolrQuery();</p>
        <p>//设置查询条件</p>
        <p>// query.setQuery("阿尔卡特");</p>
        <p>query.set("q","阿尔卡特");</p>
        <p>//设置分页条件</p>
        <p>query.setStart(1);</p>
        <p>query.setRows(2);</p>
        <p>//开启高亮</p>
        <p>query.setHighlight(<b>true</b>);</p>
        <p>query.addHighlightField("item_title");</p>
        <p>query.setHighlightSimplePre("<em>");</p><p>                  query.setHighlightSimplePost("</em>");</p>
        <p>//设置默认搜索域</p>
        <p>query.set("df", "item_title");</p>
        <p>//执行查询，得到一个QueryResponse对象。</p>
        <p>QueryResponse queryResponse = solrServer.query(query);</p>
        <p>//取查询结果总记录数</p>
        <p>SolrDocumentList solrDocumentList = queryResponse.getResults();</p>
        <p>System.<b>out</b>.println("查询结果总记录数：" + solrDocumentList.getNumFound());</p>
        <p>//取查询结果</p>
        <p>Map
          <String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();</p>
        <p> <b>for</b> (SolrDocument solrDocument : solrDocumentList) {</p>
        <p>System.<b>out</b>.println(solrDocument.get("id"));</p>
        <p>//取高亮后的结果</p>
        <p>List
          <String>list = highlighting.get(solrDocument.get("id")).get("item_title");</p>
        <p>String title= "";</p>
        <p> <b>if</b> (list != <b>null</b> && list.size() > 0) {</p>
        <p>//取高亮后的结果</p>
        <p>title = list.get(0);</p>
        <p>} <b>else</b> {</p>
        <p>title = (String) solrDocument.get("item_title");</p>
        <p>}</p>
        <p>System.<b>out</b>.println(title);</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_sell_point"));</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_price"));</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_image"));</p>
        <p>System.<b>out</b>.println(solrDocument.get("item_category_name"));</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 功能分析

把搜索结果页面添加到工程中。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

请求的url：/search

请求的方法：GET

参数：

keyword：查询条件

Page：页码。如果没有此参数，需要给默认值1。

返回的结果：

1）商品列表

2）总页数

3）总记录数

使用jsp展示，返回逻辑视图。

商品列表使用：SearchItem表示。

需要把查询结果封装到一个pojo中：

1）商品列表List&lt;SearchItem&gt;

2）总页数。Int totalPages。总记录数/每页显示的记录数向上取整。把每页显示的记录是配置到属性文件中。

3）总记录数。Int recourdCount

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> SearchResult <b>implements</b> Serializable {</p>
        <p> <b>private</b> List
          <SearchItem>itemList;</p>
        <p> <b>private</b>  <b>int</b> totalPages;</p>
        <p> <b>private</b>  <b>int</b> recourdCount;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. Dao层

跟据查询条件查询索引库，返回对应的结果。

参数：SolrQuery

返回结果：SearchResult

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Repository</p>
        <p><b>public</b>  <b>class</b> SearchDao {</p>
        <p>@Autowired</p>
        <p> <b>private</b> SolrServer solrServer;</p>
        <p> <b>public</b> SearchResult search(SolrQuery query) <b>throws</b> Exception
          {</p>
        <p>//根据查询条件查询索引库</p>
        <p>QueryResponse queryResponse = solrServer.query(query);</p>
        <p>//取查询结果总记录数</p>
        <p>SolrDocumentList solrDocumentList = queryResponse.getResults();</p>
        <p> <b>long</b> numFound = solrDocumentList.getNumFound();</p>
        <p>//创建一个返回结果对象</p>
        <p>SearchResult result = <b>new</b> SearchResult();</p>
        <p>result.setRecourdCount((<b>int</b>) numFound);</p>
        <p>//创建一个商品列表对象</p>
        <p>List
          <SearchItem>itemList = <b>new</b> ArrayList
            <>();</p>
        <p>//取商品列表</p>
        <p>//取高亮后的结果</p>
        <p>Map
          <String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();</p>
        <p> <b>for</b> (SolrDocument solrDocument : solrDocumentList) {</p>
        <p>//取商品信息</p>
        <p>SearchItem searchItem = <b>new</b> SearchItem();</p>
        <p>searchItem.setCategory_name((String) solrDocument.get("item_category_name"));</p>
        <p>searchItem.setId((String) solrDocument.get("id"));</p>
        <p>searchItem.setImage((String) solrDocument.get("item_image"));</p>
        <p>searchItem.setPrice((<b>long</b>) solrDocument.get("item_price"));</p>
        <p>searchItem.setSell_point((String) solrDocument.get("item_sell_point"));</p>
        <p>//取高亮结果</p>
        <p>List
          <String>list = highlighting.get(solrDocument.get("id")).get("item_title");</p>
        <p>String itemTitle = "";</p>
        <p> <b>if</b> (list != <b>null</b> && list.size() > 0) {</p>
        <p>itemTitle = list.get(0);</p>
        <p>} <b>else</b> {</p>
        <p>itemTitle = (String) solrDocument.get("item_title");</p>
        <p>}</p>
        <p>searchItem.setTitle(itemTitle);</p>
        <p>//添加到商品列表</p>
        <p>itemList.add(searchItem);</p>
        <p>}</p>
        <p>//把列表添加到返回结果对象中</p>
        <p>result.setItemList(itemList);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. Service层

需要有一个接口一个实现类，需要对外发布服务。

参数：String keyWord

      int page

      int rows

返回值：SearchResult

业务逻辑：

1）根据参数创建一个查询条件对象。需要指定默认搜索域，还需要配置高亮显示。

2）调用dao查询。得到一个SearchResult对象

3）计算查询总页数，每页显示记录数就是rows参数。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> SearchServiceImpl <b>implements</b> SearchService
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> SearchDao searchDao;</p>
        <p>@Value("${DEFAULT_FIELD}")</p>
        <p> <b>private</b> String DEFAULT_FIELD;</p>
        <p>@Override</p>
        <p> <b>public</b> SearchResult search(String keyWord, <b>int</b> page, <b>int</b> rows) <b>throws</b> Exception
          {</p>
        <p>//创建一个SolrQuery对象</p>
        <p>SolrQuery query = <b>new</b> SolrQuery();</p>
        <p>//设置查询条件</p>
        <p>query.setQuery(keyWord);</p>
        <p>//设置分页条件</p>
        <p>query.setStart((page - 1) * rows);</p>
        <p>//设置rows</p>
        <p>query.setRows(rows);</p>
        <p>//设置默认搜索域</p>
        <p>query.set("df", DEFAULT_FIELD);</p>
        <p>//设置高亮显示</p>
        <p>query.setHighlight(<b>true</b>);</p>
        <p>query.addHighlightField("item_title");</p>
        <p>query.setHighlightSimplePre("<em style=\ "color:red\">");</p><p>                  query.setHighlightSimplePost("</em>");</p>
        <p>//执行查询</p>
        <p>SearchResult searchResult = searchDao.search(query);</p>
        <p>//计算总页数</p>
        <p> <b>int</b> recourdCount = searchResult.getRecourdCount();</p>
        <p> <b>int</b> pages = recourdCount / rows;</p>
        <p> <b>if</b> (recourdCount % rows > 0) pages++;</p>
        <p>//设置到返回结果</p>
        <p>searchResult.setTotalPages(pages);</p>
        <p> <b>return</b> searchResult;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.4.1.                  发布服务

![](../../.gitbook/assets/image%20%288%29.png)

### 1.5. 表现层

#### 1.5.1.                  引用服务

在e3-search-web中添加接口依赖

![](../../.gitbook/assets/image%20%28263%29.png)

Springmvc.xml

![](../../.gitbook/assets/image%20%28223%29.png)

#### 1.5.2.                  Controller

请求的url：/search

请求的方法：GET

参数：

keyword：查询条件

Page：页码。如果没有此参数，需要给默认值1。

返回的结果：

使用jsp展示，返回逻辑视图。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> SearchController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> SearchService searchService;</p>
        <p>@Value("${PAGE_ROWS}")</p>
        <p> <b>private</b> Integer PAGE_ROWS;</p>
        <p>@RequestMapping("/search")</p>
        <p> <b>public</b> String search(String keyword,@RequestParam(defaultValue="1")
          Integer page, Model model) <b>throws</b> Exception {</p>
        <p>//需要转码</p>
        <p>keyword = <b>new</b> String(keyword.getBytes("iso8859-1"), "utf-8");</p>
        <p>//调用Service查询商品信息</p>
        <p>SearchResult result = searchService.search(keyword, page, PAGE_ROWS);</p>
        <p>//把结果传递给jsp页面</p>
        <p>model.addAttribute("query", keyword);</p>
        <p>model.addAttribute("totalPages", result.getTotalPages());</p>
        <p>model.addAttribute("recourdCount", result.getRecourdCount());</p>
        <p>model.addAttribute("page", page);</p>
        <p>model.addAttribute("itemList", result.getItemList());</p>
        <p>//返回逻辑视图</p>
        <p> <b>return</b> "search";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>