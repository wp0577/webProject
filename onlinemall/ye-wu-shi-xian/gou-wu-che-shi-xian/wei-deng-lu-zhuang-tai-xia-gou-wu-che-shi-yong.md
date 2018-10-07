# 未登录状态下购物车使用



### 1.1. 添加购物车

#### 1.1.1.                  功能分析

在不登陆的情况下也可以添加购物车。把购物车信息写入cookie。

优点：

1、不占用服务端存储空间

2、用户体验好。

3、代码实现简单。

缺点：

1、cookie中保存的容量有限。最大4k

2、把购物车信息保存在cookie中，更换设备购物车信息不能同步。

改造商品详情页面

![](../../../.gitbook/assets/image%20%28283%29.png)

![](../../../.gitbook/assets/image%20%28269%29.png)

请求的url：/cart/add/{itemId}

参数：

1）商品id： Long itemId  
 2）商品数量： int num

业务逻辑：

1、从cookie中查询商品列表。

2、判断商品在商品列表中是否存在。

3、如果存在，商品数量相加。

4、不存在，根据商品id查询商品信息。

5、把商品添加到购车列表。

6、把购车商品列表写入cookie。

返回值：逻辑视图

Cookie保存购物车

1）key：TT\_CART

2）Value：购物车列表转换成json数据。需要对数据进行编码。

3）Cookie的有效期：保存7天。

商品列表：

List&lt;TbItem&gt;，每个商品数据使用TbItem保存。当根据商品id查询商品信息后，取第一张图片保存到image属性中即可。

读写cookie可以使用CookieUtils工具类实现。

