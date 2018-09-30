# 商品类目选择

## 1.  商品类目选择

### 1.1. 原型

![](../../../../.gitbook/assets/image%20%2832%29.png)

### 1.2. 功能分析

![](../../../../.gitbook/assets/image%20%28174%29.png)

![](../../../../.gitbook/assets/image%20%2869%29.png)

展示商品分类列表，使用EasyUI的tree控件展示。

![](../../../../.gitbook/assets/image%20%2872%29.png)

初始化tree请求的url：/item/cat/list

参数：

初始化tree时只需要把第一级节点展示，子节点异步加载。

long id（父节点id）

返回值：json。数据格式

\[{   

    "id": 1,   

    "text": "Node 1",   

    "state": "closed"

},{   

    "id": 2,   

    "text": "Node 2",   

    "state": "closed"  

}\]  
 state：如果节点下有子节点“closed”，如果没有子节点“open”

创建一个pojo来描述tree的节点信息，包含三个属性id、text、state。放到e3-common工程中。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> EasyUITreeNode <b>implements</b> Serializable{</p>
        <p> <b>private</b>  <b>long</b> id;</p>
        <p> <b>private</b> String text;</p>
        <p> <b>private</b> String state;</p>
        <p> <b>public</b>  <b>long</b> getId() {</p>
        <p> <b>return</b> id;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setId(<b>long</b> id) {</p>
        <p> <b>this</b>.id = id;</p>
        <p>}</p>
        <p> <b>public</b> String getText() {</p>
        <p> <b>return</b> text;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setText(String text) {</p>
        <p> <b>this</b>.text = text;</p>
        <p>}</p>
        <p> <b>public</b> String getState() {</p>
        <p> <b>return</b> state;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setState(String state) {</p>
        <p> <b>this</b>.state = state;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>查询的表：

tb\_item\_cat

查询列：

Id、name、isparent

查询条件parentId

