# Mysql在linux下安装

下载文件（https://pan.baidu.com/s/1c1VBcHy）放到目录：/usr/local/

 2、解压![&#x590D;&#x5236;&#x4EE3;&#x7801;](http://common.cnblogs.com/images/copycode.gif)

```text
　　cd /usr/local/
　　tar -zxvf mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz.tar.gz
　　mv mysql-5.7.20-linux-glibc2.12-x86_64/* mysql

　　groupadd mysql  //创建用户组mysql

　　useradd -r -g mysql mysql //-r参数表示mysql用户是系统用户，不可用于登录系统，创建用户mysql并将其添加到用户组mysql中

　　chown -R mysql mysql/

　　chgrp -R mysql mysql/
```

![&#x590D;&#x5236;&#x4EE3;&#x7801;](http://common.cnblogs.com/images/copycode.gif)

 3、创建配置文件

```text
　vim /etc/my.cnf
```

 　内容如下，可以添加你需要的配置：![&#x590D;&#x5236;&#x4EE3;&#x7801;](http://common.cnblogs.com/images/copycode.gif)

```text
[client]
port = 3306
socket = /tmp/mysql.sock

[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
socket=/tmp/mysql.sock
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

![&#x590D;&#x5236;&#x4EE3;&#x7801;](http://common.cnblogs.com/images/copycode.gif)

 保存内容,按esc输入如下命令

```text
:wq!
```

 4、初始化数据库

```text
/usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --lc_messages_dir=/usr/local/mysql/share --lc_messages=en_US
```

 5、查看初始密码

```text
cat /var/log/mysqld.log
```

 执行后关注最后一点：root@localhost: **这里就是初始密码**

 6、启动服务，进入mysql，修改初始密码，运行远程连接（这里执行完后，密码将变成：123456）![&#x590D;&#x5236;&#x4EE3;&#x7801;](http://common.cnblogs.com/images/copycode.gif)

```text
 /usr/local/mysql/support-files/mysql.server start
 /usr/local/mysql/mysql -uroot -p你在上面看到的初始密码

 // 以下是进入数据库之后的sql语句
 use mysql;

 UPDATE `mysql`.`user` SET `Host`='%', `User`='root', `Select_priv`='Y', `Insert_priv`='Y', `Update_priv`='Y', `Delete_priv`='Y', `Create_priv`='Y', `Drop_priv`='Y', `Reload_priv`='Y', `Shutdown_priv`='Y', `Process_priv`='Y', `File_priv`='Y', `Grant_priv`='Y', `References_priv`='Y', `Index_priv`='Y', `Alter_priv`='Y', `Show_db_priv`='Y', `Super_priv`='Y', `Create_tmp_table_priv`='Y', `Lock_tables_priv`='Y', `Execute_priv`='Y', `Repl_slave_priv`='Y', `Repl_client_priv`='Y', `Create_view_priv`='Y', `Show_view_priv`='Y', `Create_routine_priv`='Y', `Alter_routine_priv`='Y', `Create_user_priv`='Y', `Event_priv`='Y', `Trigger_priv`='Y', `Create_tablespace_priv`='Y', `ssl_type`='', `ssl_cipher`='', `x509_issuer`='', `x509_subject`='', `max_questions`='0', `max_updates`='0', `max_connections`='0', `max_user_connections`='0', `plugin`='mysql_native_password', `authentication_string`='*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9', `password_expired`='N', `password_last_changed`='2017-11-20 12:41:07', `password_lifetime`=NULL, `account_locked`='N' WHERE  (`User`='root');
```

```text
 flush privileges;
```

![&#x590D;&#x5236;&#x4EE3;&#x7801;](http://common.cnblogs.com/images/copycode.gif)

 7、开机自启

```text
cp mysql.server /etc/init.d/mysql
```

 8、使用service mysqld命令启动/停止服务

 例如我的mysql：启动/停止/暂停：

```text
service mysqld start/stop/restart
```

 --------------------- 本文来自 huaqiangu1123 的CSDN 博客 ，全文地址请点击：https://blog.csdn.net/huaqiangu1123/article/details/78758113?utm\_source=copy