#### 1.1.2.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> CartController {</p>
        <p>@Value("${TT_CART}")</p>
        <p> <b>private</b> String TT_CART;</p>
        <p>@Value("${CART_EXPIRE}")</p>
        <p> <b>private</b> Integer CART_EXPIRE;</p>
        <p>@Autowired</p>
        <p> <b>private</b> ItemService itemService;</p>
        <p>@RequestMapping("/cart/add/{itemId}")</p>
        <p> <b>public</b> String addCartItem(@PathVariable Long itemId, Integer num,</p>
        <p>HttpServletRequest request, HttpServletResponse response) {</p>
        <p>// 1、从cookie中查询商品列表。</p>
        <p>List
          <TbItem>cartList = getCartList(request);</p>
        <p>// 2、判断商品在商品列表中是否存在。</p>
        <p> <b>boolean</b> hasItem = <b>false</b>;</p>
        <p> <b>for</b> (TbItem tbItem : cartList) {</p>
        <p>//对象比较的是地址，应该是值的比较</p>
        <p> <b>if</b> (tbItem.getId() == itemId.longValue()) {</p>
        <p>// 3、如果存在，商品数量相加。</p>
        <p>tbItem.setNum(tbItem.getNum() + num);</p>
        <p>hasItem = <b>true</b>;</p>
        <p> <b>break</b>;</p>
        <p>}</p>
        <p>}</p>
        <p> <b>if</b> (!hasItem) {</p>
        <p>// 4、不存在，根据商品id查询商品信息。</p>
        <p>TbItem tbItem = itemService.getItemById(itemId);</p>
        <p>//取一张图片</p>
        <p>String image = tbItem.getImage();</p>
        <p> <b>if</b> (StringUtils.isNoneBlank(image)) {</p>
        <p>String[] images = image.split(",");</p>
        <p>tbItem.setImage(images[0]);</p>
        <p>}</p>
        <p>//设置购买商品数量</p>
        <p>tbItem.setNum(num);</p>
        <p>// 5、把商品添加到购车列表。</p>
        <p>cartList.add(tbItem);</p>
        <p>}</p>
        <p>// 6、把购车商品列表写入cookie。</p>
        <p>CookieUtils.setCookie(request, response, TT_CART, JsonUtils.objectToJson(cartList),
          CART_EXPIRE, <b>true</b>);</p>
        <p> <b>return</b> "cartSuccess";</p>
        <p>}</p>
        <p>/**</p>
        <p>* 从cookie中取购物车列表</p>
        <p>*
          <p>Title: getCartList</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>* <b>@param</b> request</p>
        <p>* <b>@return</b>
        </p>
        <p>*/</p>
        <p> <b>private</b> List
          <TbItem>getCartList(HttpServletRequest request) {</p>
        <p>//取购物车列表</p>
        <p>String json = CookieUtils.getCookieValue(request, TT_CART, <b>true</b>);</p>
        <p>//判断json是否为null</p>
        <p> <b>if</b> (StringUtils.isNotBlank(json)) {</p>
        <p>//把json转换成商品列表返回</p>
        <p>List
          <TbItem>list = JsonUtils.jsonToList(json, TbItem.<b>class</b>);</p>
        <p> <b>return</b> list;</p>
        <p>}</p>
        <p> <b>return</b>  <b>new</b> ArrayList
          <>();</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 展示购物车商品列表

请求的url:/cart/cart

参数：无

返回值：逻辑视图

业务逻辑：

1、从cookie中取商品列表。

2、把商品列表传递给页面。

#### 1.2.1.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/cart/cart")</p>
        <p> <b>public</b> String showCartList(HttpServletRequest request, Model model)
          {</p>
        <p>//取购物车商品列表</p>
        <p>List
          <TbItem>cartList = getCartList(request);</p>
        <p>//传递给页面</p>
        <p>model.addAttribute("cartList", cartList);</p>
        <p> <b>return</b> "cart";</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 修改购物车商品数量

#### 1.3.1.                  功能分析

1、在页面中可以修改商品数量

2、重新计算小计和总计。

3、修改需要写入cookie。

4、每次修改都需要向服务端发送一个ajax请求，在服务端修改cookie中的商品数量。

![](../../../.gitbook/assets/image%20%28240%29.png)

请求的url：/cart/update/num/{itemId}/{num}

参数：long itemId、int num

业务逻辑：

1、接收两个参数

2、从cookie中取商品列表

3、遍历商品列表找到对应商品

4、更新商品数量

5、把商品列表写入cookie。

6、响应e3Result。Json数据。

返回值：

 e3Result。Json数据

#### 1.3.2.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/cart/update/num/{itemId}/{num}")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> e3Result updateNum(@PathVariable Long itemId, @PathVariable
          Integer num,</p>
        <p>HttpServletRequest request, HttpServletResponse response) {</p>
        <p>// 1、接收两个参数</p>
        <p>// 2、从cookie中取商品列表</p>
        <p>List
          <TbItem>cartList = getCartList(request);</p>
        <p>// 3、遍历商品列表找到对应商品</p>
        <p> <b>for</b> (TbItem tbItem : cartList) {</p>
        <p> <b>if</b> (tbItem.getId() == itemId.longValue()) {</p>
        <p>// 4、更新商品数量</p>
        <p>tbItem.setNum(num);</p>
        <p>}</p>
        <p>}</p>
        <p>// 5、把商品列表写入cookie。</p>
        <p>CookieUtils.setCookie(request, response, TT_CART, JsonUtils.objectToJson(cartList),
          CART_EXPIRE, <b>true</b>);</p>
        <p>// 6、响应e3Result。Json数据。</p>
        <p> <b>return</b> e3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.3.3.                  解决请求\*.html后缀无法返回json数据的问题

在springmvc中请求\*.html不可以返回json数据。

修改web.xml，添加url拦截格式。

### 1.4. 删除购物车商品

#### 1.4.1.                  功能分析

请求的url：/cart/delete/{itemId}

参数：商品id

返回值：展示购物车列表页面。Url需要做redirect跳转。

业务逻辑：

1、从url中取商品id

2、从cookie中取购物车商品列表

3、遍历列表找到对应的商品

4、删除商品。

5、把商品列表写入cookie。

6、返回逻辑视图：在逻辑视图中做redirect跳转。

#### 1.4.2.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/cart/delete/{itemId}")</p>
        <p> <b>public</b> String deleteCartItem(@PathVariable Long itemId, HttpServletRequest
          request,</p>
        <p>HttpServletResponse response) {</p>
        <p>// 1、从url中取商品id</p>
        <p>// 2、从cookie中取购物车商品列表</p>
        <p>List
          <TbItem>cartList = getCartList(request);</p>
        <p>// 3、遍历列表找到对应的商品</p>
        <p> <b>for</b> (TbItem tbItem : cartList) {</p>
        <p> <b>if</b> (tbItem.getId() == itemId.longValue()) {</p>
        <p>// 4、删除商品。</p>
        <p>cartList.remove(tbItem);</p>
        <p> <b>break</b>;</p>
        <p>}</p>
        <p>}</p>
        <p>// 5、把商品列表写入cookie。</p>
        <p>CookieUtils.setCookie(request, response, TT_CART, JsonUtils.objectToJson(cartList),
          CART_EXPIRE, <b>true</b>);</p>
        <p>// 6、返回逻辑视图：在逻辑视图中做redirect跳转。</p>
        <p> <b>return</b> "redirect:/cart/cart.html";</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>