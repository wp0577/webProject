# 安装

进入http://activemq.apache.org/下载ActiveMQ

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

使用的版本是5.12.0

### 1.1. 安装环境：

1、需要jdk

2、安装Linux系统。生产环境都是Linux系统。

### 1.2. 安装步骤

第一步： 把ActiveMQ 的压缩包上传到Linux系统。

第二步：解压缩。

第三步：启动。

使用bin目录下的activemq命令启动：

\[root@localhost bin\]\# ./activemq start

关闭：

\[root@localhost bin\]\# ./activemq stop

查看状态：

\[root@localhost bin\]\# ./activemq status

注意：如果ActiveMQ整合spring使用不要使用activemq-all-5.12.0.jar包。建议使用5.11.2

进入管理后台：

[http://192.168.25.168:8161/admin](http://192.168.25.168:8161/admin)

用户名：admin

密码：admin

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

