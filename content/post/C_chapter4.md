---
title: "C语言第四章"
date: 2022-10-17T19:54:58+08:00
lastmod: 2022-10-17T19:54:58+08:00
draft: false
keywords: []
description: "C语言学习，第四章笔记"
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

## **字符串和格式化输入/输出**

```C
#include<stdio.h>
#include<string.h>  //提供strlen()函数的原型
#define DENSITY 62.4    //人体的密度 (单位：磅/立方英尺)
int main(void)
{
    float weight, volume;
    int size, letters;
    char name[40];  //name是一个可以容纳40个字符的数组

    printf("Hi! What's your first name?\n");
    scanf("%s", name);
    printf("%s, what's your weight in pounds?\n", name);
    scanf("%f", &weight);
    size = sizeof name;
    letters = strlen(name);
    volume = weight / DENSITY;
    printf("Well, %s, your volume is %2.2f cubic feet.\n",name,volume);
    printf("Also, your first name has %d letters,\n",letters);
    printf("and we have %d bytes to store it.\n", size);

    return 0;
}
```
<!--more-->

### 4.2 字符串

字符串是一个或多个字符的序列，一般用双引号括起来，但是双引号不是字符串的一部分（就如同单引号标注字符一样）

#### 4.2.1 char类型数组和null字符

* 事实上C语言没有专门用来存储字符串的变量类型，而是存储在char 类型的数组。数组由连续的存储单元组成，相邻的数个字节，每个字节存储一个字符（这可能就是将char所占的位定义为一个字节的原因）。

* 数组末尾应当以null字符（null character）结尾（即是\0,ASCII码是0）。这也就意味着数组应当比需要存储的字符数多1。例如上面的例子中``` char name[40] ```只能存储39个字符。

* null字符并不放在第40个字节，而是根据数组结束的位置放置

* 声明数组就用char来声明，变量名后面加上中括号数字用来表示占用的字节。

* 字符串使用转换声明 %s

#### 4.2.2 使用字符串

```C
/* 使用不同类型的字符串 */
#include<stdio.h>
#define PRAISE "You are an extraordinary being."
int main(void)
{
    char name[100];

    printf("What's your name? ");
    scanf("%s",name);
    printf("Hello, %s. %s\n",name,PRAISE);

    return 0;
}
```

* 在输入是不用刻意将null放在数组结尾，scanf()会自动完成这项工作。同时在define时，双引号括起来的字符串，编译器会自动加上null字符。
* 当scanf()函数读取输入时，如果输入有空格，只能输入空格之前的内容（使用 %s 转换说明时）。C语言还有其他输入函数，例如fgets( )。
* "x"与'x'的区别在于"x"是派生类型，占用两个字节，x与null。

#### 4.2.3 strlen( )函数（包含于string.h头函数）

* printf()处理长语句方法：在不同**参数**之间换行、使用两个printf。
* strlen( )与sizeof 的区别：strlen( ) 结果是字符的个数（含空格、标点，不含null），sizeof返回的是占用的内存字节数（包含未使用的预留空间）
* 也就是说 strlen( ) 知道在哪里停止，sizeof 不知道
* %zd 转换说明可以用于strlen
* sizeof 在对象不是类型（像char， long）时可以不加括号，但推荐都加

### 4.3 常量与C预处理器

>通过使用类似``` #define TAXRATE 0.015 ```,可以将TAXRATE定义为**符号常量**。编译时，编译-器会将程序中所有TAXRATE替换为0.015，称为编译时替换。这种常量也称为**明示常量**.
>经过编译器预处理之后，与没有明示常量的写法相同。
>
>优势：①便于理解常量的含义。②对于经常使用的值，便于更改。③不同于声明变量，不会被程序更改
>
>格式：#define name value
>
>命名约定，通常使用全大写表示符号常量名，或在名称前加c_或k_，但是后者不常用
>
>#define命令也可以用来定义字符或字符串`#define OOPS "Now you have done it!"`

#### 4.3.1 const 限定符

const限定符，用于限定一个**变量**为只读。例`const int MONTHS = 12`与define类似，使用起来更加灵活。

#### 4.3.2 明示常量

许多头文件中规定了一些明示常量，比如头文件`limits.h`中有`INT_MAX,INT_MIN`等明示常量，意为int的最大值和最小值。`float.h`中也有类似的常量，具体可以看**P79**.

