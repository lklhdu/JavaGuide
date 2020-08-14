# Linux 卸载编译的内核

**卸载下面位置的文件**

* /boot/vmlinuz*KERNEL-VERSION*
* /boot/initrd*KERNEL-VERSION*
* /boot/System-map*KERNEL-VERSION*
* /boot/config-*KERNEL-VERSION*
* /lib/modules
* KERNEL-VERSION代表你想卸载的内核的版本号
* 最后必须更新grub ： update-grub

