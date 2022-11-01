---
title: "rclone配合qBittorrent在VPS使用"
date: 2022-10-30T14:36:33+08:00
lastmod: 2022-10-30T14:36:33+08:00
draft: false
keywords: ["rclone","Linux技巧"]
description: "在VPS中设置qbittorrent搭配rclone完成追番追剧"
tags: ["rclone","Linux技巧"]
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
同时在 VPS 和群晖中使用 rclone，VPS 配合 qbittorrent 的 rss 功能自动下载正在追剧和追番的视频，同时将其上传到 Google Drive 中。而群晖使用 rclone 挂载相应的文件夹通过 emby 观看视频。这里主要记录挂载 VPS 的过程。
刚好最近买了一个 VPS，在这里记载配置过程。
参考教程还是这个：[“申请google drive api并使用rclone挂载团队盘为本地磁盘”](https://blog.csdn.net/diqiudq/article/details/126070602)
<!--more-->
### 为 API 创建新的应用凭据

由于之前群晖和 VPS 共用凭据导致两者掉线，所以这里新建一个

创建凭据，选择 OAuth 客户端，应用类型选择 桌面应用，获得客户端 ID 和密钥

### 下载 Rclone 并配置 Google Drive

使用想要使用脚本的用户执行命令

```sh
sudo apt install fuse -y
curl https://rclone.org/install.sh | sudo bash
rclone config
```

根据流程配置 google drive，使用上面创建的客户端 ID 和密钥，记下设置的名称，配置注意点在[Rclone 在群晖中挂载](https://blog.whwh13.tk/post/rclone_synology/)中有写

配合 qBittorrent 不需要挂载，只需要使用脚本即可

### 下载安装 qBittorrent

[教程](https://www.jianshu.com/p/c8e323ff1890)

```sh
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:qbittorrent-team/qbittorrent-stable
sudo apt update
sudo apt install qbittorrent-nox -y
```

设置启动qbittorrent

```sh
sudo vim /etc/systemd/system/qbittorrent-nox.service
```

```sh
[Unit]
Description=qBittorrent-nox
After=network.target
[Service]
User=root
Type=forking
RemainAfterExit=yes
ExecStart=/usr/bin/qbittorrent-nox -d
[Install]
WantedBy=multi-user.target
```

重新载入

```sh
sudo systemctl daemon-reload
sudo systemctl start qbittorrent-nox
```

分别对应 停止和开机自启

```sh
sudo systemctl stop qbittorrent-nox
sudo systemctl enable qbittorrent-nox
```

默认端口是**8080**，默认用户j**admin**，默认密码**adminadmin**

### 配置脚本

[教程1](https://www.idcfq.com/632.html)
[主要参考，防删库已fork](https://github.com/whwh13/qb_rclone)
在具体使用上，参考教程中是通过使用指定的 tag 才上传，我稍微修改成即便没有 tag 也会上传(库中qb_auto_rclone1.sh)

主要是需求不一样，原教程中用的服务器应该有较大的存储空间或者有把文件保留在本地的需求

而我的同步配置放在 VPS 上，存储空间比较小，主要是通过 qBittorrent 配套的上传到多少自动删除的功能自动追番，所以需要自动化的上传。

#### 设置定时执行

需要在 qbittorrent 中先配置好对应的 tag

```sh
crontab -e
*/2 * * * * bash qb_auto_rclone.sh          /*注意路径*/
```
