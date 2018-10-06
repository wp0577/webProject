# 实现

### 1.1. Dao层

查询tb\_item, tb\_item\_desc两个表，都是单表查询。可以使用逆向工程。

### 1.2. Service层

1、根据商品id查询商品信息

参数：商品id

返回值：TbItem

2、根据商品id查询商品描述

参数：商品id

返回值：TbItemDesc

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> TbItemDesc getItemDescById(<b>long</b> itemId) {</p>
        <p>TbItemDesc itemDesc = itemDescMapper.selectByPrimaryKey(itemId);</p>
        <p> <b>return</b> itemDesc;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>![](../../../.gitbook/assets/image%20%28229%29.png)

### 1.3. 表现层

#### 1.3.1.                  Controller

请求的url：/item/{itemId}

参数：商品id

返回值：String 逻辑视图

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> ItemController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> ItemService itemService;</p>
        <p>@RequestMapping("/item/{itemId}")</p>
        <p> <b>public</b> String showItemInfo(@PathVariable Long itemId, Model model)
          {</p>
        <p>//跟据商品id查询商品信息</p>
        <p>TbItem tbItem = itemService.getItemById(itemId);</p>
        <p>//把TbItem转换成Item对象</p>
        <p>Item item = <b>new</b> Item(tbItem);</p>
        <p>//根据商品id查询商品描述</p>
        <p>TbItemDesc tbItemDesc = itemService.getItemDescById(itemId);</p>
        <p>//把数据传递给页面</p>
        <p>model.addAttribute("item", item);</p>
        <p>model.addAttribute("itemDesc", tbItemDesc);</p>
        <p> <b>return</b> "item";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. 向业务逻辑中添加缓存

#### 1.4.1.                  缓存添加分析

使用redis做缓存。

业务逻辑：

1、根据商品id到缓存中命中

2、查到缓存，直接返回。

3、差不到，查询数据库

4、把数据放到缓存中

5、返回数据

缓存中缓存热点数据，提供缓存的使用率。需要设置缓存的有效期。一般是一天的时间，可以根据实际情况跳转。

需要使用String类型来保存商品数据。

可以加前缀方法对象redis中的key进行归类。

ITEM\_INFO:123456:BASE

ITEM\_INFO:123456:DESC

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

如果把二维表保存到redis中:

1、表名就是第一层

2、主键是第二层

3、字段名第三次

三层使用“:”分隔作为key，value就是字段中的内容。

#### 1.4.2.                  把redis相关的jar包添加到工程

![](../../../.gitbook/assets/image%20%2854%29.png)

#### 1.4.3.                  添加缓存

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> TbItem getItemById(<b>long</b> itemId) {</p>
        <p> <b>try</b> {</p>
        <p>//查询缓存</p>
        <p>String json = jedisClient.get(ITEM_INFO_PRE + ":" + itemId + ":BASE");</p>
        <p> <b>if</b> (StringUtils.isNotBlank(json)) {</p>
        <p>//把json转换为java对象</p>
        <p>TbItem item = JsonUtils.jsonToPojo(json, TbItem.<b>class</b>);</p>
        <p> <b>return</b> item;</p>
        <p>}</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>//根据商品id查询商品信息</p>
        <p>//TbItem tbItem = itemMapper.selectByPrimaryKey(itemId);</p>
        <p>TbItemExample example = <b>new</b> TbItemExample();</p>
        <p>//设置查询条件</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>criteria.andIdEqualTo(itemId);</p>
        <p>List
          <TbItem>list = itemMapper.selectByExample(example);</p>
        <p> <b>if</b> (list != <b>null</b> && list.size() > 0) {</p>
        <p>TbItem item = list.get(0);</p>
        <p> <b>try</b> {</p>
        <p>//把数据保存到缓存</p>
        <p>jedisClient.set(ITEM_INFO_PRE + ":" + itemId + ":BASE", JsonUtils.objectToJson(item));</p>
        <p>//设置缓存的有效期</p>
        <p>jedisClient.expire(ITEM_INFO_PRE + ":" + itemId + ":BASE", ITEM_INFO_EXPIRE);</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p> <b>return</b> item;</p>
        <p>}</p>
        <p> <b>return</b>  <b>null</b>;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>取商品描述添加缓存：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> TbItemDesc getItemDescById(<b>long</b> itemId) {</p>
        <p> <b>try</b> {</p>
        <p>String json = jedisClient.get(ITEM_INFO_PRE + ":" + itemId + ":DESC");</p>
        <p>//判断缓存是否命中</p>
        <p> <b>if</b> (StringUtils.isNotBlank(json) ) {</p>
        <p>//转换为java对象</p>
        <p>TbItemDesc itemDesc = JsonUtils.jsonToPojo(json, TbItemDesc.<b>class</b>);</p>
        <p> <b>return</b> itemDesc;</p>
        <p>}</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>TbItemDesc itemDesc = itemDescMapper.selectByPrimaryKey(itemId);</p>
        <p> <b>try</b> {</p>
        <p>jedisClient.set(ITEM_INFO_PRE + ":" + itemId + ":DESC", JsonUtils.objectToJson(itemDesc));</p>
        <p>//设置过期时间</p>
        <p>jedisClient.expire(ITEM_INFO_PRE + ":" + itemId + ":DESC", ITEM_INFO_EXPIRE);</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p> <b>return</b> itemDesc;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>