---
title: "vps_azure"
date: 2022-11-05T14:58:49+08:00
lastmod: 2022-11-05T14:58:49+08:00
draft: false
keywords: []
description: "通过学生认证白嫖了微软的服务器，配置一下code-serve"
tags: ["VPS","azure"]
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
折腾是人类的本性！
<!--more-->

### 安装code-server

参考[简单3步部署code-server](https://blog.csdn.net/weixin_41521681/article/details/126535422)以及[官方wiki](https://coder.com/docs/code-server/latest/guide)
使用一键安装脚本安装`curl -fsSL https://code-server.dev/install.sh | sh`
配置config，在`~/.config/code-serve/config.yaml`，更改密码、端口并添加

```sh
disable-getting-started-override: true  # 关闭code-server促销消息
```

### 配置nginx转发和申请ssl证书

教程参考 [xray 配置](https://xtls.github.io/document/level-0/ch06-certificates.html#_6-4-%E6%AD%A3%E5%BC%8F%E8%AF%81%E4%B9%A6%E7%94%B3%E8%AF%B7)

#### 安装nginx

```sh
sudo apt update && sudo apt install nginx
```

#### 设置目录用来存放证书

```sh
mkdir -p ~/etc/nginx/ssl
mkdir -p ~/etc/nginx/ssl/example.com
```

#### 配置nginx

使用[简单3步部署code-server](https://blog.csdn.net/weixin_41521681/article/details/126535422)中 https 的参考内容

```sh
sudo vim /etc/nginx/nginx.conf
```

将代码复制到 http{} 中间，修改域名、端口和ssl证书地址`/etc/nginx/ssl/example.com/cert.key;/etc/nginx/ssl/example.com/cert.crt`、同时注意修改反代地址不应该是https

**注意**：在申请具体的ssl证书之前，应该把监听端口改成 80，删除后面的ssl，然后将下面的 80 端口的server全部注释掉，再把ssl证书部分注释掉。

#### 安装acme.sh

由于我是用的**反代**的方式代理的nginx，因此不能使用根目录验证acme.sh，而acme直接验证反代的原理是把nginx改变成预设网站，然后验证之后回复原状，因此需要**root用户**安装

```sh
curl https://get.acme.sh | sh -s email=my@example.com  #注意修改邮箱
. .bashrc #让acme.sh命令生效
acme.sh --upgrade --auto-upgrade  #设置自动升级
```

#### acme.sh 使用参数

```sh
-d          需要申请的域名
--test      测试而非直接申请
--webroot   网页的根目录
-w          等价于--webroot
--server    修改服务提供商
--nginx     不需要指定根目录自动在nginx完成配置
```

举例`acme.sh --issue --server letsencrypt --test -d www.mydomain.com --nginx --keylength ec-256`

**注意**：上面的举例用了`--test`测试成功之后应当再正常使用，需要添加参数`--force`

#### 安装证书

```sh
acme.sh --install-cert -d example.com \   
--key-file       /etc/nginx/ssl/example.com/cert.key  \
--fullchain-file /etc/nginx/ssl/example.com/cert.crt \
--reloadcmd     "service nginx force-reload"
```

该命令会被`acme.sh`记录，并在证书更改之后自动修改

**注意**：这个时候就可以把nginx内容修改成ssl状态的了

#### 配置xray

通过上面的步骤实际上code-server就以及能够使用了，但是同时有个 VPS 不搭建一个代理不太合适，所以就顺便配置上代理

配置代理是通过 nginx 反代，使用 vmess+tcp+tls 的方式进行
xray设置，配置文件在`/usr/local/etc/xray/config.json``/etc/nginx/nginx.conf`

```json
{
    "log": {
        "access": "/var/log/xray/access.log",
        "error": "/var/log/xray/error.log",
        "loglevel": "warning"
    },
    "inbounds": [
        {
            "port": 11111,               # 端口
            "listen":"127.0.0.1",
            "protocol": "vmess",
            "settings": {
            "clients": [
                {
                    "id": "uuid",
                    "alterId": 0
                }
            ]
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/path"
                }
            }
        }                              
    ],                              
    "outbounds": [
        {
            "protocol": "freedom",
            "settings": {}
        }
    ],
    "dns": {
        "servers": [
            "https+local://dns.google/dns-query",
            "8.8.8.8",
            "1.1.1.1",
            "localhost"
        ]
    }
}
```

由于我平时使用代理都是在PC或者路由器上，本身就有路由功能，所以就没有添加路由功能，然后通过 nginx 反代本身就有 tls 加密，所以就删除了对应内容，其他和[官方模板](https://github.com/XTLS/Xray-examples/blob/main/VMess-Websocket-TLS/config_server.json)类似

#### 配置 nginx

```conf
http {
server {
    listen 443 ssl;                     # 监听端口
    server_name example.com;       # 域名
    # nginx请求日志地址
    access_log  /usr/local/webserver/nginx/logs/code.access.log;
 
    # ssl证书地址
    ssl on;
    ssl_certificate      /etc/nginx/ssl/example.com/cert.crt;    # pem文件的路径
    ssl_certificate_key  /etc/nginx/ssl/example.com/cert.key;    # key文件的路径
    
    # ssl验证相关配置
    ssl_session_timeout  5m;             # 缓存有效期
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;                # 加密算法
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # 安全链接可选的加密协议
    ssl_prefer_server_ciphers on;        # 使用服务器端的首选算法
 
    location / {
        proxy_pass  http://localhost:port1;
        proxy_redirect     off;
        proxy_set_header   Host             $host;          # 传递域名
        proxy_set_header   X-Real-IP        $remote_addr;   # 传递ip
        proxy_set_header   X-Scheme         $scheme;        # 传递协议
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        # code-server的websocket连接需要Upgrade、Connection这2个头部
        proxy_set_header   Upgrade          $http_upgrade;
        proxy_set_header   Connection       upgrade; 
        proxy_set_header   Accept-Encoding  gzip;
    }

    location /path { # 与 V2Ray 配置中的 path 保持一致
    if ($http_upgrade != "websocket") { # WebSocket协商失败时返回404
        return 404;
    }
    proxy_redirect off;
    proxy_pass http://127.0.0.1:11111; # 假设WebSocket监听在环回地址的10000端口上
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    # Show real IP in v2ray access.log
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }

}
 
# http请求直接重定向到https
server {
   listen 80;                          # 监听端口
    server_name example.com;       # 域名
   return 301 https://$server_name$request_uri;
}
```

**注意**：nginx开启过程中可能会报错，有可能是未创建相应的文件夹导致的，注意修改
**注意**：azure自带有动态 ip，且 ip 仅仅且会在停止虚拟机再开始之后更改，相当于一个可以无限更换的静态ip

### code-server 配置

code-server 并没有一些独属于 vscode 的插件，但是可以通过[微软插件市场](https://marketplace.visualstudio.com/search?target=VSCode)下载对应的nsix格式，上传之后安装

主要用code-server在不方便用电脑的时候编写C语言程序，所以对其进行一下C语言配置

1. 配置编译环境

    ```sh
    sudo apt install gcc g++ gdb
    ```

2. 安装拓展

    ```name
    'C/C++' 'C/C++ Extension Pack' 'C/C++ Themes' 'CMake'
    'Cmake Tools' 'filesize' 'Markdown Preview Github Styling'
    'markdownlint' 'Chinese (Simplified)'
    ```

3. 配置任务

    配置文件`tasks.json`,该文件主要是建立两个任务，分别命名为`single file build`和`run and pause`，并且设置在执行`run and pause`之前一定会执行一次`single file build`

    ```json
    {
    "tasks": [
        {
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "-o",
                "${workspaceFolder}/.build/${fileBasenameNoExtension}",
                "-std=c11",
                "-pedantic",
                "-Wall",
                "-Wextra"
            ],
            "command": "/usr/bin/gcc-9",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "label": "single file build",
            "presentation": {
                "clear": true,
                "echo": true,
                "focus": true,
                "panel": "shared",
                "reveal": "always",
                "showReuseMessage": true
            },
            "problemMatcher": "$gcc",
            "type": "process",
            "detail": "自己配置测试"
        },
        {
            "args": [],
            "command": "${workspaceFolder}/.build/${fileBasenameNoExtension}",
            "dependsOn": "single file build",
            "label": "run and pause",
            "presentation": {
                "clear": false,
                "echo": true,
                "focus": true,
                "panel": "shared",
                "reveal": "always",
                "showReuseMessage": true
            },
            "problemMatcher": [],
            "type": "process"
        }
    ],
    "version": "2.0.0"
    }

    ```

    配置文件`launch.json`，该文件是配置任务`single file debug`，并将`single file build`设置为前置任务

    ```json
    {
    "version": "0.2.0",
    "configurations": [
    {
      "MIMode": "gdb",
      "args": [],
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
      "name": "single file debug",
      "preLaunchTask": "single file build",
      "program": "${workspaceFolder}/.build/${fileBasenameNoExtension}",
      "request": "launch",
      "stopAtEntry": false,
      "type": "cppdbg"
    }
        ]
    }

    ```

4. 配置快捷建

    如果是通过上面的问价配置，系统会默认把`F5`配置成debug，即任务`single file debug`。会默认把`Ctrl + Shift + B`配置成build，即任务`single file build`。此时`F6`命令会打开任务列表，也可以不配置直接使用，选择`run and pause`即可

    也可以编辑 `~/.local/share/code-server/User/keybindings.json` 文件（目录不一定相同，不过和全局的`settings.json`相同目录），添加

    ```json
    [
        {
      "args": "run and pause",
      "command": "workbench.action.tasks.runTask",
      "key": "f6"
        }
    ]

    ```

    把F6设定为执行`run and pause`任务

5. 开启swap

    code-server 经常会无缘无故的不能用，报`502`错误

    这是因为 code-server 更多的占用内存，通过 azure 后台监控看到内存经常占满。在没有办法增加内存的情况下，只有开启 swap 这样一个办法了把

    参考教程[在 Ubuntu 上开启 Swap](https://www.qinwg.cn/n/linux/linux-swap.html)

    ```bash
    free -m       ## 查看当前内存使用情况以及是否开启 swap [Swap: 0 0 0]表示未开启
    dd if=/dev/zero of=/swapfile count=2048 bs=1M       ## 创建名为 swapfile 的 swap 空间，大小为 2048MB
    ls / | grep swapfile        ##查看是否创建成功
    chmod 600 /swapfile
    mkswap /swapfile
    ```

    做完这些操作应该会有类似输出 `Setting up swapspace version 1, size = 2097148 KiB no label, UUID=ff3fc469-9c4b-4913-b653-ec53d6460d0e`

    这样 swap 文件就创建完成了

    开启swap

    ```bash
    swapon /swapfile            ##开启
    free -m                     ##查看是否开启成功
    vim /etc/fstab     ## 在最后添加 /swapfile none swap sw 0 0 设置开机自启
    ```
