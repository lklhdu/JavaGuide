# Lab1-Exercise4 

```c
//声明数组和指针
int a[4];
int *b = malloc(16);
int *c;
//打印得到a = 0xbfb4bf84
printf("1: a = %p, b = %p, c = %p\n", a, b, c);　　
//打印数组各元素的地址
printf("&a[0]=%p,&a[1]=%p,a[2]=%p\n",&a[0],&a[1].&a[2]);
```

得到`&a[0]=0xbfa878ec,a[1]=0xbfa878f0,a[2]=0xbfa878f4`;

所以`int`类型元素占4个字节。

指针是能够存放一个地址的一组存储单元（通常为2或4个字节），`int`类型指针占4个字节。

### 第五句打印语句

```c
c=c+1;
//打印第五句的相关操作
c=(int *)((char *)c+1);
*c=500;
printf("5: a[0] = %d, a[1] = %d, a[2] = %d, a[3] = %d\n",
   a[0], a[1], a[2], a[3]);
```

操作`c=(int *)((char *)c+1);` 先把c强制转换为char类型指针，然后指针加1，此时c的数值应该也加1，由于之前c指向a数组中1号元素的起始地址，1号元素起始地址为`0xbfb4bf88`，之后加1，变为`0xbfb4bf89`。然后再把c强制转换回int类型指针，此时c所操作的地址为`0xbfb4bf89~0xbfb4bf8c`。

把地址为`0xbfb4bf89-0xbfb4bf8c`的值换为500，此时会影响原来数组a的1,2号元素。原来一号元素在内存中存放在地址`0xbfb4bf88-0xbfb4bf8b`处，存放的值为400，十六进制为`0x00000190`

```
0xbfb4bf88  0xbfb4bf89  0xbfb4bf8a  0xbfb4bf8b   
   90         01          00          00 
```

 原来二号元素在内存中存放在地址`0xbfb4bf8c-0xbfb4bf8f`处，存放的值为301，十六进制为`0x0000012D`

```
0xbfb4bf8c  0xbfb4bf8d  0xbfb4bf8e  0xbfb4bf8f 
  2D          01          00          00 
```

而现在指针c操作的地址为`0xbfb4bf89~0xbfb4bf8c`，并赋值500，十六进制为`0x000001F4`，所以内存单元变为

```
0xbfb4bf88  0xbfb4bf89  0xbfb4bf8a  0xbfb4bf8b   
  90         F4           01          00 

0xbfb4bf8c  0xbfb4bf8d  0xbfb4bf8e  0xbfb4bf8f   
  00         01           00          00 
```

所以此时a[1]的值变为`0x0001F490 = 128144`, a[2]的值变为`0x00000100 = 256`.

### 第六句打印语句

```c
//这步操作会把b的值加1，由于b现在是int类型指针，所以数值上b的值增加4，所以b的值变为 0xbfb4bf88
b = (int *) a + 1;
//这步还是先把a转换为char型指针，然后加1，此时加1会让数值上只增加1，所以c的值变为 0xbfb4bf85
c = (int *) ((char *) a + 1);
printf("6: a = %p, b = %p, c = %p\n", a, b, c);
```

