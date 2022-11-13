---
title: "C语言第六章 C 控制语句：循环"
date: 2022-11-13T22:07:46+08:00
lastmod: 2022-11-13T22:07:46+08:00
draft: false
keywords: []
description: "C语言学习，第六章笔记"
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
## 6.1 再探 while 循环

### 6.1.1 程序6.1注释

对 while 循环，判断条件是表达式 `status == 1`, `==`是C的相等运算符(equality operator)，用来判断 status 是否等于 1。等于 1 为 true(1),否则为 false(0)

同时 `status = scanf("%d", &num)` 则是利用了 scanf 函数的两个作用，其一是为 num 赋值，其二是 scanf 的返回值是其成功读取的**项数**

伪代码(pseudocode):通过简单的语句表示程序的思路，在形式上与计算机语言对应，但是不考虑具体的格式，仅体现程序设计的逻辑。确认逻辑没有问题，再翻译成实际的编程语言
<!--more-->

举例：

```paseudocode
初始化 sum 为 0
提示用户输入数据
读取用户输入的数据
当输入数据为整数时，
    输入赋值给 sum
    提示用户输入
    读取下一个输入
输入结束，打印 sum 值
```

对于while循环，可以写成伪代码形式为

```paseudocode
获得第一个用于判断的值
如果判断为真
    处理
    获取下一个值
```

### 6.1.2 C风格读取循环

前面的

```C
status = scanf("%ld",&num);
while (status == 1)
{
    /* 循环 */
    status = scanf("%ld", &num);
}
```

可以替换为

```C
while (scanf("%ld", &num) == 1)
{
    /* 循环 */
}
```

这里由于每次都要获取值，同时 scanf 每次只要用到就会获取并有个返回值，所以可以这样写起来更精简

下面正式学习 while 语句

## 6.2 while 语句

while 语句的通用表达形式是：

```C
while (expression)
    ststement
```

其中的 statement 可以是用分号结尾的简单语句，也可以是用花括号合并起来的复合语句。

目前为止，expression 都是用的关系表达式，如果关系为真（更一般的是非零），就执行一次 statement，之后再次判断知道 expression 为假（0）。期间的每一次循环称之为一次迭代

### 6.2.1 中止 while 循环

while 循环有一个很重要的注意事项，那便是 expression 必须能够有为假（0）的变化可能
例如

```C
index = 1;
while(--index < 5)
    printf("Good morning!\n");
```

这里尽管判断式发生了改变，但是在溢出之前仍然不可能中止循环，所以也会几乎无限制的打印 `Good morning!`

### 6.2.2 何时中止循环

很容易理解但是很重要的一个问题：中止循环是在结束一次迭代之后，在对判断式求值时决定的。不会因为在执行 statement 过程中某个值发生了改变而直接停止

### 6.2.3 while:入口条件循环

while 是一个使用入口条件的有条件循环。有条件指的是需要满足一定条件才能进入循环，入口条件（entry condition）值的是必须满足条件才能进入循环体（后面和 do while 对比）

### 6.2.4 语法要点

只有跟在测试条件（判断语句）后面的单独语句（简单或复合）才是循环执行的部分
缩进并非程序所要求的，而是为了易读认为控制的，所以不能用缩进来判断是不是循环体。

即使 while 语句使用复合语句，但是在语句构成上，它仍然是一条单独的语句。从 while 开始执行，到第一个分号（或者右花括号）结束

```C
while(n++ >2);
printf("%d", n );
```

上例中 while 语句到第一个分号就已经结束了，因此循环部分是空语句（*null statement*）

空语句的利用：

```C
while(scanf("%d", &n) == 1)
    ; /* 跳过整数输出 */
```

使用上例程序，可以持续获取输入，直到获得一个非整数的输入。**注意**：为了易读性考虑，这里的分号最好还是独占一行。提醒这里的空语句是有意为之而且更容易看到

## 6.3 用关系运算符和表达式比较大小

while 循环经常依赖表达式做比较，这样的表达式称为关系表达式(relational expression)，在其中间的运算符叫做关系运算符(relation oprator)
C语言所有的关系运算符列在下面

|运算符|含义|
|---|---|
|<  |小于|
|<= |小于或等于|
|== |等于|
|>= |大于或等于|
|>  |大于|
|!= |不等于|

关系运算符也可以用来比较字符，int型的变量也能拿来和字符比较，通过ASCII码

```C
#include<stdio.h>
int main(void)
{
    int ch = 97;
    while (ch++ != 'g')
        printf("%c", ch);
    return 0;
}
```

