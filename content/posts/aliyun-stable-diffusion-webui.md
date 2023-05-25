---
title: "阿里云部署Stable Diffusion WebUI"
keywords: ["Stable Diffusion WebUI","SD","阿里云"]
description: "在阿里云GPU服务器上部署Stable Diffusion WebUI"
tags: ["Stable Diffusion"]
categories: ["人工智能"]
series: ["Stable Diffusion"]
date: 2023-05-25
draft: false
author: "壹點漫談"
---

# 设备环境

## 服务器选购

* 系统：Ubuntu 22.04 64位
* GPU：NVIDIA Tesla T4
* 系统盘：128GB，各种模型较大，建议使用128GB以上
* 选用按量付费方式，停机时选择“节省停机模式”节省开支

## 环境配置

### 安装基础工具

安装Python3.10以上版本、pip和git等基础工具，已按照的忽略本步骤。基础工具按照参考：

> 通过yum安装一些基础工具`sudo yum install -y git conda mesa-libGL zlib-devel libjpeg-turbo-devel`
> 使用`python --version`确认版本，最好是3.10以上版本。以Python3.8版本为例`sudo yum install -y python38 python38-pip`

### 安装CUDA

访问[NVIDIA官网](https://developer.nvidia.com/cuda-downloads)下载CUDA，选择对应的系统版本，以Ubuntu 22.04为例，选择Linux-x86_64-ubuntu22.04，下载deb(network)版本。

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda=
```
apt-cache madison cuda 查看可安装的版本


### 创建python虚拟环境

```bash
mkdir sd_venv  # 放置python虚拟环境的目录
cd sd_venv # 进入目录
python3 -m venv sd-venv  # 创建python虚拟环境，其中sd-venv为虚拟环境名称
source bin/activate  # 激活虚拟环境
```

**以下命令操作全部在python虚拟环境下操作**

# 安装Stable Diffusion WebUI

从Github上获取最新的Stable Diffusion WebUI代码。

```bash
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

克隆完成后，进入stable-diffusion-webui目录，安装依赖。

```bash
cd stable-diffusion-webui
pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu117
pip install opencv-python-headless gfpgan open-clip-torch xformers pyngrok clip-anytorch
pip install -r requirements_versions.txt
```

# 启动Stable Diffusion WebUI

当安装全部完成后，使用以下命令启动：

```bash
python launch.py --listen
```

若需要后台运行启用可以使用nohup命令：

```bash
nohup python launch.py --listen &
```

> 启用完成后，在阿里云安全组中添加规则，允许对应的端口访问，例如：7860