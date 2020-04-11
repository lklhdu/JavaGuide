# Service层开发



## DAO层(Mapper)编码后的思考

2. DAO层工作演变为：接口设计+XML文件的SQL编写
3. DAO拼接等**逻辑**应该在Service层完成

## Service接口设计

业务接口应该站**使用者角度**设计接口

* 定义业务方法的颗粒度要细
* 接口参数简练，返回类型友好
* 方法的return返回值，除了应该明确返回值类型，还应该指明方法执行可能产生的异常(`RuntimeException`)，并应该**手动封装**一些通用的异常处理机制

## Service层

#### Service接口

1. 获得某一条商品的信息
2. 获得所有商品的信息
3. 秒杀开始暴露秒杀地址（返回一个Exposer类）
4.  执行秒杀操作（返回一个SeckillExcution类），允许抛出自定义的异常

#### Service接口实现

##### 接口3-`exportSeckillUrl`：

* 情况1：id对应商品不存在
* 情况2：秒杀未开始或秒杀未结束
* 情况3：成功获得秒杀地址

##### 接口4-`executeSeckill`：

**使用事务注解**

* 判断 md5是否可用
* 执行秒杀逻辑：**1. 减库存，2.插入秒杀成功订单**

1. 减库存

调用`DAO`层`seckillMapper`接口的`reduceNumber`方法

得到更新记录数进行判断，若没有更新记录数就抛出**秒杀结束异常**

2. 插入秒杀成功订单

调用`DAO`层`successKilledMapper`接口的`insertSuccessKilled`方法

得到成功插入数进行判断，若没有成功插入则抛出**重复插入异常**

## dto包

dto层关注的是**Web和Service的数据传递**

* Exposer类
* SeckillExcution类

## enums包（代码不熟悉）

枚举几种秒杀执行结果的**状态**和**状态表示**

## exception异常包

* `SeckillExpection`(继承`RuntimeExpection`)

* `RepeatKillExpection`(重复秒杀异常---继承第一个异常)

* `SeckillCloseExpection`