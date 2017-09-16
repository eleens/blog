---
layout: post
title:  python学习
date:   2017-06-21 16:12:01 +0800
categories: python
tag: 笔记
---

* content
{:toc}

cmd 命令
chcp  活动代码页

高阶函数
接收函数为参数，或者把函数作为结果返回的函数是高阶函数

### 归约函数

sum()  all()  any()  这些都是内置的归约函数，

+ sum()求序列之和，

+ all()序列全部为True才会返回True,否则返回False，

+ any()序列中只要有一个是True就返回True，否则返回False。

注意：all([])返回True，any([])返回False

    >>> all(range(4))
    False
    >>> all(range(1,4))
    True
    >>> all(range(0))
    True

    >>> any(range(4))
    True
    >>> any(range(1,4))
    True
    >>> any(range(0))
    False
    >>> range(0)
    []
    >>> sum(range(100))
    4950

### 可调用对象

使用callable()函数查看是否是可被调用的对象

7种可调用对象

1. 用户自定义的函数
2. 内置函数
3. 内置方法，如dict.get()
4. 方法  在类里面定义的函数
5. 类
6. 类的实例
7. 生成器函数

random

    >>> import random
    >>> random.shuffle(rr)  ##把列表顺序打乱
    >>> random.random()  在[0,1)之间去随机数
    0.8762703186526293

python 2.x默认是ascii码的,所以输出中文需要在前面添上以下代码：

    >>> # -*-coding:UTF-8 -*-
    #或者
    >>> #coding=utf-8

以单下划线开头_foo的代表不能直接访问的类属性，需要通过类提供的接口进行访问，不能直接import导入
以双下划线开肉的__foo代表类的私有成员
以双下划线开头和结尾的__foo__代表python里特殊方法专用的标识

repr(x) 将对象x转换成表达式字符串
str(x) 将对象转换成字符串
两者的区别？

 id() 函数用于获取对象内存地址