---
layout: post
title:  docker初探
date:   2018-04-19 16:12:01 +0800
categories: document
tag: docker
---

* content
{:toc}

    A moment on the lips, forever on the hips.

                      ——————— 贪得一时嘴馋，平添一生赘肉。

---------------------------------------

一直都听周围的人说docker，说docker多么多么好，但是从来没接触过，就只知道docker很轻量，用起来很方便。这次接到个项目是关于docker的，就利用这次机会，研究了一下docker。

#### 1、docker是什么？

“Docker is an open platform for developing, shipping, and running applications。” 这是官网的解释。百度百科上解释是 “Docker 是一个开源的应用容器引擎”，其实docker就是一个可以提供容器的服务。在一台主机上安装了docker服务，然后就可以创建n个容器，每个容器间采用的是沙箱机制，相互隔离。

#### 2、docker的作用和优点？
- 快速、完善的工作流

  这个主要是针对开发测试说的，开发人员可以利用容器模拟线上环境，进行代码测试，测试完成，代码ok，提交代码，测试人员直接通过容器模拟环境进行测试。

- docker的可移植

    docker可以移植到不同的平台，所以完全不用担心平台的兼容性。你在linux上的程序，利用容器，在window也一样能运行。

- docker的隔离

    在同一台宿主机上可以创建很多个容器，不同的容器可以有不同的服务，而且因为容器的轻量级，所以比虚拟机会快，更小的消耗资源，能容纳的工作负载更多。

#### 3、为什么说docker比虚拟机更轻量

假设宿主机都是物理机，虚拟机是在这个物理机上虚拟出来有完整内核的机子，有自己独立的操作系统，共享的是物理机的硬件，而容器是共享物理机的内核，每个容器里面只是有不同的包管理程序等，不包含内核，所以docker比虚拟机更轻量。


#### 4、docker的组成

docker分成四个部分：
- docker client：用来与用户交互的接口
- docker daemon：docker守护进程，管理着容器、镜像、网络等的生命周期
- docker containers：docker容器
- docker registries：docker 镜像仓库，主要是存放docker镜像的。

这是docker官网的架构图。
![docker 架构图](https://docs.docker.com/engine/images/architecture.svg)

#### 5、docker的简单使用

虚拟机：10.0.50.107 Centos7.2 (我是在虚拟机上搭建docker的)

- 安装docker

    yum install -y docker
    systemctl start docker

- 构建镜像
  （利用Dockerfile构建镜像）

    Dockerfile文件

        # Use an official Python runtime as a parent image
        FROM python:2.7-slim  #因为程序需要python环境，所以这里继承的是官方的python镜像

        # Set the working directory to /app
        WORKDIR /app

        # Copy the current directory contents into the container at /app
        ADD . /app

        # Install any needed packages specified in requirements.txt
        RUN pip install --trusted-host pypi.python.org -r requirements.txt

        # Make port 80 available to the world outside this container
        EXPOSE 80

        # Define environment variable
        ENV NAME World

        # Run app.py when the container launches
        CMD ["python", "app.py"]

    app.py 测试程序

        from flask import Flask
        from redis import Redis, RedisError
        import os
        import socket

        # Connect to Redis
        redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

        app = Flask(__name__)

        @app.route("/")
        def hello():
            try:
                visits = redis.incr("counter")
            except RedisError:
                visits = "<i>cannot connect to Redis, counter disabled</i>"

            html = "<h3>Hello {name}!</h3>" \
                   "<b>Hostname:</b> {hostname}<br/>" \
                   "<b>Visits:</b> {visits}"
            return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

        if __name__ == "__main__":
            app.run(host='0.0.0.0', port=80)

    requirements.txt  程序所需要的安转包文件

        Flask
        Redis

    这三个文件放在同一个目录

        [root@yutt-docker make_image]# ls
        app.py  Dockerfile  requirements.txt

    然后在make_image目录中执行

        docker build -t image_name .

    如果需要上传，则需要执行

        docker tag image_name localhost:5000/image_name #然后给镜像打tag

        docker push localhost:5000/image_name # 上传到镜像库

    查看本地镜像

        docker images


- 根据构建好的创建容器

        docker run -p 4000:80 --name container_name  image_name（镜像可以是本地已经下载好的镜像，也可以是镜像仓库的镜像，有版本的需要加上版本号）

- 测试

 在浏览器输入
http://10.0.50.107:4000/
可以看到程序输出的信息

- 查看运行中的容器

        [root@yutt-docker ~]# docker ps
        CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                    NAMES
        65c22bb829a7        image_name          "python app.py"          4 minutes ago       Up 4 minutes        0.0.0.0:4000->80/tcp     container_name




