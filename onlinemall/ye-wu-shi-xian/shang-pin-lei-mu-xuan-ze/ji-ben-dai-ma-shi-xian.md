# 基本代码实现

### 1.1. Dao层

tb\_item\_cat

可以使用逆向工程生成的代码

### 1.2. Service层

参数：long parentId

业务逻辑：

1、根据parentId查询节点列表

2、转换成EasyUITreeNode列表。

3、返回。

返回值：List&lt;EasyUITreeNode&gt;

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> ItemCatServiceImpl <b>implements</b> ItemCatService
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbItemCatMapper itemCatMapper;</p>
        <p>@Override</p>
        <p> <b>public</b> List
          <EasyUITreeNode>getCatList(<b>long</b> parentId) {</p>
        <p>// 1、根据parentId查询节点列表</p>
        <p>TbItemCatExample example = <b>new</b> TbItemCatExample();</p>
        <p>//设置查询条件</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>criteria.andParentIdEqualTo(parentId);</p>
        <p>List
          <TbItemCat>list = itemCatMapper.selectByExample(example);</p>
        <p>// 2、转换成EasyUITreeNode列表。</p>
        <p>List
          <EasyUITreeNode>resultList = <b>new</b> ArrayList
            <>();</p>
        <p> <b>for</b> (TbItemCat tbItemCat : list) {</p>
        <p>EasyUITreeNode node = <b>new</b> EasyUITreeNode();</p>
        <p>node.setId(tbItemCat.getId());</p>
        <p>node.setText(tbItemCat.getName());</p>
        <p>node.setState(tbItemCat.getIsParent()?"closed":"open");</p>
        <p>//添加到列表</p>
        <p>resultList.add(node);</p>
        <p>}</p>
        <p>// 3、返回。</p>
        <p> <b>return</b> resultList;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.1.                  发布服务

### 1.3. 表现层

#### 1.3.1.                  引用服务

#### 1.3.2.                  Controller

初始化tree请求的url：/item/cat/list

参数：

long id（父节点id）

返回值：json。数据格式

 List&lt;EasyUITreeNode&gt;

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> ItemCatController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> ItemCatService itemCatService;</p>
        <p>@RequestMapping("/item/cat/list")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> List
          <EasyUITreeNode>getItemCatList(@RequestParam(value="id", defaultValue="0")Long parentId)
            {</p>
        <p>List
          <EasyUITreeNode>list = itemCatService.getCatList(parentId);</p>
        <p> <b>return</b> list;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>