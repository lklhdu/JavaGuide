# Redis 五种数据结构的命令

Redis 是速度非常快的非关系型（NoSQL）内存键值数据库，可以存储键和五种不同类型的值之间的映射。

**键的类型只能为字符串**，值支持五种数据类型：字符串、列表、集合、散列表、有序集合。

## 相关操作

1. 检测是否安装有redis-cli和redis-server

   ```shell
   whereis redis-cli
   whereis redis-server
   ```

1. `redis-server` 启动服务端，然后才能通过`redis-cli`启动服务端

1. 在redis的交互式客户端输入命令会有相应的语法提示

1. 使用python来测试Redis（需要使用`pip install redis`来安装redis模块）

   ```
   python
   >>>import redis
   >>>conn=redis.Redis()
   ```

## 数据类型

| 数据类型 |      可以存储的值      |                             操作                             |
| :------: | :--------------------: | :----------------------------------------------------------: |
|  STRING  | 字符串、整数或者浮点数 | 对整个字符串或者字符串的其中一部分执行操作</br> 对整数和浮点数执行自增或者自减操作 |
|   LIST   |          列表          | 从两端压入或者弹出元素 </br> 对单个或者多个元素进行修剪，</br> 只保留一个范围内的元素 |
|   SET    |        无序集合        | 添加、获取、移除单个元素</br> 检查一个元素是否存在于集合中</br> 计算交集、并集、差集</br> 从集合里面随机获取元素 |
|   HASH   | 包含键值对的无序散列表 | 添加、获取、移除单个键值对</br> 获取所有键值对</br> 检查某个键是否存在 |
|   ZSET   |        有序集合        | 添加、获取、删除元素</br> 根据分值范围或者成员来获取元素</br> 计算一个键的排名 |

### STRING

在Redis里面，字符串可以存储一下3种类型的值

* 字节串
* 整数
* 浮点数

**常用命令**

1. GET（获取值）、SET（设置值）、DEL（删除值）

   ```html
   > set hello world
   OK
   > get hello
   "world"
   > del hello
   (integer) 1
   > get hello
   (nil)
   ```

2. 另外，Redis还提供一些能对字符串存储的数值执行自增或自减操作的命令

* `INCR`、`DECR` —— 将键存储的值+1或-1
* `INCRBY`、`DECRBY` —— 将键存储的值加上或减去指定的整数

3. 除了自增操作和自减操作以外，Redis还拥有对字节串的一部分内容进行读取或写入的操作

* `APPEND` —— 将值value追加到给定键key-name当前存储的值的末尾
* `GETRANGE` —— 获取一个从偏移量start到偏移量end范围内的子串
* `SETRANGE` —— 把从偏移量start开始的子串设置为给定的值

### 列表LIST

每个列表结构可以有序地存储多个字符串

**常见命令**

1. 列表可执行的操作和很多编程语言中的列表操作非常相似

* `LPUSH`、`RPUSH` —— 将元素推入列表的左端和右端

* `LPOP`、`RPOP` —— 用于从列表的左端和右端弹出元素

* `LINDEX` —— 获取列表在给定位置上的一个元素

* `LRANGE` —— 获取列表在给定范围上的所有元素

  ```html
  > rpush list-key item
  (integer) 1
  > rpush list-key item2
  (integer) 2
  > rpush list-key item
  (integer) 3
  
  > lrange list-key 0 -1
  1) "item"
  2) "item2"
  3) "item"
  
  > lindex list-key 1
  "item2"
  
  > lpop list-key
  "item"
  
  > lrange list-key 0 -1
  1) "item2"
  2) "item"
  ```

2. 除了上面这些命令，Redis的列表还可以从列表里面移除元素、将元素插入列表中间、将链表修剪至指定的长度

* `LTRIM` —— 对列表进行修剪，只保留从偏移量start到偏移量end范围内的元素

### 集合SET

Redis的集合和列表都可以存储多个字符串，区别在于集合会通过使用散列表来保证存储的每个字符串都是不相同的

### 常见命令

