# 常见权限控制方法

privilege-demo

## 常见的权限控制方式 

###  URL拦截权限控制 

#### 底层基于拦截器或者过滤器实现

![](../../.gitbook/assets/image%20%2853%29.png)

###  方法注解权限控制 底层基于代理技术实现，为Action创建代理对象，由代理对象进行权限校验

![](../../.gitbook/assets/image%20%2834%29.png)



