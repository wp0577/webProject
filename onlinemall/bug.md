# bug

{% hint style="info" %}
```text
报错：redis.clients.jedis.exceptions.JedisClusterMaxRedirectionsException: Too many Cluster redirections?
是因为在redis服务器上我们搭建的指令是用服务器本地地址譬如127.0.0.1，而客户端调用的时候用的是公网地址，所以才会报这个错
```
{% endhint %}



