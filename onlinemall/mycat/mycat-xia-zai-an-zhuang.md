# Mycat下载安装

### 1.1    安装环境

1、jdk：要求jdk必须是1.7及以上版本

2、Mysql：推荐mysql是5.5以上版本

3、Mycat：

Mycat的官方网站：

[http://www.mycat.org.cn/](http://www.mycat.org.cn/)

下载地址：

[https://github.com/MyCATApache/Mycat-download](https://github.com/MyCATApache/Mycat-download)

### 1.2    安装步骤

Mycat有windows、linux多种版本。本教程为linux安装步骤，windows基本相同。

第一步：下载Mycat-server-xxxx-linux.tar.gz

第二步：将压缩包解压缩。建议将mycat放到/usr/local/mycat目录下。

第三步：进入mycat目录，启动mycat

./mycat start

停止：

./mycat stop

mycat 支持的命令{ console \| start \| stop \| restart \| status \| dump }

Mycat的默认端口号为：8066

