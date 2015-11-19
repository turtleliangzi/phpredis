# phpredis
##一、redis简介
redis是一个key-value存储系统。和Memcached类似，它支持存储的value类型相对更多，包括string(字符串)、list(链表)、set(集合)、zset(sorted set --有序集合)和hash（哈希类型）。这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。在此基础上，redis支持各种不同方式的排序。与memcached一样，为了保证效率，数据都是缓存在内存中。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。
##二、安装redis
**1、下载redis**
```linux
　wget wget http://download.redis.io/releases/redis-3.0.5.tar.gz
```
**2、编辑和安装redis**
```linux
cd redis-3.0.5
make && make install
```
然后根据提示make test
```linux
make test
```
可能会需要8.5版本以上的tcl软件
```linux
yum istall tcl
```
**3、启动redis**
```linux
./redis-server ../redis.conf
```
会出现3条警告信息，一次按照提示解决就可以。
解决问题之后，重新启动redis
```linux
./redis-server ../redis.conf &
```
**4、测试redis**
```linux
[root@iZ237kng08vZ ~]# telnet localhost 6379
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
set testdata hello
+OK
get testdata  
$5
hello
quit
```
也可以通过redis客户端测试
```linux
[root@iZ237kng08vZ ~]# redis-cli
127.0.0.1:6379> set testdata turtletl
OK
127.0.0.1:6379> get testdata
"turtletl"
127.0.0.1:6379> exit
```
**5、改变redis的目录**
在/var下创击如下目录
```linux
mkdir /var/redis
mkdir /var/redis/6379
mkdir /var/redis/run
mkdir /var/redus/log

```
在/etc下创建redis目录
```linux
mkdir /etc/redis
```
复制/root/redis-3.0.5/redis.conf到/etc/redis/xia
```linux
cp /root/redis-3.0.5/redis.conf /etc/redis/6379.conf
```
修改6379.conf
```conf
daemonize yes
pidfile /var/redis/run/redis.pid
logfile /var/redis/log/redis_6379.log
dir /var/redis/6379
```
启动redis
```linux
[root@iZ237kng08vZ redis]# redis-server  /etc/redis/6379.conf 
```

使用redis-cli测试
```linux
127.0.0.1:6379> get testdata
(nil)
127.0.0.1:6379> set test hello
OK
127.0.0.1:6379> get test
"hello"
127.0.0.1:6379> exit

```
测试redis
```linux
[root@iZ237kng08vZ redis]# telnet localhost 6379
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
get test
$5
hello
quit
+OK
Connection closed by foreign host.
[root@iZ237kng08vZ redis]# 

```
##三、安装php-redis扩展
