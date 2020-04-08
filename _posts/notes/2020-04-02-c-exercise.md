---
layout: post
category: "notes"
title:  "c编程练习和习题"
tags: [c]
---



# 编程练习




最大公约数：

```c
#include <stdio.h>

int gcd(int a, int b)
{
	if (a%b == 0)
		return b;
	else {
		int c = a%b;
		a = b;
		gcd(a, c);
	}
}

int main(int argc, char *argv[])
{
	printf("%d\n", gcd(11, 6));
}
```







汉诺塔问题：

传送门：https://www.bilibili.com/video/av9830115?share_medium=android&share_source=copy_link&bbid=8472E161-0EC9-4248-AAEE-B5ED946102C5110245infoc&ts=1585825706585

```c
#include <stdio.h>

// n@ tower numbers
int hanoi(int n ,char x, char y, char z)
{
    void move(int, char, char);
    if (n == 1) {
        move(n, x, z);
    } else {
        hanoi(n-1, x, z, y);
        move(n, x, z);
        hanoi(n-1, y, x, z);
    }
}

// put marked towner number from x to y
void move(int num, char x, char y)
{
    static int k = 1; //steps number
    printf("%2d:%3d # %c -> %c\n", k, num, x, y);
    k++;
}

int main()
{
    hanoi(4, 'A', 'B', 'C');

    return 0;
}
```













# 习题

## sizeof

```c
#include <stdio.h>
int main()
{
    int i = 1;
    printf("%d, %d\n", sizeof(i++), i);
    return 0;
}
```

这个题和编译器无关，输出一定是 4,1

分析：sizeof是编译器的武器，编译器用它只看结果不会计算的。sizeof(i++)中i++肯定是int类型，所以在编译期间这个语句就变成4了，i++没有执行。



## printf返回值

下面程序输出什么？

```c
int main()
{
    int i = 43;
    int n = printf("%d\n", i);
    printf("%d\n", n);
    return 0;
}
```

输出结果：

43

3

好，大家深入考虑一下，为什么返回是3 。这背后有什么鲜为人知的秘密，到底是C语言离奇的规定，还是深思熟虑后的决定？

相信大家都在学习驱动的时候应该知道有一种字符设备驱动。在 linux 中一切东东都是文件，外设也是文件，也就是说显示器也是文件，那么 printf 的实现其实就是调用显示器的驱动程序往这种外设写入数据，所以我们来考虑一下，显示器属于什么设备呢，字符型设备。所以 printf 返回的其实不应该是输出的字符个数，准确的说应该是向字符设备写入的数据的字节数。因为 char 占用一个字节，所以碰巧“printf 返回输出字符的个数”，这个说法正确了。

4 3 \n，三个字符个数



又如下例：

```c
#include<stdio.h>
int main() 
{   
   int i=43;     
   printf("%d\n",printf("%d",printf("%d",i)));  
   return 0;
}
```

输出：4321



## 溢出，补码，类型自动转换

下面代码输出什么？为什么？

```c
int main()
{
    char a[1000];
    int i;
    for(i=0; i<1000; i++)
    {
        a[i] = (char)(-1 - i);
    }
    printf("%d\n", strlen(a));
    return 0;
}
```

> 输出结果为 255
>
> 分析：
> 我们来逐一计算 a 中元素的值：
> a[0] = -1, a[1] = -2, a[2] = -3, … , a[126] = -127, a[127] = -128, a[128]= 127, a[129] = 126, … , a[255] = 0; ……
> 在 C 语言中 char 的取值范围是[-128, 127]，所以当 i=127 的时候 a中的元素 a[127]将存储 char 类型所能存储的最小值-128；当 i=128的时候(-1 - 128)的值为-129，这个时候 char 类型将无法表示这个数值，于是产生了溢出（为何溢出后的值为 127，请参考《计算机组成原理》的相关内容），其结果反转为 char 所能表示的最大值 127，以此类推，当 i=255 的时候，a 数组中出现第一个 0 值（就是'\0'）。所以，将数组 a 作为参数调用 strlen 得到的结果为 255。



