# 配置

数据库读写分离对于大型系统或者访问量很高的互联网应用来说，是必不可少的一个重要功能。对于MySQL来说，标准的读写分离是主从模式，一个写节点Master后面跟着多个读节点，读节点的数量取决于系统的压力，通常是1-3个读节点的配置

![](../../../.gitbook/assets/image%20%28188%29.png)

Mycat读写分离和自动切换机制，需要mysql的主从复制机制配合。

### 1.1    Mysql的主从复制

![](../../../.gitbook/assets/image%20%28177%29.png)

主从配置需要注意的地方

1、主DB server和从DB server数据库的版本一致

2、主DB server和从DB server数据库数据名称一致

3、主DB server开启二进制日志,主DB server和从DB server的server\_id都必须唯一

### 1.2    Mysql主服务器配置

第一步：修改my.conf文件：

在\[mysqld\]段下添加：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>binlog-do-db=db1</p>
        <p>binlog-ignore-db=mysql</p>
        <p>#启用二进制日志</p>
        <p>log-bin=mysql-bin</p>
        <p>#服务器唯一ID，一般取IP最后一段</p>
        <p>server-id=134</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第二步：重启mysql服务

service mysqld restart

第三步：建立帐户并授权slave

mysql&gt;GRANT FILE ON \*.\* TO 'backup'@'%' IDENTIFIED BY '123456';

mysql&gt;GRANT REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* to 'backup'@'%' identified by '123456';

\#一般不用root帐号，“%”表示所有客户端都可能连，只要帐号，密码正确，此处可用具体客户端IP代替，如192.168.145.226，加强安全。

刷新权限

mysql&gt; FLUSH PRIVILEGES;

查看mysql现在有哪些用户

mysql&gt;select user,host from mysql.user;

第四步：查询master的状态

mysql&gt; show master status;

+------------------+----------+--------------+------------------+-------------------+

\| File             \| Position \| Binlog\_Do\_DB \| Binlog\_Ignore\_DB \| Executed\_Gtid\_Set \|

+------------------+----------+--------------+------------------+-------------------+

\| mysql-bin.000001 \|      120 \| db1          \| mysql            \|                   \|

+------------------+----------+--------------+------------------+-------------------+

1 row in set  
  


### 1.3    Mysql从服务器配置

第一步：修改my.conf文件

\[mysqld\]

server-id=166

第二步：配置从服务器

mysql&gt;change master to master\_host='192.168.25.134',master\_port=3306,master\_user='backup',master\_password='123456',master\_log\_file='mysql-bin.000001',master\_log\_pos=120

注意语句中间不要断开，master\_port为mysql服务器端口号\(无引号\)，master\_user为执行同步操作的数据库账户，“120”无单引号\(此处的120就是show master status 中看到的position的值，这里的mysql-bin.000001就是file对应的值\)。

第二步：启动从服务器复制功能

Mysql&gt;start slave;

第三步：检查从服务器复制功能状态：

mysql&gt; show slave status

……………………\(省略部分\)

Slave\_IO\_Running: Yes //此状态必须YES

Slave\_SQL\_Running: Yes //此状态必须YES

……………………\(省略部分\)

注：Slave\_IO及Slave\_SQL进程必须正常运行，即YES状态，否则都是错误的状态\(如：其中一个NO均属错误\)。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>错误处理：</p>
        <p>如果出现此错误：</p>
        <p>Fatal error: The slave I/O thread stops because master and slave have
          equal MySQL server UUIDs; these UUIDs must be different for replication
          to work.</p>
        <p>因为是mysql是克隆的系统所以mysql的uuid是一样的，所以需要修改。</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>解决方法：</p>
        <p>删除/var/lib/mysql/auto.cnf文件，重新启动服务。</p>
      </td>
    </tr>
  </tbody>
</table>![](../../../.gitbook/assets/image%20%28194%29.png)

以上操作过程，从服务器配置完成。

### 1.4    Mycat配置

