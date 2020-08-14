# Redis——跳跃表

Redis使用跳跃表作为**有序集合（ZSET）**的底层实现之一

## 跳跃表的实现

![image-20200722160357006](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200722160400-530731.png)

### 跳跃表节点

```c
typedef struct zskiplistNode{
    //层
    struct zskiplistLevel{
        //前进指针
        struct zskiplistNode *forward;
        //跨度
        unsigned int span;
    }level[];
    //后退指针
    struct zskiplistNode *backward;
    //分值
    double score;
    //成员对象
    robj *obj;
}zskiplistNode;
```

> 当使用命令`zadd zset-key 728 member1`添加一个元素时，该元素对应的跳跃表节点中属性`score=728`，属性`obj`就是指向`member1`的指针。

接下来是对跳跃表节点更详细的介绍

#### 1.层

每次创建一个新的跳跃表节点时，程序会随机生成一个介于`1-32`的值作为`level`数组的大小，即层的高度。

#### 2.前进指针

#### 3.跨度

层的跨度（即`level[i].span`）用于记录两个节点之间的距离

跨度的作用是用来计算**排位**的：在查找某个节点的过程中，把沿途访问过的所有层的跨度累加起来，得到的就是目标节点在跳跃表中的排位

#### 4.后退指针

由于每个节点只有一个后退指针，所以每次只能后退至前一个节点

#### 5.分值和成员

分值：浮点数        成员：指向字符串对象的指针（此处应联系有序集合的特点）

跳跃表中的所有节点**按照分值从小到大排序**。

在条约表中，各个节点的成员对象必须唯一，而分值可以相同。分值相同的节点按照成员对象在**字典序中的大小**从小到大排序。

### 跳跃表

```c
typedef struct zskiplist{
    //表头节点和表尾节点
	struct zskiplidtNode *header,*tail;
    //表中节点的数量
    unsigned long length;
    //表中层数最大的节点的层数
    int level;
}
```

上图最左边的就是`zskiplist`结构。

其中属性`level`和`length`都不把表头节点相关的信息计算在内

## 补充

参考自博文[Redis 为什么用跳表而不用平衡树？](https://juejin.im/post/57fa935b0e3dd90057c50fbc#comment)

### 跳跃表的查找

查找数据的时候先在高层的链表中进行查找，然后逐层降低。

假设在下面这个`skiplist`查找23，红线标出了查找路径

![image-20200722164653685](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200722164658-120250.png)

### Redis中的sorted set

Redis中的`sorted set`，是在`skiplist, dict和ziplist`基础上构建起来的:

* 当数据较少时，`sorted set`是由一个`ziplist`来实现的。
* 当数据多的时候，`sorted set`是由一个叫`zset`的数据结构来实现的，这个`zset`包含一个`dict + 一个skiplist`。`dict`用来查询数据到分数(score)的对应关系，而skiplist用来根据分数查询数据（可能是范围查找）。

### Redis为什么用skiplist而不用平衡树

redis的作者从内存占用、对范围查找的支持和实现的难易程度三方面进行了总结，具体可以参考上面给出的博文。