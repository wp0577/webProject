# 后台首页和页面跳转

### 1.1. 展示后台首页

#### 1.1.1.                  功能分析

请求的url：/

参数：无

返回值：逻辑视图String

#### 1.1.2.                  Controller



```text
/*  使用RESTful风格开发的接口，根据id查询商品，接口地址是：
    http://127.0.0.1/item/1
    我们需要从url上获取商品id，步骤如下：
    1. 使用注解@RequestMapping("item/{id}")声明请求的url
    {xxx}叫做占位符，请求的URL可以是“item /1”或“item/2”
    2. 使用(@PathVariable() Integer id)获取url上的数据*/
```

 

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> PageController {</p>
        <p>@RequestMapping("/")</p>
        <p> <b>public</b> String showIndex() {</p>
        <p> <b>return</b> "index";</p>
        <p>}</p>
        <p></p>
        <p> </p>
        <p>@RequestMapping("/{page}")</p>
        <p> <b>public</b> String showPage(@PathVariable String page) {</p>
        <p> <b>return</b> page;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 功能分析

#### 1.2.1.                  整合静态页面

静态页面位置：02.第二天（三大框架整合，后台系统搭建）\01.参考资料\后台管理系统静态页面

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

**使用方法:**

把静态页面添加到e3-manager-web工程中的WEB-INF下：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

由于在web.xml中定义的url拦截形式为“/”表示拦截所有的url请求，包括静态资源例如css、js等。所以需要在springmvc.xml中添加资源映射标签：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <mvc:resources location="/WEB-INF/js/" mapping="/js/**" />
        </p>
        <p>
          <mvc:resources location="/WEB-INF/css/" mapping="/css/**" />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>