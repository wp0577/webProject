# 登录状态下使用

我的业务逻辑是登录状态和未登录时的购物车不共用

而该案例是将两个购物车合并

### 1.1. 功能分析

1、购物车数据保存的位置：

未登录状态下，把购物车数据保存到cookie中。

登录状态下，需要把购物车数据保存到服务端。需要永久保存，可以保存到数据库中。可以把购物车数据保存到redis中。

2、redis使用的数据类型

a\)       使用hash数据类型

b\)      Hash的key应该是用户id。Hash中的field是商品id，value可以把商品信息转换成json

3、添加购物车

登录状态下直接包商品数据保存到redis中。

未登录状态保存到cookie中。

4、如何判断是否登录？

a\)       从cookie中取token

b\)      取不到未登录

c\)       取到token，到redis中查询token是否过期。

d\)      如果过期，未登录状态

e\)       没过期登录状态。

### 1.2. 判断用户是否登录

#### 1.2.1.                  功能分析

应该使用拦截器实现。

1、实现一个HandlerInterceptor接口。

2、在执行handler方法之前做业务处理

3、从cookie中取token。使用CookieUtils工具类实现。

4、没有取到token，用户未登录。放行

5、取到token，调用sso系统的服务，根据token查询用户信息。

6、没有返回用户信息。登录已经过期，未登录，放行。

7、返回用户信息。用户是登录状态。可以把用户对象保存到request中，在Controller中可以通过判断request中是否包含用户对象，确定是否为登录状态。

#### 1.2.2.                  LoginInterceptor

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 判断用户是否登录的拦截器</p>
        <p>*
          <p>Title: LoginInterceptor</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>*
          <p>Company: www.itcast.cn</p>
        </p>
        <p>* <b>@version</b> 1.0</p>
        <p>*/</p>
        <p><b>public</b>  <b>class</b> LoginInterceptor <b>implements</b> HandlerInterceptor
          {</p>
        <p>@Value("${COOKIE_TOKEN_KEY}")</p>
        <p> <b>private</b> String COOKIE_TOKEN_KEY;</p>
        <p>@Autowired</p>
        <p> <b>private</b> UserService userService;</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>boolean</b> preHandle(HttpServletRequest request, HttpServletResponse
          response, Object handler) <b>throws</b> Exception {</p>
        <p>// 执行handler方法之前执行此方法</p>
        <p>// 1、实现一个HandlerInterceptor接口。</p>
        <p>// 2、在执行handler方法之前做业务处理</p>
        <p>// 3、从cookie中取token。使用CookieUtils工具类实现。</p>
        <p>String token = CookieUtils.getCookieValue(request, COOKIE_TOKEN_KEY);</p>
        <p>// 4、没有取到token，用户未登录。放行</p>
        <p> <b>if</b> (StringUtils.isBlank(token)) {</p>
        <p> <b>return</b>  <b>true</b>;</p>
        <p>}</p>
        <p>// 5、取到token，调用sso系统的服务，根据token查询用户信息。</p>
        <p>E3Result e3Result = userService.getUserByToken(token);</p>
        <p>// 6、没有返回用户信息。登录已经过期，未登录，放行。</p>
        <p> <b>if</b> (e3Result.getStatus() != 200) {</p>
        <p> <b>return</b>  <b>true</b>;</p>
        <p>}</p>
        <p>// 7、返回用户信息。用户是登录状态。可以把用户对象保存到request中，在Controller中可以通过判断request中是否包含用户对象，确定是否为登录状态。</p>
        <p>TbUser user = (TbUser) e3Result.getData();</p>
        <p>request.setAttribute("user", user);</p>
        <p>//返回true放行</p>
        <p>//返回false拦截</p>
        <p> <b>return</b>  <b>true</b>;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> postHandle(HttpServletRequest request, HttpServletResponse
          response, Object handler, ModelAndView modelAndView)</p>
        <p> <b>throws</b> Exception {</p>
        <p>// 执行handler方法之后，并且是返回ModelAndView对象之前</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> afterCompletion(HttpServletRequest request, HttpServletResponse
          response, Object handler, Exception ex)</p>
        <p> <b>throws</b> Exception {</p>
        <p>// 返回ModelAndView之后。可以捕获异常。</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.3.                  Springmvc.xml配置拦截器

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 拦截器配置 -->
        </p>
        <p>
          <mvc:interceptors>
        </p>
        <p>
          <mvc:interceptor>
        </p>
        <p>
          <mvc:mapping path="/**" />
        </p>
        <p>
          <bean class="cn.e3mall.cart.interceptor.LoginInterceptor" />
        </p>
        <p>
          </mvc:interceptor>
        </p>
        <p>
          </mvc:interceptors>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 添加购物车

#### 1.3.1.                  功能分析

登录状态下添加购物车，直接把数据保存到redis中。需要调用购物车服务，使用redis的hash来保存数据。

Key：用户id

Field：商品id

Value：商品对象转换成json

参数：

1、用户id

2、商品id

3、商品数量

业务逻辑：

1、根据商品id查询商品信息

2、把商品信息保存到redis

a\)       判断购物车中是否有此商品

b\)      如果有，数量相加

c\)       如果没有，根据商品id查询商品信息。

