# Mycat分片

### 1.1    需求

把商品表分片存储到三个数据节点上。

### 1.2    安装环境分析

两台mysql数据库服务器：

Host1：192.168.25.134

Host2：192.168.25.166

host1环境

操作系统版本 : centos6.4

数据库版本 : mysql-5.6

mycat版本 ：1.4 release

数据库名 : db1、db3

mysql节点2环境

操作系统版本 : centos6.4

数据库版本 : mysql-5.6

mycat版本 ：1.4 release

数据库名 : db2

MyCat安装到节点1上（需要安装jdk）

### 1.3    配置schema.xml

#### 1.3.1Schema.xml介绍

Schema.xml作为MyCat中重要的配置文件之一，管理着MyCat的逻辑库、表、分片规则、DataNode以及DataSource。弄懂这些配置，是正确使用MyCat的前提。这里就一层层对该文件进行解析。

schema 标签用于定义MyCat实例中的逻辑库

Table 标签定义了MyCat中的逻辑表

dataNode 标签定义了MyCat中的数据节点，也就是我们通常说所的数据分片。

dataHost标签在mycat逻辑库中也是作为最底层的标签存在，直接定义了具体的数据库实例、读写分离配置和心跳语句。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>注意：若是LINUX版本的MYSQL，则需要设置为Mysql大小写不敏感，否则可能会发生表找不到的问题。</p>
        <p>在MySQL的配置文件中/etc/my.cnf [mysqld] 中增加一行</p>
        <p>　　lower_case_table_names=1</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.3.2Schema.xml配置

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" ?>
        </p>
        <p>
          <!DOCTYPE mycat:schema SYSTEM "schema.dtd">
        </p>
        <p>
          <mycat:schema xmlns:mycat="http://org.opencloudb/">
        </p>
        <p>
          <schema name="e3mall" checkSQLschema="false" sqlMaxLimit="100">
        </p>
        <p>
          <!-- auto sharding by id (long) -->
        </p>
        <p>
          <table name="tb_item" dataNode="dn1,dn2,dn3" rule="auto-sharding-long"
          />
        </p>
        <p>
          </schema>
        </p>
        <p>
          <dataNode name="dn1" dataHost="localhost1" database="db1" />
        </p>
        <p>
          <dataNode name="dn2" dataHost="localhost2" database="db2" />
        </p>
        <p>
          <dataNode name="dn3" dataHost="localhost1" database="db3" />
        </p>
        <p>
          <dataHost name="localhost1" maxCon="1000" minCon="10" balance="0" </p>
            <p>writeType="0" dbType="mysql" dbDriver="native" switchType="1" slaveThreshold="100"></p>
            <p>
              <heartbeat>select user()</heartbeat>
            </p>
            <p>
              <!-- can have multi write hosts -->
            </p>
            <p>
              <writeHost host="hostM1" url="192.168.25.134:3306" user="root" </p>
                <p>password="root"></p>
                <p>
                  <!-- can have multi read hosts -->
                </p>
                <p>
              </writeHost>
              </p>
              <p>
          </dataHost>
          </p>
          <p>
            <dataHost name="localhost2" maxCon="1000" minCon="10" balance="0" </p>
              <p>writeType="0" dbType="mysql" dbDriver="native" switchType="1" slaveThreshold="100"></p>
              <p>
                <heartbeat>select user()</heartbeat>
              </p>
              <p>
                <!-- can have multi write hosts -->
              </p>
              <p>
                <writeHost host="hostM1" url="192.168.25.166:3306" user="root" </p>
                  <p>password="root"></p>
                  <p>
                    <!-- can have multi read hosts -->
                  </p>
                  <p>
                </writeHost>
                </p>
                <p>
            </dataHost>
            </p>
            <p>
              </mycat:schema>
            </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4    配置server.xml

#### 1.4.1Server.xml介绍

server.xml几乎保存了所有mycat需要的系统配置信息。最常用的是在此配置用户名、密码及权限。

#### 1.4.2Server.xml配置

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <user name="test">
        </p>
        <p>
          <property name="password">test</property>
        </p>
        <p>
          <property name="schemas">e3mall</property>
        </p>
        <p>
          <property name="readOnly">false</property>
        </p>
        <p>
          </user>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.5    配置rule.xml

rule.xml里面就定义了我们对表进行拆分所涉及到的规则定义。我们可以灵活的对表使用不同的分片算法，或者对表使用相同的算法但具体的参数不同。这个文件里面主要有tableRule和function这两个标签。在具体使用过程中可以按照需求添加tableRule

和function。

此配置文件可以不用修改，使用默认即可。

### 1.6    测试分片

#### 1.6.1创建表

配置完毕后，重新启动mycat。使用mysql客户端连接mycat，创建表。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>-- ----------------------------</p>
        <p>-- Table structure for tb_item</p>
        <p>-- ----------------------------</p>
        <p>DROP TABLE IF EXISTS `tb_item`;</p>
        <p>CREATE TABLE `tb_item` (</p>
        <p>`id` bigint(20) NOT NULL COMMENT '商品id，同时也是商品编号',</p>
        <p>`title` varchar(100) NOT NULL COMMENT '商品标题',</p>
        <p>`sell_point` varchar(500) DEFAULT NULL COMMENT '商品卖点',</p>
        <p>`price` bigint(20) NOT NULL COMMENT '商品价格，单位为：分',</p>
        <p>`num` int(10) NOT NULL COMMENT '库存数量',</p>
        <p>`barcode` varchar(30) DEFAULT NULL COMMENT '商品条形码',</p>
        <p>`image` varchar(500) DEFAULT NULL COMMENT '商品图片',</p>
        <p>`cid` bigint(10) NOT NULL COMMENT '所属类目，叶子类目',</p>
        <p>`status` tinyint(4) NOT NULL DEFAULT '1' COMMENT '商品状态，1-正常，2-下架，3-删除',</p>
        <p>`created` datetime NOT NULL COMMENT '创建时间',</p>
        <p>`updated` datetime NOT NULL COMMENT '更新时间',</p>
        <p>PRIMARY KEY (`id`),</p>
        <p>KEY `cid` (`cid`),</p>
        <p>KEY `status` (`status`),</p>
        <p>KEY `updated` (`updated`)</p>
        <p>) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='商品表';</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.6.2插入数据

将此文件中的数据插入到数据库：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

#### 1.6.3分片测试

由于配置的分片规则为“auto-sharding-long”，所以mycat会根据此规则自动分片。

每个datanode中保存一定数量的数据。根据id进行分片

经测试id范围为：

Datanode1：1~5000000

Datanode2：5000000~10000000

Datanode3：10000001~15000000

当15000000以上的id插入时报错：

\[Err\] 1064 - can't find any valid datanode :TB\_ITEM -&gt; ID -&gt; 15000001

此时需要添加节点了。

