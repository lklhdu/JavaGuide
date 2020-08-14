# C指针 specific point

* **first point**

  当将整数添加到指针时，该整数将隐式乘以指针指向的对象的大小

  ```c
  int *p=(int *)100;
  (int)p+1=101;
  (int)(p+1)=104;
  ```

* **second point**

  ```c
  int *p;
  p[i]=*(p+i);//都指的是p指向的内存中的第i个对象（对象可能大于一字节）
  ```

* **third point**

  ```c
  int *p;
  &p[i]=(p+i);//都表示p指向的内存中的第i个对象的地址
  ```

* `uintptr_t`表示`virtual addresses`;`physaddr_t`表示`physical addresses`.它们都是整数类型

* 物理地址-->虚拟地址 `KADDR(pa)`;虚拟地址-->物理地址 `PADDR(pa)`