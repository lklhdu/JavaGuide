# IDEA操作相关

### 在一个工程中启动多个实例

1. 在IDEA上点击Application右边的下三角,弹出选项后，点击Edit Configuration

![image-20200617082910179](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200617082911-717145.png)

2. 打开配置后，勾选Allow parallel run

![image-20200617083009193](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200721103808-808919.png)

3. 通过修改application.yml文件的server.port的端口，启动。多个实例，需要多个端口，分别启动。

### 创建项目的子模块

可以参考博客[spring cloud微服务项目搭建](https://www.cnblogs.com/sxdcgaq8080/p/9035724.html)

遇到的问题：pom文件没有被加载，在maven管理界面是灰色的

可以参考这篇博客解决：[在idea中创建maven父子工程，子工程无法导入父工程依赖的问题](https://www.cnblogs.com/sbk613/p/10414823.html)

### 无法加载或找不到主类

网上的解决方法很多，但是对我的情况却基本不管用

感谢这位博主提供的解决方法：[idea启动spring boot无法加载或找不到主类](https://blog.csdn.net/zpf_940810653842/article/details/81386319?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

步骤如下：

1. `mvn clean compile` 将项目重新编译
1. `mvn install` 打包
1. `mvn spring-boot:run` 启动项目

补充：感觉是我项目的打开方式不对，因为下载的源代码是一份maven父子模块的项目，使用下面的打开方式后没有出现过*找不到主类*的情况

### idea打开多模块的maven项目

以前直接用`open`打开项目文件夹，后来好几次无法正常打开

下面这种导入方式算是成功导入了：

1. `import project`
1. 选择导入类型为`maven`
1. 勾选`search for projects recursively`来递归搜索所有项目
1. 跳过`profiles`
1. 选择搜索到的项目（应该有多个模块），点击`finish`完成

### 垂直分屏、水平分屏

* 垂直分屏快捷键关键字：Split Vertically
* 水平分屏快捷键关键字：Split Horizontally

### 软分行

对于横向太长的代码可以进行软分行查看

### Ctrl + O

展示该类中所有覆盖或者实现的方法列表