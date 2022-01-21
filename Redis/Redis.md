# 基础

## 简介

官网：[http://redis.io](http://redis.io/)

redis定义：是开源、BSD许可、高级的key-value存储系统。可以用来存储字符串、哈希结构、链表、集合，因此常用来提供数据结构服务。

## 安装



### 1. 在主机上安装

| 名称            | 操作                                         |
| :-------------- | :------------------------------------------- |
| 下载redis源代码 | wget http://t.cn/Eqw5XJ3                     |
| 解压            | tar -zxvf 5.0.2.tar                          |
| 进入目录        | cd redis-5.0.2                               |
| 编译            | make                                         |
| 安装指定目录    | make install PREFIX=/usr/local/redis install |
| 复制配置文件    | cp redis.conf /usr/local/redis/              |

**(1) 修改后修改配置文件**

```bash
# 打开配置文件
vim /usr/local/redis/redis.conf

# 使redis启动后在后台运行
daemonize yes

# 设置密码，默认密码为空
requirepass 123456

# 允许远程连接，取消绑定本机
#bind 127.0.0.1
```



**(2) 设置开机启动**

在自启动/etc/init.d/文件夹下新建名字为redis的启动脚本，内容如下：

```bash
#!/bin/sh

# chkconfig:   2345 90 10
# description:  Redis is a persistent key-value database
  
PATH=/usr/local/bin:/sbin:/usr/bin:/bin  
     
REDISPORT=6379  
EXEC=/usr/local/redis/bin/redis-server  
REDIS_CLI=/usr/local/redis/bin/redis-cli  
     
PIDFILE=/var/run/redis_6379.pid 
CONF="/usr/local/redis/redis.conf"  
     
case "$1" in  
    start)  
        if [ -f $PIDFILE ]  
        then  
                echo "$PIDFILE exists, process is already running or crashed"  
        else  
                echo "Starting Redis server..."  
                $EXEC $CONF  
        fi  
        if [ "$?"="0" ]   
        then  
              echo "Redis is running..."  
        fi  
        ;;  
    stop)  
        if [ ! -f $PIDFILE ]  
        then  
                echo "$PIDFILE does not exist, process is not running"  
        else  
                PID=$(cat $PIDFILE)  
                echo "Stopping ..."  
                $REDIS_CLI -p $REDISPORT SHUTDOWN  
                while [ -x ${PIDFILE} ]  
               do  
                    echo "Waiting for Redis to shutdown ..."  
                    sleep 1  
                done  
                echo "Redis stopped"  
        fi  
        ;;  
   restart|force-reload)  
        ${0} stop  
        ${0} start  
        ;;  
  *)  
    echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2  
        exit 1  
esac  

exit 0
```



```bash
# 设置脚本文件的可执行权限
chmod +x /etc/init.d/redis

# 添加服务到service，开机启动
chkconfig --add /etc/init.d/redis

#启动服务
service redis start

# 停止服务
service redis stop
```



**(3) 将redis的bin命令添加到PATH**

```bash
# 打开profile文件
vim /etc/profile
# 在文件末尾添加内容如下：
export PATH=$PATH:/usr/local/redis/bin

# 使profile生效
source /etc/profile

# 停止无密码redis服务
redis-cli shutdown
# 停止有密码redis服务
redis-cli -a 123456 shutdown
# 启动客户端
redis-cli
# 授权
auth 123456
```

### 2. 在docker安装redis

**(1) 无持久化方式**

操作的数据只在内存中，容器关闭了，数据会消失。

docker-compose.yml配置内容如下：

```dockerfile
version: "3"
services:
  redis:
    image: redis:latest
    restart: always
    container_name: "redis-app"
    command: redis-server --requirepass 123456
    ports:
    - 6379:6379
```

其中参数–requirepass表示客户端连接redis服务端时需要密码。



**(2) 有持久化方式**

AOF模式持久化，每秒钟强制写入磁盘一次。

docker-compose.yml配置内容如下：

```dockerfile
version: "3"
services:
  redis:
    image: redis:latest
    restart: always
    container_name: "redis-app"
    command: redis-server --requirepass 123456 --appendonly yes --appendfsync everysec
    ports:
    - 6379:6379
    volumes:
    - /data/redis:/data
```

其中–requirepass 表示客户端连接redis服务端时需要密码； –appendonly表示使用AOF模式持久化， –appendfsync表示多长时间把数据写入硬盘



**(3) 自定义redis配置**

docker-compose.yml配置内容如下：

```dockerfile
version: "3"
services:
  redis:
    image: redis:latest
    restart: always
    container_name: "redis-app"
    command: redis-server /usr/local/etc/redis/redis.conf
    ports:
    - 6379:6379
    volumes:
    - ./redis.conf:/usr/local/etc/redis/redis.conf
    - /data/redis:/data
```

自定义配置文件，为了支持远程连接，在默认的配置修改了以下信息：

- bind：默认绑定本机，已取消
- protected-mode：默认开启，已取消
- requirepass：默认密码为空，已开启和设置密码

## 库和key操作命令

### key操作命令

1、set 设置key的值

```bash
set key value

# 示例
127.0.0.1:6379> set name zhangsan
OK
127.0.0.1:6379> set age 25
OK
```

2、get 获取key的值

```bash
get key

# 示例
127.0.0.1:6379> get name
"zhangsan"
```

3、del 删除key

```bash
del key

# 示例
127.0.0.1:6379> del name
(integer) 1
```

4、exists 判断key是否存在

```bash
exists key
# 判断key是否存在，存在返回1，不存在返回0

# 示例
127.0.0.1:6379> exists name
(integer) 1
127.0.0.1:6379> exists title
(integer) 0
```

5、type 获取key类型

```bash
type key
# 获取key存储的值的类型

# 示例
127.0.0.1:6379> type name
string
127.0.0.1:6379> type age
string
```

6、expire 设置key有效期

```bash
expire key
# 设置key的生命周期
# pexpire key 表示以毫秒为单位设置声明周期

# 示例
127.0.0.1:6379[1]> expire login 60
(integer) 1
127.0.0.1:6379[1]> ttl login
(integer) 47
```

7、tll 查看key有效期

```bash
ttl key
# 查询key的生命周期
# 大于0 ：生命周期单位为秒，
# 等于-1：永久有效
# 等于-2：该key不存在
# pttl key表示毫秒为单位

# 示例
127.0.0.1:6379> ttl name
(integer) -1
127.0.0.1:6379> ttl title
(integer) -2
```

8、rename 重命名key

```bash
rename key newkey
# 重命名key，如果newkey已经存在，修改后则替换新key的值

# 示例
127.0.0.1:6379> set title "redis test"
OK
127.0.0.1:6379> exists title
(integer) 1
127.0.0.1:6379> rename title biaoti
OK
127.0.0.1:6379> get biaoti
"redis test"
```

9、renamenx 重命名不存在的key

```bash
renamenx key newkey
# 重命名key，如果newkey已经存在则不修改。
# nx表示not exists

# 示例
127.0.0.1:6379> keys *
1) "biaoti"
2) "age"
3) "name"
127.0.0.1:6379> renamenx biaoti name
(integer) 0
```

10、persist 设置key永久有效

```bash
persist key
# 设置key永久有效

# 示例
127.0.0.1:6379> set login on
OK
127.0.0.1:6379> expire login 60
(integer) 1
127.0.0.1:6379> ttl login
(integer) 55
127.0.0.1:6379> persist login
(integer) 1
127.0.0.1:6379> ttl login
(integer) -1
```

11、move 把key移动到其他库

```bash
move key db
# 把key移动到另一个数据库，db为整数

# 示例
127.0.0.1:6379> keys *
1) "biaoti"
2) "age"
3) "name"
127.0.0.1:6379> move biaoti 1
(integer) 1
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> keys *
1) "biaoti"
```

### 库操作命令

1、dbsize 查看当前有多少个key

```bash
dbsize
# 查看当前有多少个key

# 示例
127.0.0.1:6379> dbsize
12
```

2、select 选择库

```bash
select db
# 选择使用哪个数据库，db为整数
# 默认有16个数据库0~15，如果想修改数据库数量，修改redis.conf配置文件的databases值

# 示例
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[2]> select 15
OK
```

3、flushdb 删除选中数据库中的key

```bash
flushdb
# 删除当前选择数据库中的所有key

# 示例
127.0.0.1:6379[1]> keys *
1) "biaoti"
127.0.0.1:6379[1]> flushdb
OK
127.0.0.1:6379[1]> keys *
(empty list or set)
```

4、flushall 删除所有库的key

```bash
flushall
# 删除所有数据库中的key

# 示例
127.0.0.1:6379[1]> flushall
OK
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> keys *
(empty list or set)
```

## 字符串类型操作

1、set 设置kv、效期、判断key是否存在

```bash
set key value [ex 秒数]|[px 毫秒数] [nx]|[xx]
# 设置kv时也可以设置有效期和判断key是否存在
# ex和px不要同时写，否则以后面有效期为准
# nx表示key不存在时执行操作
# xx表示key存在时执行操作


# 示例
127.0.0.1:6379> set name zhangsan
OK
127.0.0.1:6379> set name zhangsan ex 100
OK
127.0.0.1:6379> ttl name
(integer) 78
127.0.0.1:6379> set name lisi nx
(nil)
127.0.0.1:6379> get name
"zhangsan"
```

2、mset 一次性输入多个kv

```bash
mset key1 value1 key2 value2......
# 一次性输入多个key-value

# 示例
127.0.0.1:6379> mset x 1 y 2 z 3
OK
127.0.0.1:6379> keys *
1) "y"
2) "z"
3) "x"
```

3、setrange 修改偏移字节值为valuev

```bash
setrange key offset value
# 把字符串的偏移字节改为value
# 如果偏移量大于字符长度，中间字符自动补0x00

# 示例
127.0.0.1:6379> set name zhangsan
OK
127.0.0.1:6379> setrange name 5 ***
(integer) 8
127.0.0.1:6379> get name
"zhang***"
```

4、append 在key的值后面追加字符串

```bash
append key value
# 在key的值后面追加value字符串

# 示例
127.0.0.1:6379> set name zhangsan
OK
127.0.0.1:6379> append name "@126.com"
(integer) 16
127.0.0.1:6379> get name
"zhangsan@126.com"
```

5、getrange 获取key值的部分内容

```bash
getrange key start stop
# 获取key值的一部分内容
# start表示起始位置
# stop表示结束位置，可以为为负数，表示从最后数起
# start>length 空字符串
# stop>length 截取到结尾

# 示例
127.0.0.1:6379> set title "hello world"
OK
127.0.0.1:6379> getrange title 6 11
"world"
127.0.0.1:6379> getrange title 0 -7
"hello"
```

6、getset 设置新值返回旧值

```bash
getset key newvalue
# 设置新值，并返回旧值

# 示例
127.0.0.1:6379> set login on
OK
127.0.0.1:6379> get login
"on"
127.0.0.1:6379> getset login off
"on"
127.0.0.1:6379> get login
"off"
```

7、incr/decr 指定key的值加/减1

```bash
incr/decr key
# 指定key的值加/减1，返回结果
# key不存在时，自动创建并加减1
# key的值为字符串时无效

# 示例
127.0.0.1:6379> set num 100
OK
127.0.0.1:6379> incr num
(integer) 101
127.0.0.1:6379> decr num 
(integer) 100
```

8、incrby/decrby 指定key的值加/减number

```bash
incrby/decrby key number
# 指定key的值加减number大小

# 示例
127.0.0.1:6379> set num 100
OK
127.0.0.1:6379> incrby num 50
(integer) 150
127.0.0.1:6379> decrby num 100
(integer) 50
```

9、incrbyfloat 指定key的值加浮点数

```bash
incrbyfloat key floatnumber
# 指定key的值加浮点数

# 示例
127.0.0.1:6379> set num 10
OK
127.0.0.1:6379> incrbyfloat num 0.5
"10.5"
127.0.0.1:6379> incrbyfloat num -1.5
"9"
```

10、setbit 设置二进制位上的值

```bash
setbit key offset value
# 设置offset对应二进制位上的值
# 返回改位的旧值
# 如果offset过大则会在中间填充0
# offset最大为2^32-1，即512M

# 示例
127.0.0.1:6379> set letter A
OK
127.0.0.1:6379> setbit letter 2 1
(integer) 0
127.0.0.1:6379> get letter
"a"
# 把0100 0001(65)改为0110 0001(97)即把大写A改为了小写a
```

11、getbit 获取二进制位上的值

```bash
getbit key offset
# 获取二进制offset对应位的值

# 示例
127.0.0.1:6379> set letter A
OK
127.0.0.1:6379> getbit letter 0
(integer) 0
127.0.0.1:6379> getbit letter 1
(integer) 1
127.0.0.1:6379> getbit letter 7
(integer) 1
```

12、bitop 对多个key逻辑操作

```bash
bitop operation destkey key1 [key2 ......]
# 对key1 key2 keyN做operation，并把结果保存到destkey 上
# operation有AND、OR、NOT、XOR

# 示例
127.0.0.1:6379> setbit lower 2 1
(integer) 0
127.0.0.1:6379> setbit upper 2 0
(integer) 0
127.0.0.1:6379> set letter A
OK
127.0.0.1:6379> bitop or letter letter lower
(integer) 1
127.0.0.1:6379> get letter
"a"
```

## 链表类型操作

1、lpush/rpush 在链表头/尾增加一个成员

```bash
lpush/rpush key value
# 在链表头/尾增加一个成员，返回链表成员的个数

# 示例
127.0.0.1:6379> lpush letters A
(integer) 1
127.0.0.1:6379> rpush letters B
(integer) 2
127.0.0.1:6379> rpush letters C
(integer) 3
127.0.0.1:6379> rpush letters D
(integer) 4
```

2、lrange 获取链表成员

```bash
lrange key start stop
# 返回链表中[start,stop]范围的成员
# 规律: 左数从0开始,右数从-1开始

# 示例
127.0.0.1:6379> lrange letters 0 -1
1) "A"
2) "B"
3) "C"
4) "D"
127.0.0.1:6379> lrange letters 1 2
1) "B"
2) "C"
```

3、lpop/rpop 弹出链表中头/尾的成员

```bash
lpop/rpop key
# 弹出链表中头/尾的成员

# 示例
127.0.0.1:6379> lrange letters 0 -1
1) "A"
2) "B"
3) "C"
4) "D"
127.0.0.1:6379> lpop letters
"A"
127.0.0.1:6379> rpop letters
"D"
127.0.0.1:6379> lrange letters 0 -1
1) "B"
2) "C"
```

4、lrem 删除链表成员

```bash
lrem key count value
# 从key链表中删除 value值
# 删除count的绝对值个value后结束
# count>0 从表头删除
# count<0 从表尾删除

# 示例
127.0.0.1:6379> rpush letters A B C D A B C D A B C D
(integer) 12
127.0.0.1:6379> lrem letters 2 A
(integer) 2
127.0.0.1:6379> lrange letters 0 -1
 1) "B"
 2) "C"
 3) "D"
 4) "B"
 5) "C"
 6) "D"
 7) "A"
 8) "B"
 9) "C"
10) "D"
127.0.0.1:6379> lrem letters -3 D
(integer) 3
127.0.0.1:6379> lrange letters 0 -1
1) "B"
2) "C"
3) "B"
4) "C"
5) "A"
6) "B"
7) "C"
```

5、lindex 获取链表索引对应的值

```bash
lindex key index
# 获取链表索引index对应的值

# 示例
127.0.0.1:6379> rpush letters A B C D
(integer) 4
127.0.0.1:6379> lindex letters 1
"B"
127.0.0.1:6379> lindex letters 2
"C"
```

6、llen key 获取链表成员个数

```bash
BASHllen key
# 获取链表成员个数

# 示例
127.0.0.1:6379> rpush letters A B C D
(integer) 4
127.0.0.1:6379> llen letters
(integer) 4
```

7、linsert 在链表中指定位置插入成员

```bash
linsert key after|before search value
# 在key链表中寻找"search"，并在search值之前|之后插入value
# 如果没有找到，不插入值
# 如果找到一个search后，命令就结束了，因此不会插入多个value

# 示例
127.0.0.1:6379> rpush id 1 3 5 7
(integer) 4
127.0.0.1:6379> linsert id before 3 2
(integer) 5
127.0.0.1:6379> lrange id 0 -1
1) "1"
2) "2"
3) "3"
4) "5"
5) "7"
127.0.0.1:6379> linsert id after 5 6
(integer) 6
127.0.0.1:6379> lrange id 0 -1
1) "1"
2) "2"
3) "3"
4) "5"
5) "6"
6) "7"
```

8、blpop/brpop 一直等待弹出头/尾成员

```bash
blpop/brpop key timeout
# 等待弹出key的头/尾成员
# Timeout为等待超时时间
# 如果timeout为0,则一直等待
# 应用s场景: 长轮询Ajax,在线聊天时,能够用到

# 示例
# 第一个终端操作：
127.0.0.1:6379> brpop chat 0
1) "chat"
2) "hello"
(40.97s)

# 第二个终端操作：
127.0.0.1:6379> rpush chat "hello"
(integer) 1
```

## 无序集合操作

集合特性：无序性、唯一性、确定性

1、sadd 往集合添加成员

```bash
sadd key value1 value2 ...
# 往集合key中增加成员
# 增加相同成员时只会添加一个(唯一性)

# 示例
127.0.0.1:6379> sadd names zhangsan lisi
(integer) 2
127.0.0.1:6379> sadd names wangwu wangwu
(integer) 1
```



2、srem 删除集合成员

```bash
srem key value1 value2 ...
# 删除集合中为value1 value2...成员
# 返回真正删除掉的成员个数(不包括不存在的成员)

# 示例
127.0.0.1:6379> sadd names zhangsan lisi wangwu
(integer) 3
127.0.0.1:6379> srem names zhangsan lisi
(integer) 2
127.0.0.1:6379> smembers names
1) "wangwu"
```

3、spop 随机删除集合一个成员

```bash
spop key
# 随机删除集合key中的一个成员
# 应用场景：抽奖，抽中的人已经排除，不可能会被再次抽中了

# 示例
127.0.0.1:6379> sadd letters A B C D E F
(integer) 6
127.0.0.1:6379> spop letters
"A"
127.0.0.1:6379> spop letters
"F"
127.0.0.1:6379> spop letters
"B"
127.0.0.1:6379> spop letters
"D"
```

4、srandmember 随机获取集合成员

```bash
srandmember key [count]
# 随机获取集合key的count个成员，默认count是1

# 示例
127.0.0.1:6379> srandmember letters
"C"
127.0.0.1:6379> srandmember letters 2
1) "E"
2) "B"
127.0.0.1:6379> srandmember letters 3
1) "D"
2) "C"
3) "E"
```

5、smembers 获取集合所有的成员

```bash
smembers key
# 返回集合所有的成员
# 返回值的顺序不一定是添加成员的顺序(无序性)

# 示例
127.0.0.1:6379> sadd names zhangsan lisi wangwu
(integer) 3
127.0.0.1:6379> smembers names
1) "lisi"
2) "wangwu"
3) "zhangsan"
```

6、sismember 判断成员是否存在集合中

```bash
sismember key value
# 判断value是否存在集合key中，存在返回1，不存在返回0

# 示例
127.0.0.1:6379> sadd names zhangsan lisi wangwu
(integer) 3
127.0.0.1:6379> sismember names lisi
(integer) 1
127.0.0.1:6379> sismember names zhaoliu
(integer) 0
```

7、scard 获取集合成员的个数

```bash
scard key
# 获取集合成员的个数

# 示例
127.0.0.1:6379> sadd letters A B C D 
(integer) 4
127.0.0.1:6379> sadd letters E F
(integer) 2
127.0.0.1:6379> scard letters
(integer) 6
```

8、smove 把一个集合中成员移动到另一个集合

```bash
smove <source> <dest> value
# 把集合source中的value删除，并添加到集合dest中

# 示例
127.0.0.1:6379> sadd letters A B C
(integer) 3
127.0.0.1:6379> sadd num 1 2 3
(integer) 3
127.0.0.1:6379> smove letters num A
(integer) 1
127.0.0.1:6379> smembers letters
1) "C"
2) "B"
127.0.0.1:6379> smembers num
1) "3"
2) "1"
3) "A"
4) "2"
```

9、sunion 获取多个集合的并集

```bash
sunion key1 key2 ...
# 获取多个集合的并集

# 示例
127.0.0.1:6379> sadd zhangsan A E G
(integer) 3
127.0.0.1:6379> sadd lisi B E F
(integer) 3
127.0.0.1:6379> sadd wangwu C D E
(integer) 3
127.0.0.1:6379> sunion zhangsan lisi wangwu
1) "B"
2) "G"
3) "D"
4) "C"
5) "E"
6) "F"
7) "A"
```

10、sdiff 获取多个集合的差集

```bash
sdiff key1 key2 ...
# 获取key1与key2...的差集
# 即key1-key2...(key1有其他集合没有的成员)

# 示例
127.0.0.1:6379> sadd zhangsan A B C
(integer) 3
127.0.0.1:6379> sadd lisi B D E
(integer) 3
127.0.0.1:6379> sadd wangwu C E F
(integer) 3
127.0.0.1:6379> sdiff zhangsan lisi wangwu
1) "A"
```

11、sinterstore 获取多个集合的交集并储存

```bash
sinterstore dest key1 key2 ...
# 求出key1 key2 ...集合中的交集，并赋给dest

# 示例
127.0.0.1:6379> sadd zhangsan A C D
(integer) 3
127.0.0.1:6379> sadd lisi B D E
(integer) 3
127.0.0.1:6379> sadd wangwu D E G
(integer) 3
127.0.0.1:6379> sinterstore class zhangsan lisi wangwu
(integer) 1
127.0.0.1:6379> smembers class
1) "D"
```

## 有序集合操作



1、zadd 往有序集合添加成员

```bash
zadd key score1 key2 score2 key2 ...
# 往有序集合key添加成员

# 示例
127.0.0.1:6379> zadd ages 28 zhangsan 24 lisi 26 wangwu
(integer) 0
127.0.0.1:6379> zrange ages 0 -1
1) "lisi"
2) "wangwu"
3) "zhangsan"
```

2、zrange 按名次取成员

```bash
zrange key start stop [WITHSCORES]
# 把集合排序后，返回名次[start,stop]的成员按名次取成员
# 默认是升续排列，withscores 是把score也打印出来

# 示例
127.0.0.1:6379> zrange ages 0 -1 withscores
1) "lisi"
2) "24"
3) "wangwu"
4) "26"
5) "zhangsan"
6) "28"
```

3、zrangebyscore 按分数取成员

```bash
zrangebyscore  key min max [withscores] limit offset N
# 集合(升序)排序后，取score在[min,max]内的成员，并跳过offset个，取出N个，按分数取成员

# 示例
127.0.0.1:6379> zadd ages 28 zhangsan 24 lisi 26 wangwu
(integer) 3
127.0.0.1:6379> zrangebyscore ages 25 30
1) "wangwu"
2) "zhangsan"
127.0.0.1:6379> zrangebyscore ages 25 30 limit 1 1
1) "zhangsan"
```

4、zscore 获取指定成员的分数

```bash
ZSCORE key member
# 获取指定成员的分数

# 示例
127.0.0.1:6379> zadd height 175 zhangsan 167 lisi 185 wangwu
(integer) 3
127.0.0.1:6379> zscore height lisi
"167"
```

5、zcount 计算分数区间成员个数

```bash
zcount key min max
# 计算[min,max]区间内成员的数量

# 示例
127.0.0.1:6379> zadd height 175 zhangsan 167 lisi 185 wangwu
(integer) 3
127.0.0.1:6379> zcount height 170 180
(integer) 1
```

6、zrank/zrevrank 获取成员升序/降序的排名

```bash
zrank/zrevrank key member
# 查询member的升序/降序排名，名次从0开始

# 示例
127.0.0.1:6379> zadd ages 28 zhangsan 24 lisi 26 wangwu
(integer) 0
127.0.0.1:6379> zrange ages 0 -1
1) "lisi"
2) "wangwu"
3) "zhangsan"
127.0.0.1:6379> zrank ages zhangsan
(integer) 2
127.0.0.1:6379> zrevrank ages zhangsan
(integer) 0
```

7、zrem 删除有序集合成员

```bash
zrem key value1 value2 ..
# 删除集合中的成员

# 示例
127.0.0.1:6379> zrem ages wangwu
(integer) 1
127.0.0.1:6379> zrange ages 0 -1
1) "lisi"
2) "zhangsan"
```

#8、zremrangebyrank 按排名删除成员

```bash
zremrangebyrank key start end
# 按排名删除成员，删除名次在[start,end]之间的

# 示例
127.0.0.1:6379> zadd height 175 zhangsan 167 lisi 185 wangwu 178 zhaoliu
(integer) 1
127.0.0.1:6379> zremrangebyrank height 0 1
(integer) 2
127.0.0.1:6379> zrange height 0 -1
1) "zhaoliu"
2) "wangwu"
```

9、zremrangebyscore 按分数删除成员

```bash
zremrangebyscore key min max
# 按照socre来删除成员，删除score在[min,max]之间的

# 示例
127.0.0.1:6379> zadd height 175 zhangsan 167 lisi 185 wangwu 178 zhaoliu
(integer) 2
127.0.0.1:6379> zremrangebyscore height 170 180
(integer) 2
127.0.0.1:6379> zrange height 0 -1
1) "lisi"
2) "wangwu"
```

10、zinterstore 求交集再计算

```bash
zinterstore destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]
# 求key1、key2...的交集，key1、key2...的权重分别是 weight1、weight2...
# 聚合方法用: sum|min|max
# 聚合的结果保存在destination集合内

# 示例
127.0.0.1:6379> zadd zhangsan 5 iphone6s 7 galaxyS7 6 huaweiP9
(integer) 3
127.0.0.1:6379> zadd lisi 3 iphone6s 9 galaxyS7 4 huaweiP9 2 HTC10
(integer) 4
127.0.0.1:6379> zinterstore result 2 zhangsan lisi
(integer) 3
127.0.0.1:6379> zrange result 0 -1 withscores
1) "iphone6s"
2) "8"
3) "huaweiP9"
4) "10"
5) "galaxyS7"
6) "16"
127.0.0.1:6379> zinterstore result 2 zhangsan lisi aggregate max
(integer) 3
127.0.0.1:6379> zrange result 0 -1 withscores
1) "iphone6s"
2) "5"
3) "huaweiP9"
4) "6"
5) "galaxyS7"
6) "9"
```

11、zunionstore 求并集再计算

```bash
zunionstore destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]
# 求key1、key2...的并集，key1、key2...的权重分别是 weight1、weight2...
# 聚合方法用: sum|min|max
# 聚合的结果保存在destination集合内

# 示例
127.0.0.1:6379> zadd zhangsan 4 iphone6s 6 huaweiP9 8 xiaomi5
(integer) 3
127.0.0.1:6379> zadd lisi 2 iphone6s 8 galaxS7 5 meizu6
(integer) 3
127.0.0.1:6379> zunionstore result 2 zhangsan lisi
(integer) 5
127.0.0.1:6379> zrange result 0 -1 withscores
 1) "meizu6"
 2) "5"
 3) "huaweiP9"
 4) "6"
 5) "iphone6s"
 6) "6"
 7) "galaxS7"
 8) "8"
 9) "xiaomi5"
10) "8"
127.0.0.1:6379> zunionstore result 2 zhangsan lisi aggregate max
(integer) 5
127.0.0.1:6379> zrange result 0 -1 withscores
 1) "iphone6s"
 2) "4"
 3) "meizu6"
 4) "5"
 5) "huaweiP9"
 6) "6"
 7) "galaxS7"
 8) "8"
 9) "xiaomi5"
10) "8"
```

## 哈希数组类操作

1. hset 设置哈希field域的值

```bash
hset key field value
# 把key中 filed域的值设为value
# 注:如果没有field域，直接添加，如果有，则覆盖原field域的值

# 示例
127.0.0.1:6379> hset user name zhangsan
(integer) 1
127.0.0.1:6379> hset user age 25
(integer) 1
127.0.0.1:6379> hset user gender male
(integer) 1
```

2. hmset 设置哈希多个field域的值

```bash
hmset key field1 value1 [field2 value2 field3 value3 ......fieldn valuen]
# 一次设置多个field和对应的value

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
OK
```

3. hget 获取field域的值

```bash
hget key field
# 获取field域的值

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
OK
127.0.0.1:6379> hget user age
"26
```

4. hmget 获取多个field域的值

```bash
hget key field
# 获取多个field域的值

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
OK
127.0.0.1:6379> hmget user name age
1) "lisi"
2) "26"
```

5. hgetall 获取所有field域和值

```bash
hgetall key
# 获取哈希key的所有field域和值

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
127.0.0.1:6379> hgetall user
1) "name"
2) "lisi"
3) "age"
4) "26"
5) "gender"
6) "male"
```

6. hlen 获取field的数量

```bash
hlen key
# 获取field的数量

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
127.0.0.1:6379> hlen user
(integer) 3
```

7. hdel 删除field域

```bash
hdel key field
# 删除key中 field域

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
127.0.0.1:6379> hdel user age
(integer) 1
127.0.0.1:6379> hgetall user
1) "name"
2) "lisi"
3) "gender"
4) "male"
```

8. hexists 判断field域是否存在

```bash
hexists key field
# 判断key中有没有field域

# 示例
127.0.0.1:6379> hmset user name lisi age 26 gender male
OK
127.0.0.1:6379> hexists user age
(integer) 1
127.0.0.1:6379> hexists user height
(integer) 0
```

9. hincrby 使field域的值加上整数

```bash
hincrby key field value
# 使key中的field域的值加上整型值value

# 示例
127.0.0.1:6379> hmset user name zhangsan height 158
OK
127.0.0.1:6379> hincrby user height 2
(integer) 160
```

10. hincrbyfloat 使field域的值加上浮点数

```bash
hincrbyfloat key field value
# 使key中的field域的值加上浮点值value

# 示例
127.0.0.1:6379> hmset user name zhangsan height 158
OK
127.0.0.1:6379> hincrbyfloat user height 5.5
"165.5"
```

11. hkeys 获取所有所有field域的名字

```bash
hkeys key
# 获取key中所有的field

# 示例
127.0.0.1:6379> hmset user name zhangsan age 25 gender male
OK
127.0.0.1:6379> hkeys user
1) "name"
2) "age"
3) "gender"
```

12. kvals 获取所有所有field域的值

```bash
kvals key
# 返回key中所有的value

# 示例
127.0.0.1:6379> hmset user name zhangsan age 25 gender male
OK
127.0.0.1:6379> hvals user
1) "zhangsan"
2) "25"
3) "male"
```

## 经纬度数据操作



1. geoadd 添加地理位置信息

命令：`geoadd key longitude latitude member [longitude latitude member ...]` 

longitude表示经度，latitude表示纬度，member表示成员

命令描述：将指定的地理空间位置（纬度、经度、名称）添加到指定的key中。

返回值：添加到sorted set元素的数目，但不包括已更新score的元素。

```bash
# 示例
127.0.0.1:6379> geoadd Guangdong-cities 113.2278442 23.1255978 Guangzhou 113.106308 23.0088312 Foshan 113.7943267 22.9761989 Dongguan 114.0538788 22.5551603 Shenzhen
(integer) 4
```

2. geopos 查询位置的坐标

命令：`geopos location-set name [name ...]`

命令描述：从key里返回所有给定位置元素的位置（经度和纬度）。

返回值：GEOPOS 命令返回一个数组， 数组中的每个项都由两个元素组成： 第一个元素为给定位置元素的经度， 而第二个元素则为给定位置元素的纬度。当给定的位置元素不存在时， 对应的数组项为空值。

```bash
# 示例
127.0.0.1:6379> geopos Guangdong-cities Guangzhou Shenzhen
1) 1) "113.22784155607223511"
   2) "23.1255982020608073"
2) 1) "114.05388146638870239"
   2) "22.55515920515157546"
```

3. geodist 查询位置距离

命令：`geodist key member1 member2 [m|km|mi|ft]`

命令描述：

返回两个给定位置之间的距离。如果两个位置之间的其中一个不存在， 那么命令返回空值。指定单位的参数 unit 必须是以下单位的其中一个：

​	m 表示单位为米。

​	km 表示单位为千米。

​	mi 表示单位为英里。

​	ft 表示单位为英尺。

```bash
# 示例
127.0.0.1:6379> geodist Guangdong-cities Guangzhou Shenzhen
"105806.7782"
```

4. georadius 查询某点的附近点

命令：`GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]`

命令描述：以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。

范围可以使用以下其中一个单位：

​	m 表示单位为米。

​	km 表示单位为千米。

​	mi 表示单位为英里。

​	ft 表示单位为英尺。

在给定以下可选项时， 命令会返回额外的信息：

**WITHDIST**: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。 距离的单位和用户给定的范围单位保持一致。

**WITHCOORD**: 将位置元素的经度和维度也一并返回。

**WITHHASH**: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。

命令默认返回未排序的位置元素。 通过以下两个参数， 用户可以指定被返回位置元素的排序方式：

**ASC**: 根据中心的位置， 按照从近到远的方式返回位置元素。

**DESC**: 根据中心的位置， 按照从远到近的方式返回位置元素。

在默认情况下， GEORADIUS 命令会返回所有匹配的位置元素。 虽然用户可以使用 COUNT <count> 选项去获取前 N 个匹配元素， 但是因为命令在内部可能会需要对所有被匹配的元素进行处理， 所以在对一个非常大的区域进行搜索时， 即使只使用 COUNT 选项去获取少量元素， 命令的执行速度也可能会非常慢。 但是从另一方面来说， 使用 COUNT 选项去减少需要返回的元素数量， 对于减少带宽来说仍然是非常有用的。

返回值：

在没有给定任何 WITH 选项的情况下， 命令只会返回一个像 [“New York”,”Milan”,”Paris”] 这样的线性（linear）列表。

在指定了 WITHCOORD 、 WITHDIST 、 WITHHASH 等选项的情况下， 命令返回一个二层嵌套数组， 内层的每个子数组就表示一个元素。

在返回嵌套数组时， 子数组的第一个元素总是位置元素的名字。 至于额外的信息， 则会作为子数组的后续元素， 按照以下顺序被返回：

1. 以浮点数格式返回的中心与位置元素之间的距离， 单位与用户指定范围时的单位一致。
2. geohash 整数。
3. 由两个元素组成的坐标，分别为经度和纬度。

```bash
# 示例
127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km
1) "Foshan"
2) "Guangzhou"
3) "Dongguan"

127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km withcoord
1) 1) "Foshan"
   2) 1) "113.10631066560745"
      2) "23.008831202413539"
2) 1) "Guangzhou"
   2) 1) "113.22784155607224"
      2) "23.125598202060807"
3) 1) "Dongguan"
   2) 1) "113.79432410001755"
      2) "22.9761992022082"
      
127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km withcoord withdist
1) 1) "Foshan"
   2) "17.9820"
   3) 1) "113.10631066560745"
      2) "23.008831202413539"
2) 1) "Guangzhou"
   2) "0.0003"
   3) 1) "113.22784155607224"
      2) "23.125598202060807"
3) 1) "Dongguan"
   2) "60.3111"
   3) 1) "113.79432410001755"
      2) "22.9761992022082"

127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km withcoord withdist withhash
1) 1) "Foshan"
   2) "17.9820"
   3) (integer) 4046506835759376
   4) 1) "113.10631066560745"
      2) "23.008831202413539"
2) 1) "Guangzhou"
   2) "0.0003"
   3) (integer) 4046533621643967
   4) 1) "113.22784155607224"
      2) "23.125598202060807"
3) 1) "Dongguan"
   2) "60.3111"
   3) (integer) 4046540375616238
   4) 1) "113.79432410001755"
      2) "22.9761992022082"

127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km withcoord withdist withhash count 2
1) 1) "Guangzhou"
   2) "0.0003"
   3) (integer) 4046533621643967
   4) 1) "113.22784155607224"
      2) "23.125598202060807"
2) 1) "Foshan"
   2) "17.9820"
   3) (integer) 4046506835759376
   4) 1) "113.10631066560745"
      2) "23.008831202413539"

127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km withcoord withdist withhash asc
1) 1) "Guangzhou"
   2) "0.0003"
   3) (integer) 4046533621643967
   4) 1) "113.22784155607224"
      2) "23.125598202060807"
2) 1) "Foshan"
   2) "17.9820"
   3) (integer) 4046506835759376
   4) 1) "113.10631066560745"
      2) "23.008831202413539"
3) 1) "Dongguan"
   2) "60.3111"
   3) (integer) 4046540375616238
   4) 1) "113.79432410001755"
      2) "22.9761992022082"

127.0.0.1:6379>georadius Guangdong-cities 113.2278442 23.1255978 100 km withcoord withdist withhash desc
1) 1) "Dongguan"
   2) "60.3111"
   3) (integer) 4046540375616238
   4) 1) "113.79432410001755"
      2) "22.9761992022082"
2) 1) "Foshan"
   2) "17.9820"
   3) (integer) 4046506835759376
   4) 1) "113.10631066560745"
      2) "23.008831202413539"
3) 1) "Guangzhou"
   2) "0.0003"
   3) (integer) 4046533621643967
   4) 1) "113.22784155607224"
      2) "23.125598202060807"
```

5. georadiusbymember 查询某位置距离的附近点

```bash
georadiusbymember key member radius m|km|ft|mi
# 指定成员作为中心来查询附近的点

# 示例
127.0.0.1:6379> georadiusbymember Guangdong-cities Guangzhou 100 km
1) "Foshan"
2) "Guangzhou"
3) "Dongguan"
```

6. geohash 查询位置GEOHASH编码

命令：`geohash key member [member ...]`

命令描述：返回一个或多个位置元素的 Geohash 表示。通常使用表示位置的元素使用不同的技术，使用Geohash位置52点整数编码。由于编码和解码过程中所使用的初始最小和最大坐标不同，编码的编码也不同于标准。此命令返回一个标准的Geohash

返回值：一个数组， 数组的每个项都是一个 geohash 。 命令返回的 geohash 的位置与用户给定的位置元素的位置一一对应。

```bash
# 示例
127.0.0.1:6379> geohash Guangdong-cities Guangzhou
1) "ws0e89curg0"
```

## 事务

1. 事务命令

| 命令    | 说明         |
| :------ | :----------- |
| muitl   | 开启事务命令 |
| command | 普通命令     |
| discard | 在提交前取消 |
| exec    | 提交         |

注：discard只是结束本次事务，前2条语句已经执行，造成的影响仍然还在。

语句出错有两种情况： - 语法有问题，exec时报错，所有语句取消执行，没有对数据造成影响。 - 语法本身没错，但适用对象有问题(比如 zadd 操作list对象)，exec之后，会执行正确的语句，并跳过有不适当的语句，对数据会造成影响，这点由程序员负责。

2. 乐观锁

redis的事务中启用的是乐观锁，只负责监测key没有被改动，如果在事务中发现key被改动，则取消事务。使用watch命令来监控一个或多个key，使用unwatch命令来取消监控所有key。

```bash
# 示例
watch key
muitl
操作数据...
exec
unwatch
```

## 频道发布与订阅

- 发布端：publish
- 订阅端：subscribe，psubscribe

1. publish 发布频道

```bash
publish 频道名称 发布内容

# 示例
127.0.0.1:6379> publish music_2 "It's Not Goodbye"
(integer) 1
127.0.0.1:6379> publish music "just one last dance"
(integer) 2
127.0.0.1:6379> publish music "stay"
(integer) 2

127.0.0.1:6379> publish music_2 "It's Not Goodbye"
(integer) 1
```

2. subscribe 订阅指定频道

```bash
subscribe 频道名称

# 示例
127.0.0.1:6379> subscribe music
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "music"
3) (integer) 1
1) "message"
2) "music"
3) "just one last dance"
1) "message"
2) "music"
3) "stay" "music"
3) (integer) 1
1) "message"
2) "music"
3) "just one last dance"
1) "message"
2) "music"
3) "stay"
```

3. psubscribe 订阅已匹配频道

```bash
psubscribe 匹配频道名称

# 示例
127.0.0.1:6379> psubscribe music*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "music*"
3) (integer) 1
1) "pmessage"
2) "music*"
3) "music"
4) "just one last dance"
1) "pmessage"
2) "music*"
3) "music"
4) "stay"

1) "pmessage"
2) "music*"
3) "music_2"
4) "It's Not Goodbye"
```

## redis持久化和导入导出数据库

### 1. redis持久化

1.rdb快照持久化

rdb的工作原理：每隔N分钟或N次写操作后，从内存dump数据形成rdb文件，压缩放在备份目录，设置配置文件参数：

```bash
# 打开配置文件
vim /usr/local/redis/ redis.conf

    save 900 1       # 900秒内，有1条写入，则产生快照 
    save 300 1000   # 如果300秒内有1000次写入，则产生快照
    save 60 10000   # 如果60秒内有10000次写入，则产生快照
    (这3个选项都屏蔽,则rdb禁用)

    stop-writes-on-bgsave-error yes  # 后台备份进程出错时，主进程停不停止写入
    rdbcompression yes       # 导出的rdb文件是否压缩
    Rdbchecksum   yes       # 导入rbd恢复时数据时，要不要检验rdb的完整性

    dbfilename dump.rdb      # 导出来的rdb文件名
    dir /usr/local/redis/data   # rdb的放置路径

# 压力测试来检测是否启用了rdb快照
/usr/local/redis/bin/redis-benchmark
```

rdb的缺陷：在2个保存点之间断电，将会丢失1-N分钟的数据

2. aof日志持久化

工作原理：redis主进程 –> 后台日志进程 –> aof文件

设置配置文件参数：

```bash
# 打开配置文件
vim /usr/local/redis/ redis.conf

    appendonly no        # 是否打开 aof日志功能
    appendfilename "appendonly.aof" # aof文件名，和rdb的dir公用一个路径

    #appendfsync always   # 每1个写命令都立即同步到aof文件，安全但速度慢
    appendfsync everysec # 折衷方案，每秒写1次
    上面方案选择一种，一般选择everysec

    appendfsync no       # 写入工作交给操作系统，由操作系统判断缓冲区大小，统一写入到aof文件，同步频率低，但速度快

    no-appendfsync-on-rewrite  yes  # 正在导出rdb快照的过程中，要不要停止同步aof
    auto-aof-rewrite-percentage 100  # aof文件大小比起上次重写时的大小，增长率100%时重写
    auto-aof-rewrite-min-size 64mb   # aof文件至少超过64M时才重写
```

注意：如果需要持久化，一般推荐rdb和aof同时开启，同时开启后redis进程启动优先选择aof恢复数据。rdb恢复速度快。

在dump rdb过程中，aof如果停止同步，会不会丢失? 不会，所有的操作缓存在内存的队列里，dump完成后统一操作.

aof重写是指什么? aof重写是指把内存中的数据，逆化成命令，写入到.aof日志里，以解决 aof日志过大的问题，手动重写aof命令：bgrewriteaof 

### 导入和导出数据库

```bash
(1) 安装redis-dump工具
yum install ruby rubygems ruby-devel
gem sources --remove http://ruby.taobao.org/
gem sources -a https://ruby.taobao.org/
gem install redis-dump -V

(2) 导出redis数据
redis-dump -u 127.0.0.1:6379 > test.json

(3) 导入redis数据
< test.json redis-load
```

## 应用示例



### 1. 统计活跃用户

场景： 1亿个用户，用户登陆，标记为今天活跃，否则记为不活跃，记录最活跃用户。

```bash
# 思路
每个用户在数据库都有一个id，用第id个位的0和1来表示是否登录，例如：
login0721:  '011001...............0'
......
login0726:  '011001...............0'
login0727:  '0110000.............1'

# 实现过程
 (1) 记录用户登陆，每天按日期生成一个位图，用户登陆后，把user_id位上的bit值置为1
首先把所有用户的位置位0
redis 127.0.0.1:6379> setbit login0721 100000000 0
(integer) 0
redis 127.0.0.1:6379> setbit login0721 3 1
(integer) 0
redis 127.0.0.1:6379> setbit login0721 5 1
(integer) 0
redis 127.0.0.1:6379> setbit login0721 7 1
(integer) 0
......
redis 127.0.0.1:6379> setbit login0722 100000000 0
(integer) 0
redis 127.0.0.1:6379> setbit login0722 3 1
(integer) 0
redis 127.0.0.1:6379> setbit login0722 5 1
(integer) 0
redis 127.0.0.1:6379> setbit login0722 8 1
(integer) 0
......
redis 127.0.0.1:6379> setbit login0723 100000000 0
(integer) 0
redis 127.0.0.1:6379> setbit login0723 3 1
(integer) 0
redis 127.0.0.1:6379> setbit login0723 4 1
(integer) 0
redis 127.0.0.1:6379> setbit login0723 6 1
(integer) 0
......
(2)把1周/月的位图用and计算, 位为1的是连续登陆的用户。
redis 127.0.0.1:6379> bitop and  login0721 login0722 login0723......
(integer) 12500001

# 优点
(1) 节约空间，用1亿bit表示1亿人每天的登陆情况，1亿bit约为10M。
(2) 计算方便，计算速度非常快
```

### 2. 搭建高可用redis集群

**常见redis集群**

常见redis集群有RedisCluster、Codis、Twemproxy，其中Codis、Twemproxy是有中心节点的，而RedisCluster是没中心节点，而且是redis内置的集群方案，推荐使用redisCluster集群。

redisCluster特点：

- 无中心节点，客户端与 redis节点直连，不需要中间代理层。
- 数据可以被分片存储，集群数据加起来就是全量数据。
- 可以通过任意一个节点，读写不属于本节点的数据。
- 管理方便，后续可自行增加或删除节点。

由于RedisCluster是分片存储，当集群中一个节点故障时，会损失该节点数据，为了实现高可用，需要对每个节点各添加一个从节点，形成主从同步，当主redis(集群节点)出现故障时，从节点替换故障的主节点。

- Redist集群中的数据库复制是通过主从同步来实现的。
- 主节点( Master)把数据分发给从节点(Slave)。
- 主从同步的好处在于高可用， Redis节点有冗余设计。
- 集群节点个数最好是奇数并且3个以上， 奇数个的好处是，有一个节点出现故障了，集群数量过半数，集群可以继续正常使用，数量少于半数则认为集群瘫痪了。

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200601094320.png)

