# 代码实现



### 1.1. Service层

参数：int page ，int rows

业务逻辑：查询所有商品列表，要进行分页处理。

返回值：EasyUIDataGridResult

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> EasyUIDataGridResult getItemList(<b>int</b> page, <b>int</b> rows)
          {</p>
        <p>//设置分页信息</p>
        <p>PageHelper.startPage(page, rows);</p>
        <p>//执行查询</p>
        <p>TbItemExample example = <b>new</b> TbItemExample();</p>
        <p>List
          <TbItem>list = itemMapper.selectByExample(example);</p>
        <p>//取分页信息</p>
        <p>PageInfo
          <TbItem>pageInfo = <b>new</b> PageInfo
            <>(list);</p>
        <p>//创建返回结果对象</p>
        <p>EasyUIDataGridResult result = <b>new</b> EasyUIDataGridResult();</p>
        <p>result.setTotal(pageInfo.getTotal());</p>
        <p>result.setRows(list);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.1.                  发布服务

![](../../../.gitbook/assets/image%20%2832%29.png)

### 1.2. 表现层

引用服务：

![](../../../.gitbook/assets/image%20%28158%29.png)

1、初始化表格请求的url：/item/list

2、Datagrid默认请求参数：

1、page：当前的页码，从1开始。

2、rows：每页显示的记录数。

3、响应的数据：json数据。EasyUIDataGridResult

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/item/list")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> EasyUIDataGridResult getItemList(Integer page, Integer rows)
          {</p>
        <p>EasyUIDataGridResult result = itemService.getItemList(page, rows);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>可以设置服务超时时间：

服务调用超时时间默认1秒，

![](../../../.gitbook/assets/image%20%28105%29.png)

Debug设置源代码：



### 1.3. 安装maven工程跳过测试

clean install -DskipTests

