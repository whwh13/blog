---
title: "C语言第三章"
date: 2022-10-17T19:54:54+08:00
lastmod: 2022-10-17T19:54:54+08:00
draft: false
keywords: []
description: "C语言学习，第三章笔记"
tags: ["C语言","学习","笔记"]
categories: ["C语言"]
author: "whwh13"

# Uncomment to pin article to front page
# weight: 1
# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: true
autoCollapseToc: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false

# Uncomment to add to the homepage's dropdown menu; weight = order of article
# menu:
#   main:
#     parent: "docs"
#     weight: 1
---
## 基本数据类型

### int类型

#### 1. 声明

可以通过int进行声明，可以在同一行声明多个变量，中间用逗号分隔

#### 2. 初始化变量

在声明变量的过程中可以为变量赋予初始值，例如：

``` C
int cows=32,goats=46;
```

在同一条声明中也同时可以由赋予初始值的声明与不赋予初始值的声明，但这种格式容易让人误解，不太适合

#### 3. int常量

例如22，44都是整型常量，但是 22.0、2.2E1 不是。C语言将大多数整型常量视为int类型。

#### 4. 打印int值

%d 指明在一行中整数的位置。每个 %d 应当与变量列表中的int值相对应，未对应数量会导致编译器无法识别的错误。
例如：

<!--more-->

```C
printf("%d minus %d is %d\n"，ten);           
```

在 C17 中 printf 变量必须对应数量，否则编译失败

#### 5. 八进制与十六进制

0x 与 0X 表示十六进制，0 表示八进制，都放在前缀。无论将一个数写作 0x10 、10 或是 08 在计算机的存储中是相同的（都是数字0）

#### 6. 打印输出八进制与十六进制

%d,%x %X,%o，分别是十进制、十六进制、八进制的占位符
而使用 %#x,%#o 则会显示前缀 0x、0 ,使用 %#X 会显示前缀 0X .

```C
#include<stdio.h>
int main(void)
{
int x =100;
printf("dec=%d,octal=%o,hex=%X\n",x,x,x);
printf("dec=%d,octal=%#o,hex=%#X",x,x,x);

return 0;
}
```

### 其他整数类型

C语言提供3个附属关键字对整数类型修饰：short、long以及unsigned。
C语言并不对溢出行为进行规定

* short int 可以简写为 short 占用的存储空间比 int 少
* long int (long)，long long int（long long） 分别是长与更长，适用于不同的大数值场合
* 在win11 64位中，分别占用 16位、32、64、128位
* unsigned int(unsigned) 表示无符号，范围是 0~2^32-1，也可以与其他进行组合，占用相同位数表示更大的值
* signed long int 与 long int 并无区别
* 选用时如不必使用负数应当首先考虑 unsigned 类型，如非必要不要使用long和long long ，但是应当考虑到部分机器中 int 可能只占用16位的移植性问题。
* short 在通常的使用中不必要考虑

* 常量：编译器会依次将常数视为：int, long, unsigned long, long long, unsigned long long
* 如果想特地用long或者longlong存储常量，仅需要在数字后面加上 l 或者 L 、ll 或者 LL ，例如：0X12、0ll

#### 打印short,long,unsigned类型

>unsigned:   %u
long: %ld
long long: %lld
long十六进制:  %lx
short: %h
八进制short:  %ho
unsigned long:   %lu

**原则是h和l和ll是前缀，u，o/x/d是后缀,可以相互搭配
使用printf时应保证每个值都有其占位符以及每个占位符与其类型一致。**

### char类型

char用于存储字符，实际存储的是整数，用整数的作为字符的代号。例如ASCII编码，范围0~127，例如65表示 A 。由于ASCII码仅仅用7位二进制数即可表示。因此 char 通常定义为8位的存储单元。其他系统提供拓展ASCII码也在8位表示中。

```ruby
许多字符集超过127甚至超过255，例如Unicode超过110000个
```

C语言将一字节定义为char占用的位（bit）数，而非一般意义上的8位，因此无论是16位还是32位都能用 char 类型。

#### 字符常量与初始化

* 在C语言中，将**单引号**括起来的**单个字符**称为字符常量，编译器发现即会将其转换成相应的代码值
* 如果没有单引号，编译器会认为是一个变量，如果是双引号，编译器会认为是字符串

```C
char broiled;       /* 声明变量，可以同时赋初始值 */
broiled = 'T';      /* 赋值，正确 */
broiled = T;        /* 赋值错误，此时T表示一个变量 */
broiled = "T";      /* 赋值错误，此时T是一个字符串 */
```

* 事实上，字符是以数值形式存储的，所以可以用数字代码赋值 `char grade = 65;`,该代码会给 grade 赋值 A（如果系统能够使用ASCII码），但是这是一种非常不好的编程风格
* 奇怪问题，C语言将字符视为int类型，即在int为32位，char为8位的系统中`char grade = 'FATE'`等价于`char grade = 1178686533 或者 OX46415445`,在ASCII中，F为`0X46`。因此这样反而相当于`char grade = 'E'`，因为只占8位，只有最后8位。（但是报错）

#### 非打印字符

单引号只适用于字符、数字和标点符号，但是有些ASCII字符无法打印，比如：退格、换行、蜂鸣等，无法使用单引号字符表示。C语言提供了3种方法表示。

