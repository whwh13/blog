---
title: "C语言第七章 C控制语句：分支和跳转"
date: 2022-11-21T15:47:05+08:00
lastmod: 2022-11-21T15:47:05+08:00
draft: false
keywords: []
description: "C语言学习，第七章笔记"
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
## 7.1 if 语句

if 语句被称为**分支语句(branching statement)**或者**选择语句(selection statement)**，相当于一个交叉点，程序需要在两条分支中选择一条，通用形式是：

```C
if (expression)
    statement
```

如果对 `expression` 求值为真，则执行 `statement`，否则跳过 `statement`。`statement` 可以是简单语句或复合语句。与 `while` 相比，`if` 的 `statement` 只会执行一次。即使 `if` 由复合语句组成，仍然被视为一条语句

<!--more-->
## 7.2 if else 语句

通用形式：

```C
if (expression)
    statement1
else
    statement2
```

如果 expression 为真，则 statement1，否则 statement2.

如果 statement1 或者 statement2 由很多条语句组成，那么必须要用花括号括起来，即复合语句（不同于 matlab，因为 C 语言没有 end）

```C
if (x >0)
    printf("Incrementing x:\n");
    x++;            //这里是不和语法的
else
    printf("x<=0\n");
```

![if 语句流程](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/19/20221119161647.png)

### 7.2.1 getchar( ) 和 putchar( )

getchar() 函数没有参数，用于读取输入队列中的下一个**字符**

```C
ch = getchar();
scanf("%C", &c);
```

上面的两个语句是等价的

putchar( ) 函数是打印它的参数

```C
putchar(ch);
printf("%c", ch);
```

上面的两个语句也是等价的

这两个函数只处理字符，所以相比而言更块更简洁，不需要转换说明，而且是被定义在 `stdio.h` 中

### 7.2.2 ctype.h 系列的字符函数

前面的7.2代码中不仅会将字母加一，也会处理非字母，所以就用到 `ctype.h` 了

`ctype.h` 中有很多函数，大多数是用来判断参数是否是某一类型，称之为**字符测试函数**

![字符测试函数](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/19/20221119184212.png)

还有一些称之为**字符映射函数**
![字符映射函数](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/19/20221119184237.png)

字符映射函数并不会改变其本身值（不像 x++ 这种）

```C
tolower(ch);
ch = tolower(ch);//前者不改变ch
```

### 7.2.3 多重选择 else if

```C
if (expression1)
    statement1
else if (expression2)
    statement2
else 
    statement3
```

本质上与下式等价

```C
if (expression1)
    statement1
else
    if (expression2)        // 整个 if else 被视为一条语句，不用花括号
        statement2
    else
        statement3
```

也就是相当于嵌套的分支语句可以简写成 else if 形式，两者完全等价

在`7.4.electric.c`中预处理器中定义了一个计算表达式，预处理器不进行计算，会在用到对应变量时原状替换，但是编译器会在替换之后进行计算

理论上可以无限嵌套，只要编译器支持，自从 C99 标准，编译器至少应该支持 127 层嵌套

### 7.2.4 else 和 if 配对

由于 C 语言中没有 end 所以else和哪个 if 配对是一个问题

C 语言标准是， else 与最近的 if 配对，除非其被花括号括起来，比如：

```C
if
{
    if

}
else
```

**注意**：C语言的编译器是忽略缩进的，但是缩进容易影响读者的判断，应当尽量符合标准

### 7.2.5 多层嵌套的 if 语句

从头写一个程序，给定一个整数，给出它的所有约数，没有则报告为素数，使用嵌套的 if 语句

伪代码：

```C
提示用户输入整数
如果输入整数（scanf）
    分析该数
    提示继续输入数字
```

用户输入整数的过程以及确定是否是整数的过程可以放在一起通过while循环进行`while(scanf("%d" ,&num))`

求整数的约数，最容易想到的方式是

```C
for (div = 2; div < num; div++)
    if(num % div == 0)
        printf("%d ", div);
```

继续思考一下，整数的约数时成对存在的，也就是说 `div` 的范围可以到 `num` 的平方根就结束（实际不用平方根，而用平方，因为整数运算速度快）

```C
for (div = 2; div*div <= num; div++)
    if(num % div ==0)
        printf("%d %d ", div, num / div);
```

还有一个问题，如果输入的整数是一个完全平方数，平方根会输出两次，所以嵌套一个 if 语句用来判断

```C
for (div = 2; div*div <= num; div++)
{
    if(num % div ==0)
    {                               //花括号为了易读性，也防止忘加其他花括号
        if(num / div != div)        //这里用 div *div = num  怎么样，是不是更好
            printf("%d %d ", div, num / div);
        else
            printf("%d ", div);
    }
}
```

