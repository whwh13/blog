---
title: "C语言第五章思考题"
date: 2022-11-07T16:38:06+08:00
lastmod: 2022-11-07T16:38:06+08:00
draft: true
keywords: []
description: "C语言第五章思考题"
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

1.
    a.30;b.27;c.1;d.9
2.
    a.x=6;

    b.x=52;

    c.x=0;

    d.x=13;

3.
    a.~~36.25~~;<font color="#dd0000">37.5（数都能算错来）</font>

    b.1.5;

    c.35;

    d.37

    e.~~36.25~~;<font color="#dd0000">37.5（数都能算错来）</font>

    f.35.0;
4.
    头文件、1/i值是int类型，始终为0、while复合语句花括号、i++、返回值

    <font color="#dd0000">int一行结尾不是分号，打印值中没有换行</font>
5.
    初始的sec值可能是不符合要求，直接不进入循环
    输入值为0或负数时不能直接退出，会报错

    <font color="#dd0000">处理类似问题的常用手段就是两次 scanf ，视为以后的解决方案</font>
6.
    无法重复套娃字符串，被转换说明所指代的字符串中存在转换说明不会套娃

    %s! C is cool!

    ! C is cool!

    11

    11

    12

    11
7.
    SOS:4 4.00
8.
    打印结果

    ```C
    1    2    3    4    5    6    7    8    9   10
    ```

9.
    见 t9.c 文件
10.
    a.    1    2

    b. 101

       101

       102

       102

    c.stuvw
11.
    ~~COMPUTER BYTES DOG
    That's all.~~

    由于没有括起来，这个程序不会停止
12.
    a.x=x+10

    b.x++

    c.c=(a+b)*2

    d.c=2*a+2*b

13.
    a.x--

    b.m=n%k

    c.q/(b-a)

    d.x=(a+b)/(c*d)
