---
layout: post
title:  python练习题
date:   2017-06-21 16:12:01 +0800
categories: document
tag: python
---

* content
{:toc}

**1.根据单词长度给一个列表排序**

    >>> fruits = ['strawberry', 'fig', 'apple', 'cherry', 'raspberry', 'banana']
    >>> sorted(fruits, key=len)
    ['fig', 'apple', 'cherry', 'banana', 'raspberry', 'strawberry']

**2.根据反向拼写给一个单词列表排序**

    >>> def reverse(word):
    ...     return word[::-1]
    ...
    >>> reverse('testing')
    'gnitset'
    >>> sorted(fruits,key=reverse)
    ['banana', 'apple', 'fig', 'raspberry', 'strawberry', 'cherry']
    #或者
    >>> sorted(fruits, key = lambda n: n[::-1])
    ['banana', 'apple', 'fig', 'raspberry', 'strawberry', 'cherry']


**3.计算阶乘列表:map和filter与列表推导的比较**

    >>> map_list = list(map(fact,range(6)))  #map方式
    >>> map_list
    [1, 1, 2, 6, 24, 120]
    >>> list_list = [fact(n) for n in range(6)]
    >>> list_list
    [1, 1, 2, 6, 24, 120]
    >>> map_filter = list(map(fact, filter(lambda n: n % 2,range(6))))  #map+filter方式
    >>> map_filter
    [1, 6, 120]
    >>> list_filter = [fact(n) for n in range(6) if n % 2]
    >>> list_filter
    [1, 6, 120]
    # 可用列表推导替代map和filter

**4.使用reduce和sum计算0~99之和**

    >>> from operator import add
    >>> reduce(add, range(4))
    6
    >>> reduce(add, range(100))
    4950
    >>> sum(range(100))
    4950
    ##自己写的两个数字相加的方法
    >>> def my_add(d1,d2):
    ...     return d1 + d2
    ...
    >>> reduce(my_add, range(100))




题目1：有1、2、3、4个数字，能组成多少个互不相同且无重复数字的三位数？都是多少？

题目2：企业发放的奖金根据利润提成。利润(I)低于或等于10万元时，奖金可提10%；利润高于10万元，低于20万元时，低于10万元的部分按10%提成，高于10万元的部分，可可提成7.5%；20万到40万之间时，高于20万元的部分，可提成5%；40万到60万之间时高于40万元的部分，可提成3%；60万到100万之间时，高于60万元的部分，可提成1.5%，高于100万元时，超过100万元的部分按1%提成，从键盘输入当月利润I，求应发放奖金总数？

1.斐波契纳数列1,2,3,5,8,13,21............根据这样的规律，求出400万以内最大的斐波契纳数,并求出它是第几个斐波契纳数？
2.一句话求素数？
3.自己写一个.txt文件，在里面写多行数字，每行100位数字（每行一个数，这个数有100位），将每行的100位数相加，取最后结果的前十位输出来.
4.公式Pn=n(3n-1)/2,符合此公式的数叫做五角数，找出两个五角数，这两个五角数符合(相加为五角数，相减也为五角数)

5.100元能换成1元，5元，10元。求一共有多少种换法？
6.写一个python脚本，运行这个脚本能够实现同时创建n个文件
7.找出一个目录下所有文件中包含abc的行
8.求100以内所有数的平方和跟100以内所有数的和的平方的差的绝对值？

9.自己写一个.txt文件，里面写多个英文人名(不区分大小写)将这些人名排序。


10.要求算任意字符串中不同的字符以及它的个数？