如果客户端本身带有负载均衡功能，可以省去使用haproxy搭建负载均衡，也可以在程序简单的实现负载均衡功能。

**单机版redis集群**

在单机上搭建3个节点的集群，3个主节点分别有各自的从节点，如图所示：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200601094436.png)

**修改redis配置文件**

因为官方镜像中没有redis配置，也没有启动redis，需要去官方下载对应版本的redis源码获取redis.conf和redis-trib.rb文件，下载官方源码地址：http://download.redis.io/releases/

```bash
mkdir redis-cluster
cd redis-cluster
wget http://download.redis.io/releases/redis-4.0.11.tar.gz
tar zxvf redis-4.0.11.tar.gz

# 从源码中复制redis.conf和redis-trib.rb文件到当前目录
cp redis-4.0.11/redis.conf .
cp redis-4.0.11/src/redis-trib.rb .

# 删除redis源码文件
rm -rf redis-4.0.11 redis-4.0.11.tar.gz
```

因为redis默认没有开启集群功能，需要修改redis配置文件redis.conf，主要修改下面几个参数

```bash
bind 0.0.0.0 # 允许外部登录
cluster-enabled yes   # 开启集群
cluster-config-file nodes-6379.conf   # 集群配置文件
cluster-node-timeout 15000  # 超时时间
appendonly yes    # 并开启AOF模式
```

