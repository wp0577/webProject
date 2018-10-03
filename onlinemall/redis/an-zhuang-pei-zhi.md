# 安装配置

### 1.1. Redis的安装

Redis是c语言开发的。

安装redis需要c语言的编译环境。如果没有gcc需要在线安装。yum install gcc-c++

安装步骤：

第一步：redis的源码包上传到linux系统。

第二步：解压缩redis。

第三步：编译。进入redis源码目录。make

第四步：安装。make install PREFIX=/usr/local/redis

PREFIX参数指定redis的安装目录。一般软件安装到/usr目录下

### 1.2. 连接redis

#### 1.2.1.                  redis的启动：

前端启动：在redis的安装目录下直接启动redis-server

\[root@localhost bin\]\# ./redis-server

后台启动：

把/root/redis-3.0.0/redis.conf复制到/usr/local/redis/bin目录下

\[root@localhost redis-3.0.0\]\# cp redis.conf /usr/local/redis/bin/

修改配置文件：

![](../../.gitbook/assets/image%20%2853%29.png)

\[root@localhost bin\]\# ./redis-server redis.conf

查看redis进程：

\[root@localhost bin\]\# ps aux\|grep redis

root      5190  0.1  0.3  33936  1712 ?        Ssl  18:23   0:00 ./redis-server \*:6379   

root      5196  0.0  0.1   4356   728 pts/0    S+   18:24   0:00 grep redis

\[root@localhost bin\]\#

#### 1.2.2.                  Redis-cli

\[root@localhost bin\]\# ./redis-cli

默认连接localhost运行在6379端口的redis服务。

\[root@localhost bin\]\# ./redis-cli -h 47.90.242.48 -p 6379

-h：连接的服务器的地址

-p：服务的端口号

关闭redis：\[root@localhost bin\]\# ./redis-cli shutdown

### 1.3. Redis五种数据类型

String：key-value（做缓存）

Redis中所有的数据都是字符串。命令不区分大小写，key是区分大小写的。Redis是单线程的。Redis中不适合保存内容大的数据。

get、set、

incr：加一（生成id）

Decr：减一

Hash：key-fields-values（做缓存）

相当于一个key对于一个map，map中还有key-value

使用hash对key进行归类。

Hset：向hash中添加内容

Hget：从hash中取内容

List：有顺序可重复

47.90.242.48:6379&gt; lpush list1 a b c d

\(integer\) 4

47.90.242.48:6379&gt; lrange list1 0 -1

1\) "d"

2\) "c"

3\) "b"

4\) "a"

47.90.242.48:6379&gt; rpush list1 1 2 3 4

\(integer\) 8

47.90.242.48:6379&gt; lrange list1 0 -1

1\) "d"

2\) "c"

3\) "b"

4\) "a"

5\) "1"

6\) "2"

7\) "3"

8\) "4"

47.90.242.48:6379&gt;

47.90.242.48:6379&gt; lpop list1

"d"

47.90.242.48:6379&gt; lrange list1 0 -1

1\) "c"

2\) "b"

3\) "a"

4\) "1"

5\) "2"

6\) "3"

7\) "4"

47.90.242.48:6379&gt; rpop list1

"4"

47.90.242.48:6379&gt; lrange list1 0 -1

1\) "c"

2\) "b"

3\) "a"

4\) "1"

5\) "2"

6\) "3"

47.90.242.48:6379&gt;

Set：元素无顺序，不能重复

47.90.242.48:6379&gt; sadd set1 a b c c c d

\(integer\) 4

47.90.242.48:6379&gt; smembers set1

1\) "b"

2\) "c"

3\) "d"

4\) "a"

47.90.242.48:6379&gt; srem set1 a

\(integer\) 1

47.90.242.48:6379&gt; smembers set1

1\) "b"

2\) "c"

3\) "d"

47.90.242.48:6379&gt;

还有集合运算命令，自学。

SortedSet（zset）：有顺序，不能重复

47.90.242.48:6379&gt; zadd zset1 2 a 5 b 1 c 6 d

\(integer\) 4

47.90.242.48:6379&gt; zrange zset1 0 -1

1\) "c"

2\) "a"

3\) "b"

4\) "d"

47.90.242.48:6379&gt; zrem zset1 a

\(integer\) 1

47.90.242.48:6379&gt; zrange zset1 0 -1

1\) "c"

2\) "b"

3\) "d"

47.90.242.48:6379&gt; zrevrange zset1 0 -1

1\) "d"

2\) "b"

3\) "c"

47.90.242.48:6379&gt; zrange zset1 0 -1 withscores

1\) "c"

2\) "1"

3\) "b"

4\) "5"

5\) "d"

6\) "6"

47.90.242.48:6379&gt; zrevrange zset1 0 -1 withscores

1\) "d"

2\) "6"

3\) "b"

4\) "5"

5\) "c"

6\) "1"

47.90.242.48:6379&gt;

### 1.4. Key命令

设置key的过期时间。

Expire key second：设置key的过期时间

Ttl key：查看key的有效期

Persist key：清除key的过期时间。Key持久化。

47.90.242.48:6379&gt; expire Hello 100

\(integer\) 1

47.90.242.48:6379&gt; ttl Hello

\(integer\) 77

