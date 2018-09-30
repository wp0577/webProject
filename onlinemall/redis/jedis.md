# Jedis



需要把jedis依赖的jar包添加到工程中。Maven工程中需要把jedis的坐标添加到依赖。

推荐添加到服务层。E3-content-Service工程中。

### 1.1. 连接单机版

第一步：创建一个Jedis对象。需要指定服务端的ip及端口。

第二步：使用Jedis对象操作数据库，每个redis命令对应一个方法。

第三步：打印结果。

第四步：关闭Jedis

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testJedis() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个Jedis对象。需要指定服务端的ip及端口。</p>
        <p>Jedis jedis = <b>new</b> Jedis("47.90.242.48", 6379);</p>
        <p>// 第二步：使用Jedis对象操作数据库，每个redis命令对应一个方法。</p>
        <p>String result = jedis.get("hello");</p>
        <p>// 第三步：打印结果。</p>
        <p>System.<b>out</b>.println(result);</p>
        <p>// 第四步：关闭Jedis</p>
        <p>jedis.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 连接单机版使用连接池

第一步：创建一个JedisPool对象。需要指定服务端的ip及端口。

第二步：从JedisPool中获得Jedis对象。

第三步：使用Jedis操作redis服务器。

第四步：操作完毕后关闭jedis对象，连接池回收资源。

第五步：关闭JedisPool对象。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testJedisPool() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个JedisPool对象。需要指定服务端的ip及端口。</p>
        <p>JedisPool jedisPool = <b>new</b> JedisPool("47.90.242.48", 6379);</p>
        <p>// 第二步：从JedisPool中获得Jedis对象。</p>
        <p>Jedis jedis = jedisPool.getResource();</p>
        <p>// 第三步：使用Jedis操作redis服务器。</p>
        <p>jedis.set("jedis", "test");</p>
        <p>String result = jedis.get("jedis");</p>
        <p>System.<b>out</b>.println(result);</p>
        <p>// 第四步：操作完毕后关闭jedis对象，连接池回收资源。</p>
        <p>jedis.close();</p>
        <p>// 第五步：关闭JedisPool对象。</p>
        <p>jedisPool.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. ？》》连接集群版

第一步：使用JedisCluster对象。需要一个Set&lt;HostAndPort&gt;参数。Redis节点的列表。

第二步：直接使用JedisCluster对象操作redis。在系统中单例存在。

第三步：打印结果

第四步：系统关闭前，关闭JedisCluster对象。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testJedisCluster() <b>throws</b> Exception {</p>
        <p>// 第一步：使用JedisCluster对象。需要一个Set
          <HostAndPort>参数。Redis节点的列表。</p>
        <p>Set
          <HostAndPort>nodes = <b>new</b> HashSet
            <>();</p>
        <p>nodes.add(<b>new</b> HostAndPort("47.90.242.48", 7001));</p>
        <p>nodes.add(<b>new</b> HostAndPort("47.90.242.48", 7002));</p>
        <p>nodes.add(<b>new</b> HostAndPort("47.90.242.48", 7003));</p>
        <p>nodes.add(<b>new</b> HostAndPort("47.90.242.48", 7004));</p>
        <p>nodes.add(<b>new</b> HostAndPort("47.90.242.48", 7005));</p>
        <p>nodes.add(<b>new</b> HostAndPort("47.90.242.48", 7006));</p>
        <p>JedisCluster jedisCluster = <b>new</b> JedisCluster(nodes);</p>
        <p>// 第二步：直接使用JedisCluster对象操作redis。在系统中单例存在。</p>
        <p>jedisCluster.set("hello", "100");</p>
        <p>String result = jedisCluster.get("hello");</p>
        <p>// 第三步：打印结果</p>
        <p>System.<b>out</b>.println(result);</p>
        <p>// 第四步：系统关闭前，关闭JedisCluster对象。</p>
        <p>jedisCluster.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>