还有一个问题，要求中还有一条是当函数是素数时，需要向用户返回结果，但是上面的代码不能做到这种功能

很明显，这可以通过查看进入 if 分支的次数（素数不会进入 if 分支）

可以通过定义一个 int(_Bool)类型的值，在循环开始时，给它一个值 （1），在 if 分支中赋值为 0，这样如果为素数，该值便始终为 1，非素数该值会变为 0.

有 `stdbool.h` 头文件可以使用 bool 代替 _Bool

相同的逻辑，自己写了一个列出所有素数的程序

```C
/*输出小于一个值的所有素数*/
#include<stdio.h>
#include<string.h>
#include<stdbool.h>
int main(void)
{
    int end;
    int j = 0;
    bool isPrime;
    int array[100000];         //定义一个数组，用来存储素数

    printf("Enter the upper(q to quit):");
    while (scanf("%d", &end))
    {
        array[0] = 2;           //第一个素数是 2
        if (end <= 4)
            printf("自己想办法，输入大于4的数");
        else
        {
            for (int order = 3; order <= end; order++)      // 数字从 3 开始，增加到指定的
            {
                isPrime = true;         //每个量初始化

                for(int i = 0; i <= j; i++)     
                {
                    if (order % array[i] == 0)
                    {
                        isPrime = false;
                    }
                }

                if (isPrime == true)
                {
                    j++;                    //素数加一
                    array[j] = order;
                }
            }
            for(int k = 0; k <= j; k++)
            {
                printf("%d ", array[k]);        //依次打印素数
                if ((k+1) % 10 == 0)
                {
                    printf("\n");               //每十个数字换行
                }
            }
            printf("\nEnter the upper(q to quit):");       //换行
        }
    }
}
```

## 7.3 逻辑运算符

|逻辑运算符|含义 |      举例     | iso646.h  |
|:-------:|:---:|:------------:|:---------:|
|&&       |与   | 5>2 && 5>3   |   and     |
|\|\|     |或   | 5>2 \|\| 4>7 |    or     |
| ！      |非   | !(4>7)       |    not    |

`!`的含义可以认为是 `!(true) = false` `!(false) = true` 这种的

在一些键盘的布局中，没有 & 这种的符号，所以 C 语言给了一个替代的方案，在 `iso646.h` 中，将 `&&` 别名为 and，`||` 别名为 or，`！`别名为 not。

### 7.3.2 优先级

`!` 的优先级与递增运算符号相同，仅低于圆括号。`&&` 优先级高于 `||` 但是两者都比关系运算符 `> <` 低，但是高于赋值运算符

比如 `a > b && b > c || b > d` 等价于 `((a > b) && (b > c)) || (b > d)` 当然，在实际运用中，更常用的是带有括号的一种，可以更明显的表现优先级

### 7.3.3 求值顺序

对于复杂的表达式 `anser = (3 + 4) * (6 + 7)`，C语言没有规定先计算前面的加法还是后面的加法，将选择权交给了编译器设置者。但是逻辑运算符并不是这样，逻辑运算符的表达式求值顺序是从左到右。同时 `&&`和`||` 都是序列点，也就是在进行右侧表达式判断之前，左侧表达式的副作用也已经生效了

这是为了确保 `(c = getchar()) != ' ' && c != '\n'` 这种的输入能够成立。同时也会有 `while( x++ <10 && x + y < 20)` 后一个表达式中的 x 是递增过的

对于 `&&` 由于测试是从左到右的，因此 C 语言规定了，如果左侧的表达式是 `false` 就不会去计算右侧的表达式了，也就是说 `x++ > 10 && y++ > 20` 这种，如果前面表达式不成立，后面 y 不会递增

### 7.3.4 范围

`&&` 运算符可用于测试范围，比如 `range >= 90 && range <= 100` 不能写成 90 <= range <= 100 这种，因为这等价于 `(90 <= range) <= 100` 也就相当于 `0 <= 100` 或者 `1 <= 100`，同时这并不是语法错误，所以编译器可能不提示

对于测试一个字符是不是小写字母，理论上可以用 `ch >= 'a' && ch <= 'z'`，但是这种计算只用于 ASCII 字符编码，对于其他编码不一定合适。所以最佳的方法是使用 `ctype.h` 中的 `islower` 函数

## 7.4 设计一个统计单词数的函数

需求：统计单词的数量（读取并报告单词的数量），同时计算字母数和行数

问题（思路）：

1. 结束的标志

   因为需要将换行符用于处理有几行，所以不能直接用换行符标志输入的结束，所以可以使用不常用的 `|` 标志输入结束，并将其定义为明示常量 `STOP`

