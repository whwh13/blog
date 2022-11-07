---
title: "Vps"
date: 2022-10-26T09:16:52+08:00
lastmod: 2022-10-26T09:16:52+08:00
draft: false
keywords: ["VPS","购物"]
description: "记录对比各个各个VPS运营商的过程，根据需求选择合适的VPS"
tags: ["VPS","购物"]
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
我使用的VPS在11月10号到期，虽然 IP 没有被完全封禁，但是443端口已经不能用了，所以打算换个VPS，以满足更高的需求，在这里记录一下各个VPS的条件。主要的需求由以下几个：

* 挂梯子以及网站伪装
* 年付在20美刀一下
* 硬盘容量相对较大，至少应该有 30GB，用于给 Google Drive 转存
* 对BT下载具有一定的容忍度,后面详细讨论
<!--more-->
### 测试脚本

几个相对用的比较多的测试脚本，不一定完全安全，用之前先看看

```sh
wget -qO-  git.io/superbench.sh | bash
bash <(curl -Lso- https://git.io/superspeed.sh)
wget -q https://github.com/Aniverse/A/raw/i/a && bash a
```

### 可以关注的低价VPS

[VIR](https://virmach.com/)
[PR](https://pacificrack.com/)
[RN](https://www.racknerd.com/)
[DDP](https://dedipath.com/)
[CC](https://cloudcone.com/)

### VPS 与 BT 下载

很多云服务提供商会禁止BT下载，主要原因是版权DMAC相关会被投诉。所以下载国内发布的一般没问题（？）

![一些人的经验之谈？](https://raw.githubusercontent.com/whwh13/picgo/master/2022/10/26/20221026200148.jpg)

### SEEDBOX

在查找VPS对于BT的容忍程度的时候了解到的 seedbox ，其本质上类似于只能使用部分BT软件和文件管理器的VPS，不能自己安装软件
[Gigabox 1欧元1个月](https://giga-rapid.com/products/seedboxes)

### NAT型VPS

原理是数个机子共用一个IP，每个机子只能使用有限个端口，通过这种方式分担IP的价格，能做到很低的价格
[2刀用优惠码 LEB-NAT-128MB](https://hosting.gullo.me/order/config/index/nat-ipv4-vps-de/?group_id=5&pricing_id=55#)
[nat型VPS](https://blog.whwh13.tk/post/new_vps_nat/)

### 临时使用

在买之前的 NAT 型 VPS 的时候买了一张 5 美刀的信用卡，还剩下 3 美刀，也就想着物尽其用买了一个 1 个月的 VPS，用于等黑五这段时间的备用
[$2.75 4核 4G 80SSD](https://lowendtalk.com/discussion/181836/flash-sale-50-off-for-2-months-directadmin-softaculous-kvm-virtualizor-nvme-storage)
[配置Rclone](https://blog.whwh13.tk/post/rclone_vps/)
