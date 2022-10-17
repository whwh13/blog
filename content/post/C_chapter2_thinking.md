---
title: "C语言第二章思考题"
date: 2022-10-17T20:01:04+08:00
lastmod: 2022-10-17T20:01:04+08:00
draft: false
keywords: []
description: "C语言第二章思考题"
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
## 1.对于编程而言，可移植性代表着什么

可移植性代表着能够仅做出很小的修改就可以使源程序在不同的系统中运行。

<!--more-->

## 2.解释源代码文件、目标代码文件和可执行文件有什么区别

源代码文件是纯文本文件，是使用C语言写作的高级语言。目标文件是由编译器编译的机器代码语言，但是仍然不能独立运行。可执行文件是将目标代码文件与C语言库合并而来，可独立运行。

## 3.编程的7个主要步骤是

确认设计目标 $\rightarrow$ 设计实现路线 $\rightarrow$ 编写代码 $\rightarrow$ 编译 $\rightarrow$ 运行 $\rightarrow$ 调试改进 $\rightarrow$ 维护

## 4.编译器的主要任务

将C语言代码编译为机器可以理解的机器代码语言。

## 5.链接器的任务

将编译器输出的目标代码文件与C语言库合并，形成可执行文件。
