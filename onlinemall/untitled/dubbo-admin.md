# Dubbo-admin

{% hint style="info" %}
项目提供的Dubbo-admin-2.5.4.war不能在jdk1.8的tomcat中运行，需要下载最新的dubbo-admin

[http://www.itmayun.com/it/files/226631678709806/resource/901920001882583/1.html](http://www.itmayun.com/it/files/226631678709806/resource/901920001882583/1.html)
{% endhint %}

需要安装tomcat，然后部署监控中心即可。

1、部署监控中心：

\[root@localhost ~\]\# cp dubbo-admin-2.5.4.war apache-tomcat-7.0.47/webapps/dubbo-admin.war

2、启动tomcat

3、访问http://192.168.25.167:8080/dubbo-admin/

用户名：root

密码：root

如果监控中心和注册中心在同一台服务器上，可以不需要任何配置。

如果不在同一台服务器，需要修改配置文件：

/root/apache-tomcat-7.0.47/webapps/dubbo-admin/WEB-INF/dubbo.properties

![](../../.gitbook/assets/image%20%2864%29.png)

