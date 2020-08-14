# Redis——字典

Redis的数据库是使用字典作为底层实现的，另外，字典也是哈希键的底层实现之一。

## 字典的实现

字典使用哈希表作为底层实现

### 相关定义

**哈希表**

```c
typedef struct dictht{
    //哈希表数组
    dictEntry **table;
    
    //哈希表大小
    unsigned long size;
    
    //哈希表大小掩码，用于计算索引值
    //总是等于size-1
    unsigned long sizemask;
    
    //该哈希表已有节点的数量
    unsigned long used;
}dictht;
```

**哈希表节点**

保存一个键值对

```c
typedef struct dictEntry{
    //键
    void *key;
    //值
    union{
        void *val;
        uint64_tu64;
        int64_ts64;
    }v;
    //指向下个哈希表节点，形成链表
    struct dictEntry *next;
}dictEntry;
```

将哈希值相同的键值对连接在一起，来解决键冲突的问题

**字典**

```c
typedef struct dict{
    //类型特定函数
    dictType *type;
    //私有数据
    void *privatdata;
    //哈希表
    dictht ht[2];
    //rehash索引
    //当rehash不在进行时，值为-1
    int rehashidx;
}dict;
```

属性ht是一个包含两个项的数组，数组中的每个项都是一个哈希表。

一般情况下，字典只使用`ht[0]`，`ht[1]`哈希表只会在对`ht[0]`哈希表进行`rehash`时使用

## 哈希算法

Redis使用`MurmurHash2`算法来计算键的哈希值

**将一个新的键值对添加到字典里：**

1. 根据键值对的键，使用字典设置的哈希函数计算出哈希值

   ```c
   hash=dict->type->hashFunction(key);
   ```

1. 使用计算得到的哈希值和哈希表的`sizemask`属性，计算出索引值

   ```
   index=hash & dict->ht[x].sizemask;
   ```

1. 根据索引值，将包含新键值对的哈希表节点放到哈希表数组里

## 解决键冲突

使用链地址法来解决键冲突

为了速度考虑，会将新节点添加到链表的表头位置（头插），排在其他已有节点的前面。

## rehash

原因：随着操作的不断执行，哈希表保存的键值对会逐渐地增多或者减少，通过对哈希表大小进行扩展或者收缩来**使哈希表的负载因子维持在一个合理的范围内**。

通过执行`rehash`（重新散列）来完成扩展和收缩哈希表的工作。

1. 为字典的`ht[1]`分配空间
1. 将保存在`ht[0]`中的所有键值对`rehash`到`ht[1]`上（`rehash`会重新计算键的哈希值和索引值）
1. 步骤2完成后，释放`ht[0]`，将`ht[1]`设置为`ht[0]`，并在`ht[1]`新创建一个空白哈希表，为下次`rehash`做准备

## 渐进式rehash

将`ht[0]`里面的所有键值对全部`rehash`到`ht[1]`的动作是分多次、渐进式地完成的。

原因：如果哈希表里保存的键值对数量非常庞大，那一次性完成`rehash`可能会导致服务器在一段时间内停止服务。

1. 为`ht[1]`分配空间
1. 将字典的属性`rehashidx`值置0，表示`rehash`工作正式开始
1. 在`rehash`进行期间，**每次对字典进行添加、删除、查找或者更新操作时**，都会顺带将`ht[0]`在`rehashidx`索引上的所有键值对`rehash`到`ht[1]`，并且完成后`rehashidx`增一。
1. 随着字典操作的不断执行，最终`ht[0]`的所有键值对都会被`rehash`至`ht[1]`。当`rehash`工作完成后，重新将`rehashidx`属性的值设为`-1`。

**核心**在于将`rehash`的工作均摊到对字典的每个添加、删除、查找和更新操作上。

在进行渐进式`rehash`过程中

1. 字典的删除、查找、更新等操作会在两个哈希表上进行
1. 字典的添加操作一律保存到`ht[1]`里面