# Ftrace使用

Ftrace位置：`/sys/kernel/debug/tracing`

## Event

在`event/`目录下可以查看支持的事件

每个具体的事件目录下都有一个 `enable` 的文件，我们只需要往里面写入 1，就可以开始 trace 这个事件。

以`sched_switch`为例

```shell
echo 1 > events/sched/sched_switch/enable
echo 1 > tracing_on
# 让内核运行一段时间，这样 ftrace 可以收集一些跟踪信息，之后再停止跟踪
echo 0 > tracing_on
cat trace
# 将trace文件复制到指定的位置
cp trace /home/lkl/Documents/
# 清除trace内的记录
echo > trace
```

**数据分析**

```shell
[root@linux tracing]# cat trace | head -10 
# tracer: sched_switch 
# 
#  TASK-PID    CPU#    TIMESTAMP  FUNCTION 
#     | |       |          |         | 
     bash-1408  [000] 26208.816058:   1408:120:S   + [000]  1408:120:S bash 
     bash-1408  [000] 26208.816070:   1408:120:S   + [000]  1408:120:S bash 
     bash-1408  [000] 26208.816921:   1408:120:R   + [000]     9:120:R events/0 
     bash-1408  [000] 26208.816939:   1408:120:R ==> [000]     9:120:R events/0 
 events/0-9     [000] 26208.817081:      9:120:R   + [000]  1377:120:R gnome-terminal
 events/0-9     [000] 26208.817088:      9:120:S ==> [000]  1377:120:R gnome-terminal

```

在 `sched_swich` 跟踪器获取的跟踪信息中记录了**进程间的唤醒操作**和**调度切换**信息，可以通过符号‘ + ’和‘ ==> ’区分；唤醒操作记录给出了当前进程唤醒运行的进程，进程调度切换记录中显示了接替当前进程运行的后续进程。

描述进程状态的格式为“Task-PID:Priority:Task-State”。以示例跟踪信息中的第一条跟踪记录为例，可以看到进程 bash 的 PID 为 1408 ，其对应的内核态优先级为 120 ，当前状态为 S（可中断睡眠状态），当前 bash 并没有唤醒其它进程；从第 3 条记录可以看到，进程 bash 将进程 events/0 唤醒，而在第 4 条记录中发生了进程调度，进程 bash 切换到进程 events/0 执行。

在 Linux 内核中，进程的状态在内核头文件 include/linux/sched.h 中定义，包括**可运行状态 TASK_RUNNING**（对应跟踪信息中的符号`R`）、可中断阻塞状态 **TASK_INTERRUPTIBLE**（对应跟踪信息中的符号` S`）等。同时该头文件也定义了用户态进程所使用的优先级的范围，最小值为 MAX_USER_RT_PRIO（值为 100 ），最大值为 MAX_PRIO - 1（对应值为 139 ），缺省为 DEFAULT_PRIO（值为 120 ）；在本例中，进程优先级都是缺省值 120 。

## 跟踪内核函数调用

```shell
echo function_graph > current_tracer
echo smp_apic_timer_interrupt > set_graph_function
cat trace
```



### 参考资料

1. 使用 ftrace 调试 Linux 内核，第 2 部分 https://www.ibm.com/developerworks/cn/linux/l-cn-ftrace2/index.html
1. Linux使用 ftrace 来跟踪内核函数调用 https://blog.csdn.net/SweeNeil/article/details/90038286