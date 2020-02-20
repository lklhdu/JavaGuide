## Matplotlib绘图

### Chapter 1

1. 导入模块：`import matplotlib.pyplot as plt`

2. 定义图像窗口：`plt.figure()`

3. 画图：`plt.plot(x, y)`

4. 定义坐标轴范围：`plt.xlim()` /`plt.ylim()`

5. 定义坐标轴名称：`plt.xlabel()`/`plt.ylabel()`

6. 定义坐标轴刻度及名称：`plt.xticks()`/`plt.yticks()`

7. 设置图像边框颜色

   ```python
   #获取当前坐标轴信息
   ax = plt.gca()
   ax.spines[].set_color()
   ```

8. 调整刻度位置：`ax.xaxis.set_ticks_position()`/`ax.yaxis.set_ticks_position()`

9. 调整边框（坐标轴）位置：`ax.spines[].set_position()`

### Chapter2

1. 添加图例：`plt.legend()`

   > 个性化
   >
   > * 参数 loc ：决定图例的位置
   > * 参数handles：选择图例中显示的内容
   > * 参数labels:用来单独修改之前的 label 信息

2. 画点：`plt.scatter()`

3. 添加标注：`plt.annotate()`

4. 添加注释：`plt.text()`

### Chapter3

#### 柱状图

* `plt.bar()`

* 修改颜色和数据标签

  > * 参数`facecolor` 设置柱状图主体颜色
  > * 参数`edgecolor` 设置边框颜色
  > * 函数 `plt.text()` 帮助在柱体上（下）方加上数值

### Chapter4

#### 多图显示合并

* `plt.subplot()` （推荐）

* `plt.subplot2grid()` 

* `gridspec.GridSpec()` （推荐）

  > 需导入`matplotlib.gridspec`

* `plt.subplots()`

#### 图中图

#### 次坐标轴

### Expand

* `Seaborn` : 在 Matplotlib 基础上进行了更高级封装的工具



### 参考资料

* Matplotlib绘图详解`https://www.kesci.com/home/column/5b87a78131902f000f668549`