2. 识别单词

    单词定义为一个不含空白的（空格、制表符、换行符）字符序列，从非空白开始，到空白结束

    使用 `c != ' '  && c != '\n' && c != '\t'` 来判断单词是否结束，其实也能用 `ctype.h` 中的 `isspace` 函数更简单

3. 避免重复计数单词

    可以使用一个 bool 类型的量用来标记单词，比如 `inword` 当读取到第一个非字符单词时，让单词数加一，并将 `inword = 1`。如果 `inword == 0`，读取到非空白时单词数加一，如果 `inword == 1`，则不加一

4. 布尔变量的使用

    当使用 `bool` 类型的变量时，在判断时，可以用 `if(inword)` 或者 `if(!inword)` 代替 `if (inword == 1)`，其中前者更常用

## 7.5 条件运算符(?:)

**条件表达式** ***(conditional expression)*** 使用**条件运算符** `? :`

该运算符需要 3 个运算对象，通过`?` `:`分割开，称为三元运算符

通用陈述

```C
expression1 ? expression2 : expression3
```

意为，如果 `expression1` 为 `true`，则该表达式等价于 `expression2`，否则该表达式等价于 `expression3`

条件运算符的优先级高于**赋值运算符**

举例

```C
max = (a > b) ? a : b
```

就是将 a、b 中的更大值赋值给 max

这种和 if else 是等价的，但是会更简洁更紧凑

## 7.6 循环辅助：continue 和 break

continue 和 break 语句可以根据测试结果，忽略一部分循环，甚至直接结束循环

### 7.6.1 continue 语句

三种循环都可以使用 `continue` 语句，执行该语句会跳过本次循环剩余部分，开始下一次循环测试，仅跳过循环体。当存在嵌套循环时，仅跳过一层循环

在 `7.9.skippart.c` 中的求一系列数据的最大最小值时，其赋予最大值用来比较的初始是 下限，用来比较的最小值是 上限。这是很好的处理技巧

continue 存在的原因是为了不要让 if 或者 else 下面的 statement 太长，长段的缩进在语句较长和嵌套较多时，会影响可读性

continue也可以用于占位，比如一个循环读取输入，但是什么都不做，仅当换行时退出循环。这种的 while 循环如果使用空语句会难以发现，所以用 continue； 就比较方便

**注意**：如果使用continue没有简化代码就不要用

**注意**：在 for 循环中，continue之后会执行第三条语句然后判断表达式。而相等价的 while 语句，会把递增语句放到循环体中被跳过

**注意**：`while (getchar() != '\n')  continue;` 这种代码可以让队列中非换行符全部被消耗，相当于空语句

### 7.6.2 break 语句

break 与 continue 的区别在于，continue 是跳过本次循环，break 是结束整个循环。比如在 `7.9.skippart.c` 中，如果把 continue 替换成 break 会当输入大于 100 的值时，相当于输入了 q

![break和continue](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/20/20221120164646.png)

break 还能用于其他情况退出的循环，比如计算面积时，输入值为非数字

**注意**：如果使用break会更复杂就不要用，尽量将判断条件集中在判断式上

在 if 循环中，由于 break 是直接结束循环，也不会再次执行第三个语句（一般是递增语句）。对于嵌套循环，如果想要退出两个循环，一般需要两次判断之后分别 break（连着两个 break 后面的会被跳过）

break 还能用于 switch 中，continue 不能用于

## 7.7 多重选择：switch 和 break

if else 很容易编写二选一的程序，但是很多程序不只是二选一，这个时候，switch 语句会更方便，而非多层 if else if else if，这样很乱

用法举例：

```C
/* animals.c -- uses a switch statement */
#include <stdio.h>
#include <ctype.h>
int main(void)
{
    char ch;
    
    printf("Give me a letter of the alphabet, and I will give ");
    printf("an animal name\nbeginning with that letter.\n");
    printf("Please type in a letter; type # to end my act.\n");
    while ((ch = getchar()) != '#')
    {
        if('\n' == ch)          //反这些防止少些一个等于号
            continue;
        if (islower(ch))     /* lowercase only          */
            switch (ch)
        {
            case 'a' :
                printf("argali, a wild sheep of Asia\n");
                break;
            case 'b' :
                printf("babirusa, a wild pig of Malay\n");
                break;
            case 'c' :
                printf("coati, racoonlike mammal\n");
                break;
            case 'd' :
                printf("desman, aquatic, molelike critter\n");
                break;
            case 'e' :
                printf("echidna, the spiny anteater\n");
                break;
            case 'f' :
                printf("fisher, brownish marten\n");
                break;
            default :
                printf("That's a stumper!\n");
        }                /* end of switch           */
        else
            printf("I recognize only lowercase letters.\n");
        while (getchar() != '\n')
            continue;      /* skip rest of input line */
        printf("Please type another letter or a #.\n");
    }                        /* while loop end          */
    printf("Bye!\n");
    
    return 0;
}
```