不能通过关系运算符比较字符串

在比较浮点数时，由于浮点数的舍入误差，以及二进制存储中一些有限的十进制小数，比如 0.1，在二进制中是无限的。所以应当尽量只比较 < 或 >。

同时在 math.h 中有 `fabs()` 函数，该函数返回浮点数的绝对值，是经常能用到的函数

### 6.3.1 什么是真

C 语言中将表达式为真，会给表达式赋值为 1，为假会赋值为 0

可以通过这种机制写出无限循环的程序

```c
while(1)
{
    ...
}
```

### 6.3.2 其他真值

通过程序 6.7.truth.c 结果可以看到，在**关系表达式**中 C语言中将所有非零值视为真，只有 0 视为假。

正因为如此，很多程序员会将 `while(n != 0)`写成`while(n)`。后者是更常用的表达

### 6.3.3 真值的问题

C语言对于真值的规定过于宽泛了，也容易导致一些其他的问题。比如 `6.8.trouble.c` 修改自 `6.1.summing`，仅仅把`status == 1`换成了`status = 1`，这就将**关系表达式**错误的写成了**赋值表达式**

前面有提到，赋值**表达式的值**就是右值。所以这里的 `while(status = 1)` 等价于 `while(1)` 所以输入 q 不仅不会退出，反而因为 scanf 的特性，不断打印输入数字的提示

这是因为如果 scanf 读取失败，会把无法读取的数留在队列中等下次读取，而循环中下次仍然只读取整数，所以还会失败。

**注意**：这种类型的错误编译器一般不会给提示，不过有经验的程序员在需要比较的值是常量的时候会将其写在左边。即采用`5 == n` 而不是 `n == 5` 这是因为如果写错成 `5 = n` 编译器会报错，因为 5 不是一个左值

### _BOOL类型

在C语言中，一直用的 int 类型的变量表示真或者假。直到 C99 专门新增了 _BOOL 类型。用来专门表示布尔变量(Boolean variable)(专门用来表示真假的变量)。_Bool 类型只能存储 0 或者 1，如果给其赋值其他非零值，会被设置为 1。

在写程序的时候，例如在`6.9.boolean.c`中，定义了一个布尔变量叫`input_is_good`这是常用做法，指给布尔变量命名时使用能表示真假的名字

C99提供了 stdbool.h 该头文件将 bool 作为 _Bool 的别名，同时将 true 和 false 定义成了符号常量 1 和 0。这是兼容C++的，因为 C++ 中，`bool,true,false`都是关键字

### 优先级和关系运算符

关系运算符的优先级小于算数运算符(+ -)，但是高于赋值运算符，因此`xtest = x > y`等价于 `xtest = (x > y)`
`x>y+2`等价于`x>(y+2)`

关系运算符之间，(>、>=、<、<=)四者具有相同优先级，且高于(!=、==)。同时也是有从左向右的运算规则，即`ex != wye ==zee`等价于`(ez != wye) == zee`

### 不确定循环和计数循环

**不确定循环**(indefinite loop), 指的是不确定结束循环前需要执行多少次的循环。**确定循环**(counting loop)，指循环之前事先规定循环次数的循环。

计数循环要素：

- 初始化计数器
- 计数器与有限值比较
- 每次循环递增计数器

使用 while 循环，初始化计数器需要在循环体之外，有可能发生忘记，而下面的 for 循环则避免了这种情况

## 6.5 for 循环

for 循环将初始化、判断、更新组合在一起。

for 后面圆括号中有 3 个通过两个分号分开的表达式。第 1 个是初始化表达式，只会在 for 循环开始时执行一次。第二个是判断表达式，判断为假，结束循环。第三个是表达式更新，每次循环结束之后求值。

同时这里的三个表达式称为**控制表达式**，都是完整的表达式，因此每个表达式的副作用（比如变量递增）都发生在单独表达式结束之后。

![for循环](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/10/20221110222001.png)

### 6.5.1 利用 for 的灵活性

1. 使用递减运算符递减计数器（用于显示倒计时等） `for(sec = 5; sec > 0; sec--)`

2. 计数器每次递增不止是 1 `for(n = 0; n > 100; n = n + 10)`

3. 字符代替数字计数 `for(ch = 'a'; ch <= 'z'; ch++)`

4. 判别式不一定要是迭代次数 `for(num = 1; num*num*num <=1000; num++)`

5. 可以让递增量以任何方式增长 `for(debt 100.0; debt < 150.0; debt = debt*1.1)`

