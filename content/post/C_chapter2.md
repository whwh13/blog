---
title: "C_chapter2"
date: 2022-10-17T19:50:12+08:00
lastmod: 2022-10-17T19:50:12+08:00
draft: false
keywords: []
description: "C语言学习，第二章笔记"
tags: ["C语言","学习","笔记"]
categories: ["C语言"]
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

转义字符以 " \ " 开始," \t "表示Tab，" \b "表示Backspace，" \n "表示换行
"%d"名为占位符号，用于指明变量的位置

## 提升可读性

1. 有意义的函数名
2. 写注释（当函数名可以明显的表示意义时，可以（应当）不写注释）
3. 用空行分隔多个部分，比如声明部分与其他部分
4. 每个语句独占一行（并非C语言的要求，事实上换行在C语言中与空格类似，分号代表语句的结束）

## 多条声明可以使用不同的形式，但是两者具有相同的意义

``` C
int feet, fathoms;
与
int feet;
int fathoms;
```

printf()命令待打印的值不一定是变量，可以是能够得出适合前面描写的占位符的任意项

``` C
printf("Hello, please print %d !\n",6*fathoms);
```

## 调试程序

1. 语法错误
    转义字符仅限制后面的一位字符，即

    ```C
    printf("hello world! \nnihao");
    ```

    输出为

    ```C
    hello world! 
    nihao
    ```

2. 语义错误