**2.2 重新打包redis镜像**

下载指定版本redis官方镜像

> docker pull redis:4.0.11

因为官方redis镜像不能满足搭建redis集群要求，需要在官方镜像基础上添加ruby和启动redis命令功能，在当前目录创建Dockerfile文件，文件内容如下：

```dockerfile
FROM redis

# 安装ruby
RUN apt-get -y update
RUN apt-get -y install ruby
RUN apt-get -y install rubygems
# ruby 安装redis接口
RUN gem install redis
# 安装vim
RUN apt-get -y install vim

# 启动redis
CMD redis-server /usr/local/etc/redis/redis.conf
```

**2.3 编辑docker-compose.yml文件**

在当前目录创建docker-compose.yml文件来管理容器，共6个容器，文件内容如下：

```yaml
version: "3"

services:

  redis-node1:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 20001:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
      # 创建集群的配置文件，只需创建一次，其他节点不需要映射此文件
      - ./redis-trib.rb:/usr/local/etc/redis/redis-trib.rb
    networks:
      - redis-net

  redis-node2:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 20002:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-net

  redis-node3:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 20003:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-net

  redis-node4:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 20004:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-net

  redis-node5:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 20005:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-net

  redis-node6:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 20006:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
    networks:
      - redis-net

networks:
  redis-net:
    driver: bridge
```

