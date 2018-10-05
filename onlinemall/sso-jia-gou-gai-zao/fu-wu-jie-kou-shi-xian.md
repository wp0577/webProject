# 服务接口实现

### 1.1. 检查数据是否可用

#### 1.1.1.                  功能分析

请求的url：/user/check/{param}/{type}

参数：从url中取参数1、String param（要校验的数据）2、Integer type（校验的数据类型）

响应的数据：json数据。e3Result，封装的数据校验的结果true：成功false：失败。

业务逻辑：

1、从tb\_user表中查询数据

2、查询条件根据参数动态生成。

3、判断查询结果，如果查询到数据返回false。

4、如果没有返回true。

5、使用e3Result包装，并返回。

#### 1.1.2.                  Dao层

从tb\_user表查询。可以使用逆向工程。

#### 1.1.3.                  Service

参数：

1、要校验的数据：String param

2、数据类型：int type（1、2、3分别代表username、phone、email）

返回值：e3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p><b>public</b>  <b>class</b> UserServiceImpl <b>implements</b> UserService {</p>
        <p>@Autowired</p>
        <p> <b>private</b> TbUserMapper userMapper;</p>
        <p>@Override</p>
        <p> <b>public</b> e3Result checkData(String param, <b>int</b> type) {</p>
        <p>// 1、从tb_user表中查询数据</p>
        <p>TbUserExample example = <b>new</b> TbUserExample();</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>// 2、查询条件根据参数动态生成。</p>
        <p>//1、2、3分别代表username、phone、email</p>
        <p> <b>if</b> (type == 1) {</p>
        <p>criteria.andUsernameEqualTo(param);</p>
        <p>} <b>else</b>  <b>if</b> (type == 2) {</p>
        <p>criteria.andPhoneEqualTo(param);</p>
        <p>} <b>else</b>  <b>if</b> (type == 3) {</p>
        <p>criteria.andEmailEqualTo(param);</p>
        <p>} <b>else</b> {</p>
        <p> <b>return</b> e3Result.build(400, "非法的参数");</p>
        <p>}</p>
        <p>//执行查询</p>
        <p>List
          <TbUser>list = userMapper.selectByExample(example);</p>
        <p>// 3、判断查询结果，如果查询到数据返回false。</p>
        <p> <b>if</b> (list == <b>null</b> || list.size() == 0) {</p>
        <p>// 4、如果没有返回true。</p>
        <p> <b>return</b> e3Result.ok(<b>true</b>);</p>
        <p>}</p>
        <p>// 5、使用e3Result包装，并返回。</p>
        <p> <b>return</b> e3Result.ok(<b>false</b>);</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**发布服务**

#### 1.1.4.                  表现层

需要在e3-sso-web中实现。

**引用服务**

**Controller**

请求的url：/user/check/{param}/{type}

参数：从url中取参数1、String param（要校验的数据）2、Integer type（校验的数据类型）

响应的数据：json数据。e3Result，封装的数据校验的结果true：成功false：失败。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> UserController {</p>
        <p>@Autowired</p>
        <p> <b>private</b> UserService userService;</p>
        <p>@RequestMapping("/user/check/{param}/{type}")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> e3Result checkData(@PathVariable String param, @PathVariable
          Integer type) {</p>
        <p>e3Result e3Result = userService.checkData(param, type);</p>
        <p> <b>return</b> e3Result;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 用户注册

#### 1.2.1.                  功能分析

请求的url：/user/register

参数：表单的数据：username、password、phone、email

返回值：json数据。e3Result

接收参数：使用TbUser对象接收。

请求的方法：post

业务逻辑：

1、使用TbUser接收提交的请求。

2、补全TbUser其他属性。

3、密码要进行MD5加密。

4、把用户信息插入到数据库中。

5、返回e3Result。

#### 1.2.2.                  Dao层

可以使用逆向工程。

#### 1.2.3.                  Service层

参数：TbUser

返回值：e3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> e3Result createUser(TbUser user) {</p>
        <p>// 1、使用TbUser接收提交的请求。</p>
        <p> <b>if</b> (StringUtils.isBlank(user.getUsername())) {</p>
        <p> <b>return</b> e3Result.build(400, "用户名不能为空");</p>
        <p>}</p>
        <p> <b>if</b> (StringUtils.isBlank(user.getPassword())) {</p>
        <p> <b>return</b> e3Result.build(400, "密码不能为空");</p>
        <p>}</p>
        <p>//校验数据是否可用</p>
        <p>e3Result result = checkData(user.getUsername(), 1);</p>
        <p> <b>if</b> (!(<b>boolean</b>) result.getData()) {</p>
        <p> <b>return</b> e3Result.build(400, "此用户名已经被使用");</p>
        <p>}</p>
        <p>//校验电话是否可以</p>
        <p> <b>if</b> (StringUtils.isNotBlank(user.getPhone())) {</p>
        <p>result = checkData(user.getPhone(), 2);</p>
        <p> <b>if</b> (!(<b>boolean</b>) result.getData()) {</p>
        <p> <b>return</b> e3Result.build(400, "此手机号已经被使用");</p>
        <p>}</p>
        <p>}</p>
        <p>//校验email是否可用</p>
        <p> <b>if</b> (StringUtils.isNotBlank(user.getEmail())) {</p>
        <p>result = checkData(user.getEmail(), 3);</p>
        <p> <b>if</b> (!(<b>boolean</b>) result.getData()) {</p>
        <p> <b>return</b> e3Result.build(400, "此邮件地址已经被使用");</p>
        <p>}</p>
        <p>}</p>
        <p>// 2、补全TbUser其他属性。</p>
        <p>user.setCreated(<b>new</b> Date());</p>
        <p>user.setUpdated(<b>new</b> Date());</p>
        <p>// 3、密码要进行MD5加密。</p>
        <p>String md5Pass = DigestUtils.md5DigestAsHex(user.getPassword().getBytes());</p>
        <p>user.setPassword(md5Pass);</p>
        <p>// 4、把用户信息插入到数据库中。</p>
        <p>userMapper.insert(user);</p>
        <p>// 5、返回e3Result。</p>
        <p> <b>return</b> e3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**发布服务**

#### 1.2.4.                  表现层

引用服务。

Controller：

请求的url：/user/register

参数：表单的数据：username、password、phone、email

返回值：json数据。e3Result

接收参数：使用TbUser对象接收。

请求的方法：post

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping(value="/user/register", method=RequestMethod.<b>POST</b>)</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> e3Result register(TbUser user) {</p>
        <p>e3Result result = userService.createUser(user);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.5.                  测试

可以使用restclient-ui-3.5-jar-with-dependencies.jar测试接口。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

表单提交的content-type：application/x-www-form-urlencoded

### 1.3. 用户登录

#### 1.3.1.                  功能分析

请求的url：/user/login

请求的方法：POST

参数：username、password，表单提交的数据。可以使用方法的形参接收。

返回值：json数据，使用e3Result包含一个token。

业务逻辑：

登录的业务流程：

![](../../.gitbook/assets/image%20%28273%29.png)

![](../../.gitbook/assets/image%20%28134%29.png)

登录的处理流程：

1、登录页面提交用户名密码。

2、登录成功后生成token。Token相当于原来的jsessionid，字符串，可以使用uuid。

3、把用户信息保存到redis。Key就是token，value就是TbUser对象转换成json。

4、使用String类型保存Session信息。可以使用“前缀:token”为key

5、设置key的过期时间。模拟Session的过期时间。一般半个小时。

6、把token写入cookie中。

7、Cookie需要跨域。例如www.e3.com\sso.e3.com\order.e3.com，可以使用工具类。

8、Cookie的有效期。关闭浏览器失效。

9、登录成功。

#### 1.3.2.                  Dao层

查询tb\_user表。单表查询。可以使用逆向工程。

#### 1.3.3.                  Service层

参数：

1、用户名：String username

2、密码：String password

返回值：e3Result，包装token。

业务逻辑：

1、判断用户名密码是否正确。

2、登录成功后生成token。Token相当于原来的jsessionid，字符串，可以使用uuid。

3、把用户信息保存到redis。Key就是token，value就是TbUser对象转换成json。

4、使用String类型保存Session信息。可以使用“前缀:token”为key

5、设置key的过期时间。模拟Session的过期时间。一般半个小时。

6、返回e3Result包装token。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> e3Result login(String username, String password) {</p>
        <p>// 1、判断用户名密码是否正确。</p>
        <p>TbUserExample example = <b>new</b> TbUserExample();</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>criteria.andUsernameEqualTo(username);</p>
        <p>//查询用户信息</p>
        <p>List
          <TbUser>list = userMapper.selectByExample(example);</p>
        <p> <b>if</b> (list == <b>null</b> || list.size() == 0) {</p>
        <p> <b>return</b> e3Result.build(400, "用户名或密码错误");</p>
        <p>}</p>
        <p>TbUser user = list.get(0);</p>
        <p>//校验密码</p>
        <p> <b>if</b> (!user.getPassword().equals(DigestUtils.md5DigestAsHex(password.getBytes())))
          {</p>
        <p> <b>return</b> e3Result.build(400, "用户名或密码错误");</p>
        <p>}</p>
        <p>// 2、登录成功后生成token。Token相当于原来的jsessionid，字符串，可以使用uuid。</p>
        <p>String token = UUID.randomUUID().toString();</p>
        <p>// 3、把用户信息保存到redis。Key就是token，value就是TbUser对象转换成json。</p>
        <p>// 4、使用String类型保存Session信息。可以使用“前缀:token”为key</p>
        <p>user.setPassword(<b>null</b>);</p>
        <p>jedisClient.set(USER_INFO + ":" + token, JsonUtils.objectToJson(user));</p>
        <p>// 5、设置key的过期时间。模拟Session的过期时间。一般半个小时。</p>
        <p>jedisClient.expire(USER_INFO + ":" + token, SESSION_EXPIRE);</p>
        <p>// 6、返回e3Result包装token。</p>
        <p> <b>return</b> e3Result.ok(token);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**发布服务**

#### 1.3.4.                  表现层

引用服务：

**Controller**

请求的url：/user/login

请求的方法：POST

参数：username、password，表单提交的数据。可以使用方法的形参接收。

HttpServletRequest、HttpServletResponse

返回值：json数据，使用e3Result包含一个token。

业务逻辑：

1、接收两个参数。

2、调用Service进行登录。

3、从返回结果中取token，写入cookie。Cookie要跨域。

Cookie二级域名跨域需要设置:

1）setDomain，设置一级域名：

.itcatst.cn

.e3.com

.e3.com.cn

2）setPath。设置为“/”

工具类放到e3-common工程中。

4、响应数据。Json数据。e3Result，其中包含Token。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping(value="/user/login", method=RequestMethod.<b>POST</b>)</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> e3Result login(String username, String password,</p>
        <p>HttpServletRequest request, HttpServletResponse response) {</p>
        <p>// 1、接收两个参数。</p>
        <p>// 2、调用Service进行登录。</p>
        <p>e3Result result = userService.login(username, password);</p>
        <p>// 3、从返回结果中取token，写入cookie。Cookie要跨域。</p>
        <p>String token = result.getData().toString();</p>
        <p>CookieUtils.setCookie(request, response, COOKIE_TOKEN_KEY, token);</p>
        <p>// 4、响应数据。Json数据。e3Result，其中包含Token。</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. 通过token查询用户信息

#### 1.4.1.                  功能分析

请求的url：/user/token/{token}

参数：String token需要从url中取。

返回值：json数据。使用e3Result包装Tbuser对象。

业务逻辑：

1、从url中取参数。

2、根据token查询redis。

3、如果查询不到数据。返回用户已经过期。

4、如果查询到数据，说明用户已经登录。

5、需要重置key的过期时间。

6、把json数据转换成TbUser对象，然后使用e3Result包装并返回。

#### 1.4.2.                  Dao层

使用JedisClient对象。

#### 1.4.3.                  Service层

参数：String token

返回值：e3Result

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> e3Result getUserByToken(String token) {</p>
        <p>// 2、根据token查询redis。</p>
        <p>String json = jedisClient.get(USER_INFO + ":" + token);</p>
        <p> <b>if</b> (StringUtils.isBlank(json)) {</p>
        <p>// 3、如果查询不到数据。返回用户已经过期。</p>
        <p> <b>return</b> e3Result.build(400, "用户登录已经过期，请重新登录。");</p>
        <p>}</p>
        <p>// 4、如果查询到数据，说明用户已经登录。</p>
        <p>// 5、需要重置key的过期时间。</p>
        <p>jedisClient.expire(USER_INFO + ":" + token, SESSION_EXPIRE);</p>
        <p>// 6、把json数据转换成TbUser对象，然后使用e3Result包装并返回。</p>
        <p>TbUser user = JsonUtils.jsonToPojo(json, TbUser.<b>class</b>);</p>
        <p> <b>return</b> e3Result.ok(user);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.4.4.                  表现层

请求的url：/user/token/{token}

参数：String token需要从url中取。

返回值：json数据。使用e3Result包装Tbuser对象。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@RequestMapping("/user/token/{token}")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> e3Result getUserByToken(@PathVariable String token) {</p>
        <p>e3Result result = userService.getUserByToken(token);</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.4.5.                  安全退出

作业

需要根据token删除redis中的key。

