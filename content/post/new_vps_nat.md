---
title: "新的 nat vps"
date: 2022-10-27T21:42:45+08:00
lastmod: 2022-10-27T21:42:45+08:00
draft: false
keywords: ["VPS","IPV6","NAT"]
description: "买了个nat vps，确实难用，就这样吧，开摆"
tags: ["VPS","IPV6","NAT"]
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
最近正值黑五，打算换一台VPS，正巧低价买了一台NAT型的vps，这里练一下手，记录一下新拿到VPS应该怎样操作
我比较习惯的系统是Ubuntu，一般使用 20.04 ，这篇文章会建立在 ubuntu 20.04 的基础上
<!--more-->

### 新建用户并添加sudo权限

```shell
# adduser 用户名
# passwd 用户名
# visudo
添加 用户名 ALL=(ALL:ALL) ALL
```

### 修改ssh端口以及密钥

```sh
# su whwh
$ cd ~ & mkdir .ssh
$ vim .ssh/authorized_keys

$ sudo vim /etc/ssh/sshd_config
更改 PermiRootLogin no
PasswordAuthentication no

AddressFamily any                       /*用于启用ipv6 ssh*/

$  
```

还需要更改端口，但是我买的这个NAT型的，有一个自动的端口映射，感觉不需要改端口

### 软件更新

```sh
sudo apt update
sudo apt upgrade
```

### 搭建探针

当时买这个机子的时候是看重的价格便宜，并没有想好要拿来做什么，再加上这个ip是被墙的，也搭建不了梯子，索性就搭建个[探针](https://nezhahq.github.io/)随便玩玩
[参考](https://zhuanlan.zhihu.com/p/337443320)
失败了，精简系统太精简，连 /etc/sudoers 都没有。而更习惯用的 22.04 ，甚至因为内存太小安装不了软件
太难用了，这个就放着吃灰吧
尝试用ipv6搭建一个梯子，最后因为systemctl的问题，不会设置代理了，就这样吧
