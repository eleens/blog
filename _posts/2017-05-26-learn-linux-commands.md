---
layout: post
title: 学习linux命令
date: 2017-05-25 13:12:01 +0800
categories: document
tag: linux
---

* content
{:toc}

### 命令格式


    command [-options] parameter1 parameter2 ....

    命令 选项 参数（1） 参数（2）...

### 基础常识

+ man command  查看command 命令的操作说明
+ man 1-7 man
+ man -f man
+ man -k man
+ info info

### 热键

+ Tab键   命令补全、文件补齐
+ Ctrl+c  中断目前程序的按键
+ Ctrl+d  断开连接，相当于exit

### ls 命令  查看文件夹下面的文件


ls  [-al/-a -l] [文件夹目录]  #查看目录下所有显示文件，加上参数 al，表示查看所有文件，包括隐藏的文件

    [root@vs-controller ~]# ls /home/
    proxy  tem.py  vs_cloud  vscloud-dashboard-mariadb.spec
    [root@vs-controller ~]# ls -al /home/
    total 32
    drwxr-xr-x.  3 root     root        83 May 18 16:51 .
    dr-xr-xr-x. 18 root     root      4096 May 16 12:48 ..
    -rw-r--r--   1 root     root        75 May 15 09:14 proxy
    -rw-r--r--   1 root     root       676 May 18 16:51 tem.py
    drwxr-xr-x. 22 vs_cloud vs_cloud  4096 May 25 20:06 vs_cloud
    -rwxr-x---   1 root     root     15714 May  8 19:36 vscloud-dashboard-mariadb.spec
    [root@vs-controller ~]# ls -a -l /home/
    total 32
    drwxr-xr-x.  3 root     root        83 May 18 16:51 .
    dr-xr-xr-x. 18 root     root      4096 May 16 12:48 ..
    -rw-r--r--   1 root     root        75 May 15 09:14 proxy
    -rw-r--r--   1 root     root       676 May 18 16:51 tem.py
    drwxr-xr-x. 22 vs_cloud vs_cloud  4096 May 25 20:06 vs_cloud
    -rwxr-x---   1 root     root     15714 May  8 19:36 vscloud-dashboard-mariadb.spec
    [root@vs-controller ~]# ls        -al /home/
    total 32
    drwxr-xr-x.  3 root     root        83 May 18 16:51 .
    dr-xr-xr-x. 18 root     root      4096 May 16 12:48 ..
    -rw-r--r--   1 root     root        75 May 15 09:14 proxy
    -rw-r--r--   1 root     root       676 May 18 16:51 tem.py
    drwxr-xr-x. 22 vs_cloud vs_cloud  4096 May 25 20:06 vs_cloud
    -rwxr-x---   1 root     root     15714 May  8 19:36 vscloud-dashboard-mariadb.spec

### LANG  设置语言

注意：用下述方式设置语言只是在当次登录有效

    [root@vs-controller ~]# echo $LANG
    en_US.UTF-8
    [root@vs-controller ~]# LANG=zh_CN    #注意=号前后都没有空格
    [root@vs-controller ~]# echo $LANG
    zh_CN

### date 查看当前系统日期时间

    [root@vs-controller ~]# date
    Fri May 26 09:27:38 CST 2017
    [root@vs-controller ~]# date +%Y/%m/%d/
    2017/05/26/
    [root@vs-controller ~]# date +%Y/%m/%d/%H:%M
    2017/05/26/09:27

### cal 日历

cal [[month] year]   查看某年某月的日历

    [root@vs-controller ~]# cal
          May 2017
    Su Mo Tu We Th Fr Sa
        1  2  3  4  5  6
     7  8  9 10 11 12 13
    14 15 16 17 18 19 20
    21 22 23 24 25 26 27
    28 29 30 31

    [root@vs-controller ~]# cal 12 2009
        December 2009
    Su Mo Tu We Th Fr Sa
           1  2  3  4  5
     6  7  8  9 10 11 12
    13 14 15 16 17 18 19
    20 21 22 23 24 25 26
    27 28 29 30 31

### bc  计算器

注意：默认的bc除法是仅输出整数的，如果要输出小数，要设置**scale=number**（number代表保留的小数点位数）

    [root@vs-controller ~]# bc
    bc 1.06.95
    Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006 Free Software Foundation, Inc.
    This is free software with ABSOLUTELY NO WARRANTY.
    For details type `warranty'.
    1+56
    57
    56-50+2
    8
    9/3
    3
    3/9
    0
    quit
    [root@vs-controller ~]# bc
    bc 1.06.95
    Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006 Free Software Foundation, Inc.
    This is free software with ABSOLUTELY NO WARRANTY.
    For details type `warranty'.
    scale=3
    3/9
    .333

### 查看系统的使用状态

+ who 查看目前谁在线
+ netstat -a 查看网络的联机状态
+ ps -aux 查看后台执行的程序

### 关机
+ sync 将数据同步写入到硬盘
+ shutdown 关机
+ reboot   重启
+ halt     关机
+ poweroff  关机





