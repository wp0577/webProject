# 杂项



{% hint style="info" %}
```text
报错：redis.clients.jedis.exceptions.JedisClusterMaxRedirectionsException: Too many Cluster redirections?
是因为在redis服务器上我们搭建的指令是用服务器本地地址譬如127.0.0.1，而客户端调用的时候用的是公网地址，所以才会报这个错
```
{% endhint %}

{% hint style="info" %}
## 异常解决 java.io.FileNotFoundException: class path resource \[spring/springmvc.xml\]

[https://blog.csdn.net/moneyshi/article/details/53287036](https://blog.csdn.net/moneyshi/article/details/53287036)
{% endhint %}

{% hint style="info" %}
## IDEA下maven编译打包Java项目成jar包但是resource下配置文件打包不成功



```text
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
                <include>**/*.tld</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

 
{% endhint %}