### 7.7.1 switch 语句

switch 语句对 switch 后面的圆括号中的表达式求值。然后程序会扫描标签（`case 'a' :`、`case 'b' :`）这种都称之为标签。switch 查找的标签是跟在 switch 后面的花括号中的标签，且对应的是 case 后面的值与 switch 圆括号中的表达式的值相同时

switch的本质（通用用法）：

```C
switch( 整形表达式 )
{
    case 常量1: 语句
    case 常量2：语句

    default ： 语句
}
```

其中的 case 常量加冒号(:)以及 default： 称之为标签。

switch 语句的实际运用就是对括号中的表达式求值（一般就是整数），然后跳转到对应的**标签**处。如果没有符合的标签，就跳转到 default，这些都是可选的。没有default会直接跳过

一般来说，在对应标签的语句下面都有 break 语句，用来跳出下面对应其他标签对应的语句

#### 有没有break的区别

![switch中有break和没有break的程序流](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/21/20221121110212.png)

其实如果 switch 语句在循环中，continue 也会跳过其他部分，进入下一次循环，达到类似 continue 的作用

- C语言的 case 一般只能指定一个值，不能指定范围。

- switch 在圆括号中的表达式的值应该是一个整数值（含 char）。case 标签必须是整数类型的常量或是常量表达式（表达式中只含有整型常量，不含有变量），不能使用变量（即便是等于常量的变量）。

- 同一个 switch 中标签不能重复（浮点数精度不容易控制，所以没有）

### 7.7.2 只读每行的首字符

通过下列的代码能够实现一行只读取首字母

```C
while (getchar() != '\n')
    continue;
```

这个代码的作用是循环读取字符并丢弃，直到换行（终端中确定），回到主程序

### 7.7.3 多重标签

```C
case 'e' :case 'E' :  e_ct++;
    break;
case 'a' :
case 'A' :  a_ct++;
    break;
```

在一个语句之前可以添加多个标签，也不用换行或空格（最好有）。其本质是，当 switch 识别到标签时就会跳转到对应语句。而前面的标签没有 break 就会执行下一个语句

- 在一些时候 default 会只有break语句，最后一个 case 理论上就不用 break 了。但是，为了以后的拓展性还是加上为好

- 当然上面如果使用 `ctype.h` 中的 `toupper` 就可以避免多重标签，将小写字母转换成大写字母

### 7.7.4 switch 与 if else

如果根据浮点数的判断，或者是某个范围，只能使用 if else。而对于具有多个不同选择的选择，switch 效率更高

## 7.8 goto 语句

goto 语句的模式是

```C
goto 标签名；
···
···
标签名: statement
```

goto 后面应该跟着标签名，同时应该有一个有着对应标签的语句。在运行 goto 之后，跳转到标签标记的语句

标签名的命名规则与变量名相同。以字母或下划线开头，包含数字、字母、下划线

goto 语句易被滥用，所以应该谨慎使用最好不用

### 7.8.1 避免使用 goto

C语言中完全不必要使用 goto，有些人有 basic 或 fortran 经验，会经常使用 goto。但是，使用 goto 会让程序杂乱无章，应该尽量避免。常用替换方案：

- 处理包含多条语句的if语句

```C
if (size > 12)
    goto a;
goto b;
a: cost = cost * 1.05;
flag = 2;
b: bill = cost * flag;
```

这是因为在 basic 和 fortran 中只有紧跟 if 的语句才属于 if

```C
if (size > 12)
{
    cost = cost * 1.05;
    flag = 2;
}
bill = cost * flag;
```

- 二选一

```C
if (ibex > 14)
    goto a;
sheds = 2;
goto b;
a: sheds = 3;
b: help = 2 * sheds;
```

新版 basic 和 fortran 已经加入了 else

```C
if (ibox > 14)
    sheds = 2;
else
    sheds = 3;
help = 2 * sheds;
```

- 创建不确定循环

```C
readin: scanf("%d", &score);
if (score < 0)
    goto stage2;
lots of statement
goto readin;
stage2: more stuff;
```

C语言可以用 while 函数替代

- 跳至循环末尾，开启下一次迭代（continue）

- 跳出循环（break）

break 和 continue 是 goto 的特殊形式。同时不用使用标签，可以防止标签误用

- 随便跳转

不要这样用，会破坏程序逻辑和结构

当然，goto 有其存在的必要性，在嵌套循环中，break 只能跳出一层循环，而 goto 可以跳转到其他循环，用起来更加方便

其实 C 语言的 goto 函数比其他语言的 goto 更方便，但是还是，不熟悉不要用
