# 登陆注册页面

### 1.1. 注册功能

第一步：把静态页面添加到工程中。

第二步：展示页面。

请求的url：

登录：/page/login

注册：/page/register

参数：无

返回结果：逻辑视图String

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> PageController {</p>
        <p>@RequestMapping("/page/register")</p>
        <p> <b>public</b> String showRegister() {</p>
        <p> <b>return</b> "register";</p>
        <p>}</p>
        <p>@RequestMapping("/page/login")</p>
        <p> <b>public</b> String showLogin() {</p>
        <p> <b>return</b> "login";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第三步：js处理。

![](../../.gitbook/assets/image%20%28289%29.png)

### 1.2. 登录功能

参考login.jsp

