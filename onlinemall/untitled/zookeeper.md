# Zookeeper

## 1.1.1. Zookeeper介绍

官方推荐使用zookeeper注册中心。

注册中心负责服务地址的注册与查找，相当于目录服务，服务提供者和消费者只在启动时与注册中心交互，注册中心不转发请求，压力较小。使用dubbo-2.3.3以上版本，建议使用zookeeper注册中心。

Zookeeper是Apacahe Hadoop的子项目，是一个树型的目录服务，支持变更推送，适合作为Dubbo服务的注册中心，工业强度较高，可用于生产环境，并推荐使用

Zookeeper：

1、可以作为集群的管理工具使用。

2、可以集中管理配置文件。

## 1.1.2. Zookeeper的安装

安装环境：

Linux：centos6.4

Jdk:1.7以上版本

Zookeeper是java开发的可以运行在windows、linux环境。需要先安装jdk。

安装步骤：

第一步：安装jdk

第二步：把zookeeper的压缩包上传到linux系统。

第三步：解压缩压缩包

tar -zxvf zookeeper-3.4.6.tar.gz

第四步：进入zookeeper-3.4.6目录，创建data文件夹。

第五步：把/conf/zoo\_sample.cfg改名为zoo.cfg

\[root@localhost conf\]\# mv zoo\_sample.cfg zoo.cfg

第六步：修改data属性：dataDir=/root/zookeeper-3.4.6/data

第七步：启动zookeeper

\[root@localhost bin\]\# ./zkServer.sh start

关闭：\[root@localhost bin\]\# ./zkServer.sh stop

查看状态：\[root@localhost bin\]\# ./zkServer.sh status

**注意：需要关闭防火墙。**