### 4.4 printf( ) 与 scanf( )

#### 4.4.1 printf( ) 函数

printf( ) 函数使用转换说明应当与待打印的数据类型相匹配。

| 转换说明  | 输出                                                      |
| :------- | :-------------------------------------------------------- |
| %a       | 浮点数，使用十六进制以及 p 计数法                            |
| %A       | 浮点数，使用十六进制以及 p 计数法，区别在于大小写             |
| %c       | 单个字符                                                  |
| %d       | 十进制整数，有符号                                         |
| %e       | 浮点数，十进制以及 e 计数法                                |
| %E       | 浮点数，十进制以及 e 计数法，同上，区别在于大小写            |
| %f       | 浮点数，十进制小数                                         |
| %g       | 根据不同的值，自动选择 %f 和 %e。自动选择占用宽度小的方式     |
| %G       | 根据不同的值，自动选择 %f 和 %E。自动选择占用宽度小的方式     |
| %i       | 十进制整数，有符号，同 %d                                  |
| %o       | 八进制整数，无符号                                         |
| %p       | 指针                                                      |
| %s       | 字符串                                                    |
| %u       | 十进制整数，无符号                                         |
| %x       | 十六进制整数，无符号，前缀 0f                              |
| %X       | 十六进制整数，无符号，前缀 0F                              |
| %%       | 打印百分号 %                                              |

#### 4.4.2 使用 printf

printf 函数的格式是：

> `printf(格式化字符串，待打印项1，待打印项2，...);`

格式化字符串包含要打印的字符串和后面待打印项对应的转换说明

printf 参数中的待打印项可以是变量、常量或者是计算表达式。使用%%来表示一个百分号。

#### 4.4.3 printf 转换说明修饰符

> **表 4.4 使用多个修饰符时，顺序应与本表相同**
>
> | 修饰符 | 含义                                                         |
> | ------ | ------------------------------------------------------------ |
> | 标记   | 如下表，共 5 种标记，可以使用多个或不使用<br/>示例：" %-10d " |
> | 数字   | **最小**字段宽度<br/>如果该字段不能容纳数字或字符，系统自动使用更宽的<br/>如果字段不足该宽度，系统在打印前加空格<br/>示例：" %4d " |
> | .数字  | 精度<br/>对与%e、%E、%f，表示小数点后的位数<br/>对于%g、%G，表示有效数字的**最大**位数<br/>对于%s，表示打印字符串的最大数量<br/>对于整数转换，表示待打印数字的最小位数<br/>不足补零<br/>仅使用一个点时与后面加0相同<br/>示例：" %5.2f "，五宽度，两位小数的浮点数 |
> | h      | **以下都**和整型转换一起用，表示 short int 或 unsigned short int<br/>示例：%hu，%hx，%6.4hd |
> | hh     | 表示 signed char 或 unsigned char<br/>示例：%hhu，%hhx，%6.4hhd |
> | j      | 表示 intmax_t 或 uintmax_t。定义在stdint.h<br/>示例：%jd，%8jd |
> | l      | 表示 long int 或unsigned long int <br/>示例：%ld，%8lu       |
> | ll     | 表示 long long int 或 unsigned long long int                 |
> | L      | 与浮点数转换说明使用，表示 long double<br/>示例：%Lf，%10.4Le |
> | t      | 整型，表示 ptrdiff_t（两个指针差）<br/>示例：%td，%12ti      |
> | z      | 整型，表示 size_t（sizeof返回值）<br/>示例：%zd，%12zd       |
>
> **表 4.5 用来修饰转换说明的标记**
>
> | 标记 | 含义                                                         |
> | ---- | ------------------------------------------------------------ |
> | -    | 待打印项左对齐                                               |
> | +    | 对有符号值，为正加正号，为负加负号                           |
> | 空格 | 对有符号值，为正加空格，为负加负号<br/>'+'会使后面的空格被忽略，编译器可能报错 |
> | #    | 将结果转换形式，例如：%#o，就表示以0开头的八进制。%#x或%#X同理<br/>对于浮点数，#代表即使没有小数部分，也打印小数点`%.e`与`%#.e`后者会打印小数点<br/>对于%g和%G，能防止后面的0被删除<> |
> | 0    | 用前面0填充字符宽度，与 '-' 以及精度标记小数点 '.' 冲突 ,会被忽略                            |

