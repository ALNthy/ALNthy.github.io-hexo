---
title: docker学习笔记
date: 2023-02-02 07:57:45
tags:
- docker
- 学习
categories:
- docker
- 学习
---

## 什么是docker？

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中,然后发布到任何流行的Linux或Windows操作系统的机器上,也可以实现虚拟化,容器是完全使用沙箱机制,相互之间不会有任何接口。 ---- [百度百科](http://nodejs.cn/download/)
<!--more-->
## 安装docker

操作系统：ubuntu

```shell
sudo curl -sSL get.docker.com | sh
```

## 🐳docker核心工作流

    Dockerfile -> Image -> Container

### Dockerfile 定制镜像文件

Dockerfile是一个包含用于组合映像的命令的文本文档。可以使用在命令行中调用任何命令。 Docker通过读取Dockerfile中的指令自动生成映像。

### Image 镜像

Image是通过Dockerfile build生成的一个特殊的文件系统，包含了容器运行所需的所有程序，库，资源。不包含任何动态数据，一旦构建之后内容也不会被改变。

### Container 容器

Dockerfile,Image,Container的关系类似于建筑工程图纸，建筑材料，建筑。容器是镜像运行时的实体，容器可以被创建、启动、停止、删除、暂停等。

## 使用🐳docker

### 制作Dockerfile文件并创建镜像

新建一个文件名称必须为Dockerfile

```Dockerfile
FROM python:3.10.9-bullseye   #FROM 从docker hub拉取一个镜像 这里我们拉取python3.10.9 系统是debian bullseye
WORKDIR /app                  #WORKDIR 指定后面Dockerfile命令的工作路径 
COPY . .                      #COPY 将本地的所有文件复制到镜像中
RUN  pip3 install -r requirements.txt    #RUN 在容器中运行命令
CMD ["python3","app.py"]      #CMD 当容器运行起来后要执行的命令
```

#### 创建镜像

```shell
docker build -t name .   # 在当前目录下寻找Dockerfile 创建一个镜像名为name的镜像
```

#### 通过镜像运行容器

```shell
docker build -p 8080:8080 --name my_Container -d name  #创建一个名为my_Container的容器
```

```txt
参数说明
--name="名字"           指定容器名字
-d                     后台方式运行
-it                    使用交互方式运行,进入容器查看内容
-p                     指定容器的端口
	-p ip:主机端口:容器端口  配置主机端口映射到容器端口
	-p 主机端口:容器端口（常用）
	-p 容器端口
-P                     随机指定端口
-e					   环境设置
-v					   容器数据卷挂载
```