**创建redis集群**

当前目录下完整文件列表如下所示：

```bash
.
├── docker-compose.yml
├── Dockerfile
├── redis.conf
└── redis-trib.rb
```

准备好文件后，开始创建redis集群:

```bash
# 先打包好新的redis镜像
docker-compose build

# 启动容器
docker-compose up -d

# 获取所有启动redis容器ip地址，获取ip地址是用来创建集群准备
docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq) | grep redis

    # 执行命令的结果如下：
    # /redis-cluster_redis-node2_1 - 172.20.0.6
    # /redis-cluster_redis-node4_1 - 172.20.0.7
    # /redis-cluster_redis-node6_1 - 172.20.0.4
    # /redis-cluster_redis-node1_1 - 172.20.0.5
    # /redis-cluster_redis-node5_1 - 172.20.0.3
    # /redis-cluster_redis-node3_1 - 172.20.0.2

# 进入redis-node1容器，因为只有redis-node1容器映射了redis-trib.rb文件
docker-compose exec redis-node1 bash

# 创建集群
/usr/local/etc/redis/redis-trib.rb create --replicas 1 172.20.0.2:6379 172.20.0.3:6379 172.20.0.4:6379 172.20.0.5:6379 172.20.0.6:6379 172.20.0.7:6379

  # 其中参数--replicas 1表示每个主节点有一个从节点。
  # 在创建集群过程会提示Can I set the above configuration? 在终端输入yes即可，如果不出现错误，很快就完成集群的创建。

# 创建完集群后，在任意一个redis节点都可以进入redis集群，-c表示连接的是集群
redis-cli -c

# 查看集群节点
127.0.0.1:6379> cluster nodes

  # afb979236e96b6e9aac972af7508e4cd9505676d 172.20.0.6:6379@16379 slave 9715f836310b872961a7defc64bdb4a319f9848d 0 1537636386000 5 connected
  # 1ffa00413a5f75465e1bb5aaef551c38fb3db1f3 172.20.0.5:6379@16379 myself,master - 0 1537636387000 7 connected 10923-16383
  # 9715f836310b872961a7defc64bdb4a319f9848d 172.20.0.2:6379@16379 master - 0 1537636387811 1 connected 0-5460
  # d59a0ad710f35fe349f62eda4279e02972983fd3 172.20.0.3:6379@16379 master - 0 1537636386000 2 connected 5461-10922
  # c9371c4936d12b9f764e5bf5a7257928aa36cbc2 172.20.0.7:6379@16379 slave d59a0ad710f35fe349f62eda4279e02972983fd3 0 1537636385000 6 connected
  # b39a684ad281f03c1befcd90048c503d95526645 172.20.0.4:6379@16379 slave 1ffa00413a5f75465e1bb5aaef551c38fb3db1f3 0 1537636386807 7 connected

# 测试集群的主从复制
127.0.0.1:6379> set testkey 100
  # -> Redirected to slot [4757] located at 172.20.0.2:6379
  # OK

# 根据172.20.0.2查到对应节点为redis-node3，暂停该容器
docker-compose pause redis-node3

# 然后再次读看是否能够读取到数据
172.20.0.2:6379> get testkey
# "100"

# 此时可以查看到集群节点状态，显示172.20.0.2和集群连接失败状态
127.0.0.1:6379> cluster nodes
```

