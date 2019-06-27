# Docker 上线部署AI项目到阿里云服务器

## 登录云服务器

`ssh root@39.97.224.231`

## 进入AIDockerProject目录

`cd AIDockerProject && mkdir 自己的项目名字`


## 进入自己专用的目录通过git拉取示例代码

`git clone https://github.com/yumingvvv111/AIDocker.git`


## docker配置文件(Dockerfile)说明

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

4. 安装TensorFlow等项目依赖的模块

* 通过pip安装

```
#CPU版本

pip install --upgrade tensorflow

#GPU版本

pip install --upgrade tensorflow-gpu

```

同样的方法, 安装其它依赖模块

* 或者通过conda安装
  
  前提文件夹内有environment.txt或者environment.yaml文件
  生成环境文件的命令是`conda env export > environment.yaml `
  导入环境的命令是`conda env create -f environment.yaml `


5. 测试是否成功安装

```
python

import tensorflow as tf
```
没报错即成功

6. 安装依赖之后运行项目
   
在命令行里启动项目

7. 保存项目

执行`exit`退出docker容器
然后输入`docker ps -a`查看刚才退出的是哪个容器, 复制它的id
执行`docker commit 80cdc99d89dd new_container:tag1`, 其中80cdc99d89dd就是容器的id号, 自行替换. 后边的参数分别是容器名和标签名

8. 再次进入项目不需要重新安装环境

只用执行`docker images`查看上一步创建的新镜像名'new_container', 复制他的id号, 假如是12345678

然后`docker run -it 12345678` 则再次进入容器, 直接运行项目而不需再次安装依赖.

