# 登陆拦截器

#### 1.1.1.                  功能分析

1、从cookie中取token

2、如果没有取到，没有登录，跳转到sso系统的登录页面。拦截

3、如果取到token。判断登录是否过期，需要调用sso系统的服务，根据token取用户信息

4、如果没有取到用户信息，登录已经过期，重新登录。跳转到登录页面。拦截

5、如果取到用户信息，用户已经是登录状态，把用户信息保存到request中。放行

6、判断cookie中是否有购物车信息，如果有合并购物车

#### 1.1.2.                  拦截器实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 用户登录判断拦截器</p>
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
        <p>@Value("${COOKIE_CART_KEY}")</p>
        <p> <b>private</b> String COOKIE_CART_KEY;</p>
        <p>@Value("${SSO_URL}")</p>
        <p> <b>private</b> String SSO_URL;</p>
        <p>@Autowired</p>
        <p> <b>private</b> UserService userService;</p>
        <p>@Autowired</p>
        <p> <b>private</b> CartService cartService;</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> afterCompletion(HttpServletRequest arg0, HttpServletResponse
          arg1, Object arg2, Exception arg3)</p>
        <p> <b>throws</b> Exception {</p>
        <p>// <b>TODO</b> Auto-generated method stub</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> postHandle(HttpServletRequest arg0, HttpServletResponse
          arg1, Object arg2, ModelAndView arg3)</p>
        <p> <b>throws</b> Exception {</p>
        <p>// <b>TODO</b> Auto-generated method stub</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>boolean</b> preHandle(HttpServletRequest request, HttpServletResponse
          response, Object handler) <b>throws</b> Exception {</p>
        <p>// 1、从cookie中取token</p>
        <p>String token = CookieUtils.getCookieValue(request, COOKIE_TOKEN_KEY);</p>
        <p>// 2、如果没有取到，没有登录，跳转到sso系统的登录页面。拦截</p>
        <p> <b>if</b> (StringUtils.isBlank(token)) {</p>
        <p>//跳转到登录页面</p>
        <p>response.sendRedirect(SSO_URL + "/page/login?redirect=" + request.getRequestURL());</p>
        <p> <b>return</b>  <b>false</b>;</p>
        <p>}</p>
        <p>// 3、如果取到token。判断登录是否过期，需要调用sso系统的服务，根据token取用户信息</p>
        <p>E3Result e3Result = userService.getUserByToken(token);</p>
        <p>// 4、如果没有取到用户信息，登录已经过期，重新登录。跳转到登录页面。拦截</p>
        <p> <b>if</b> (e3Result.getStatus() != 200) {</p>
        <p>response.sendRedirect(SSO_URL + "/page/login?redirect=" + request.getRequestURL());</p>
        <p> <b>return</b>  <b>false</b>;</p>
        <p>}</p>
        <p>// 5、如果取到用户信息，用户已经是登录状态，把用户信息保存到request中。放行</p>
        <p>TbUser user = (TbUser) e3Result.getData();</p>
        <p>request.setAttribute("user", user);</p>
        <p>// 6、判断cookie中是否有购物车信息，如果有合并购物车</p>
        <p>String json = CookieUtils.getCookieValue(request, COOKIE_CART_KEY, <b>true</b>);</p>
        <p> <b>if</b> (StringUtils.isNotBlank(json)) {</p>
        <p>cartService.mergeCart(user.getId(), JsonUtils.jsonToList(json, TbItem.<b>class</b>));</p>
        <p>//删除cookie中的购物车数据</p>
        <p>CookieUtils.setCookie(request, response, COOKIE_CART_KEY, "");</p>
        <p>}</p>
        <p>//放行</p>
        <p> <b>return</b>  <b>true</b>;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.3.                  Springmvc配置

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
          <bean class="cn.e3mall.order.interceptor.LoginInterceptor" />
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
</table>#### 1.1.4.                  实现sso系统的回调

![](../../../.gitbook/assets/image%20%28134%29.png)