**创建带有密码的redis集群**

实际使用中一般连接redis是需要密码授权的，添加密码很简单，创建集群前修改redis.conf配置和gems的redis客户端client.rb配置。

删除没有密码的集群容器，重新建一个集群，然后在原来基础上修改配置文件。

> docker-compose down



修改redis.conf配置

```bash
masterauth 123456
requirepass 123456
```



进入redis-node1容器，修改redis客户端client.rb配置

```bash
# 找出client.rb配置文件位置
find / -name client.rb

# 在配置中添加密码
# 把 :password => nil 改为 :password => 123456

# 创建集群
/usr/local/etc/redis/redis-trib.rb create --replicas 1 172.20.0.2:6379 172.20.0.3:6379 172.20.0.4:6379 172.20.0.5:6379 172.20.0.6:6379 172.20.0.7:6379

# 带密码登录集群
redis-cli -a 123456 -c
```

**创建集群中遇到的问题**

> redis-trib.rb create –replicas 1 redis-node1:6379 redis-node2:6379 redis-node3:6379 redis-node4:6379 redis-node5:6379 redis-node6:6379

问题1：使用容器名创建去创建集群报错：”ERR Invalid node address specified”

> 答：redis-trib.rb 不支持域名或主机名，必须使用ip:port的方式



问题2：创建集群时报错：err slot 0 is already busy (redis::commanderror)

> 答：由于第一次创建集群没有成功，但还在待创建状态，需要将nodes.conf和dir里面的文件全部删除，如果是在docker创建的话，删除容器重新创建。