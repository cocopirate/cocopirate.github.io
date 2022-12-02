---
title: "使用frp内网穿透"
date: 2022-12-02T10:01:19+08:00
draft: true
description: "使用frp内网穿透，将内网服务通过公网IP中转暴露到公网"
tags: [
    "frp",
    "内网穿透",
]
categories: [
    "运维",
]
series: ["教程"]
---


# 准备条件

* 公网服务器：一台具有公网IP的服务器，作为服务端
* 官方文档：https://gofrp.org/docs/
* frp下载页面：https://github.com/fatedier/frp/releases

# 服务端配置

* 下载最新版本：`wget https://github.com/fatedier/frp/releases/download/v0.44.0/frp_0.44.0_linux_386.tar.gz`
* 解压下载文件：`tar -zxvf frp_0.44.0_linux_386.tar.gz`
* frps和frps.ini是客户端相关文件与配置
* 进入解压目录配置frps.ini文件：

```shell
[common]
# 绑定端口配置
bind_port = 7000
vhost_https_port = 7022
vhost_http_port = 7080
token = 1520

# 控制面板配置
dashboard_port = 7001
dashboard_user = <登录账号>
dashboard_pwd = <登录密码>
enable_prometheus = true

log_file = /var/log/frps.log
log_level = info
log_max_days = 3
```

* 在etc目录创建目录：`sudo mkdir -p /etc/frp`
* 将frps.ini复制至/etc/frp目录：`sudo cp frps.ini /etc/frp`
* 将解压文件中的frps目录复制至/usr/bin目录：`sudo cp frps /usr/bin`
* 创建并编辑frps.service文件`vim /etc/systemd/system/frps.service`

```shell
[Unit]
# 服务名称，可自定义
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
# 启动frps的命令，需修改为您的frps的安装路径
ExecStart = /usr/bin/frps -c /etc/frp/frps.ini

[Install]
WantedBy = multi-user.target
```

* 启动frp：`systemctl start frps`
* 停止frp：`systemctl stop frps`
* 重启frp：`systemctl restart frps`
* 查看frp状态：`systemctl status frps`
* 配置frps开机自启：`systemctl enable frps`

# 客户端配置

> 客户端指内网中需要暴露服务的设备

* 从下载页面下载压缩包并解压
* frpc和frpc.ini是客户端相关文件与配置
* 配置frpc.ini文件

```shell
[common]
# 服务器的ip地址
server_addr = <服务器IP>
server_port = 7000
token = 1520

# 配置http服务
[web]
type = http
local_port = 8000
custom_domains = <自定义域名>

# 配置https服务
[test_htts2http]
type = https
custom_domains = <自定义域名>

plugin = https2http
plugin_local_addr = 127.0.0.1:8000

# HTTPS 证书相关的配置
plugin_crt_path = <服务器证书路径>.crt
plugin_key_path = <服务器证书路径>.key
plugin_host_header_rewrite = 127.0.0.1
plugin_header_X-From-Where = frp
```

**SSL域名证书说明**

> HTTPS证书文件需放在服务器端对应的文件目录
> pem转crt格式：`openssl x509 -in fullchain.pem -out fullchain.crt`
> pem转key格式：`openssl rsa -in privkey.pem -out privkey.key`

* 客户端启动：`./frpc -c frpc.ini`
* 更改客户端host文件：<服务器IP> <域名>
* http访问内网8000端口服务：<http域名>:7080
* https访问内网8000端口服务：<https域名>:7022