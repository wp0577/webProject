# 内容管理

![](../../../../.gitbook/assets/image%20%2827%29.png)



#### 1.1.1.                  功能点分析

1、内容列表查询（作业）

2、新增内容

3、编辑内容（作业）

4、删除内容（作业）

#### 1.1.2.                  内容列表查询

请求的url：/content/query/list

参数：categoryId 分类id

响应的数据：json数据

{total:查询结果总数量,rows\[{id:1,title:aaa,subtitle:bb,...}\]}

EasyUIDataGridResult

描述商品数据List&lt;TbContent&gt;

查询的表：tb\_content

业务逻辑：

根据内容分类id查询内容列表。要进行分页处理。

#### 1.1.3.                  新增内容

**功能分析**

![](../../../../.gitbook/assets/image%20%28125%29.png)

新增内容，必须指定一个内容分类。

![](../../../../.gitbook/assets/image%20%28264%29.png)

提交表单请求的url：/content/save

参数：表单的数据。使用pojo接收TbContent

返回值：E3Result（json数据）

业务逻辑：

1、把TbContent对象属性补全。

2、向tb\_content表中插入数据。

3、返回E3Result

**Dao**

逆向工程

**Service**

参数：TbContent

返回值：E3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> ContentServiceImpl <b>implements</b> ContentService
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbContentMapper contentMapper;</p>
        <p>@Override</p>
        <p> <b>public</b> E3Result addContent(TbContent content) {</p>
        <p>//补全属性</p>
        <p>content.setCreated(<b>new</b> Date());</p>
        <p>content.setUpdated(<b>new</b> Date());</p>
        <p>//插入数据</p>
        <p>contentMapper.insert(content);</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**发布服务**

![](../../../../.gitbook/assets/image%20%2826%29.png)

**引用服务**

Toatao-manager-web工程中引用。

**Controller**

提交表单请求的url：/content/save

参数：表单的数据。使用pojo接收TbContent

返回值：E3Result（json数据）

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> ContentController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> ContentService contentService;</p>
        <p>@RequestMapping("/content/save")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> E3Result addContent(TbContent content) {</p>
        <p>E3Result result = contentService.addContent(content);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>