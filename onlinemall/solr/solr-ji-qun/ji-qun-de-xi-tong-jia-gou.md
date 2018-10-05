# 集群的系统架构

## 1.  Solr集群的系统架构

![](../../../.gitbook/assets/image%20%2870%29.png)

### 1.1. 物理结构

三个Solr实例（ 每个实例包括两个Core），组成一个SolrCloud。

### 1.2. 逻辑结构

索引集合包括两个Shard（shard1和shard2），shard1和shard2分别由三个Core组成，其中一个Leader两个Replication，Leader是由zookeeper选举产生，zookeeper控制每个shard上三个Core的索引数据一致，解决高可用问题。

用户发起索引请求分别从shard1和shard2上获取，解决高并发问题。

#### 1.2.1.                  collection

Collection在SolrCloud集群中是一个逻辑意义上的完整的索引结构。它常常被划分为一个或多个Shard（分片），它们使用相同的配置信息。

比如：针对商品信息搜索可以创建一个collection。

 collection=shard1+shard2+....+shardX

#### 1.2.2.                  Core

每个Core是Solr中一个独立运行单位，提供 索引和搜索服务。一个shard需要由一个Core或多个Core组成。由于collection由多个shard组成所以collection一般由多个core组成。

#### 1.2.3.                  Master或Slave

Master是master-slave结构中的主结点（通常说主服务器），Slave是master-slave结构中的从结点（通常说从服务器或备服务器）。同一个Shard下master和slave存储的数据是一致的，这是为了达到高可用目的。

#### 1.2.4.                  Shard

Collection的逻辑分片。每个Shard被化成一个或者多个replication，通过选举确定哪个是Leader。

### 1.3. 需要实现的solr集群架构

![](../../../.gitbook/assets/image%20%28155%29.png)

Zookeeper作为集群的管理工具。

1、集群管理：容错、负载均衡。

2、配置文件的集中管理

3、集群的入口

需要实现zookeeper 高可用。需要搭建集群。建议是奇数节点。需要三个zookeeper服务器。

搭建solr集群需要7台服务器。

搭建伪分布式：

需要三个zookeeper节点

需要四个tomcat节点。

建议虚拟机的内容1G以上。

