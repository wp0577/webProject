# Redis集群搭建

### 1.1. redis-cluster架构图

![](../../.gitbook/assets/image%20%28173%29.png)

redis-cluster投票:容错

![](../../.gitbook/assets/image%20%28156%29.png)

架构细节:

\(1\)所有的redis节点彼此互联\(PING-PONG机制\),内部使用二进制协议优化传输速度和带宽.

\(2\)节点的fail是通过集群中超过半数的节点检测失效时才生效.

\(3\)客户端与redis节点直连,不需要中间proxy层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可

\(4\)redis-cluster把所有的物理节点映射到\[0-16383\]slot上,cluster 负责维护node&lt;-&gt;slot&lt;-&gt;value

Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，redis 先对 key 使用 crc16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点

![](../../.gitbook/assets/image%20%28239%29.png)

### 1.2. Redis集群的搭建

Redis集群中至少应该有三个节点。要保证集群的高可用，需要每个节点有一个备份机。

Redis集群至少需要6台服务器。

搭建伪分布式。可以使用一台虚拟机运行6个redis实例。需要修改redis的端口号7001-7006

#### 1.2.1.                  集群搭建环境

1、使用ruby脚本搭建集群。需要ruby的运行环境。

安装ruby

yum install ruby

yum install rubygems

2、安装ruby脚本运行使用的包。

\[root@localhost ~\]\# gem install redis-3.0.0.gem

Successfully installed redis-3.0.0

1 gem installed

Installing ri documentation for redis-3.0.0...

Installing RDoc documentation for redis-3.0.0...

\[root@localhost ~\]\#

\[root@localhost ~\]\# cd redis-3.0.0/src

\[root@localhost src\]\# ll \*.rb

-rwxrwxr-x. 1 root root 48141 Apr  1  2015 redis-trib.rb

#### 1.2.2.                  搭建步骤

需要6台redis服务器。搭建伪分布式。

需要6个redis实例。

需要运行在不同的端口7001-7006

第一步：创建6个redis实例，每个实例运行在不同的端口。需要修改redis.conf配置文件。配置文件中还需要把cluster-enabled yes前的注释去掉。

![](../../.gitbook/assets/image%20%28175%29.png)

第二步：启动每个redis实例。

第三步：使用ruby脚本搭建集群。

| ./redis-trib.rb create --replicas 1 47.90.242.48:7001 47.90.242.48:7002 47.90.242.48:7003 47.90.242.48:7004 47.90.242.48:7005 47.90.242.48:7006 |
| :--- |


创建关闭集群的脚本：

\[root@localhost redis-cluster\]\# vim shutdow-all.sh

redis01/redis-cli -p 7001 shutdown

redis01/redis-cli -p 7002 shutdown

redis01/redis-cli -p 7003 shutdown

redis01/redis-cli -p 7004 shutdown

redis01/redis-cli -p 7005 shutdown

redis01/redis-cli -p 7006 shutdown

\[root@localhost redis-cluster\]\# chmod u+x shutdow-all.sh

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>[root@localhost redis-cluster]# ./redis-trib.rb create --replicas 1 47.90.242.48:7001
          47.90.242.48:7002 47.90.242.48:7003 47.90.242.48:7004 47.90.242.48:7005
          47.90.242.48:7006</p>
        <p>>>> Creating cluster</p>
        <p>Connecting to node 47.90.242.48:7001: OK</p>
        <p>Connecting to node 47.90.242.48:7002: OK</p>
        <p>Connecting to node 47.90.242.48:7003: OK</p>
        <p>Connecting to node 47.90.242.48:7004: OK</p>
        <p>Connecting to node 47.90.242.48:7005: OK</p>
        <p>Connecting to node 47.90.242.48:7006: OK</p>
        <p>>>> Performing hash slots allocation on 6 nodes...</p>
        <p>Using 3 masters:</p>
        <p>47.90.242.48:7001</p>
        <p>47.90.242.48:7002</p>
        <p>47.90.242.48:7003</p>
        <p>Adding replica 47.90.242.48:7004 to 47.90.242.48:7001</p>
        <p>Adding replica 47.90.242.48:7005 to 47.90.242.48:7002</p>
        <p>Adding replica 47.90.242.48:7006 to 47.90.242.48:7003</p>
        <p>M: 2e48ae301e9c32b04a7d4d92e15e98e78de8c1f3 47.90.242.48:7001</p>
        <p>slots:0-5460 (5461 slots) master</p>
        <p>M: 8cd93a9a943b4ef851af6a03edd699a6061ace01 47.90.242.48:7002</p>
        <p>slots:5461-10922 (5462 slots) master</p>
        <p>M: 2935007902d83f20b1253d7f43dae32aab9744e6 47.90.242.48:7003</p>
        <p>slots:10923-16383 (5461 slots) master</p>
        <p>S: 74f9d9706f848471583929fc8bbde3c8e99e211b 47.90.242.48:7004</p>
        <p>replicates 2e48ae301e9c32b04a7d4d92e15e98e78de8c1f3</p>
        <p>S: 42cc9e25ebb19dda92591364c1df4b3a518b795b 47.90.242.48:7005</p>
        <p>replicates 8cd93a9a943b4ef851af6a03edd699a6061ace01</p>
        <p>S: 8b1b11d509d29659c2831e7a9f6469c060dfcd39 47.90.242.48:7006</p>
        <p>replicates 2935007902d83f20b1253d7f43dae32aab9744e6</p>
        <p>Can I set the above configuration? (type 'yes' to accept): yes</p>
        <p>>>> Nodes configuration updated</p>
        <p>>>> Assign a different config epoch to each node</p>
        <p>>>> Sending CLUSTER MEET messages to join the cluster</p>
        <p>Waiting for the cluster to join.....</p>
        <p>>>> Performing Cluster Check (using node 47.90.242.48:7001)</p>
        <p>M: 2e48ae301e9c32b04a7d4d92e15e98e78de8c1f3 47.90.242.48:7001</p>
        <p>slots:0-5460 (5461 slots) master</p>
        <p>M: 8cd93a9a943b4ef851af6a03edd699a6061ace01 47.90.242.48:7002</p>
        <p>slots:5461-10922 (5462 slots) master</p>
        <p>M: 2935007902d83f20b1253d7f43dae32aab9744e6 47.90.242.48:7003</p>
        <p>slots:10923-16383 (5461 slots) master</p>
        <p>M: 74f9d9706f848471583929fc8bbde3c8e99e211b 47.90.242.48:7004</p>
        <p>slots: (0 slots) master</p>
        <p>replicates 2e48ae301e9c32b04a7d4d92e15e98e78de8c1f3</p>
        <p>M: 42cc9e25ebb19dda92591364c1df4b3a518b795b 47.90.242.48:7005</p>
        <p>slots: (0 slots) master</p>
        <p>replicates 8cd93a9a943b4ef851af6a03edd699a6061ace01</p>
        <p>M: 8b1b11d509d29659c2831e7a9f6469c060dfcd39 47.90.242.48:7006</p>
        <p>slots: (0 slots) master</p>
        <p>replicates 2935007902d83f20b1253d7f43dae32aab9744e6</p>
        <p>[OK] All nodes agree about slots configuration.</p>
        <p>>>> Check for open slots...</p>
        <p>>>> Check slots coverage...</p>
        <p>[OK] All 16384 slots covered.</p>
        <p>[root@localhost redis-cluster]#</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 集群的使用方法

Redis-cli连接集群。

\[root@localhost redis-cluster\]\# redis01/redis-cli -p 7002 -c

-c：代表连接的是redis集群

