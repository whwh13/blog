---
title: "新的 nat vps"
date: 2022-10-27T21:42:45+08:00
lastmod: 2022-10-27T21:42:45+08:00
draft: false
keywords: []
description: "买了个nat vps，确实难用，就这样吧，开摆"
tags: ["VPS","IPV6","NAT","Linux技巧"]
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
cd ~ 
mkdir .ssh
vim .ssh/authorized_keys

$ sudo vim /etc/ssh/sshd_config
更改 PermitRootLogin no
PasswordAuthentication no
AddressFamily any                       /*用于启用ipv6 ssh*/

sudo systemctl reload sshd
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

### 搭建梯子

今天突然心血来潮，果然我还是喜欢折腾，换了一个系统 centos7，然后[一键脚本](https://github.com/233boy/v2ray/wiki/V2Ray一键安装脚本)就成功了，当然，在安装的时候需要同时把 IPV6 和 IPV4 解析到同一个域名，然后脚本之后就能用了
不过虽然速度还算不错，但是延迟很高，不是很好用

### ssh 配置

因为是 nat 型机子，在连接机子的 ssh 只能使用服务商提供的指定端口（经鉴定是其做了端口映射，机子仍然要开放 22 端口）。于是就想能不能通过 IPV6 自定义端口，同时考虑到不是任何时候都有 IPV6，所以对应的 22 端口也要保留。同时机子的 IPV6 的 22 端口被墙了，不能使用，必须换个端口

修改 `/etc/ssh/sshd_config`，将原来的端口注释掉，通过`ListenAddress`项配置端口监听

```sh
 ListenAddress
    指定 sshd(8) 监听的网络地址，默认监听所有地址。可以使用下面的格式：

        ListenAddress host|IPv4_addr|IPv6_addr
        ListenAddress host|IPv4_addr:port
        ListenAddress [host|IPv6_addr]:port
 
        如果未指定 port ，那么将使用 Port 指令的值。
        可以使用多个 ListenAddress 指令监听多个地址。
```

注意监听 ipv6 时的中括号。
