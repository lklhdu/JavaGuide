# Linux时钟中断处理

**arch/x86/include/asm/irq_vectors.h**

```c
#define LOCAL_TIMER_VECTOR              0xef
```

时钟中断函数 `smp_apic_timer_interrupt`

```c

static void local_apic_timer_interrupt(void)
{
        int cpu = smp_processor_id();
        struct clock_event_device *evt = &per_cpu(lapic_events, cpu);
        if (!evt->event_handler) {
                pr_warning("Spurious LAPIC timer interrupt on cpu %d\n", cpu);
                /* Switch it off */
                lapic_timer_shutdown(evt);
                return;
        }
        
        inc_irq_stat(apic_timer_irqs);
 
        evt->event_handler(evt);
}
```

evt->event_handler 指向的是函数`hrtimer_interrupt()`

## 本地时钟中断处理程序

汇编语言函数`apic_timer_interrupt()`等价于下面的代码

```c
apic_timer_interrupt:
	pushl $(239-256)
	SAVE_ALL
	movl %esp,%eax
	call smp_apic_timer_interrupt
	jmp ret_from_intr
```

可以看到，该低级处理函数与中断部分的低级处理函数非常相似

## 中断亲和力

中断亲和力是指将一个或多个中断源绑定到特定的 CPU 上运行。

| irq  | name           | affinity | cpu  |
| ---- | -------------- | -------- | ---- |
| 0    | timer          | -        | -    |
| 1    | i8042          | 8        | 3    |
| 8    | rtc0           | 4        | 2    |
| 9    | acpi           | 4        | 2    |
| 12   | i8042          | 2        | 1    |
| 16   | vmwgfx,snd_ens | 2        | 1    |
| 17   | ehci_hcd,ioc0  | 8        | 3    |
| 18   | uhci_hcd       | 8        | 3    |
| 19   | ens33          | 4        | 2    |
| 57   | vmw_vmci       | 8        | 3    |

### 参考资料

1. Linux时钟中断处理 https://blog.csdn.net/bgao86/article/details/51793853