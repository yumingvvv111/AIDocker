# Docker 部署AI项目


### 基本用法
1. 运行 `make build` 
2. 再运行 `make up` 

## 编写docker配置文件

```
FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

# 安装基本依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        wget \
        libopencv-dev \
        libsnappy-dev \
        python-dev \
        python-pip \
        tzdata \
        vim


# 安装anaconda 
RUN wget --quiet https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    echo "export PATH=/opt/conda/bin:$PATH" >> ~/.bashrc


# 设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime


# 设置编码
ENV LANG C.UTF-8 LC_ALL=C.UTF-8


# 初始化工作空间
RUN mkdir /workspace

```

## 构建docker镜像

`make build`

根据以上配置文件生成镜像

## 使用镜像

`docker run -it docker-django_web`

-it 进入容器的命令行

## 测试anaconda是否安装完成

`conda list`

## 安装TensorFlow

1. 添加清华的镜像库提高下载速度

```
conda config --addchannels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --setshow_channel_urls yes
```

2. 用anaconda创建环境

```
conda create -n tensorflow python=3.6
```

3. 激活TensorFlow

`activate tensorflow` (或者前边加 source)

4. 正式安装TensorFlow

```
#CPU版本

pip install --upgrade tensorflow

#GPU版本

pip install --upgrade tensorflow-gpu

```

5. 测试是否成功安装

```
python

import tensorflow as tf
```
没报错即成功

6. 同样的方法, 安装其它依赖模块



## 部署 Django 使用 Nginx, Celery, Redis, Postgresql, Docker

### 预览
项目代码在 `src` 目录中. 开始运行时会开放 `8000` 端口:


### 所有命令
1. `make up` 启动项目和容器
2. `make build` 构建项目
3. `make start` 只启动容器
4. `make stop` 停止容器
5. `make shell-web` web容器shell脚本.
6. `make shell-db` 数据库容器的shell.
7. `make shell-nginx` nginx的shell.
8.  `make collectstatic` 放置静态文件.
9.  `make log-web` web日志.
10. `make log-db` 数据库日志.
11. `make log-nginx`nginx日志.
12. `make restart` 重启容器.
