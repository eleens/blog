---
layout: post
title: 笔记
data: 2017-05-27 13:12:01 +0800
categories: document
tag: linux
---

* content
{:toc}

### 文件权限
ls -al

    [root@vs-controller ~]# ls -al
    total 14810992
    dr-xr-x---. 12 root root       4096 May 27 09:56 .
    dr-xr-xr-x. 18 root root       4096 May 16 12:48 ..

dr-xr-xr-x  总共10个字符
* 1 代表文件类型
* 234 文件的所有的权限
* 567 文件所属用户组的权限
* 890 其他人对此文件的权限

文件类型
+ [d] ----表示目录
+ [-] ----表示文件
+ [l] ----表示链接文件
+ [b] ----表示设备文件里面的可供存储的接口设备
+ [c] ----表示设备文件里面的串行端口设备，例如键盘、鼠标（一次性读取设备）

权限

- [r] ----4-------可读（read）
- [w] ----2-------可写（write）
- [x] ----1-------可执行（execute）
- [-] ----0-------无权限

修改权限的命令

* chgrp:改变文件所属用户组

        chgtp [-R] group dirname/filename ...
        # -R 表示递归，即同子目录下的所有文件都会更改
        # group必须是/etc/group中存在的才行，否则会报错

* chown:改变文件所有者

        chown [-R] user[:group] dirname/filename

* chmod:改变文件的权限

        chmod [-R] xyz dirname/filename
        # xyz 代表owner group others 的数字类型的权限  比如775（4+2+1 4+2+1 4+1）

        chmod [-R] u=rwx,g=rw,o=x dirname/filename
        chmod [-R] u=rwx,go=rw dirname/filename

        # u----表示user
        # g------表示group
        # o------表示others
        # a--------表示全部
        # +------加上
        # - -----减去
        # = -------设置
