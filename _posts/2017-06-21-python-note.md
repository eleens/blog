---
layout: post
title:  python学习
date:   2017-06-21 16:12:01 +0800
categories: document
tag: python
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

