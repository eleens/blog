---
layout: post
title:  docker搭建wordpress,并实现备份
date:   2018-04-21 16:12:01 +0800
categories: document
tag: 其他
---

* content
{:toc}

    A moment on the lips, forever on the hips.

                      ——————— 贪得一时嘴馋，平添一生赘肉。

---------------------------------------


##### **创建mysql容器**

        docker run --name some-mysql -h mysql -v $PWD/var/lib/mysql:/var/lib/mysql -v $PWD/etc/mysql:/etc/mysql   -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7

        # -v 后面的两个参数表示把mysql的文件映射到本地，因为mysql的Dockerfile中有一个 volume  指向的是/var/lib/mysql，所以这里把volume映射到宿主机上，实现数据的持久化


##### **创建wordpress 容器**

        docker run --name some-wordpress -h wordpress -v $PWD/var/www/html:/var/www/html -v ~/wordpressDof/config/php.ini:/etc/php5/apache2/php.ini --link some-mysql:mysql -p 8080:80 -d wordpress

##### **在页面上更改主题等信息，更新站点**

##### **用更改后的容器构建一个新的镜像**

        格式：
        docker commit -p container_id IMAGE_NAME # -p 表示暂停容器

        docker commit -p d32ec78bb164 my_sql_img

        docker commit -p 7cf5c51a3f1d my_press_img

##### **用新创建的镜像起mysql**

        docker run --name my-mysql -h mysql -v $PWD/var/lib/mysql:/var/lib/mysql -v $PWD/etc/mysql:/etc/mysql -e MYSQL_ROOT_PASSWORD=123456 -d my_sql_img

##### **用新创建的镜像起wordpress**

        docker run --name my-wordpress -h wordpress -v $PWD/var/www/html:/var/www/html -v ~/wordpressDof/config/php.ini:/etc/php5/apache2/php.ini --link my-mysql:mysql -p 8080:80 -d my_press_img

注意：

1）在启动新的wordpress的时候需要把原来的stop，然后用原来的端口映射（具体原因未知）

  2）要实现备份的功能，一定要加上上面的-v，否则数据不会持久化，无法达到备份效果，备份打包最好把本地的映射目录的文件也备份一遍

##### **打包镜像（save）**

        docker save busybox > busybox.tar
        docker save --output busybox.tar busybox
        docker save -o fedora-latest.tar fedora:latest

[https://docs.docker.com/engine/reference/commandline/save/](https://docs.docker.com/engine/reference/commandline/save/)

##### **根据包创建镜像(load)**

        docker load < busybox.tar.gz
        docker load --input fedora.tar

[https://docs.docker.com/engine/reference/commandline/load/#parent-command](https://docs.docker.com/engine/reference/commandline/load/#parent-command)

参考：

[https://github.com/docker-library/mysql/issues/44](https://github.com/docker-library/mysql/issues/44)

[https://github.com/docker-library/wordpress/issues/99](https://github.com/docker-library/wordpress/issues/99)
