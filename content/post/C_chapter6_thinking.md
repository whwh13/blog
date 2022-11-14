---
title: "C语言第六章思考题"
date: 2022-11-14T15:26:07+08:00
lastmod: 2022-11-14T15:26:07+08:00
draft: false
keywords: []
description: "C语言第六章思考题"
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

<!--more-->
1. quack的值

    2；7；70；64；8；2

2. 循环输出

    int类型： 36 18  9  4  2  1
    double：不会停止，无限循环，<font color="#dd0000">此时转换说明也不对</font>

3. 测试条件

    a. x>5

    b. scanf("%ld",&x) != 1

    c. x == 5

4. 测试条件

    a. scanf("%d",&int)

    b. x != 5

    c. x >= 20

5. 问题

    定义数组应该用中括号；无返回值；中间循环，应该用`j = 10; ;j--`原来用法中，`i = 2` 时，不能进入循环。<font color="#dd0000">分号写成了逗号</font>

6. 程序

    ```C
    #include<stdio.h>
    int main(void)
    {
        int row, column;

        for (row = 1; row<=4; row++)
        {
            for (column = 1;column <= 8; column++)
            {
                printf("$");
            }
            printf("\n");
        }

        return 0;
    }
    ```

7. 打印内容

    a. Hi!  Hi! Hi! Bye!  Bye!  Bye!  Bye!  <font color="#dd0000">Bye! 这里是用的 i++</font>

    b. ACGM

8. 输出<font color="#dd0000">这里问题都出在 %c 是能读取空格的</font>

    a. ~~Gowest,youn~~ Go west, youn

    b. ~~Hpxftuz-pvo~~ Hp!xftu-!zpvo

    c. ~~Gowest,young~~ Go west, young

    d. ~~$owest,youn~~ $o west, youn

9. 打印

    ```C
    31|32|33|30|31|32|33|
    ***
    1
    5
    9
    13

    ***
    2 6
    4 8
    8 10

    ***
    ======
    =====
    ====
    ===
    ==
    ```

10. 声明

    a. mint
    b. 最多10个
    c. double
    d. ii

11. 错误

    第一个循环，`by_twos[index-1]` 其实更改整个循环更好，但这样简单
    第二个循环，没有加中括号

12. 函数定义

    返回值类型(long)、函数名、函数参数、函数体

13. 函数

    ```C
    long int_long(int n)
    {
        long n2;

        n2 = (long)n *(long)n;

        return n2;
    }
    ```

    <font color="#dd0000">下面是答案中的简单的</font>

    ```C
    long square(int num)
    {
        return ((long) num) * num;
    }

14. 打印

    ```C
    1: Hi!
    k = 1
    k is 1 in the loop
    Now k is 3
    k = 3
    k is 3 in the loop
    Now k is 5
    k = 5
    k is 5 in the loop
    Now k is 7
    k = 7       //这里是答案后补的，没考虑到测试的时候也会打印值
    ```
