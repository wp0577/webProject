# 商品列表页面

#### 1.1.1.                  商品列表页面

**对应的jsp为**：

item-list.jsp

**请求的url：**

/item/list

**请求的参数：**

page=1&rows=30

**响应的json数据格式：**

Easyui中datagrid控件要求的数据格式为：

{total:”2”,rows:\[{“id”:”1”,”name”:”张三”},{“id”:”2”,”name”:”李四”}\]}

![](../../../.gitbook/assets/image%20%28267%29.png)

#### 1.1.2.                  响应的json数据格式EasyUIResult

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> EasyUIDataGridResult {</p>
        <p> <b>private</b> Integer total;</p>
        <p> <b>private</b> List
          <?>rows;</p>
        <p> <b>public</b> EasyUIResult(Integer total, List
          <?>rows) {</p>
        <p> <b>this</b>.total = total;</p>
        <p> <b>this</b>.rows = rows;</p>
        <p>}</p>
        <p> <b>public</b> EasyUIResult(Long total, List
          <?>rows) {</p>
        <p> <b>this</b>.total = total.intValue();</p>
        <p> <b>this</b>.rows = rows;</p>
        <p>}</p>
        <p> <b>public</b> Integer getTotal() {</p>
        <p> <b>return</b> total;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setTotal(Integer total) {</p>
        <p> <b>this</b>.total = total;</p>
        <p>}</p>
        <p> <b>public</b> List
          <?>getRows() {</p>
        <p> <b>return</b> rows;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setRows(List
          <?>rows) {</p>
        <p> <b>this</b>.rows = rows;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>