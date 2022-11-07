---
title: "Rclone 在群晖中挂载"
date: 2022-10-26T09:13:50+08:00
lastmod: 2022-10-26T09:13:50+08:00
draft: false
keywords: []
description: "在群晖上设置rclone并配置自动挂载"
tags: ["rclone","Linux技巧","影视库"]
categories: ["Linux技巧"]
author: "whwh13"

# Uncomment to pin article to front page
# weight: 1
# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: true
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
同时在 VPS 和群晖中使用 rclone，VPS 配合 qbittorrent 的 rss 功能自动下载正在追剧和追番的视频，同时将其上传到 Google Drive 中。而群晖使用 rclone 挂载相应的文件夹通过 emby 观看视频。这里主要记录挂载群晖的过程。

### 挂载过程

使用群晖挂载rclone主要参照了两个教程
 [“申请google drive api并使用rclone挂载团队盘为本地磁盘”](https://blog.csdn.net/diqiudq/article/details/126070602)
[Rclone 安装配置教程](https://www.bilibili.com/read/cv16130034)

#### 遇到问题及解决

##### 代理问题

在本机的电脑终端运行rclone authorize时总会无法获取cookie，这是因为终端不会使用系统代理，需要运行下列代码为终端提供代理。

```sh
poweshell:
$Env:http_proxy="http://127.0.0.1:port"
$Env:https_proxy="http://127.0.0.1:port"

cmd & linux
set http_proxy=http://127.0.0.1：port
set https_proxy=http://127.0.0.1：port
```

##### 虚拟机下载速度

虚拟机安装rclone时速度很慢，像卡住一样，**猜测是**安装过程中需要下载文件。注意不要因为安装的慢不耐烦提前退出。

<!--more-->

### 群晖自动挂载失败

#### 猜想

~~在群晖中尽管设置了开机自动挂载rclone，但是往往并不能完成，初步的估计是因为开机的时候，rclone的优先级并不是很高，使用计划任务的命令挂载rclone时，rclone主进程还没有开启~~
后面证明该猜测错误

#### 验证猜想

1. 手动运行计划任务，仍然无法挂载
2. 思考是不是计划任务出了问题
查看运行结果，是已中断，该命令在终端中往往运行之后会卡住，而且不能结束任务，必须直接关闭终端才行。
3. **猜测**：任务计划运行时，如果程序卡住的时候会直接关闭程序
4. 这是任务计划自身的问题，无法直接解决

#### 更换脚本

在网上查了一下，["群晖/Linux挂载阿里云盘实现Emby播放"](https://zhuanlan.zhihu.com/p/400651565),在这里找到了一个脚本，想拿来试试能不能行

```sh
#!/bin/bash
### BEGIN INIT INFO
# Provides:          rclone
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start rclone at boot time
# Description:       Enable rclone by daemon.
### END INIT INFO

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

REMOTE='googledrive:video'
LOCAL='/volume2/Rclone/google'
CONFIG='/root/.config/rclone/rclone.conf'
DEMO='rclone'

[ -n "$REMOTE" ] || exit 1;
[ -x "$(which fusermount)" ] || exit 1;
[ -x "$(which $DEMO)" ] || exit 1;

case "$1" in
start)
  ps -ef |grep -v grep |grep -q "$REMOTE"
  [ $? -eq '0' ] && {
    DEMOPID="$(ps -C $DEMO -o pid= |head -n1 |grep -o '[0-9]\{1,\}')"
    [ -n "$DEMOPID" ] && echo "$DEMO already in running.[$DEMOPID]";
    exit 1;
  }
  fusermount -zuq $LOCAL >/dev/null 2>&1          # 这里>/dev/null 2>&1意思是把输出全部丢弃
  mkdir -p $LOCAL
  rclone mount $REMOTE $LOCAL --config $CONFIG --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 >/dev/null 2>&1 &
  sleep 3;                                                 ## sleep 3 表示暂停三秒再执行下一个任务
  DEMOPID="$(ps -C $DEMO -o pid=|head -n1 |grep -o '[0-9]\{1,\}')"
  [ -n "$DEMOPID" ] && {                                                              # -n 表示如果字符串长度不为0，返回true
    echo -ne "$DEMO start running.[$DEMOPID]\n$REMOTE --> $LOCAL\n\n"
    echo 'ok' >/root/ok
    exit 0;
  } || {
    echo "$DEMO start fail! "
    exit 1;
  }
  ;;
stop)
  DEMOPID="$(ps -C $DEMO -o pid= |head -n1 |grep -o '[0-9]\{1,\}')"
```

运行之后发现仍然不行，报错为"rclone start fail!"

#### 分析脚本寻找问题

对比之前手动执行的代码

```sh
rclone mount googledrive:video /volume2/Rclone/google --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
```

对比发现不同在于有了一个`--config`参数
同时执行脚本报错之后尝试手动执行一次，发现手动执行也开始报错了，报错代码大概的意思是没有对 rclone 进行配置

对该现象进行思考，提出一个猜想，rclone会在配置rclone的用户目录下建立一个config，位置在`~/.config/rclone/rclone.conf`，我是用admin用户配置的rclone，所以并没有在root文件夹中对应的配置文件。手动执行的过程也是使用了 root 用户，所以会报未配置。所以修改脚本中对应位置

```sh
CONFIG='/volume1/homes/admin/.config/rclone/rclone.conf'
```

#### 成功配置

通过群晖计划任务运行成功，下一步是重启查看emby能否自动挂载，~~写文章时正在重启中~~

#### 完美成功

开机自启并挂载EMBY成功

#### 插曲

在查找解决方案的时候有想过使用screen配合命令实现挂载，于是安装了screen，通过教程 [群晖利用ipkg代替apt-get yum 安装 screen](https://lil.cx/527.html)。
步骤：

##### 1. 安装ipkg

```sh
wget http://ipkg.nslu2-linux.org/feeds/optware/syno-i686/cross/unstable/syno-i686-bootstrap_1.2-7_i686.xsh
chmod +x syno-i686-bootstrap_1.2-7_i686.xsh
sh syno-i686-bootstrap_1.2-7_i686.xsh
ipkg update
```

##### 2.使用ipkg安装screen

```sh
ipkg install screen
```

##### 3.使用screen报错

```er
Cannot find termcap entry for 'xterm-256color'.
```

##### 4.解决

使用 `TERM=xterm screen` 代替 `screen`
来源：[领悟书生](https://m.656463.com/wenda/Unixpmsycxcwzbdxterm256colordter_95)

##### 5.使用ipkg安装wget-ssl

群晖自带的 wget 不能使用 https, 报错为`HTTPS support not compiled in`

```sh
sudo -i

##卸载原有wget
ipkg remove wget

##安装支持https的wget
ipkg install wget-ssl
```

来源：[Geowo](https://www.cnblogs.com/geowo/p/14941705.html)