具体示例见 4.7.width.c 和 4.8.floats.c
表4.4中数字定义的是打印出来的最小**宽度**，包含 'E''+' '.' 等字符占用位置

#### 4.4.4 转换说明的意义

转换说明本质上是将计算机中存储的二进制值转换成一系列用于显示的字符。不同的转换说明翻译方式不同，对于相同的二进制存储 01001100，%d转换说明会翻译成76，%x转换说明会翻译成 4c。

##### 1. 转换不匹配（整数）

* 负值使用 %u 来转换，虽然转换说明识别的位数是正确的，但是翻译方式错误.
* 当整数的位数超过 char 的位数（一字节）会发生截断，即仅有后面8位的二级制数字被使用。**用数学来看就是用整数除以256的余数**，称为**以256为模**
* 使用 %hd （-32768~32767）来转换大于65536的整数，例如 65618 也会发生类似的情况。short int 占用两字节，int 占用四字节。故65618除以65536的余数是表示的值。而当该值又大于32767，则会类似负值使用 %u 转换的情况

##### 2. 转换不匹配（浮点数）

##### 3. printf 的返回值

大部分C函数有返回值，例如 sqrt( ) 返回参数的平方根。printf 也有，printf 的返回值是打印的字符数量（宽度？），包括空格以及换行符。如果输出有误，则输出一个负值。

对printf，返回值更多的是输出的副产物，等于号 = 不会影响输出。

##### 4. 打印较长的字符串

* 在参数之间换行，注意需要在逗号之后

* 字符串中间换行有两种方式
  * 使用转义字符 \ 标记，换行之后注意不能有缩进
  * 使用多个双引号，打印多个字符串：printf("hello " "world")

#### 4.4.5 使用scanf

* scanf 读取基本变量值，变量名前加 &

* 读取字符串，变量名前不加 &

sanf通过空白（包括换行、空格、制表），将输入分成多个字段。依次将转换说明和输入字段匹配，跳过空白。例外的是 %c ,该转换说明会读取每个字符，包括空白。

转换说明与 printf 基本相同，区别主要在于%f、%e、%E、%g以及%G都只代表单精度浮点数，有修饰符号 l 才是双精度。

![表 4.6](https://raw.githubusercontent.com/whwh13/picgo/master/2022/10/15/20221015190704.jpg)

**修饰符：**

| 修饰符 | 含义                                                |
| ------ | --------------------------------------------------- |
| *      | 抑制赋值（后面说明）                                |
| 数字   | 最大宽度，输入达到最大宽度或空白时停止              |
| hh     | 整数作为signed char 或 unsigned char：%hhd、%hhu    |
| ll     | 整数作为long long 或 unsigned long long：%lld、%llu |

![表4.7](https://raw.githubusercontent.com/whwh13/picgo/master/2022/10/15/20221015192625.jpg)

%lo、%ho确实会被编译器视为无符号，但输入负值也能识别，但是具体的运用不甚明确，有用到再查吧。

**P94**有描述scanf的原理，这里不再赘述。

使用scanf(“ %c”,&ch),也就是在%c之前放一个空格，会跳过所有空白（任意个）

##### scanf返回值

返回成功读取的项数。当读取数字而用户输入非数值字符，返回 0 .当检测到文件结尾，返回EOF。

#### 4.4.6 printf 与 scanf 的 * 修饰符

在 printf 中，可以通过 * 修饰符代替数字，其值由后面的参数确定。例如`printf("Weight = %*.*f\n",wigth, precision, weight)`

在 scanf 中，*放在% 与转换字符中，表示跳过该输出项（读取对应输入，但是并不对应参数）

```C
#include<stdio.h>    
int main(void)
{
  int n;

  printf("Please enter three integers:\n");
  scanf("%*d %*d %d", &n);
  printf("The last integer was %d\n", n);
    
  return 0;
}
```

#### 4.4.7 printf 一些用法

* 使用多次printf打印数字时，可以通过指定宽度，让数字相互对齐，美观
* 使用printf 时，不同数字之间应该加一个空格，避免数字超出指定宽度时，两个数字相互贴近被视为一个数字
