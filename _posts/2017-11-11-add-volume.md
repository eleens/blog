---
layout: post
title:  linux 和 windows 如何新增一块硬盘
date:   2017-11-11 11:09:01 +0800
categories: linux
tag: 笔记
---

* content
{:toc}

### windows

1.把硬盘安装到电脑上

2.初始化硬盘

    安装之后，打开机子，点击开始——》选择计算机——》右击选择管理，会出现一个初始化的界面，如下图，

![Alt text]({{ '/styles/images/2017_11/win_volume1.jpg' | prepend: site.baseurl }} "管理磁盘")

选择的分区方式是MBR,然后点击确定，出现如下情况，会出现10G的未分配的区域，这10G就是我安装的硬盘。

![Alt text]({{ '/styles/images/2017_11/win_volume2.jpg' | prepend: site.baseurl }} "管理磁盘")

3.创建磁盘分区

  选择未分配的磁盘，然后右击，选择 新建简单卷，

![Alt text]({{ '/styles/images/2017_11/win_volume3.jpg' | prepend: site.baseurl }} "管理磁盘")

![Alt text]({{ '/styles/images/2017_11/win_volume4.jpg'| prepend: site.baseurl }} "管理磁盘")

![Alt text]({{ '/styles/images/2017_11/win_volume5.jpg' | prepend: site.baseurl }} "管理磁盘")

![Alt text]({{ '/styles/images/2017_11/win_volume6.jpg' | prepend: site.baseurl }} "管理磁盘")

4.格式化分区

![Alt text]({{ '/styles/images/2017_11/win_volume7.jpg' | prepend: site.baseurl }} "管理磁盘")

![Alt text]({{ '/styles/images/2017_11/win_volume8.jpg' | prepend: site.baseurl}} "管理磁盘")

![Alt text]({{ '/styles/images/2017_11/win_volume9.jpg' | prepend: site.baseurl}} "管理磁盘")

![Alt text]({{ '/styles/images/2017_11/win_volume10.jpg' | prepend: site.baseurl}} "管理磁盘")


### linux



