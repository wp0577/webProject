# 添加商品通过ActiveMQ同步索引库

### 1.1. Producer

e3-manager-server工程中发送消息。

当商品添加完成后发送一个TextMessage，包含一个商品id。

![](../../.gitbook/assets/image%20%28238%29.png)

![](../../.gitbook/assets/image%20%2863%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> e3Result addItem(TbItem item, String desc) {</p>
        <p>// 1、生成商品id</p>
        <p> <b>final</b>  <b>long</b> itemId = IDUtils.genItemId();</p>
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
        <p>//发送一个商品添加消息</p>
        <p>jmsTemplate.send(topicDestination, <b>new</b> MessageCreator() {</p>
        <p>@Override</p>
        <p> <b>public</b> Message createMessage(Session session) <b>throws</b> JMSException
          {</p>
        <p>TextMessage textMessage = session.createTextMessage(itemId + "");</p>
        <p> <b>return</b> textMessage;</p>
        <p>}</p>
        <p>});</p>
        <p>// 7、e3Result.ok()</p>
        <p> <b>return</b> e3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>### 1.2. Consumer

#### 1.2.1.                  功能分析

1、接收消息。需要创建MessageListener接口的实现类。

2、取消息，取商品id。

3、根据商品id查询数据库。

4、创建一SolrInputDocument对象。

5、使用SolrServer对象写入索引库。

6、返回成功，返回e3Result。

#### 1.2.2.                  Dao层

根据商品id查询商品信息。

![](../../.gitbook/assets/image%20%28104%29.png)

映射文件：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <select id="getItemById" parameterType="long" resultType="cn.e3mall.common.pojo.SearchItem">
        </p>
        <p>SELECT</p>
        <p>a.id,</p>
        <p>a.title,</p>
        <p>a.sell_point,</p>
        <p>a.price,</p>
        <p>a.image,</p>
        <p>b. NAME category_name,</p>
        <p>c.item_desc</p>
        <p>FROM</p>
        <p>tb_item a</p>
        <p>JOIN tb_item_cat b ON a.cid = b.id</p>
        <p>JOIN tb_item_desc c ON a.id = c.item_id</p>
        <p>WHERE a.status = 1</p>
        <p>AND a.id=#{itemId}</p>
        <p>
          </select>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.3.                  Service层

参数：商品ID

业务逻辑：

1、根据商品id查询商品信息。

2、创建一SolrInputDocument对象。

3、使用SolrServer对象写入索引库。

4、返回成功，返回e3Result。

返回值：e3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b> e3Result addDocument(<b>long</b> itemId) <b>throws</b> Exception
          {</p>
        <p>// 1、根据商品id查询商品信息。</p>
        <p>SearchItem searchItem = searchItemMapper.getItemById(itemId);</p>
        <p>// 2、创建一SolrInputDocument对象。</p>
        <p>SolrInputDocument document = <b>new</b> SolrInputDocument();</p>
        <p>// 3、使用SolrServer对象写入索引库。</p>
        <p>document.addField("id", searchItem.getId());</p>
        <p>document.addField("item_title", searchItem.getTitle());</p>
        <p>document.addField("item_sell_point", searchItem.getSell_point());</p>
        <p>document.addField("item_price", searchItem.getPrice());</p>
        <p>document.addField("item_image", searchItem.getImage());</p>
        <p>document.addField("item_category_name", searchItem.getCategory_name());</p>
        <p>document.addField("item_desc", searchItem.getItem_desc());</p>
        <p>// 5、向索引库中添加文档。</p>
        <p>solrServer.add(document);</p>
        <p>solrServer.commit();</p>
        <p>// 4、返回成功，返回e3Result。</p>
        <p> <b>return</b> e3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.4.                  Listener

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> ItemChangeListener <b>implements</b> MessageListener
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> SearchItemServiceImpl searchItemServiceImpl;</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> onMessage(Message message) {</p>
        <p> <b>try</b> {</p>
        <p>TextMessage textMessage = <b>null</b>;</p>
        <p>Long itemId = <b>null</b>;</p>
        <p>//取商品id</p>
        <p> <b>if</b> (message <b>instanceof</b> TextMessage) {</p>
        <p>textMessage = (TextMessage) message;</p>
        <p>itemId = Long.parseLong(textMessage.getText());</p>
        <p>}</p>
        <p>//向索引库添加文档</p>
        <p>searchItemServiceImpl.addDocument(itemId);</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.5.                  Spring配置监听

![](../../.gitbook/assets/image%20%2831%29.png)

#### 1.2.6.                  实现流程

![](../../.gitbook/assets/image%20%2888%29.png)

