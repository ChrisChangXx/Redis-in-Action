# 初识Redis

## 安装Redis

* Ubuntu环境为例

```shell
$ wget https://github.com/redis/redis/archive/7.0.0.tar.gz
$ tar -zxvf 7.0.0.tar.gz
$ cd redis-7.0.0
$ make
$ make install
$ redis-server redis.conf
$ redis-cli ping
# 返回PONG即成功
```

## Redis特性和功能

* 类型：使用内存存储的非关系数据库
* 数据存储选项：字符串、列表、集合、散列表、有序集合
* 查询类型：每种数据类型都有自己的专属命令，另外还有批量操作和不完全的事务支持
* 附加功能：发布和订阅，主从复制，持久化，脚本

## Redis的数据结构

* 字符串（STRING）：可以是字符串、整数或者浮点数
* 列表（LIST）：一个链表，链表上的每个元素都包含了一个字符串
* 集合（SET）：包含字符串的无序收集器，并且被包含的每个字符串都是独一无二、各不相同的
* 有序集合（ZSET）：包含键值对的无序散列表
* 散列表（HASH）：字符串成员与浮点数分值之间的有序映射，元素的排列序由分值的大小决定

### 字符串（STRING）

* set
* get
* del

```
> set hello world
OK
> get hello
world
> del hello
1
> get hello
null
```

### 列表（LIST）

* lpush
* lpop
* rpush
* rpop
* lrange
* lindex

```
> lpush list-key item1
1
> lpush list-key item2
2
> rpush list-key item3
3
> lrange list-key 0 -1
item2
item1
item3
> lindex list-key 2
item3
> lindex list-key 1
item1
> lindex list-key 0
item2
> rpop list-key
item3
> lrange list-key 0 -1
item2
item1
```

### 集合（SET）

* sadd
* srem
* smembers
* sismember

```
> sadd set-key member1
1
> sadd set-key member2
1
> sadd set-key member2
0
> sadd set-key member3
1
> smembers set-key
member3
member2
member1
> sismember sset-key member1
0
> srem set-key member1
1
> sismember sset-key member1
0
```

### 散列（HASH）

* hset
* hget
* hdel
* hgetall

```
> hset hash-key key1 value1
1
> hset hash-key key1 value1
0
> hset hash-key key2 value2
1
> hget hash-key key1
value1
> hgetall hash-key
key1
value1
key2
value2
> hdel hash-key key1
1
> hgetall hash-key
key2
value2
```

### 有序集合（ZSET）

* zadd
* zrem
* zrange
* zrangebyscore

```
> zadd zset-key 100 xiaoming
1
> zadd zset-key 99 xiaoqiang
1
> zadd zset-key 200 xiaohui
1
> zrange zset-key 0 -1
xiaoqiang
xiaoming
xiaohui
> zrange zset-key 0 -1 withscores
xiaoqiang
99
xiaoming
100
xiaohui
200
> zrangebyscore zset-key 0 200 withscores
xiaoqiang
99
xiaoming
100
xiaohui
200
> zrem zset-key xiaoming
1
> zrange zset-key 0 -1 withscores
xiaoqiang
99
xiaohui
200
```