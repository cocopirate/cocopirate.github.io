---
title: "docker部署scrapyd+scrapyweb"
date: 2022-11-24T11:05:22+08:00
draft: false
description: "通过docker快捷部署scrapyd和scrapyweb"
tags: ["scrapy","爬虫","docker","scrapyd","scrapyweb"]
categories: ["爬虫"]
series: ["教程"]
---


# docker部署scrapyd

创建一个文件夹，并在文件夹下创建三个文件：

* Dockerfile：通过docker build 制作镜像
* scrapyd.conf：scrapyd的配置文件
* requestments.txt：相关依赖包，项目会在scrapyd所在环境上运行

## Dockerfile

```shell
FROM python:3.8
WORKDIR /code
RUN mkdir ./logs
COPY scrapyd.conf /etc/scrapyd/
COPY requirements.txt .
EXPOSE 6800
RUN pip3 install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple
RUN pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
# 默认的logs位置是在 /root/logs; RUN mkdir /root/logs
CMD logparser -dir ./logs & scrapyd
```

## requirements.txt

```
beautifulsoup4==4.11.1
fake-useragent==0.1.11
lxml==4.8.0
pymongo==3.5.0
PyMySQL==1.0.2
pyquery==1.4.3
python-scrapyd-api==2.1.2
redis==4.3.1
requests==2.27.1
Scrapy==2.6.1
scrapy-redis==0.7.2
Scrapy-Redis-BloomFilter==0.8.1
scrapy-splash==0.8.0
scrapyd==1.3.0
logparser==0.8.2
pyopenssl==22.0.0
```

## scrapyd.conf

```
[scrapyd]
eggs_dir    = eggs
logs_dir    = logs
items_dir   =
jobs_to_keep = 5
dbs_dir     = dbs
max_proc    = 0
max_proc_per_cpu = 4
finished_to_keep = 100
poll_interval = 5.0
bind_address = 0.0.0.0
http_port   = 6800
username    =
password    =
debug       = off
runner      = scrapyd.runner
jobstorage  = scrapyd.jobstorage.MemoryJobStorage
application = scrapyd.app.application
launcher    = scrapyd.launcher.Launcher
webroot     = scrapyd.website.Root
eggstorage  = scrapyd.eggstorage.FilesystemEggStorage

[services]
schedule.json     = scrapyd.webservice.Schedule
cancel.json       = scrapyd.webservice.Cancel
addversion.json   = scrapyd.webservice.AddVersion
listprojects.json = scrapyd.webservice.ListProjects
listversions.json = scrapyd.webservice.ListVersions
listspiders.json  = scrapyd.webservice.ListSpiders
delproject.json   = scrapyd.webservice.DeleteProject
delversion.json   = scrapyd.webservice.DeleteVersion
listjobs.json     = scrapyd.webservice.ListJobs
daemonstatus.json = scrapyd.webservice.DaemonStatus
```

## 制作镜像和启动容器

```shell
# 制作镜像
docker build -t scrapyd .
# 启动容器
docker run -d -p 6800:6800 --name scrapyd scrapyd
```

# docker部署scrapyweb

创建一个文件夹，并在文件夹下创建三个文件：

* Dockerfile：通过docker build 制作镜像
* scrapydweb_settings_v10.py：scrapydweb的配置文件，为方便修改使用挂载方式 

## Dockerfile

```
FROM python:3.10
WORKDIR /code
EXPOSE 5000
RUN pip3 install Werkzeug==2.0.3 -i https://pypi.tuna.tsinghua.edu.cn/simple
RUN pip3 install scrapydweb -i https://pypi.tuna.tsinghua.edu.cn/simple
CMD scrapydweb
```

## scrapydweb_settings_v10.py

在宿主机先通过`pip install scrapydweb`安装scrapydweb，安装后执行scrapydweb生成scrapydweb_settings_v10.py文件，修改文件中的SCRAPYD_SERVERS配置

``` shell
# 其他配置...
SCRAPYD_SERVERS = [
    'my_scrapyd:6800' # 使用容器的链接配置SCRAPYD_SERVERS地址
]
# 其他配置...
```

## 制作镜像和启动容器

* 将已创建的scrapyd容器与scrapydweb容器链接并命名为my_scrapyd；
* 将宿主机的/root/scrapydweb目录挂载至容器的code目录，方便配置scrapydweb_settings_v10.py文件。

``` shell
docker build -t scrapydweb .
docker run -d -p 5000:5000 --link scrapyd:my_scrapyd -v /root/scrapydweb:/code --name scrapydweb scrapydweb
```

## 其他事项

访问scrapyweb如果出现Http400处理方式：

``` shell
pip install flask==2.0.2
pip install flask-compress==1.12
```

在容器中通过scrapydweb命令重启即可。