6. 第三个表达式可以使用任意合法表达式 `for(x = 1; y <=75; y = (++x * 5) + 50)`

7. 表达式可以使用空表达式 `for (n = 3; ans = 25;)` 省略中间循环不会停止

8. 第一个表达式也不用必须给变量赋值（用于printf打印某个值）`for (printf("Start!");num != 6)`

9. 循环体中的行为可任意改变循环表达式 `for (n = 1; n< 10000; n = n + delta)` 如果步长太小，可以在循环中识别并改变

**总结**：自己决定 for 循环头中的三个表达式，实现各种创意的循环

>for循环通用形式
>
> ```C
>for ( initialize; test; update)
>   ststement
>```
>
>initialize 在循环开始前执行，只执行一次。然后对 test 求值，如果为真，则执行 statement。执行 statement 之后执行一次 update，然后继续执行 test

## 6.6 其他赋值运算符（+=、-=、*=、/=、%=）

可以通用的表示为 `*=` 的类型，右侧是等于号，左侧是一个运算符，总运算符左侧是左值，右侧是右值。相当于 **左值本身通过 右值经过运算符处理后 重新赋值给左值**

列出

```C
scores += 20          scores = scores + 20
time /= 2.73          time = time / 2.73
x *= 3*y + 12         x = x*(3*y + 12)
```

这些运算符优先级与等于号相同，低于 + 或 *。这些运算符让代码更加紧凑，编译出来也更加高效。当在 for 循环中使用复杂表达式会更方便且高效

## 6.7 逗号运算符

逗号运算符最常见在 for 循环中，但也可以用在别的地方。

- 逗号运算符规定被分割的表达式从左到右求值（副作用与求值同时）

- 含逗号的表达式的**值**是逗号右侧的表达式的值
例如`x = (y = 3, (z = ++y + 2) + 5);`，过程是先对 y 赋值 3，然后 ++ 之后加 2 赋值给 z 再加 5，即是其值，但是很蠢不要用

- 注意不要错用
例如`houseprice = 249,500;`表示`houseprice = 249;500;`。`displacement = (249,500);`赋值给 displacement 的是逗号右边的值 500

- 运算符有限级高于 =，小于 +

- 逗号也能用于分隔符，不一定表示逗号运算符

### 6.7.1 示例 每行半步

```C
#include<stdio.h>

int main(void)
{
    int t_ct;     //项计数
    double time, power_of_2;
    int limit;

    printf("Enter the number of terms you want: ");
    scanf("%d", &limit);
    for(time = 0, power_of_2 = 1, t_ct = 1; t_ct <= limit; t_ct++, power_of_2 *=2.0)
    {
        time += 1.0/power_of_2;
        printf("time = %f when terms = %d.\n", time, t_ct);
    }

    return 0;
}
```

## 出口条件循环： do while

while和 for 都是入口条件循环，迭代之前检查测试条件。而 do while 是出口条件循环（exit-condition loop）在每次迭代之后检查测试条件，能保证至少执行一次循环体的内容

使用 while 也能写出相应的程序，但是会长很多，需要在循环体之前在加一次 statement

```C
do
    statement
while ( expression );
```

这是通用形式，statement 可以是简单语句或者复合语句，while 后以分号结尾

![do_while](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/13/20221113162400.png)

同时在使用时应当避免

```C
do
{
    是否继续；
    执行；
}while(继续)
```

这种会导致用户不想继续的时候仍然会继续执行

## 6.9 如何选择循环

入口循环使用的更多：其一，原则上在执行循环之前测试条件更好；其二，测试条件放在开头更加易读；其三，很多程序会要求不满足的直接跳过循环

for 和 while 可以相互转化，省略第一和第三个表达式，for 循环就是 while 循环

while 循环之前加初始化，循环最后加参数更新，就是 for 循环

一般而言，当设计初始化以及更新变量，for 循环更合适，其他情况 while 更好

## 6.10 嵌套循环

嵌套循环就是在一个循环中加另一个循环，通常用来表示矩阵的行列，也就是有两个参数的函数

每次外层循环都会执行内层循环的全部次数，可以控制内层循环使用外层循环的变量，使内层循环每次做的事情不同

## 6.11 数组简介

数组相当于**向量**

声明数组的方法类似于声明字符串的方法**字符串的本质就是声明了一个 char 的数组，并将字符串带着 \0 存储在了数组中**

声明数组可以包含任意类型的元素：

```C
float debts[20];        //声明可以存储20个float值的数组
int nannies[22];        //22个int
char actors[26];        //26个char
long big[500];          //500个big
```