Mycat 1.4 支持MySQL主从复制状态绑定的读写分离机制，让读更加安全可靠，配置如下：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <dataNode name="dn1" dataHost="localhost1" database="db1" />
        </p>
        <p>
          <dataNode name="dn2" dataHost="localhost1" database="db2" />
        </p>
        <p>
          <dataNode name="dn3" dataHost="localhost1" database="db3" />
        </p>
        <p>
          <dataHost name="localhost1" maxCon="1000" minCon="10" <b>balance="1"</b>
        </p>
        <p> <b>writeType="0"</b> dbType="mysql" dbDriver="native" <b>switchType="2"  slaveThreshold="100"</b>></p>
        <p>
          <heartbeat><b>show slave status</b>
          </heartbeat>
        </p>
        <p>
          <writeHost host="hostM" url="192.168.25.134:3306" user="root" </p>
            <p>password="root"></p>
            <p>
              <readHost host="hostS" url="192.168.25.166:3306" user="root" </p>
                <p>password="root" /></p>
                <p>
          </writeHost>
          </p>
          <p>
            </dataHost>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**\(1\)** **设置** **balance="1"与writeType="0"**

Balance参数设置：

1. balance=“0”, 所有读操作都发送到当前可用的writeHost上。

2. balance=“1”，所有读操作都随机的发送到readHost。

3. balance=“2”，所有读操作都随机的在writeHost、readhost上分发

WriteType参数设置：

1. writeType=“0”, 所有写操作都发送到可用的writeHost上。

2. writeType=“1”，所有写操作都随机的发送到readHost。

3. writeType=“2”，所有写操作都随机的在writeHost、readhost分上发。

 “readHost是从属于writeHost的，即意味着它从那个writeHost获取同步数据，因此，当它所属的writeHost宕机了，则它也不会再参与到读写分离中来，即“不工作了”，这是因为此时，它的数据已经“不可靠”了。基于这个考虑，目前mycat 1.3和1.4版本中，若想支持MySQL一主一从的标准配置，并且在主节点宕机的情况下，从节点还能读取数据，则需要在Mycat里配置为两个writeHost并设置banlance=1。”

**\(2\)** **设置** **switchType="2" 与slaveThreshold="100"**

**switchType 目前有三种选择：**

**-1：表示不自动切换**

**1 ：默认值，自动切换**

**2 ：基于MySQL主从同步的状态决定是否切换**

“Mycat心跳检查语句配置为 show slave status ，dataHost 上定义两个新属性： switchType="2" 与slaveThreshold="100"，此时意味着开启MySQL主从复制状态绑定的读写分离与切换机制。Mycat心跳机制通过检测 show slave status 中的 "Seconds\_Behind\_Master", "Slave\_IO\_Running", "Slave\_SQL\_Running" 三个字段来确定当前主从同步的状态以及Seconds\_Behind\_Master主从复制时延。“

## 2   附：Centos6.5下安装mysql

第一步：查看mysql是否安装。

rpm -qa\|grep mysql

第二步：如果mysql的版本不是想要的版本。需要把mysql卸载。

yum remove mysql mysql-server mysql-libs mysql-common

rm -rf /var/lib/mysql

rm /etc/my.cnf

第三步：安装mysql。需要使用yum命令安装。在安装mysql之前需要安装mysql的下载源。需要从oracle的官方网站下载。

1）下载mysql的源包。

我们是centos6.4对应的rpm包为：mysql-community-release-el6-5.noarch.rpm

2）安装mysql下载源：

yum localinstall mysql-community-release-el6-5.noarch.rpm

\(![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)\)此附件可保存

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

3）在线安装mysql：

yum install mysql-community-server

第四步：启动mysql

service mysqld start

第五步：需要给root用户设置密码。

/usr/bin/mysqladmin -u root password 'new-password'　　// 为root账号设置密码

第六步：远程连接授权。

GRANT ALL PRIVILEGES ON \*.\* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;

注意：'myuser'、'mypassword' 需要替换成实际的用户名和密码。

