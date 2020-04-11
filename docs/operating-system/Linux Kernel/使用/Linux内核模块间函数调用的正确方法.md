# Linux内核模块间函数调用的正确方法

转载自 http://blog.csdn.net/xhz1234/article/details/44278137 Copyright 徐洪志(MacroSAN). All rights reserved.

### 模块之间发生调用关系是常有的事情，下面以两个模块A、B，B使用A模块提供的函数为例，讲解正确使用的方法。

模块A中使用`EXPORT_SYMBOL`或`EXPORT_SYMBOL_GPL`将要提供给B模块的函数导出；

模块B中用`extern`声明需要用到的A模块提供的函数。

代码如下：

模块A的代码 – A_func.c

```c
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/jiffies.h>
// Print jiffies
void A_print_jiffies(void){
    printk("jiffies is : %llu\n", (u64)jiffies);
    return；
}
EXPORT_SYMBOL(A_print_jiffies);
static int __init A_init(void){
    printk("A_func module init!\n");
    return 0;
}
static void __exit A_exit(void)
{
    printk("A_func module exit!\n");
    return;
}
module_init(A_init);
module_exit(A_exit);
MODULE_AUTHOR("XuHongzhi@MacroSAN");
MODULE_DESCRIPTION("Module A");
MODULE_VERSION("0.1");
MODULE_LICENSE("GPL");
```

模块B的代码 – B_func.c

```c
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/jiffies.h>
extern void A_print_jiffies(void);
static int __init B_init(void){
    printk("B_func module init!\n");
    A_print_jiffies();
    return 0;
}
static void __exit B_exit(void){
    printk("B_func module exit!\n");
    return;
}
module_init(B_init);
module_exit(B_exit);
MODULE_AUTHOR("XuHongzhi@MacroSAN");
MODULE_DESCRIPTION("Module B!");
MODULE_VERSION("0.1");
MODULE_LICENSE("GPL");
```

模块A的Makefile

```shell
obj-m := A_func.o
KERNELDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)
default:
        $(MAKE) -C $(KERNELDIR) M=$(PWD) modules
clean:
        rm -f *.o *.ko *.mod.c *.order *.symvers
```

模块B的Makefile

```shell
obj-m := B_func.o
KERNELDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)
default:
        $(MAKE) -C $(KERNELDIR) M=$(PWD) modules
clean:
        rm -f *.o *.ko *.mod.c *.order *.symvers
```

**接下来，有3种方式使得模块B编译及加载不出现Warning或Failed.**

*方法一：*

A模块在make之后，会产生一个Module.symvers文件，将该文件拷贝到B模块源文件目录中，然后执行make

*方法二：*

修改B模块的Makefile文件：

添加

```shell
KBUILD_EXTRA_SYMBOLS += /path_to_module_A/Module.symvers 
export KBUILD_EXTRA_SYMBOLS
```

此时B模块Makefile如下：

```shell
obj-m := xhz2_func.o
KERNELDIR := /lib/modules/$(shell uname -r)/build
PWD := $(shell pwd)
KBUILD_EXTRA_SYMBOLS += /home/xhz/Project/Temp_Module/Module.symvers
export KBUILD_EXTRA_SYMBOLS
default:
        $(MAKE) -C $(KERNELDIR) M=$(PWD) modules
clean:
        rm -f *.o *.ko *.mod.c *.order *.symvers
```

*方法三：*

修改Linux内核源码树中的Module.symers文件，将A模块编译产生的Module.symvers的内容添加在此文件中。（注意将空格替换为Tab，否则编译B时会报错）。