下面程序在80x86架构下，输出什么值？

```c
union Test
{
    char a[4];
    short b;
}
Test t;
t.a[0] = 256;
t.a[1] = 255;
t.a[2] = 254;
t.a[3] = 253;
printf("%d\n", t.b);
```

> 答案： -256 
> 分析：
> char类型的取值范围是-128～127，unsigned char的取值范围是0～256
> 这里a[0]=256,出现了正溢出，将其转换到取值范围内就是0，即a[0]=0;
> 同理，a[1]=-1, a[2]=-2, a[3]=-3,在C语言标准里面，用补码表示有符号数，故其在计算机中的表示形式如下：
> a[0]=0, 0000 0000
> a[1]=-1, 1111 1111（反码+1 = -1）
> a[2]=-2, 1111 1110
> a[3]=-3, 1111 1101
> short是2字节（a[0]和a[1]），由于80X86是小端模式，即数据的低位保存在内存的低地址中，而数据的高位保存在内存的高地址中，在本例中，a[0]中存放的是b的低位，a[1]中存放的是b的高位，即b的二进制表示是：1111 1111 0000 0000，表示-256



有符号整型和无符号整型混合运算时，有符号型要转换成无符号型，运算的结果是无符号的。

```c
# include <stdio.h>
int main(void)
{
    int a = -10;
    unsigned b = 5;
    if ((a+b) > 0)
    {
        printf("Hello\n");
    }
    return 0;
}
```

输出结果是：Hello



下面代码的输出结果是？

```c

int main()
{
	unsigned char a = 0;
	for (a = 0; a < 256; a++)
		printf("%d\n", a);
	system("pause");
	return 0;
}
```

这是个死循环。

unsigined char是个无符号型，for循环，不受控制。





## #宏

下面的代码输出什么？为什么？

```c
#include <stdio.h>

#define f(a,b) a##b
#define g(a) #a
#define h(a) g(a)

int main()
{
    printf("%s\n", h(f(1, 2)));
    printf("%s\n", g(f(1, 2)));
    return 0;
}
```

> 结果：12 f(1, 2)
>
> 分析：
> 1：#在宏中表示将后面的符号变成C语言字符串；##表示将两个符号连接成一个新的符号。
> 2：宏和函数的调用不同，当宏定义中还使用了其他宏的时候会先扩展嵌套的宏，否则就直接进行宏替换。如h(f(1, 2))会先扩展f(1, 2)是12，然后在h(12) = "12"



下面的代码输出什么？为什么？

```c
#include <stdio.h>

#define PrintInt(expr) printf("%s: %d\n", #expr, (expr))

int FiveTimes(int a)
{
    int t;
    t = a << 2 + a;
    return t;
}

int main()
{
    int a = 1;
    PrintInt(FiveTimes(a));
    return 0;
}
```

结果：FiveTimes(a): 8



下面代码输出的结果为？

```c
#define a 10
void foo();
int main()
{
    printf("%d.", a);
    foo();
    printf("%d", a);
}
void foo()
{
    #undef a
    #define a 50
}
```

> 答案：10.10
> 分析：#undef 是在后面取消以前定义的宏定义。define在预处理阶段就把main中的a全部替换为10了.
> 另外，不管是在某个函数内，还是在函数外，define都是从定义开始知道文件结尾，所以如果把foo函数放到main上面的话，则结果会是50 ，50



#define中用到了array，但是array在后面才定义的，合法吗？为什么？最后程序输出什么？

```c
#include <stdio.h>
#define TOTAL_ELEMENTS (sizeof(array) / sizeof(array[0]))
int array[] = {23,34,12,17,204,99,16};
int main()
{
    int d;
    for(d=-1;d <= (TOTAL_ELEMENTS-2);d++)
    printf("%d\n",array[d+1]);
    return 0;
}
```

