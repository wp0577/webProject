# Jedis密码连接



```text
<!--配置jedis接口
    之所以在这用集群和单机的接口是因为初始化spring容器后，我们可以通过调用他们的父类来直接获得
    他们子类-->
<!--redis单机版-->
<bean id="jedisClientPool" class="com.wp.common.jedis.JedisClientPool">
    <property name="jedisPool" ref="jedisPool"></property>
</bean>
<bean id="jedisPool" class="redis.clients.jedis.JedisPool">
    <constructor-arg name="host" value="47.90.242.48"/>
    <constructor-arg name="port" value="6380"/>
    <constructor-arg name="password" value="wupan0577" />
    <constructor-arg name="poolConfig" ref="jedisPoolConfig"/>
    <constructor-arg name="timeout" value="2000" />
</bean>
<bean class="redis.clients.jedis.JedisPoolConfig" id="jedisPoolConfig">
</bean>
```

 