1. `SADD` —— 将元素添加到集合，`SREM` —— 从集合里面移除元素，`SISMEMBER` —— 检查一个元素是否已经存在于集合中，`SMEMBERS` —— 获取集合包含的所有元素

   ```html
   > sadd set-key item
   (integer) 1
   > sadd set-key item2
   (integer) 1
   > sadd set-key item3
   (integer) 1
   > sadd set-key item
   (integer) 0
   
   > smembers set-key
   1) "item"
   2) "item2"
   3) "item3"
   
   > sismember set-key item4
   (integer) 0
   > sismember set-key item
   (integer) 1
   
   > srem set-key item2
   (integer) 1
   > srem set-key item2
   (integer) 0
   
   > smembers set-key
   1) "item"
   2) "item3"
   ```

1. 集合真正厉害的地方在于组合和关联多个集合

* `SDIFF` —— 返回那些存在于第一个集合，但不存在于其他集合中的元素
* `SINTER` —— 返回那些同时存在于所有集合中的元素
* `SUNION` —— 返回那些至少存在于一个集合中的元素

### 散列HASH

Redis的散列可以存储多个键值对之间的映射。

1. 散列存储的值既可以是字符串又可以是数字值，并且用户同样可以对散列存储的数字值进行自增、自减操作

* `HSET` —— 在散列里关联给定的键值对

* `HGET` —— 获取指定散列键的值

* `HGETALL` —— 获取散列包含的所有键值对

* `HDEL` —— 移除指定的键（如果存在）

  ```html
  > hset hash-key sub-key1 value1
  (integer) 1
  > hset hash-key sub-key2 value2
  (integer) 1
  > hset hash-key sub-key1 value1
  (integer) 0
  
  > hgetall hash-key
  1) "sub-key1"
  2) "value1"
  3) "sub-key2"
  4) "value2"
  
  > hdel hash-key sub-key2
  (integer) 1
  > hdel hash-key sub-key2
  (integer) 0
  
  > hget hash-key sub-key1
  "value1"
  
  > hgetall hash-key
  1) "sub-key1"
  2) "value1"
  ```

2. 批量操作的命令

* `HMSET`、`HMGET` —— 获取或设置散列里面的一或多个键的值
* `HLEN` —— 返回散列包含的键值对数量
* `HEXISTS` —— 检查给定键是否存在于散列中
* `HKEYS` —— 获取散列包含的所有键
* `HVALS` —— 获取散列包含的所有值
* `HGETALL` —— 获取散列包含的所有键值对
* `HINCRBY` —— 将键key存储的值加上指定的整数increment

在对散列进行处理的时候，如果键值对的体积很大，那么用户可以先使用`HEYS`获取散列的所有键，然后再去选择获取必要的值

### 有序集合ZSET

有序集合和散列一样，都用于存储键值对

有序集合的键被称为成员，每个成员都是各不相同的；

有序集合的值被称为分值，分值必须为浮点数，**并根据数字大小进行排序**

1. 有序集合既可以根据成员访问元素，又可以根据分值及分值的排列顺序来访问元素

* `ZADD`（添加元素）、`ZREM`（移除元素）
* `ZRANGE` —— 根据元素所处的位置，从有序集合里获取多个元素
* `ZRANGEBYSCORE` —— 给定一个分值的范围，返回在这个范围内的所有元素

```html
> zadd zset-key 728 member1
(integer) 1
> zadd zset-key 982 member0
(integer) 1
> zadd zset-key 982 member0
(integer) 0

> zrange zset-key 0 -1 withscores
1) "member1"
2) "728"
3) "member0"
4) "982"

> zrangebyscore zset-key 0 800 withscores
1) "member1"
2) "728"

> zrem zset-key member1
(integer) 1
> zrem zset-key member1
(integer) 0

> zrange zset-key 0 -1 withscores
1) "member0"
2) "982"
```

2. 其他一些常用的命令

* `ZSCORE` —— 返回成员member的分值
* `ZRANK` —— 返回成员member在有序集合里的排名
* `ZINCRBY` —— 将成员member的分值加上指定数increment
* `ZCARD` —— 返回有序几个包含的数量

3. 一些有序集合的并集和交集命令

* `ZINTERSTORE`（交集运算）—— 会将同时存在于zset-1和zset-2里的元素添加到指定的集合zset-i中，并且输出的有序集合成员的分值是**通过相加得到的**
* `ZUNIONSTORE` —— 输出的有序集合的分值设置为多个输入集合中的**最小分值**