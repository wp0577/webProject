# 功能实现

### 1.1. 功能分析

请求的url：/item/save

参数：表单的数据。可以使用pojo接收表单的数据，要求pojo的属性和input的name属性要一致。

使用TbItem对象接收表单的数据。

TbItem item,String desc

返回值：

Json数据。应该包含一个status的属性。

可以使用E3Result，放到e3-common中。

业务逻辑：

1、生成商品id

实现方案：

a\)       Uuid，字符串，不推荐使用。

b\)      数值类型，不重复。日期+时间+随机数20160402151333123123

c\)       可以直接去毫秒值+随机数。可以使用。

d\)      使用redis。Incr。推荐使用。

使用IDUtils生成商品id

2、补全TbItem对象的属性

3、向商品表插入数据

4、创建一个TbItemDesc对象

5、补全TbItemDesc的属性

6、向商品描述表插入数据

7、E3Result.ok\(\)

### 1.2. Dao层

向tb\_item, tb\_item\_desc表中插入数据

可以使用逆向工程

### 1.3. Service层

参数：TbItem item,String desc

业务逻辑：略，参加上面

返回值：E3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> E3Result addItem(TbItem item, String desc) {</p>
        <p>// 1、生成商品id</p>
        <p> <b>long</b> itemId = IDUtils.genItemId();</p>
        <p>// 2、补全TbItem对象的属性</p>
        <p>item.setId(itemId);</p>
        <p>//商品状态，1-正常，2-下架，3-删除</p>
        <p>item.setStatus((<b>byte</b>) 1);</p>
        <p>Date date = <b>new</b> Date();</p>
        <p>item.setCreated(date);</p>
        <p>item.setUpdated(date);</p>
        <p>// 3、向商品表插入数据</p>
        <p>itemMapper.insert(item);</p>
        <p>// 4、创建一个TbItemDesc对象</p>
        <p>TbItemDesc itemDesc = <b>new</b> TbItemDesc();</p>
        <p>// 5、补全TbItemDesc的属性</p>
        <p>itemDesc.setItemId(itemId);</p>
        <p>itemDesc.setItemDesc(desc);</p>
        <p>itemDesc.setCreated(date);</p>
        <p>itemDesc.setUpdated(date);</p>
        <p>// 6、向商品描述表插入数据</p>
        <p>itemDescMapper.insert(itemDesc);</p>
        <p>// 7、E3Result.ok()</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>发布服务

### 1.4. 表现层

引用服务

#### 1.4.1.                  Controller

请求的url：/item/save

参数：TbItem item,String desc

返回值：E3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/save")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> E3Result saveItem(TbItem item, String desc) {</p>
        <p>E3Result result = itemService.addItem(item, desc);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>