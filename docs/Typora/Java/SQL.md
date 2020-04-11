# SQL

#### 编写顺序

`select 列a,聚合函数（聚合函数规范） from 表明 where 过滤条件 group by 列a​`

* 一般`group by`是和聚合函数配合使用

  `group by` 有一个原则,**就是`select` 后面的所有列中,没有使用聚合函数的列,必须出现在 `group by` 后面（重要）**

* `group by` 子句也和`where`条件语句结合在一起使用。当结合在一起时，`where`在前，`group by` 在后。即先对`select xx from xx`的记录集合用`where`进行筛选，然后再使用`group by` 对筛选后的结果进行分组。

* 使用having字句对**分组后的结果**进行筛选，语法和where差不多:having 条件表达式
  需要注意having和where的用法区别：

  1. **where后的条件表达式里不允许使用聚合函数（count(),sum(),avg(),max(),min()），而having可以**。
  2. having一般用在group by之后，对分组后的结果进行筛选。
  3. where肯定在group by 之前，即也在having之前。

#### 执行顺序

当一个查询语句同时出现了`where`,`group by`,`having`,`order by`的时候，执行顺序是：

1. 执行`where`对全表数据做筛选，返回第1个结果集。
2. 针对第1个结果集使用`group by`分组，返回第2个结果集。
3. 针对第2个结果集中的每1组数据执行`select xx`，有几组就执行几次，返回第3个结果集。
4. 针对第3个结集执行`having`进行筛选，返回第4个结果集
5. 针对第4个结果集排序`order by`

### Mybatis中sql语句（大于，小于，等于，不等于）表示

| 特殊字符 | 代替符号         |
| -------- | ---------------- |
| <        | `&lt;`           |
| >        | `&gt;`           |
| <=       | `&lt;=`          |
| >=       | `&gt;=`          |
| !=       | `<![CDATA[<>]]>` |
| &        | `&amp;`          |
| '        | `&apos;`         |
| "        | `&quot;`         |



### 参考资料

1. CSDN-superhosts https://blog.csdn.net/superhosts/article/details/39298529
2. group by 和 having 的用法 https://www.cnblogs.com/myhsg/archive/2008/08/05/1261386.html