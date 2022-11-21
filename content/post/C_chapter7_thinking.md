---
title: "C语言第七章思考题"
date: 2022-11-21T21:51:48+08:00
lastmod: 2022-11-21T21:51:48+08:00
draft: false
keywords: []
description: "C语言第七章思考题"
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
1. 判断

    1. false
    2. true
    3. false

2. 表达式

   1. number >= 90 && number < 100
   2. char != 'q' && char != 'k'
   3. number >= 1 && number <= 9 && number != 5
   4. !(number >= 1 && number<= 9)

3. 简化

    没读懂源代码，看答案吧

4. 表达式的值(这里的 true 和 false 是 1 和 0 的意思)

    1. true
    2. false
    3. true
    4. 6
    5. 10
    6. false

5. 打印

    *#%*#%￥#%*#%*#%&#%*#%*#%￥#%*#%*#%

6. 打印

    ```sh
    fat hat cat Oh no!
    hat cat Oh no!
    cat Oh no!
    ```

7. 错误

    1. 没有给用户输入提示
    2. 第一个 if 改成 `'a' <= ch && 'z' >= ch`
    3. else if 处 `||` 改成 `&&，后面补个括号
    4. oc++ 上面加一个 else
    5. printif 引号
    6. <font color="#dd0000">注释不完全</font>

8. 打印

    ~~You are 40. Here is a raise.~~

    ~~You are 60. Here is a raise.~~

    ~~You are 65. Here is your gold watch.~~

    <font color="#dd0000">只打印`You are 65. Here is your gole watch.` 而且会不断重复，因为 if 使用了 `=` 导致赋值了</font>

9. 打印

    ```C
    Step 1
    Step 2
    Step 3
    Step 1
    Step 1
    Step 3
    Step 1
    Done
    ```

10. 重写

    思路：

    获取一个输入，当输入为 # 时结束循环。当输入为 \n 时直接获取下一个输入

    执行 Step 1

    当输入为 'c'，直接下一个循环

    当输入为 'b'，结束循环

    当输入为 'h'，执行 Step 3

    均不是时，执行 Step 2, Step 3

    ```C
    #include<stdio.h>
    int main(void)
    {
        char ch;

        while ((ch = getchar()) != '#')
        {
            switch(ch)
            {
                case '\n': break;
                case 'c':
                case 'b':
                    printf("Step 1\n");
                    break;
                case 'h':
                    printf("Step 1\nStep 3\n");
                    break;
                default:
                    printf("Step 1\nStep 2\nStep 3\n");
                    break;
            }
            if (ch = 'b')
                break;
        }
    }
    ```

    <font color="#dd0000">答案中的思路</font>

    ```C
    #include<stdio.h>
    int main(void)
    {
        char ch;

        while ((ch = getchar()) != '#')
        {
            if ( ch != '\n');
            {
                printf("Step1\n");
                if (ch = 'b')
                    break;
                else if (ch != 'c')
                {
                    if (ch != 'h')
                        printf("Step 2\n");
                    printf("Step 3\n");
                }
            }
        }
        printf("Done\n");
    }
    ```
