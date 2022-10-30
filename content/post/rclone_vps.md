---
title: "rclone配合vps使用"
date: 2022-10-30T14:36:33+08:00
lastmod: 2022-10-30T14:36:33+08:00
draft: true
keywords: ["rclone","Linux技巧"]
description: "在VPS中设置qbittorrent搭配rclone完成追番追剧"
tags: ["rclone","Linux技巧"]
categories: ["Linux技巧"]
author: ""

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
<!--more-->