> 第一个： #define 中用到了 array， 但是 array 在后面才定义的，合法吗？为什么？
> 其实合法的，在编译之前是预编译，预编译会处理#define之流的东东，在编译时这个define就没了。
> 第二个：程序输出什么？
> 程序不会输出任何东西。因为 int 和 unsigned int 比较时会被转换为无符号的，因此-1就直
> 接被看成 0xFFFFFFFF 了，这样 d 不可能小于条件中的表达式。自然 for 不会执行。 注意一点 sizeof 是编译的工具，它的计算结果是无符号的。



## 字符串

对下面两个文件编译后，运行会输出什么？

```c
// 第一个文件a.c
#include <stdio.h>
extern char p[];
extern void f();

int main()
{
    f();
    printf("a.c: %s\n", p);
    return 0;
}
```

```c
// 第二个文件b.c
char* p = "Hello World";
void f()
{
    printf("b.c: %s\n", p);
}
```

> 打印结果：
>
>  b.c: Hello World
>  a.c: 0@
>
>
> 分析：在我们看来，虽然使用字符数组和字符指针差不多，printf都可以打印出字符串出来，但是编译器对他们的处理完全不同。
>
> 对于字符指针，编译器看到后，会把里边保存的值取出来，然后在去这个地址值处，将字符串取出来（进行一次寻址）；对于字符数组，编译器直接到数组首地址处打印字符串。
>
> 在这里b.c定义的是字符指，也就是说p的地址不是“helloworld”的地址，p中保存的才是“helloworld”的地址。但是到了a.c里面，却声明成了数组，所以编译的代码就不会进行寻址了，直接把p的地址当成了“helloworld”的地址打印出来。





## 数组

下面程序输出什么？

```c
#include <stdio.h>
int main()
{
	int a[5][5];
	int (*p)[4];
	p = a[0];
	printf("%d\n", &p[3][3] - &a[3][3]);
	return 0;
}
```

> 答案是 -3，而不是 0，为什么呢？
>
> 数组 int a[5][5]; 可以等价于
> typedef int(AA)[5];
> AA a[5];
>
> a[3][3]的地址是：
> (unsigned int)a + 3 * sizeof(AA) + 3 * sizeof(int) ==>
> (unsigned int)a + 3 * 20 + 3 * 4
>
> p[3][3]的地址：
> (unsgined int)a + 3 * 16 + 3 * 4
>
> 于是：&p[3][3] - &a[3][3]就等于：
> （3 * 20 - 3 * 16）/4 = -3





关于 int a[10]; 问下面哪些不可以表示 a[1] 的地址？

```c
A. a+sizeof(int) 
B. &a[0]+1 
C. (int*)&a+1 
D. (int*)((char*)&a+sizeof(int))
```

分析：
 A. a+sizeof(int)
 // No， 在32位机器上相当于指针运算 a + 4
 B. &a[0]+1
 // Yes，数组首元素地址加1，根据指针运算就是a[1]的地址
 C. (int)&a+1
 // Yes，数组地址被强制类型转换为int，然后加1，这样和B表示的一个意思
 D. (int)((char)&a+sizeof(int))
 // Yes，数据地址先被转换为char，然后加4，根据指针运算公式，向前移动4  sizeof(char)，之后被转换为int*，显然是a[1]的地址



是地址之差还是地址下标之差？

```c
int* p = 0;
printf("%d\n", &(p[9]));
printf("%d\n", &(p[9])-p);
```

打印：36, 9 

地址相减则为下标差。



下面程序输出什么？

```c
#include <stdio.h>
int main()
{
    int a[5] = {1, 2, 3, 4, 5};
    int* p1 = (int*)(&a + 1);
    int* p2 = (int*)((int)a + 1);
    int* p3 = (int*)(a + 1);
    printf("%d, %d, %d\n", p1[-1], p2[0], p3[1]);
    return 0;
}
```