访问数组中的元素或者给数组中的元素赋值，使用中括号加数字，例如定义了 `float debts[20]`，则`debts[0]~debts[19]`都可以视为变量对其操作。**注意**：C语言数组元素从 0 开始

可以直接对其赋值 `debts[0]=112.1`，也可以通过scanf赋值 `scanf("%f", &debts[19])`，注意这里仍然有`&`符号

需要**注意**，C语言编译器并不会检查下标是否正确，所以当定义了20个元素的数组，给`debts[20]`进行赋值，会影响内存中的其他数据，导致程序被破坏

**字符串与数组**：字符串的本质就是存储在 char 数组中的元素，不过其必须以 \0 结尾，不以 \0 结尾不会被视为字符串（C语言自动处理）

下标(subscript)、索引(indice)、偏移量(offset)都是指中括号中的数字，只能为整数，只能以0开始计数

在内存中，数组中的元素相互相邻存储

### 6.11.1 在 for 循环中使用数组

```C
#include<stdio.h>
#define SIZE 10
#define PAR 72
int main(void)
{
    int index, score[SIZE];
    int sum = 0;
    float average;

    printf("Enter %d golf scores:\n", SIZE);
    for ( index = 0; index < SIZE; index++)
    {
        scanf("%d",&score[index]);      //分别读取10个分数
    }
    printf("The scores read in are as follows:\n");
    for(index = 0; index<SIZE; index++)
        printf("%5d", score[index]);
    printf("\n");
    for(index = 0; index < SIZE; index++)
        sum += score[index];
    average = (float) sum / SIZE;
    printf("Sum of scores = %d, average = %.2f\n", sum, average);
    printf("That's a handicap of %.0f.\n", average - PAR);

    return 0;
}
```

- 程序使用数组和循环处理数据，这样不用使用很多个 scanf 函数，也不用定义很多变量

- 使用 #define 创建的明示常量定义数组的大小，方便后需更改升级

- 使用类似`for(index = 0; index<SIZE; index++)`可以非常方便的处理数组值

- 用户输入之后程序重新显示了用户的输入，易于调试

## 6.12 使用函数返回值

```C
#include<stdio.h>
double power(double n, int p);//ANSI函数原型
int main(void)
{
    double x, xpow;
    int exp;

    printf("Enter a number and the positive integer power ");
    printf("to which\nthe number will be raised. Enter q to quit.\n");
    while (scanf("%lf%d", &x, &exp) == 2)
    {
        xpow = power(x, exp);   //函数调用
        printf("%.3g to the power %d is %.5g\n", x, exp, xpow);
        printf("Enter next pair of numbers or q to quit.\n");
    }
    printf("Hope you enjoyed this power trip -- bye!\n");

    return 0;
}

double power(double n, int p)           //定义函数
{
    double pow = 1;
    int i;

    for(i = 1; i < p + 1; i++)
        pow *= n;

    return pow;         //返回pow
}
```

该程序定义了一个能求数字的整数幂的函数，并使用其返回值

1. 分析算法：简化函数，只处理正整数次幂，只需要 n 和 n 相乘 p 次就可以。使用for循环，将初始值设定为 1，让其 *= n。循环则定义为当i < p 继续循环，P 就是已知的迭代次数

2. 为了扩大计算范围，n pow 都是 double，p 为幂次，自然是 int

3. 编写有返回值的函数

    - 定义函数时确定函数返回值类型（定义函数时，写在函数名前）
    - 使用 return 表明待返回的值（返回值也可以是表达式）

4. 继续分析程序

    - main 是一个驱动程序（driver），即被设计用来测试函数的小程序

    - 但是输入 q 会使 scanf 的返回值为 0，因为 q 与 scanf 中的转换说明 %1f 不匹配。scanf 将返回 0，循环结束。类似地，输入 2.8 q会使 scanf 的返回值为 1，循环也会结束
    - power 函数在程序中出现了3次

        - 函数原型`double power(double n, int p);`

        - 函数调用

        - 函数定义`double power(double n, int p)` **没有分号**

### 6.12.2 使用带返回值的函数

1. 为什么定义函数已经说明其返回值类型，还是需要单独声明

    编译器在首次遇到函数时，就要知道返回类型，所以需要函数原型，称为**前置声明（forward declaration）**。如果将函数定义在main前面，可以省略函数原型，但是不是C的风格。

    因为对于C语言来说，main 函数更多的是框架，函数通常放在别的文件中

2. 为什么有些函数`scanf`不用声明就能使用，也能有其返回值

    头文件中包含了相应的函数原型