d\)      把商品信息添加到购物车

3、返回值。E3Result

#### 1.3.2.                  dao层

根据商品id查询商品信息，单表查询。可以使用逆向工程。

#### 1.3.3.                  Service层

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> CartServiceImpl <b>implements</b> CartService {</p>
        <p>@Value("${CART_REDIS_KEY}")</p>
        <p> <b>private</b> String CART_REDIS_KEY;</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbItemMapper itemMapper;</p>
        <p>@Autowired</p>
        <p> <b>private</b> JedisClient jedisClient;</p>
        <p>@Override</p>
        <p> <b>public</b> E3Result addCart(<b>long</b> userId, <b>long</b> itemId, <b>int</b> num)
          {</p>
        <p>// a)判断购物车中是否有此商品</p>
        <p>Boolean flag = jedisClient.hexists(CART_REDIS_KEY + ":" + userId, itemId
          + "");</p>
        <p>// b)如果有，数量相加</p>
        <p> <b>if</b> (flag) {</p>
        <p>//从hash中取商品数据</p>
        <p>String json = jedisClient.hget(CART_REDIS_KEY + ":" + userId, itemId +
          "");</p>
        <p>//转换成java对象</p>
        <p>TbItem tbItem = JsonUtils.jsonToPojo(json, TbItem.<b>class</b>);</p>
        <p>//数量相加</p>
        <p>tbItem.setNum(tbItem.getNum() + num);</p>
        <p>//写入hash</p>
        <p>jedisClient.hset(CART_REDIS_KEY + ":" + userId, itemId + "", JsonUtils.objectToJson(tbItem));</p>
        <p>//返回添加成功</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
        <p>// c)如果没有，根据商品id查询商品信息。</p>
        <p>TbItem tbItem = itemMapper.selectByPrimaryKey(itemId);</p>
        <p>//设置商品数量</p>
        <p>tbItem.setNum(num);</p>
        <p>String image = tbItem.getImage();</p>
        <p>//取一张图片</p>
        <p> <b>if</b> (StringUtils.isNotBlank(image)) {</p>
        <p>tbItem.setImage(image.split(",")[0]);</p>
        <p>}</p>
        <p>// d)把商品信息添加到购物车</p>
        <p>jedisClient.hset(CART_REDIS_KEY + ":" + userId, itemId + "", JsonUtils.objectToJson(tbItem));</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>发布服务：

![](../../../.gitbook/assets/image%20%28202%29.png)

#### 1.3.4.                  Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/cart/add/{itemId}")</p>
        <p> <b>public</b> String addCart(@PathVariable Long itemId, Integer num,</p>
        <p>HttpServletRequest request, HttpServletResponse response) {</p>
        <p>//判断用户是否为登录状态</p>
        <p>Object object = request.getAttribute("user");</p>
        <p> <b>if</b> (object != <b>null</b>) {</p>
        <p>TbUser user = (TbUser) object;</p>
        <p>//取用户id</p>
        <p>Long userId = user.getId();</p>
        <p>//添加到服务端</p>
        <p>E3Result e3Result = cartService.addCart(userId, itemId, num);</p>
        <p> <b>return</b> "cartSuccess";</p>
        <p>}</p>
        <p>//如果登录直接把购物车信息添加到服务端</p>
        <p>//如果未登录保存到cookie中</p>
        <p>// 1、从cookie中取购物车列表。</p>
        <p>List
          <TbItem>cartList = getItemListFromCookie(request);</p>
        <p>// 2、判断商品列表是否存在此商品。</p>
        <p> <b>boolean</b> falg = <b>false</b>;</p>
        <p> <b>for</b> (TbItem tbItem : cartList) {</p>
        <p> <b>if</b> (tbItem.getId() == itemId.longValue()) {</p>
        <p>//数量相加</p>
        <p>// 4、如果存在，数量相加。</p>
        <p>tbItem.setNum(tbItem.getNum() + num);</p>
        <p>falg = <b>true</b>;</p>
        <p> <b>break</b>;</p>
        <p>}</p>
        <p>}</p>
        <p>// 3、如果不存在添加到列表</p>
        <p> <b>if</b> (!falg) {</p>
        <p>//根据商品id取商品信息</p>
        <p>TbItem tbItem = itemService.getItemById(itemId);</p>
        <p>//设置数量</p>
        <p>tbItem.setNum(num);</p>
        <p>String image = tbItem.getImage();</p>
        <p>//取一张图片</p>
        <p> <b>if</b> (StringUtils.isNotBlank(image)) {</p>
        <p>tbItem.setImage(image.split(",")[0]);</p>
        <p>}</p>
        <p>//添加到列表</p>
        <p>cartList.add(tbItem);</p>
        <p>}</p>
        <p>// 5、把购车列表写入cookie</p>
        <p>CookieUtils.setCookie(request, response, COOKIE_CART_KEY, JsonUtils.objectToJson(cartList),
          COOKIE_CART_EXPIRE, <b>true</b>);</p>
        <p>// 6、返回逻辑视图，提示添加成功</p>
        <p> <b>return</b> "cartSuccess";</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. 登录状态下访问购物车列表

#### 1.4.1.                  功能分析

1、未登录状态下购物车列表是从cookie中取。

2、登录状态下购物车应该是从服务端取。

3、如果cookie中有购物车数据，应该吧cookie中的购物车和服务端合并，合并后删除cookie中的购物车数据。

4、合并购物车时，如果商品存在，数量相加，如果不存在，添加一个新的商品。

5、从服务端取购物车列表展示

#### 1.4.2.                  Dao层

不需要访问数据库，只需要访问redis。

#### 1.4.3.                  Service层

1、合并购物车

参数：用户id

      List&lt;TbItem&gt;

返回值：E3Result

业务逻辑：

1）遍历商品列表

2）如果服务端有相同商品，数量相加

3）如果没有相同商品，添加一个新的商品

2、取购物车列表

参数：用户id

返回值：List&lt;TbItem&gt;

业务逻辑：

1）从hash中取所有商品数据

2）返回

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 合并购物车</p>
        <p>*
          <p>Title: mergeCart</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>* <b>@param</b> userId</p>
        <p>* <b>@param</b> itemList</p>
        <p>* <b>@return</b>
        </p>
        <p>* <b>@see</b> cn.e3mall.cart.service.CartService#mergeCart(long, java.util.List)</p>
        <p>*/</p>
        <p>@Override</p>
        <p> <b>public</b> E3Result mergeCart(<b>long</b> userId, List
          <TbItem>itemList) {</p>
        <p>//遍历商品列表</p>
        <p> <b>for</b> (TbItem tbItem : itemList) {</p>
        <p>addCart(userId, tbItem.getId(), tbItem.getNum());</p>
        <p>}</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
        <p>/**</p>
        <p>* 取购物车列表</p>
        <p>*
          <p>Title: getCartList</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>* <b>@param</b> userId</p>
        <p>* <b>@return</b>
        </p>
        <p>* <b>@see</b> cn.e3mall.cart.service.CartService#getCartList(long)</p>
        <p>*/</p>
        <p>@Override</p>
        <p> <b>public</b> List
          <TbItem>getCartList(<b>long</b> userId) {</p>
        <p>//从redis中根据用户id查询商品列表</p>
        <p>List
          <String>strList = jedisClient.hvals(CART_REDIS_KEY + ":" + userId);</p>
        <p>List
          <TbItem>resultList = <b>new</b> ArrayList
            <>();</p>
        <p>//把json列表转换成TbItem列表</p>
        <p> <b>for</b> (String string : strList) {</p>
        <p>TbItem tbItem = JsonUtils.jsonToPojo(string, TbItem.<b>class</b>);</p>
        <p>//添加到列表</p>
        <p>resultList.add(tbItem);</p>
        <p>}</p>
        <p> <b>return</b> resultList;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.4.4.                  表现层

1、判断用户是否登录。

2、如果已经登录，判断cookie中是否有购物车信息

3、如果有合并购物车，并删除cookie中的购物车。

4、如果是登录状态，应从服务端取购物车列表。

5、如果是未登录状态，从cookie中取购物车列表

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/cart/cart")</p>
        <p> <b>public</b> String showCartList(HttpServletRequest request, HttpServletResponse
          response) {</p>
        <p>//从cookie中取购物车列表</p>
        <p>List
          <TbItem>cartList = getItemListFromCookie(request);</p>
        <p>//判断用户是否登录</p>
        <p>Object object = request.getAttribute("user");</p>
        <p> <b>if</b> (object != <b>null</b>) {</p>
        <p>TbUser user = (TbUser) object;</p>
        <p>//用户已经登录</p>
        <p>System.<b>out</b>.println("用户已经登录，用户名为：" + user.getUsername());</p>
        <p>//判断给我吃列表是否为空</p>
        <p> <b>if</b> (!cartList.isEmpty()) {</p>
        <p>//合并购物车</p>
        <p>cartService.mergeCart(user.getId(), cartList);</p>
        <p>//删除cookie中的购物车</p>
        <p>CookieUtils.setCookie(request, response, COOKIE_CART_KEY, "");</p>
        <p>}</p>
        <p>//从服务端取购物车列表</p>
        <p>List
          <TbItem>list = cartService.getCartList(user.getId());</p>
        <p>request.setAttribute("cartList", list);</p>
        <p> <b>return</b> "cart";</p>
        <p>} <b>else</b> {</p>
        <p>System.<b>out</b>.println("用户未登录");</p>
        <p>}</p>
        <p>//传递给页面</p>
        <p>request.setAttribute("cartList", cartList);</p>
        <p> <b>return</b> "cart";</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.5. 修改购物车数量

只需要更新hash中商品的数量即可。

不需要对数据库进行操作，只需要对redis操作即可。

#### 1.5.1.                  Server

参数：

1、用户id

2、商品id

3、数量

返回值：

E3Result

业务逻辑：

1、根据商品id从hash中取商品信息。

2、把json转换成java对象

3、更新商品数量

4、把商品数据写回hash

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> E3Result updateCartItemNum(<b>long</b> userId, <b>long</b> itemId, <b>int</b> num)
          {</p>
        <p>//从hash中取商品信息</p>
        <p>String json = jedisClient.hget(CART_REDIS_KEY + ":" + userId, itemId +
          "");</p>
        <p>//转换成java对象</p>
        <p>TbItem tbItem = JsonUtils.jsonToPojo(json, TbItem.<b>class</b>);</p>
        <p>//更新数量</p>
        <p>tbItem.setNum(num);</p>
        <p>//写入hash</p>
        <p>jedisClient.hset(CART_REDIS_KEY + ":" + userId, itemId + "", JsonUtils.objectToJson(tbItem));</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.5.2.                  Controller

1、判断是否为登录状态

2、如果是登录状态，更新服务端商品数量

3、如果未登录，更新cookie中是商品数量

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 更新商品数量</p>
        <p>*
          <p>Title: updateCartItemNum</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>* <b>@param</b> itemId</p>
        <p>* <b>@param</b> num</p>
        <p>* <b>@return</b>
        </p>
        <p>*/</p>
        <p>@RequestMapping("/cart/update/num/{itemId}/{num}")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> E3Result updateCartItemNum(@PathVariable Long itemId, @PathVariable
          Integer num,</p>
        <p>HttpServletRequest request, HttpServletResponse response) {</p>
        <p>//判断是否为登录状态</p>
        <p>Object object = request.getAttribute("user");</p>
        <p> <b>if</b> (object != <b>null</b>) {</p>
        <p>TbUser user = (TbUser) object;</p>
        <p>//更新服务端的购物车</p>
        <p>cartService.updateCartItemNum(user.getId(), itemId, num);</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
        <p>// 1、从cookie中取购物车列表</p>
        <p>List
          <TbItem>cartList = getItemListFromCookie(request);</p>
        <p>// 2、遍历列表找到对应的商品</p>
        <p> <b>for</b> (TbItem tbItem : cartList) {</p>
        <p> <b>if</b> (tbItem.getId() == itemId.longValue()) {</p>
        <p>// 3、更新商品数量</p>
        <p>tbItem.setNum(num);</p>
        <p> <b>break</b>;</p>
        <p>}</p>
        <p>}</p>
        <p>// 4、把购物车列表写入cookie</p>
        <p>CookieUtils.setCookie(request, response, COOKIE_CART_KEY, JsonUtils.objectToJson(cartList),
          COOKIE_CART_EXPIRE, <b>true</b>);</p>
        <p>//返回OK</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.6. 删除购物车商品

#### 1.6.1.                  业务逻辑

1、判断是否为登录状态

2、如果是登录状态，直接删除hash中的商品。

3、如果不是登录状态，对cookie中的购物车进行操作

#### 1.6.2.                  Service

参数：用户id

      商品id

返回值：E3Result

业务逻辑：

根据商品id删除hash中对应的商品数据。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> E3Result deleteCartItem(<b>long</b> userId, <b>long</b> itemId)
          {</p>
        <p>// 根据商品id删除hash中对应的商品数据。</p>
        <p>jedisClient.hdel(CART_REDIS_KEY + ":" + userId, itemId + "");</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.6.3.                  Controller

1、判断用户登录状态

2、如果登录删除服务端

3、如果未登录删除cookie中的购物车商品

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 删除购物车商品</p>
        <p>*
          <p>Title: deleteCartItem</p>
        </p>
        <p>*
          <p>Description:</p>
        </p>
        <p>* <b>@param</b> itemId</p>
        <p>* <b>@return</b>
        </p>
        <p>*/</p>
        <p>@RequestMapping("/cart/delete/{itemId}")</p>
        <p> <b>public</b> String deleteCartItem(@PathVariable Long itemId,</p>
        <p>HttpServletRequest request, HttpServletResponse response) {</p>
        <p>//判断用户登录状态</p>
        <p>Object object = request.getAttribute("user");</p>
        <p> <b>if</b> (object != <b>null</b>) {</p>
        <p>TbUser user = (TbUser) object;</p>
        <p>//删除服务端的购物车商品</p>
        <p>cartService.deleteCartItem(user.getId(), itemId);</p>
        <p> <b>return</b> "redirect:/cart/cart.html";</p>
        <p>}</p>
        <p>// 1、从url 中取商品id</p>
        <p>// 2、从cookie 中取购物车列表</p>
        <p>List
          <TbItem>cartList = getItemListFromCookie(request);</p>
        <p>// 3、遍历列表找到商品</p>
        <p> <b>for</b> (TbItem tbItem : cartList) {</p>
        <p> <b>if</b> (tbItem.getId() == itemId.longValue()) {</p>
        <p>// 4、删除商品</p>
        <p>cartList.remove(tbItem);</p>
        <p>//退出循环</p>
        <p> <b>break</b>;</p>
        <p>}</p>
        <p>}</p>
        <p>// 5、把购物车列表写入cookie</p>
        <p>CookieUtils.setCookie(request, response, COOKIE_CART_KEY, JsonUtils.objectToJson(cartList),
          COOKIE_CART_EXPIRE, <b>true</b>);</p>
        <p>// 6、返回逻辑视图。做redirect跳转到购物车列表页面。</p>
        <p> <b>return</b> "redirect:/cart/cart.html";</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>