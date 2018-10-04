# 全局异常处理

### 1.1. 处理思路

![](../.gitbook/assets/image%20%28149%29.png)

全局异常处理器整个系统只有一个，使用方法：1）需要实现一个接口HandlerExceptionResolver2）需要在springmvc中配置。 处理逻辑：捕获整个系统中发生的异常。1、异常写入日志文件2、及时通知开发人员。发邮件、短信。3、展示一个错误页面，例如：您的网络故障，请重试。    

### 1.2. 创建全局异常处理器

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> GlobalExceptionReslover <b>implements</b> HandlerExceptionResolver
          {</p>
        <p>Logger logger = LoggerFactory.getLogger(GlobalExceptionReslover.<b>class</b>);</p>
        <p>@Override</p>
        <p> <b>public</b> ModelAndView resolveException(HttpServletRequest request,
          HttpServletResponse response, Object handler,</p>
        <p>Exception ex) {</p>
        <p>//写日志文件</p>
        <p>logger.error("系统发生异常", ex);</p>
        <p>//发邮件、发短信</p>
        <p>//Jmail：可以查找相关的资料</p>
        <p>//需要在购买短信。调用第三方接口即可。</p>
        <p>//展示错误页面</p>
        <p>ModelAndView modelAndView = <b>new</b> ModelAndView();</p>
        <p>modelAndView.addObject("message", "系统发生异常，请稍后重试");</p>
        <p>modelAndView.setViewName("error/exception");</p>
        <p> <b>return</b> modelAndView;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. Springmvc中配置异常处理器

![](../.gitbook/assets/image%20%2835%29.png)



