# Redis——压缩列表ziplist

压缩列表是`LIST`和`HASH`的底层实现之一，但需要满足以下条件

* `LIST`或者`HASH`只包含少量的数据项
* 每个数据项要么是小整数值，要么是长度比较短的字符串

## 压缩列表的构成

`ziplist`是Redis为了节约内存而开发的，是由一系列特殊编码的**连续内存块组成**的顺序型数据结构。

![image-20200727161852141](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200727161855-147414.png)

<center>压缩列表的各个组成部分</center>

* `zlbytes`：记录整个压缩列表占用的内存字节数
* `zltail`：记录压缩列表的表尾节点距离压缩列表的起始地址有多少个字节
* `zllen`：记录压缩列表包含的节点数量
* `entry1`、`entry2`、`entry3`、...`entryX`：压缩列表包含的各个节点
* `zlend`：用于标记压缩列表的末端

## 压缩列表节点的构成

每个压缩列表节点可以保存一个**字节数组**或者**整数值**

![image-20200727163700797](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200727163703-445727.png)

<center>压缩列表节点的各个组成部分

* `previous_entry_length`：记录了前一个节点的长度

  压缩列表从表尾向表头遍历的操作就是使用了这个属性

* `encoding`：记录了节点的`content`属性所保存数据的类型以及长度

* `content`：负责保存节点的值

## 连锁更新

添加节点或者删除节点可能会引发连锁更新的情况，但连锁更新真正造成性能问题的几率是很低的。