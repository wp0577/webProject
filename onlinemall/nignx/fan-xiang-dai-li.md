# 反向代理

### 1.1. 什么是反向代理

正向代理

![](../../.gitbook/assets/image%20%28153%29.png)

反向代理：

![](../../.gitbook/assets/image%20%2864%29.png)

反向代理服务器决定哪台服务器提供服务。

返回代理服务器不提供服务器。也是请求的转发。

### 1.2. Nginx实现反向代理

两个域名指向同一台nginx服务器，用户访问不同的域名显示不同的网页内容。

两个域名是www.sian.com.cn和www.sohu.com

nginx服务器使用虚拟机192.168.101.3

![](../../.gitbook/assets/image%20%2816%29.png)

第一步：安装两个tomcat，分别运行在8080和8081端口。

第二步：启动两个tomcat。

第三步：反向代理服务器的配置

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>upstream tomcat1 {</p>
        <p>server 192.168.25.148:8080;</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name www.sina.com.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://tomcat1;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>upstream tomcat2 {</p>
        <p>server 192.168.25.148:8081;</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name www.sohu.com;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://tomcat2;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：nginx重新加载配置文件

第五步：配置域名

在hosts文件中添加域名和ip的映射关系

192.168.25.148 www.sina.com.cn

192.168.25.148 www.sohu.com

## 2.  负载均衡

如果一个服务由多条服务器提供，需要把负载分配到不同的服务器处理，需要负载均衡。

 upstream tomcat2 {

            server 192.168.25.148:8081;

            server 192.168.25.148:8082;

  }

可以根据服务器的实际情况调整服务器权重。权重越高分配的请求越多，权重越低，请求越少。默认是都是1

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>upstream tomcat2 {</p>
        <p>server 192.168.25.148:8081;</p>
        <p>server 192.168.25.148:8082 weight=2;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>