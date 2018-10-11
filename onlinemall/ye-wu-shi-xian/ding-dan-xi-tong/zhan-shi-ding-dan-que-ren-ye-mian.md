# 展示订单确认页面

#### 1.1.1.                  功能分析

1、根据id查询用户的收货地址列表（使用静态数据）

2、从购物车中取商品列表，展示到页面。调用购物车服务查询。

#### 1.1.2.                  Dao层

直接从redis中取购车商品列表。

#### 1.1.3.                  Service层

收货地址静态数据，没有server。

调用购物车的service查询购物车商品列表。

#### 1.1.4.                  Controller

引用购物车服务

![](../../../.gitbook/assets/image%20%2817%29.png)

![](../../../.gitbook/assets/image%20%28192%29.png)

请求的url：/order/order-cart

参数：没有参数

返回值：逻辑视图

Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> OrderCartController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> CartService cartService;</p>
        <p>@RequestMapping("/order/order-cart")</p>
        <p> <b>public</b> String showOrderCart(HttpServletRequest request) {</p>
        <p>//取用户信息</p>
        <p>TbUser user = (TbUser) request.getAttribute("user");</p>
        <p>//取购物车商品列表</p>
        <p>List
          <TbItem>cartList = cartService.getCartList(user.getId());</p>
        <p>//把商品列表传递给jsp</p>
        <p>request.setAttribute("cartList", cartList);</p>
        <p>//返回逻辑视图</p>
        <p> <b>return</b> "order-cart";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>