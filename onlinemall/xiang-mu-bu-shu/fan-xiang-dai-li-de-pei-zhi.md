# 反向代理的配置

测试时使用域名访问网站，需要修改host文件。

所有的域名应该指向反向代理服务器。

配置hosts文件：

192.168.25.141 manager.mallmall.cn

192.168.25.141 www.mallmall.cn

192.168.25.141 search.mallmall.cn

192.168.25.141 item.mallmall.cn

192.168.25.141 sso.mallmall.cn

192.168.25.141 cart.mallmall.cn

192.168.25.141 order.mallmall.cn

反向代理的配置：

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
        <p>upstream manager.mallmall.cn {</p>
        <p>server 192.168.25.137:8080;</p>
        <p>}</p>
        <p>upstream www.mallmall.cn {</p>
        <p>server 192.168.25.137:8081;</p>
        <p>}</p>
        <p>upstream search.mallmall.cn {</p>
        <p>server 192.168.25.137:8082;</p>
        <p>}</p>
        <p>upstream item.mallmall.cn {</p>
        <p>server 192.168.25.138:8080;</p>
        <p>}</p>
        <p>upstream sso.mallmall.cn {</p>
        <p>server 192.168.25.138:8081;</p>
        <p>}</p>
        <p>upstream cart.mallmall.cn {</p>
        <p>server 192.168.25.139:8080;</p>
        <p>}</p>
        <p>upstream order.mallmall.cn {</p>
        <p>server 192.168.25.139:8081;</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name manager.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://manager.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name www.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://www.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name search.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://search.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name item.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://item.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name sso.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://sso.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name cart.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://cart.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>server {</p>
        <p>listen 80;</p>
        <p>server_name order.mallmall.cn;</p>
        <p>#charset koi8-r;</p>
        <p>#access_log logs/host.access.log main;</p>
        <p>location / {</p>
        <p>proxy_pass http://order.mallmall.cn;</p>
        <p>index index.html index.htm;</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>