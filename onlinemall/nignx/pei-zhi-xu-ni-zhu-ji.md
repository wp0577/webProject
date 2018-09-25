# 配置虚拟主机

就是在一台服务器启动多个网站。

如何区分不同的网站：

1、域名不同

2、端口不同

### 1.1. 通过端口区分不同虚拟机

Nginx的配置文件：

/usr/local/nginx/conf/nginx.conf

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>#user nobody;</p>
        <p>worker_processes 1;</p>
        <p>#error_log logs/error.log;</p>
        <p>#error_log logs/error.log notice;</p>
        <p>#error_log logs/error.log info;</p>
        <p>#pid logs/nginx.pid;</p>
        <p>events {</p>
        <p>worker_connections 1024;</p>
        <p>}</p>
        <p>http {</p>
        <p>include mime.types;</p>
        <p>default_type application/octet-stream;</p>
        <p>#log_format main '$remote_addr - $remote_user [$time_local] "$request"
          '</p>
        <p># '$status $body_bytes_sent "$http_referer" '</p>
        <p># '"$http_user_agent" "$http_x_forwarded_for"';</p>
        <p>#access_log logs/access.log main;</p>
        <p>sendfile on;</p>
        <p>#tcp_nopush on;</p>
        <p>#keepalive_timeout 0;</p>
        <p>keepalive_timeout 65;</p>
        <p>#gzip on;</p>
        <p>一个server节点就是一个虚拟主机
          <br />
        </p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name localhost;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>Html是nginx安装目录下的html目录
          <br />
        </p>
        <p>
          <img src="file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png"
          alt/>location / {</p>
        <p>root html;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>可以配置多个server，配置了多个虚拟主机。

添加虚拟主机：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>#user nobody;</p>
        <p>worker_processes 1;</p>
        <p>#error_log logs/error.log;</p>
        <p>#error_log logs/error.log notice;</p>
        <p>#error_log logs/error.log info;</p>
        <p>#pid logs/nginx.pid;</p>
        <p>events {</p>
        <p>worker_connections 1024;</p>
        <p>}</p>
        <p>http {</p>
        <p>include mime.types;</p>
        <p>default_type application/octet-stream;</p>
        <p>#log_format main '$remote_addr - $remote_user [$time_local] "$request"
          '</p>
        <p># '$status $body_bytes_sent "$http_referer" '</p>
        <p># '"$http_user_agent" "$http_x_forwarded_for"';</p>
        <p>#access_log logs/access.log main;</p>
        <p>sendfile on;</p>
        <p>#tcp_nopush on;</p>
        <p>#keepalive_timeout 0;</p>
        <p>keepalive_timeout 65;</p>
        <p>#gzip on;</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name localhost;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>root html;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 81;</p>
        <p>server_name localhost;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>root html-81;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>重新加载配置文件

\[root@localhost nginx\]\# sbin/nginx -s reload

### 1.2. 通过域名区分虚拟主机

#### 1.2.1.                  什么是域名

域名就是网站。

[www.baidu.com](http://www.baidu.com)

[www.taobao.com](http://www.taobao.com)

[www.jd.com](http://www.jd.com)

Tcp/ip

Dns服务器：把域名解析为ip地址。保存的就是域名和ip的映射关系。

一级域名：

Baidu.com

Taobao.com

Jd.com

二级域名：

[www.baidu.com](http://www.baidu.com)

Image.baidu.com

Item.baidu.com

三级域名：

1.Image.baidu.com

Aaa.image.baidu.com

一个域名对应一个ip地址，一个ip地址可以被多个域名绑定。

本地测试可以修改hosts文件。

修改window的hosts文件：（C:\Windows\System32\drivers\etc）

可以配置域名和ip的映射关系，如果hosts文件中配置了域名和ip的对应关系，不需要走dns服务器。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

#### 1.2.2.                  Nginx的配置

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>#user nobody;</p>
        <p>worker_processes 1;</p>
        <p>#error_log logs/error.log;</p>
        <p>#error_log logs/error.log notice;</p>
        <p>#error_log logs/error.log info;</p>
        <p>#pid logs/nginx.pid;</p>
        <p>events {</p>
        <p>worker_connections 1024;</p>
        <p>}</p>
        <p>http {</p>
        <p>include mime.types;</p>
        <p>default_type application/octet-stream;</p>
        <p>#log_format main '$remote_addr - $remote_user [$time_local] "$request"
          '</p>
        <p># '$status $body_bytes_sent "$http_referer" '</p>
        <p># '"$http_user_agent" "$http_x_forwarded_for"';</p>
        <p>#access_log logs/access.log main;</p>
        <p>sendfile on;</p>
        <p>#tcp_nopush on;</p>
        <p>#keepalive_timeout 0;</p>
        <p>keepalive_timeout 65;</p>
        <p>#gzip on;</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name localhost;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>root html;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 81;</p>
        <p>server_name localhost;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>root html-81;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name www.taobao.com;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>root html-taobao;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name www.baidu.com;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>root html-baidu;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>域名的配置：

192.168.25.148 www.taobao.com

192.168.25.148 www.baidu.com  


