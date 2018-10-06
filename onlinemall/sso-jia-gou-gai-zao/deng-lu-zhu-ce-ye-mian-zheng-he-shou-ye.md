# 登录注册页面整合首页

### 1.1. 首页跳转到登录、注册页面

![](../../.gitbook/assets/image%20%2829%29.png)

### 1.2. 首页展示用户名

1、当用户登录成功后，在cookie中有token信息。

2、从cookie中取token根据token查询用户信息。

3、把用户名展示到首页。

方案一：在Controller中取cookie中的token数据，调用sso服务查询用户信息。

方案二：当页面加载完成后使用js取token的数据，使用ajax请求查询用户信息。

问题：服务接口在sso系统中。Sso.e3.com\(localhost:8088\)，在首页显示用户名称，首页的域名是www.e3.com\(localhost:8082\)，使用ajax请求跨域了。

Js不可以跨域请求数据。

什么是跨域：

1、域名不同

2、域名相同端口不同。

解决js的跨域问题可以使用jsonp。

Jsonp不是新技术，跨域的解决方案。使用js的特性绕过跨域请求。Js可以跨域加载js文件。

### 1.3. Jsonp原理

![](../../.gitbook/assets/image%20%28220%29.png)

### 1.4. Json实现

#### 1.4.1.                  客户端

使用jQuery。

#### 1.4.2.                  服务端

1、接收callback参数，取回调的js的方法名。

2、业务逻辑处理。

3、响应结果，拼接一个js语句。

![](../../.gitbook/assets/image%20%2859%29.png)