1. 使用ASCII码直接表示赋值`char beep = 7`,表示蜂鸣，这里是十进制常量。
2. 使用特殊的符号序列，例如`char nerf ='\n'`，打印该变量，效果为在屏幕上另起一行（相当于直接用\n）。

    | 转义序列| 含义     |
    | ----   | ---- |
    | \a     | 警报       |
    | \b     | 退格       |
    | \f     | 换页       |
    | \n     | 换行       |
    | \r     | 回车       |
    | \t     | 水平制表符  |
    | \v     | 垂直制表符  |
    | \ \    | 反斜杠（\） |
    | \ '    | 单引号      |
    | \ "    | 双引号      |
    | \ ?    | 问号        |

    * \a, \b, f, \n, \r, \t, \v都是对屏幕光标的具体位置进行调整，其中\a代表发出警报但是不改变光标位置。垂直制表符和换页符在PC 中可能无法识别
    * \ \, \ ', \ " 是因为此些符号是printf() 函数的一部分，直接使用会造成混乱。

3. 使用转义字符加八进制或十六进制ASCII码
\0(xx)  \x(xx)  括号里的XX表示数字，具体使用时不加括号，分别是八进制和十六进制。十六进制x必须用小写。
例如`beep = '\007'`在使用八进制时，最前面的0可以省略，比如`beep = '\07'`或者`beep = '\7'`,注意这里的都是八进制数字
对`test='\x10'`与`char test=16`与`test = '\020'`具有相同的结果

4. 几个问题

* 使用转义字符时,如果被括在双引号内,表示字符串,就不用单引号再括起来
* 方法3中的使用ASCII码尽量不要使用,以保证格式统一以及便于记忆
* 转义字符可以直接放入字符串中.

#### 打印字符char

使用 c% 作为占位符表示字符，也可以使用 d% 作为占位符会输出对应的代码。printf() 函数中的转换说明（占位符）说明程序的显示方式，不改变存储。

### _Bool 类型

_Bool 类型用于表示布尔值，1表示true，0表示false，原则上仅占用1位空间。

### 可移植类型：stdint.h、inttypes.h

用于确保在不同系统中效果相同。P55需要时查看

### float

float至少表示6位有效数字，取值至少为 10^(-37)~10^37 ，double双精度至少表示10位有效数字。一般会占用64位。long doluble比double精度更高，但是未规定具体数值。

1. 声明
    举例：

    ```C
    float noah = 6.63e-34;
    long double gnp;
    ```

2. 常量

    浮点型常量一般表达为有小数点的数（有符号）后面加e或E。
    规则：可省略正号、可省略小数点或指数（为0时）、可省略小数或整数（为0时），不能同时省略两者，否则会像整数。
    不能在浮点型常量中间加空格（E前后）。

    默认情况，编译器使用double浮点量，
    但是对于

    ```C
    float some;
    some = 4.0 * 2.0;
    ```

    会出现4.0 和2.0 都是double ，计算之后结果再截断为float ，尽管精度较高，但会减慢程序运行速度。于是在浮点数后面加f 或者F 可以设定为float，加l 或L 可以设定long double。C99中加入十六进制表示浮点数,例如
    ```0xa.1fp10```
    a等于十进制10，.1f是1/16 加上15/256，（十六进制f 为15/256），p相当于十进制的E 但是是2的意思，即 2^10 。于是该值表示为（10+1/16+15/256）*1024。

3. 打印

    printf() 函数使用占位符%f 打印float或double，用%e 打印指数计数法。用%a 表示十六进制的的指数计数。
    打印long double使用%Lf、%Le、%La。

4. 上溢和下溢

    当数字过大（计算导致）会发生上溢，会给该函数赋予一个无穷大值，打印为inf或infinity
    下溢类似于把（0.1234E10）除以10会有（0.0123E-10）这样会失去精度叫下溢。
    NAN，如果给asin()函数大于1的量，函数返回NAN。

5. 浮点数舍入错误

    如下演示，不同系统可能出现0.0000、-13584010575872.0000、4008175468544.000000 多种可能

    ```C
    #include<stdio.h>
    #include<math.h>
    int main(void)
    {
    float a,b;

    b=2.0e20 +1.0;
    a=b-2.0e20;

    printf("%f \n",a);

    return 0;
    }   
    ```

    出现该原因是由于浮点数的大数吞小数

### 复数与虚数

``` float \_Complex、double \_Complex、long double \_Complex ```每个复数含有两个浮点数，表示实部和虚部。
``` float \_Imaginary、double \_Imaginary、long double \_Imaginary ```表示虚数
当包含complex.h时，可以使用complex代替_Complex，imaginary代替_Imaginary。

### 各种类型大小

见typesize.c

## 使用数据类型

应当注意为合适的变量选择类型，比如表示人数，就尽量用整数。在赋值时，应当保证右侧常量类型与左侧相同。但是初始的定义不与要满足，往往带来问题。例如```int cost= 12.99  float pi=3.1415926536```。赋予cost为12，而float也会有截断问题。

## 转义序列示例

### 刷新输出

C语言规定，在缓冲区满、换行或者需要输入时刷新屏幕，如果需要刷新屏幕没有刷新，可以用换行强制刷新。