> 打印结果：(32位系统，指针为32位)
>
>  5, 33554432(0x2000000), 3
>
> 分析：p1和p3没有问题，关键是p2，为什么是0x2000000呢？
> 我们来看a的地址分布：a[0] -> a[4] ==>低地址 -> 高地址
> 01 00 00 00 02 00 00 00 03 00 00 00 04 00 00 00 05 00 00 00
> 然后(int)a+1指向的地址如下：
> 00 00 00 02 00 00 00 03 00 00 00 04 00 00 00 05 00 00 00
> 然后p2就指向这个地址了，之后将这个地址当成整形输出，就是输出下面的东西：
> 00 00 00 02 00 00 00 03 00 00 00 04 00 00 00 05 00 00 00
> 组合成十六进制的形式就是0x02000000。



## 指针

char * * string = "abcdefg";　　 * (*string)++；　这个表达式的值是什么？

分析：string是一个二级指针，却保存着“abcdefg”的地址。 * string代表到string指向的内存中取一个大小为sizeof( * string)的数据。string指向“abcdefg”，那么假设取的4字节数据就是0x12345678，之后是 * （0x12345678）++，意思是到0x12345678中取值，谁知道运行时0x12345678地址的值是什么呢？？？





## 地址踩踏

下面程序输出结果是什么？为什么？

```c
int main()
{
    int i;
    int a[5];
    
    for(i=0; i<=5; i++)
    {
        a[i] = -i;
        printf("%d, %d\n", i, a[i]);
    }
    return 0;
}
```

答案：死循环

相信大家一眼就可以看出本题的主要问题是数组 a 只有 5 个元素，访问其元素的下标分别是0， 1， 2， 3， 4。当循环变量 i 的值为 5 的时候将访问 a[5]，这个时候产生了一个数组越界的错误。但问题是，为什么会产生死循环，当 i 的值为 5 的时候，程序究竟做了什么？在 C语言中产生死循环只有一个原因，就是循环条件一直为真。在本题中也就是 i<=5 将一直成立。可问题是，循环变量 i 在每次循环结束后都做了 i++。理论上，不可能产生死循环。为了弄清楚问题的本质，我们先弄清楚 a[5]究竟代表什么？在 C 中 a[5]其实等价于(a+5)，也就是说当 i 的值为 5 的时候，将对 a+5 这个内存空间进行赋值。很不幸，在本题中 a+5 这个内存空间正好就是 i 的地址，也就是说当 i 的值为 5 的时候，在 for 循环中的赋值语句其实
等价于 i=-i，即将 i 赋值为-5，因此 i<=5 永远满足。更进一步，为什么 i 正好在 a+5 这个位置呢？



在分配栈空间的时候，先分配i的地址（处于栈的高地址），然后分配a[5]的栈地址（处于栈的低地址），但是在a[5]内部的访问从a[0] -> a[4]却是从低地址 -> 高地址的，这样如果越界，就会与i的地址冲突。



我们看下面的例子：

```c
int main()
{
    int i;
    int a[5];
    
    printf("%X\n", &i);
    printf("%X\n", a+4);
    printf("%X\n", a+3);
    printf("%X\n", a+2);
    printf("%X\n", a+1);
    printf("%X\n", a);
}
```

最后得到的结果是：
BFA5F240
BFA5F23C
BFA5F238
BFA5F234
BFA5F230
BFA5F22C
从运行结果可以知道：局部变量 i 确实紧跟着数组中的第 4 个元素 a[4]在栈上分配空间，因此 a[5]就是 i。

（类似的还有之前《[C/C++练习1](http://blog.csdn.net/lvonve/article/details/53286916)》的第二小题，也是一样的道理。）







## 其他

6. 



2、下面程序输出什么？
int main()
{
    int a;
    int *p;
    p = &a;
    *p = 0x500;
    a = (int )(*(&p));
    a = (int )(&(*p));
    if(a == (int)p)
        printf("equal !\n");
    else
        printf("not equal !\n");
    return 0;
}
答案：equal !

 a = (int )((&p));和a = (int )(&(p));打印的都是p的地址。



























