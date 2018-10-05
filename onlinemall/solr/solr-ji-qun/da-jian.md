# 搭建

## 1.  环境准备

    CentOS-6.5-i386-bin-DVD1.iso

            jdk-7u72-linux-i586.tar.gz

    apache-tomcat-7.0.47.tar.gz

    zookeeper-3.4.6.tar.gz

    solr-4.10.3.tgz

## 2.  安装步骤

### 2.1. Zookeeper集群搭建

第一步：需要安装jdk环境。

第二步：把zookeeper的压缩包上传到服务器。

第三步：解压缩。

第四步：把zookeeper复制三份。

\[root@localhost ~\]\# mkdir /usr/local/solr-cloud

\[root@localhost ~\]\# cp -r zookeeper-3.4.6 /usr/local/solr-cloud/zookeeper01

\[root@localhost ~\]\# cp -r zookeeper-3.4.6 /usr/local/solr-cloud/zookeeper02

\[root@localhost ~\]\# cp -r zookeeper-3.4.6 /usr/local/solr-cloud/zookeeper03

第五步：在每个zookeeper目录下创建一个data目录。

第六步：在data目录下创建一个myid文件，文件名就叫做“myid”。内容就是每个实例的id。例如1、2、3

\[root@localhost data\]\# echo 1 &gt;&gt; myid

\[root@localhost data\]\# ll

total 4

-rw-r--r--. 1 root root 2 Apr  7 18:23 myid

\[root@localhost data\]\# cat myid

1

第七步：修改配置文件。把conf目录下的zoo\_sample.cfg文件改名为zoo.cfg

![](../../../.gitbook/assets/image%20%28212%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>server.1=192.168.25.154:2881:3881</p>
        <p>server.2=192.168.25.154:2882:3882</p>
        <p>server.3=192.168.25.154:2883:3883</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第八步：启动每个zookeeper实例。

启动bin/zkServer.sh start

查看zookeeper的状态：

bin/zkServer.sh status

### 2.2. Solr集群的搭建

第一步：创建四个tomcat实例。每个tomcat运行在不同的端口。8180、8280、8380、8480

第二步：部署solr的war包。把单机版的solr工程复制到集群中的tomcat中。

第三步：为每个solr实例创建一个对应的solrhome。使用单机版的solrhome复制四份。

第四步：需要修改solr的web.xml文件。把solrhome关联起来。

第五步：配置solrCloud相关的配置。每个solrhome下都有一个solr.xml，把其中的ip及端口号配置好。

![](../../../.gitbook/assets/image%20%28225%29.png)

第六步：让zookeeper统一管理配置文件。需要把solrhome/collection1/conf目录上传到zookeeper。上传任意solrhome中的配置文件即可。

使用工具上传配置文件：/root/solr-4.10.3/example/scripts/cloud-scripts/zkcli.sh

| ./zkcli.sh -zkhost 192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183 -cmd upconfig -confdir /usr/local/solr-cloud/solrhome01/collection1/conf -confname myconf |
| :--- |


查看zookeeper上的配置文件：

使用zookeeper目录下的bin/zkCli.sh命令查看zookeeper上的配置文件：

\[root@localhost bin\]\# ./zkCli.sh

\[zk: localhost:2181\(CONNECTED\) 0\] ls /

\[configs, zookeeper\]

\[zk: localhost:2181\(CONNECTED\) 1\] ls /configs

\[myconf\]

\[zk: localhost:2181\(CONNECTED\) 2\] ls /configs/myconf

\[admin-extra.menu-top.html, currency.xml, protwords.txt, mapping-FoldToASCII.txt, \_schema\_analysis\_synonyms\_english.json, \_rest\_managed.json, solrconfig.xml, \_schema\_analysis\_stopwords\_english.json, stopwords.txt, lang, spellings.txt, mapping-ISOLatin1Accent.txt, admin-extra.html, xslt, synonyms.txt, scripts.conf, update-script.js, velocity, elevate.xml, admin-extra.menu-bottom.html, clustering, schema.xml\]

\[zk: localhost:2181\(CONNECTED\) 3\]

退出：

\[zk: localhost:2181\(CONNECTED\) 3\] quit

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>使用以下命令连接指定的zookeeper服务：</p>
        <p>./zkCli.sh -server 192.168.25.154:2183</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第七步：修改tomcat/bin目录下的catalina.sh 文件，关联solr和zookeeper。

把此配置添加到配置文件中：

JAVA\_OPTS="-DzkHost=192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183"

![](../../../.gitbook/assets/image%20%28262%29.png)

第八步：启动每个tomcat实例。要包装zookeeper集群是启动状态。

第九步：访问集群

![](../../../.gitbook/assets/image%20%2814%29.png)

第十步：创建新的Collection进行分片处理。

http://192.168.25.154:8180/solr/admin/collections?action=CREATE&name=collection2&numShards=2&replicationFactor=2

![](../../../.gitbook/assets/image%20%28119%29.png)

![](../../../.gitbook/assets/image%20%28254%29.png)

第十一步：删除不用的Collection。

http://192.168.25.154:8180/solr/admin/collections?action=DELETE&name=collection1

![](../../../.gitbook/assets/image%20%28109%29.png)

![](../../../.gitbook/assets/image%20%28230%29.png)



