---
title: "Linux 文件权限学习"
date: 2022-11-09T11:26:08+08:00
lastmod: 2022-11-09T11:26:08+08:00
draft: false
keywords: []
description: "学习 chmod chown 以及Linux的权限相关问题"
tags: ["Linux技巧","权限"]
categories: ["Linux技巧"]
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
在 Linux 系统中，输入 `ll` 或者 `ls -a` 命令，会列出全部文件以及各个文件所对应的权限。

该处列出的权限由 10 个字符组成，第一位为 `d` 表示该项是一个文件，为 `-` 表示是文件夹

后面的 9 个字符，每 3 个为一组，分别表示**文件所有者(user)****用户组(group)**和**其他用户(other)**。每一组第一个字符是 r 或 -，用来表示对应用户是否有读权限；第二个字符是 w 或者 -，用来表示对应用户的写入权限；最后一个是执行权限，x 或者 -。
<!--more-->

每个权限不仅可以用 rwx 表示，还可以使用**一位八进制数**或者**三位二进制数**表示，在使用 二进制表示时，最左边的一位，表示读(r)权限，中间表示写(w)权限，最后表示执行(x)权限。表示权限时，用 1 表示具有该权限，0 表示无对应权限

![权限表](https://raw.githubusercontent.com/whwh13/picgo/master/2022/11/11/20221111094959.png)

由于三位二进制数表示一位八进制数，所以每个 拥有者、用户组、其他用户的权限也就能用 0~7 的八进制数来表示，对应的八进制数，就是二进制数表示的值

例如

```sh
$ chmod 664 myfile
$ ls -l myfile
-rw-rw-r--  1   57 Jul  3 10:13  myfile
```

## [chmod](https://zh.wikipedia.org/wiki/Chmod)

chmod的意思时 change mode, 用来改变对应文件的权限，即更改上面的 3 位八进制数字。

### 用法1

chmod 数字 文件名

例如

```sh
chmod 664 myfile
```

这种方法直接给对应文件赋予相应的权限

### 用法2

chmod 用户 +/-/= 权限名 文件名

这里的用户由`u g o a`三种选择，分别对应所有者、用户组、其他用户、全部三者。`+ - =` 分别表示赋予、减少和设定

可以同时选择多个用户，如果要同时对所有者和用户组增加执行权限可以使用`ug + r`。而 `a` 就相当于 `ugo`，也可以省略

如果对不同用户组给予不同权限，可以用逗号隔开，比如 `chmod u+r,go-r docs` 就是对 u 和 g、o 分别处理权限

举例

```sh
chmod a+r file
chmod +rwx file
chmod u=rw,go= file
```

### 用法3

参数 `-R` 放在 chmod 之后，表示对目录的子目录子文件进行相同操作

## [chown](https://blog.csdn.net/carefree2005/article/details/121473234)

chown 是 change owner 的缩写，用来修改文件的所有者或所有用户组，只能以 root 身份运行

```sh
chown [参数] [user][:group] 文件
```

其中参数，user，group都是可选变量

参数中 -R 表示递归所有子目录子文件
