# 提交订单

#### 1.1.1.                  功能分析

1、在订单确认页面点击“提交订单”按钮生成订单。

2、请求的url：/order/create

3、参数：提交的是表单的数据。保存的数据：订单、订单明细、配送地址。

a\)       向tb\_order中插入记录。

i.        订单号需要手动生成。

要求订单号不能重复。

订单号可读性号。

可以使用redis的incr命令生成订单号。订单号需要一个初始值。

ii.       Payment：表单数据

iii.     payment\_type：表单数据

iv.     user\_id：用户信息

v.       buyer\_nick：用户名

vi.     其他字段null

b\)      向tb\_order\_item订单明细表插入数据。

i.        Id：使用incr生成

ii.       order\_id：生成的订单号

iii.     其他的都是表单中的数据。

c\)       tb\_order\_shipping，订单配送信息

i.        order\_id：生成的订单号

ii.       其他字段都是表单中的数据。

d\)      使用pojo接收表单的数据。

可以扩展TbOrder，在子类中添加两个属性一个是商品明细列表，一个是配送信息。

把pojo放到e3-order-interface工程中。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> OrderInfo <b>extends</b> TbOrder <b>implements</b> Serializable{</p>
        <p> <b>private</b> List
          <TbOrderItem>orderItems;</p>
        <p> <b>private</b> TbOrderShipping orderShipping;</p>
        <p> <b>public</b> List
          <TbOrderItem>getOrderItems() {</p>
        <p> <b>return</b> orderItems;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setOrderItems(List
          <TbOrderItem>orderItems) {</p>
        <p> <b>this</b>.orderItems = orderItems;</p>
        <p>}</p>
        <p> <b>public</b> TbOrderShipping getOrderShipping() {</p>
        <p> <b>return</b> orderShipping;</p>
        <p>}</p>
        <p> <b>public</b>  <b>void</b> setOrderShipping(TbOrderShipping orderShipping)
          {</p>
        <p> <b>this</b>.orderShipping = orderShipping;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>业务逻辑：

1、接收表单的数据

2、生成订单id

3、向订单表插入数据。

4、向订单明细表插入数据

5、向订单物流表插入数据。

6、返回e3Result。

返回值：e3Result

#### 1.1.2.                  Dao层

可以使用逆向工程。

#### 1.1.3.                  Service层

参数：OrderInfo

返回值：e3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> OrderServiceImpl <b>implements</b> OrderService
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbOrderMapper orderMapper;</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbOrderItemMapper orderItemMapper;</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbOrderShippingMapper orderShippingMapper;</p>
        <p>@Autowired</p>
        <p> <b>private</b> JedisClient jedisClient;</p>
        <p>@Value("${ORDER_GEN_KEY}")</p>
        <p> <b>private</b> String ORDER_GEN_KEY;</p>
        <p>@Value("${ORDER_ID_BEGIN}")</p>
        <p> <b>private</b> String ORDER_ID_BEGIN;</p>
        <p>@Value("${ORDER_ITEM_ID_GEN_KEY}")</p>
        <p> <b>private</b> String ORDER_ITEM_ID_GEN_KEY;</p>
        <p>@Override</p>
        <p> <b>public</b> e3Result createOrder(OrderInfo orderInfo) {</p>
        <p>// 1、接收表单的数据</p>
        <p>// 2、生成订单id</p>
        <p> <b>if</b> (!jedisClient.exists(ORDER_GEN_KEY)) {</p>
        <p>//设置初始值</p>
        <p>jedisClient.set(ORDER_GEN_KEY, ORDER_ID_BEGIN);</p>
        <p>}</p>
        <p>String orderId = jedisClient.incr(ORDER_GEN_KEY).toString();</p>
        <p>orderInfo.setOrderId(orderId);</p>
        <p>orderInfo.setPostFee("0");</p>
        <p>//1、未付款，2、已付款，3、未发货，4、已发货，5、交易成功，6、交易关闭</p>
        <p>orderInfo.setStatus(1);</p>
        <p>Date date = <b>new</b> Date();</p>
        <p>orderInfo.setCreateTime(date);</p>
        <p>orderInfo.setUpdateTime(date);</p>
        <p>// 3、向订单表插入数据。</p>
        <p>orderMapper.insert(orderInfo);</p>
        <p>// 4、向订单明细表插入数据</p>
        <p>List
          <TbOrderItem>orderItems = orderInfo.getOrderItems();</p>
        <p> <b>for</b> (TbOrderItem tbOrderItem : orderItems) {</p>
        <p>//生成明细id</p>
        <p>Long orderItemId = jedisClient.incr(ORDER_ITEM_ID_GEN_KEY);</p>
        <p>tbOrderItem.setId(orderItemId.toString());</p>
        <p>tbOrderItem.setOrderId(orderId);</p>
        <p>//插入数据</p>
        <p>orderItemMapper.insert(tbOrderItem);</p>
        <p>}</p>
        <p>// 5、向订单物流表插入数据。</p>
        <p>TbOrderShipping orderShipping = orderInfo.getOrderShipping();</p>
        <p>orderShipping.setOrderId(orderId);</p>
        <p>orderShipping.setCreated(date);</p>
        <p>orderShipping.setUpdated(date);</p>
        <p>orderShippingMapper.insert(orderShipping);</p>
        <p>// 6、返回e3Result。</p>
        <p> <b>return</b> e3Result.ok(orderId);</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.4.                  Controller

请求的url：/order/create

参数：使用OrderInfo接收

返回值：逻辑视图。

业务逻辑：

1、接收表单提交的数据OrderInfo。

2、补全用户信息。

3、调用Service创建订单。

4、返回逻辑视图展示成功页面

a\)       需要Service返回订单号

b\)      当前日期加三天。

在拦截器中添加用户处理逻辑：

![](../../../.gitbook/assets/image%20%28253%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping(value="/order/create", method=RequestMethod.<b>POST</b>)</p>
        <p> <b>public</b> String createOrder(OrderInfo orderInfo, HttpServletRequest
          request) {</p>
        <p>// 1、接收表单提交的数据OrderInfo。</p>
        <p>// 2、补全用户信息。</p>
        <p>TbUser user = (TbUser) request.getAttribute("user");</p>
        <p>orderInfo.setUserId(user.getId());</p>
        <p>orderInfo.setBuyerNick(user.getUsername());</p>
        <p>// 3、调用Service创建订单。</p>
        <p>e3Result result = orderService.createOrder(orderInfo);</p>
        <p>//取订单号</p>
        <p>String orderId = result.getData().toString();</p>
        <p>// a)需要Service返回订单号</p>
        <p>request.setAttribute("orderId", orderId);</p>
        <p>request.setAttribute("payment", orderInfo.getPayment());</p>
        <p>// b)当前日期加三天。</p>
        <p>DateTime dateTime = <b>new</b> DateTime();</p>
        <p>dateTime = dateTime.plusDays(3);</p>
        <p>request.setAttribute("date", dateTime.toString("yyyy-MM-dd"));</p>
        <p>// 4、返回逻辑视图展示成功页面</p>
        <p> <b>return</b> "success";</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>