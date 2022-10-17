---
title: "C语言第四章思考题"
date: 2022-10-17T20:00:31+08:00
lastmod: 2022-10-17T20:00:31+08:00
draft: false
keywords: []
description: "C语言第四章思考题"
tags: ["C语言","学习","思考题"]
categories: ["C思考题"]
author: "whwh13"

# Uncomment to pin article to front page
# weight: 1
# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: false
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
##### 1. 再次运行程序清单 4.1，但是在要求输入名时，请输入名和姓（根据英文书写习惯，名和姓中间有一个空格），看看会发生什么情况？为什么？

>名字没有问题，但是数字类似随机数。因为在遇到第一个空格时，scanf 认为已经读取完毕。空格之后的被后面的 weight 所识别并使用。
<!--more-->
##### 2. 假设下列示例都是完整程序中的一部分，它们打印的结果分别是什么？

```C
a.printf("He sold the painting for $%2.2f.\n", 2.345e2);
b.printf("%c%c%c\n", 'H', 105, '\41');
c.#define Q "His Hamlet was funny without being vulgar."printf("%s\nhas %d characters.\n", Q, strlen(Q));
d.printf("Is %2.2e the same as %2.2f?\n", 1201.0, 1201.0);
```

>a. He sold the painting for $234.50.
>
>b. Hi!
>
>c. His Hamlet was funny without being vulgar.
>
>has 42 characters.
>
>d. Is 1.20e3 the same as 1201.00 ?

##### 3. 在第2题的c中，要输出包含双引号的字符串Q，应如何修改？

>使用转义字符 \

##### 4. 找出下面程序中的错误

```C
define B booboo
define X 10
main(int)
{
    int age;
    char name;
    printf("Please enter your first name.");
    scanf("%s", name);
    printf("All right, %c, what's your age?\n",name);
    scanf("%f", age);
    xp = age + X;
    printf("That's a %s! You must be at least %d.\n", B, xp);
    rerun 0;
}
```

>第一行，井号，字符串加双引号
>第二行，井号
>第三行，int main（void）
>未声明 xp 变量
>第六行，name设定初始字符串长度
>第七行，使用 %s 转换说明
>第八行，使用 %d 转换说明，age 前加 &
>未添加必要的头文件

##### 5. 假设一个程序的开头是这样

```C
#define BOOK "War and Peace"
int main(void)
{
float cost =12.99;
float percent = 80.0;
```

请构造一个使用BOOK、cost和percent的printf()语句，打印以下内容：
This copy of "War and Peace" sells for $12.99.
That is 80% of list.

`printf("This copy %s sells for $%2.2f.\nThat is %0.0f%% of list.",BOOK,cost,percent)`

##### 6. 打印下列各项内容要分别使用什么转换说明？

a.一个字段宽度与位数相同的十进制整数
b.一个形如8A、字段宽度为4的十六进制整数
c.一个形如232.346、字段宽度为10的浮点数
d.一个形如2.33e+002、字段宽度为12的浮点数
e.一个字段宽度为30、左对齐的字符串

>a. %d
b. %4X
c. %10.3f
d. %12.2e
e. %-30s

##### 7. 打印下面各项内容要分别使用什么转换说明？

a.字段宽度为15的unsigned long类型的整数
b.一个形如0x8a、字段宽度为4的十六进制整数
c.一个形如2.33E+02、字段宽度为12、左对齐的浮点数
d.一个形如+232.346、字段宽度为10的浮点数
e.一个字段宽度为8的字符串的前8个字符

>a. %15lu
b. %#4x
c. %-12.2E
d. %+10.3f
e. %8.8s

##### 8. 打印下面各项内容要分别使用什么转换说明？

a.一个字段宽度为6、最少有4位数字的十进制整数
b.一个在参数列表中给定字段宽度的八进制整数
c.一个字段宽度为2的字符
d.一个形如+3.13、字段宽度等于数字中字符数的浮点数
e.一个字段宽度为7、左对齐字符串中的前5个字符

>a. %6.4d
b. %*o
c. %2c
d. %+0.2f
e. %-7.5s

##### 9. 分别写出读取下列各输入行的scanf()语句，并声明语句中用到变量和数组

a.101                           %d
b.22.32 8.34E−09                %f %E (空格不影响)
c.linguini                      %s
d.catch 22                      %s %d
e.catch 22 （但是跳过catch)      %*s %d

##### 10. 什么是空白

##### 11. 下面的语句有什么问题？如何修正

```C
printf("The double type is %z bytes..\n", sizeof(double));
```

%zd(z是修饰符，zd 也行)

##### 12. 假设要在程序中用圆括号代替花括号，以下方法是否可行

```C
#define ( {
#define ) }
```

>不行，因为正常的圆括号不能用了
