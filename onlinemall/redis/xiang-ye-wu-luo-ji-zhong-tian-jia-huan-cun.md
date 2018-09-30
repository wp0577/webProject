# 向业务逻辑中添加缓存

### 1.1. 接口封装

常用的操作redis的方法提取出一个接口，分别对应单机版和集群版创建两个实现类。

#### 1.1.1.                  接口定义

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>interface</b> JedisClient {</p>
        <p>String set(String key, String value);</p>
        <p>String get(String key);</p>
        <p>Boolean exists(String key);</p>
        <p>Long expire(String key, <b>int</b> seconds);</p>
        <p>Long ttl(String key);</p>
        <p>Long incr(String key);</p>
        <p>Long hset(String key, String field, String value);</p>
        <p>String hget(String key, String field);</p>
        <p>Long hdel(String key, String... field);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.2.                  单机版实现类

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> JedisClientPool <b>implements</b> JedisClient {</p>
        <p>@Autowired</p>
        <p> <b>private</b> JedisPool jedisPool;</p>
        <p>@Override</p>
        <p> <b>public</b> String set(String key, String value) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>String result = jedis.set(key, value);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> String get(String key) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>String result = jedis.get(key);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Boolean exists(String key) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>Boolean result = jedis.exists(key);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long expire(String key, <b>int</b> seconds) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>Long result = jedis.expire(key, seconds);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long ttl(String key) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>Long result = jedis.ttl(key);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long incr(String key) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>Long result = jedis.incr(key);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long hset(String key, String field, String value) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>Long result = jedis.hset(key, field, value);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> String hget(String key, String field) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>String result = jedis.hget(key, field);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long hdel(String key, String... field) {</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>Long result = jedis.hdel(key, field);</p>
        <p>jedis.close();</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>配置：applicationContext-redis.xml

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"</p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util4.2.xsd"></p>
            <p>
              <!-- 配置单机版的连接 -->
            </p>
            <p>
              <bean id="jedisPool" class="redis.clients.jedis.JedisPool">
            </p>
            <p>
              <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
            </p>
            <p>
              <constructor-arg name="port" value="6379"></constructor-arg>
            </p>
            <p>
              </bean>
            </p>
            <p>
              <bean id="jedisClientPool" class="cn.e3mall.jedis.JedisClientPool" />
            </p>
            <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.3.                  集群版实现类

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>package</b> cn.e3mall.jedis;</p>
        <p><b>import</b> org.springframework.beans.factory.annotation.Autowired;</p>
        <p><b>import</b> redis.clients.jedis.JedisCluster;</p>
        <p><b>public</b>  <b>class</b> JedisClientCluster <b>implements</b> JedisClient
          {</p>
        <p>@Autowired</p>
        <p> <b>private</b> JedisCluster jedisCluster;</p>
        <p>@Override</p>
        <p> <b>public</b> String set(String key, String value) {</p>
        <p> <b>return</b> jedisCluster.set(key, value);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> String get(String key) {</p>
        <p> <b>return</b> jedisCluster.get(key);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Boolean exists(String key) {</p>
        <p> <b>return</b> jedisCluster.exists(key);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long expire(String key, <b>int</b> seconds) {</p>
        <p> <b>return</b> jedisCluster.expire(key, seconds);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long ttl(String key) {</p>
        <p> <b>return</b> jedisCluster.ttl(key);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long incr(String key) {</p>
        <p> <b>return</b> jedisCluster.incr(key);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long hset(String key, String field, String value) {</p>
        <p> <b>return</b> jedisCluster.hset(key, field, value);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> String hget(String key, String field) {</p>
        <p> <b>return</b> jedisCluster.hget(key, field);</p>
        <p>}</p>
        <p>@Override</p>
        <p> <b>public</b> Long hdel(String key, String... field) {</p>
        <p> <b>return</b> jedisCluster.hdel(key, field);</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>Spring的配置：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 集群版的配置 -->
        </p>
        <p>
          <bean id="jedisCluster" class="redis.clients.jedis.JedisCluster">
        </p>
        <p>
          <constructor-arg>
        </p>
        <p>
          <set>
        </p>
        <p>
          <bean class="redis.clients.jedis.HostAndPort">
        </p>
        <p>
          <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
        </p>
        <p>
          <constructor-arg name="port" value="7001"></constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          <bean class="redis.clients.jedis.HostAndPort">
        </p>
        <p>
          <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
        </p>
        <p>
          <constructor-arg name="port" value="7002"></constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          <bean class="redis.clients.jedis.HostAndPort">
        </p>
        <p>
          <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
        </p>
        <p>
          <constructor-arg name="port" value="7003"></constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          <bean class="redis.clients.jedis.HostAndPort">
        </p>
        <p>
          <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
        </p>
        <p>
          <constructor-arg name="port" value="7004"></constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          <bean class="redis.clients.jedis.HostAndPort">
        </p>
        <p>
          <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
        </p>
        <p>
          <constructor-arg name="port" value="7005"></constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          <bean class="redis.clients.jedis.HostAndPort">
        </p>
        <p>
          <constructor-arg name="host" value="47.90.242.48"></constructor-arg>
        </p>
        <p>
          <constructor-arg name="port" value="7006"></constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          </set>
        </p>
        <p>
          </constructor-arg>
        </p>
        <p>
          </bean>
        </p>
        <p>
          <bean id="jedisClientCluster" class="cn.e3mall.jedis.JedisClientCluster"
          />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>注意：单机版和集群版不能共存，使用单机版时注释集群版的配置。使用集群版，把单机版注释。

### 1.2. 封装代码测试

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testJedisClient() <b>throws</b> Exception {</p>
        <p>//初始化Spring容器</p>
        <p>ApplicationContext applicationContext = <b>new</b> ClassPathXmlApplicationContext("classpath:spring/applicationContext-redis.xml");</p>
        <p>//从容器中获得JedisClient对象</p>
        <p>JedisClient jedisClient = applicationContext.getBean(JedisClient.<b>class</b>);</p>
        <p>jedisClient.set("first", "100");</p>
        <p>String result = jedisClient.get("first");</p>
        <p>System.<b>out</b>.println(result);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 添加缓存

#### 1.3.1.                  功能分析

查询内容列表时添加缓存。

1、查询数据库之前先查询缓存。

2、查询到结果，直接响应结果。

3、查询不到，缓存中没有需要查询数据库。

4、把查询结果添加到缓存中。

5、返回结果。

向redis中添加缓存：

Key：cid

Value：内容列表。需要把java对象转换成json。

使用hash对key进行归类。

HASH\_KEY:HASH

            \|--KEY:VALUE

            \|--KEY:VALUE

            \|--KEY:VALUE

            \|--KEY:VALUE

**注意：添加缓存不能影响正常业务逻辑。**

#### 1.3.2.                  代码实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> List
          <TbContent>getContentList(<b>long</b> cid) {</p>
        <p>//查询缓存</p>
        <p> <b>try</b> {</p>
        <p>String json = jedisClient.hget(CONTENT_KEY, cid + "");</p>
        <p>//判断json是否为空</p>
        <p> <b>if</b> (StringUtils.isNotBlank(json)) {</p>
        <p>//把json转换成list</p>
        <p>List
          <TbContent>list = JsonUtils.jsonToList(json, TbContent.<b>class</b>);</p>
        <p> <b>return</b> list;</p>
        <p>}</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>//根据cid查询内容列表</p>
        <p>TbContentExample example = <b>new</b> TbContentExample();</p>
        <p>//设置查询条件</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>criteria.andCategoryIdEqualTo(cid);</p>
        <p>//执行查询</p>
        <p>List
          <TbContent>list = contentMapper.selectByExample(example);</p>
        <p>//向缓存中添加数据</p>
        <p> <b>try</b> {</p>
        <p>jedisClient.hset(CONTENT_KEY, cid + "", JsonUtils.objectToJson(list));</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p> <b>return</b> list;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. 缓存同步

对内容信息做增删改操作后只需要把对应缓存删除即可。

可以根据cid删除。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> E3Result addContent(TbContent content) {</p>
        <p>//补全属性</p>
        <p>content.setCreated(<b>new</b> Date());</p>
        <p>content.setUpdated(<b>new</b> Date());</p>
        <p>//插入数据</p>
        <p>contentMapper.insert(content);</p>
        <p>//缓存同步</p>
        <p>jedisClient.hdel(CONTENT_KEY, content.getCategoryId().toString());</p>
        <p> <b>return</b> E3Result.ok();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>