# Redis持久化方案

Redis的所有数据都是保存到内存中的。

Rdb：快照形式，定期把内存中当前时刻的数据保存到磁盘。Redis默认支持的持久化方案。

aof形式：append only file。把所有对redis数据库操作的命令，增删改操作的命令。保存到文件中。数据库恢复时把所有的命令执行一遍即可。

在redis.conf配置文件中配置。

Rdb：

![](../../.gitbook/assets/image%20%2813%29.png)

Aof的配置：

![](../../.gitbook/assets/image%20%28218%29.png)

两种持久化方案同时开启使用aof文件来恢复数据库。

