# 内容分类管理

![](../../../../.gitbook/assets/image%20%28156%29.png)

#### 1.1.1.                  展示内容分类

**功能分析**

![](../../../../.gitbook/assets/image%20%28191%29.png)

请求的url：/content/category/list

请求的参数：id，当前节点的id。第一次请求是没有参数，需要给默认值“0”

响应数据：List&lt;EasyUITreeNode&gt;（@ResponseBody）

Json数据。

\[{id:1,text:节点名称,state:open\(closed\)},

{id:2,text:节点名称2,state:open\(closed\)},

{id:3,text:节点名称3,state:open\(closed\)}\]

业务逻辑：

1、取查询参数id，parentId

2、根据parentId查询tb\_content\_category，查询子节点列表。

3、得到List&lt;TbContentCategory&gt;

4、把列表转换成List&lt;EasyUITreeNode&gt;

**Dao层**

使用逆向工程

**Service**

参数：long parentId

返回值：List&lt;EasyUITreeNode&gt;

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> ContentCategoryServiceImpl <b>implements</b> ContentCategoryService
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbContentCategoryMapper contentCategoryMapper;</p>
        <p>@Override</p>
        <p> <b>public</b> List
          <EasyUITreeNode>getContentCategoryList(<b>long</b> parentId) {</p>
        <p>// 1、取查询参数id，parentId</p>
        <p>// 2、根据parentId查询tb_content_category，查询子节点列表。</p>
        <p>TbContentCategoryExample example = <b>new</b> TbContentCategoryExample();</p>
        <p>//设置查询条件</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>criteria.andParentIdEqualTo(parentId);</p>
        <p>//执行查询</p>
        <p>// 3、得到List
          <TbContentCategory>
        </p>
        <p>List
          <TbContentCategory>list = contentCategoryMapper.selectByExample(example);</p>
        <p>// 4、把列表转换成List
          <EasyUITreeNode>ub</p>
        <p>List
          <EasyUITreeNode>resultList = <b>new</b> ArrayList
            <>();</p>
        <p> <b>for</b> (TbContentCategory tbContentCategory : list) {</p>
        <p>EasyUITreeNode node = <b>new</b> EasyUITreeNode();</p>
        <p>node.setId(tbContentCategory.getId());</p>
        <p>node.setText(tbContentCategory.getName());</p>
        <p>node.setState(tbContentCategory.getIsParent()?"closed":"open");</p>
        <p>//添加到列表</p>
        <p>resultList.add(node);</p>
        <p>}</p>
        <p> <b>return</b> resultList;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>发布服务

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

**表现层**

E3-manager-web

依赖e3-content-interface模块

![](../../../../.gitbook/assets/image%20%2866%29.png)

Springmvc.xml中添加服务的引用：

![](../../../../.gitbook/assets/image%20%28133%29.png)

Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p>@RequestMapping("/content/category")</p>
        <p><b>public</b>  <b>class</b> ContentCategoryController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> ContentCategoryService contentCategoryService;</p>
        <p>@RequestMapping("/list")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> List
          <EasyUITreeNode>getContentCatList(</p>
        <p>@RequestParam(value="id", defaultValue="0") Long parentId) {</p>
        <p>List
          <EasyUITreeNode>list = contentCategoryService.getContentCategoryList(parentId);</p>
        <p> <b>return</b> list;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.2.                  新增节点

**功能分析**

![](../../../../.gitbook/assets/image%20%28116%29.png)

请求的url：/content/category/create

请求的参数：

Long parentId

String name

响应的结果：

json数据，E3Result，其中包含一个对象，对象有id属性，新生产的内容分类id

业务逻辑：

1、接收两个参数：parentId、name

2、向tb\_content\_category表中插入数据。

a\)       创建一个TbContentCategory对象

b\)      补全TbContentCategory对象的属性

c\)       向tb\_content\_category表中插入数据

3、判断父节点的isparent是否为true，不是true需要改为true。

4、需要主键返回。

5、返回E3Result，其中包装TbContentCategory对象

**Dao层**

可以使用逆向工程。

需要添加主键返回:

![](../../../../.gitbook/assets/image%20%2862%29.png)

注意：修改完代码后，需要向本地仓库安装e3-manager-dao包

**Service层**

参数：parentId、name

返回值：返回E3Result，其中包装TbContentCategory对象

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> E3Result addContentCategory(<b>long</b> parentId, String name)
          {</p>
        <p>// 1、接收两个参数：parentId、name</p>
        <p>// 2、向tb_content_category表中插入数据。</p>
        <p>// a)创建一个TbContentCategory对象</p>
        <p>TbContentCategory tbContentCategory = <b>new</b> TbContentCategory();</p>
        <p>// b)补全TbContentCategory对象的属性</p>
        <p>tbContentCategory.setIsParent(<b>false</b>);</p>
        <p>tbContentCategory.setName(name);</p>
        <p>tbContentCategory.setParentId(parentId);</p>
        <p>//排列序号，表示同级类目的展现次序，如数值相等则按名称次序排列。取值范围:大于零的整数</p>
        <p>tbContentCategory.setSortOrder(1);</p>
        <p>//状态。可选值:1(正常),2(删除)</p>
        <p>tbContentCategory.setStatus(1);</p>
        <p>tbContentCategory.setCreated(<b>new</b> Date());</p>
        <p>tbContentCategory.setUpdated(<b>new</b> Date());</p>
        <p>// c)向tb_content_category表中插入数据</p>
        <p>contentCategoryMapper.insert(tbContentCategory);</p>
        <p>// 3、判断父节点的isparent是否为true，不是true需要改为true。</p>
        <p>TbContentCategory parentNode = contentCategoryMapper.selectByPrimaryKey(parentId);</p>
        <p> <b>if</b> (!parentNode.getIsParent()) {</p>
        <p>parentNode.setIsParent(<b>true</b>);</p>
        <p>//更新父节点</p>
        <p>contentCategoryMapper.updateByPrimaryKey(parentNode);</p>
        <p>}</p>
        <p>// 4、需要主键返回。</p>
        <p>// 5、返回E3Result，其中包装TbContentCategory对象</p>
        <p> <b>return</b> E3Result.ok(tbContentCategory);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>发布服务。

**表现层**

请求的url：/content/category/create

请求的参数：

Long parentId

String name

响应的结果：

json数据，E3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/create")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> E3Result createCategory(Long parentId, String name) {</p>
        <p>E3Result result = contentCategoryService.addContentCategory(parentId,
          name);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.3.                  内容分类重命名、删除

**重命名**

![](../../../../.gitbook/assets/image%20%2838%29.png)

请求的url：/content/category/update

参数：id，当前节点id。name，重命名后的名称。

业务逻辑：根据id更新记录。

返回值：返回E3Result.ok\(\)

作业。

**删除节点**

![](../../../../.gitbook/assets/image%20%2899%29.png)

请求的url：/content/category/delete/

参数：id，当前节点的id。

响应的数据：json。E3Result。

业务逻辑：

1、根据id删除记录。

2、判断父节点下是否还有子节点，如果没有需要把父节点的isparent改为false

3、如果删除的是父节点，子节点要级联删除。

两种解决方案：

1）如果判断是父节点不允许删除。

2）递归删除。

