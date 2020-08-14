# Java 数据类型间的转换

## char数组 -> String

1. `String str = new String(char[] c);`
1. `String str = String.valueOf(char[] c)`

## 数组 -> String

`Arrays.toString()`

![image-20200806092632712](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200806092635-229687.png)

返回的是逗号分割的String类型格式`[a, b, c, d]`