---
layout: post
title:  如何使用github搭建个人博客
date:   2017-05-24 11:56:01 +0800
categories: document
tag: 其他
---

* content
{:toc}

    It does not do dwell dreams and forget to live.

                      ——————— 不要依赖于梦想而忘记了生活。

---------------------------------------

之前一直想要一个拥有自己的个人博客，后来发现github不仅可以用作自己的代码库，还可以利用gitpages搭建个人博客，这里我简单的介绍如何构建一个个人的博客。


github账号
-------------

使用github搭建个人博客，首先得需要一个github的账号，当然，如果你有github的账号，那可以直接跳过这一步了。

1、输入以下网址,进入注册界面，如果有账号，直接点击sign in登录即可

>[https://github.com/](https://github.com/)

2、进入下面界面，填写用户信息，注册账号

第一步

![注册界面]({{ '/styles/images/doc_img/github_sign_up1.png' | prepend: site.baseurl  }} "Sign Up")

第二步 选择免费的

![注册界面]({{ '/styles/images/doc_img/github_sign_up2.png' | prepend: site.baseurl  }} "Sign Up")

第三步

![注册界面]({{ '/styles/images/doc_img/github_sign_up3.png' | prepend: site.baseurl  }} "Sign Up")


第四步  到这里就注册成功了

![成功界面]({{ '/styles/images/doc_img/github_sign_up4.png' | prepend: site.baseurl  }} "Sign Up")


第五步  注册成功之后需要去邮箱激活，才可以使用

![激活界面]({{ '/styles/images/doc_img/github_sign_up5.png' | prepend: site.baseurl  }} "Sign Up")


创建blog项目
-------------
1、进入登录界面，登录

![登录界面]({{ '/styles/images/doc_img/github_sign_in.png' | prepend: site.baseurl  }} "Sign Ip")

2、创建blog项目

![创建blog]({{ '/styles/images/doc_img/blog2.png' | prepend: site.baseurl  }} "Blog")

3、创建成功，看到如下界面，把blog项目pull到本地，就可以开始写blog的代码了，github可以把你的静态代码运行起来成一个静态的网站。

![blog]({{ '/styles/images/doc_img/blog3.png' | prepend: site.baseurl  }} "Blog")

4、这里介绍的是基于jekyll写的blog，可以直接去网上寻找blog主题，也可以自己写。

>[http://jekyllthemes.org/](http://jekyllthemes.org/)

5、我这里是直接用的主题，拉下来的代码结构如下

![代码结构]({{ '/styles/images/doc_img/blog1.png' | prepend: site.baseurl  }} "Blog")

_config.yml

这是针对 Jekyll 的配置文件

_layouts

这个目录存放着一些网页模板文件，为网站所有网页提供一个基本模板

_includes

存放的也是一些模板文件，供_layouts下面的模板引用

_posts

存放的就是你要写的文章，扩展名是.md，是用markdown语法写的，关于markdown语法，上一篇有介绍

_sass

存放的是一些样式文件

_site

是服务运行时候自动生成的文件夹，里面就是jekyll服务转换之后的html结构的文件


6、如果需要让服务在本地跑起来，需要在本地安装ruby和gem-jekyll
+ 安装ruby，下载ruby的安装包，安装，在doc窗口输入ruby -v,如显示版本信息，则表示安装成功

        C:\Users\yutingting>ruby -v
        ruby 2.3.1p112 (2016-04-26 revision 54768) [x64-mingw32]

+ 安装devkit

+ 安装jekyll

        gem install jekyll

+ 安装成功之后，进入目录，运行jekyll server，即可在本地http://127.0.0.1:4000/blog/ 看到你自己的blog

        jekyll server


7、把代码push到github上，在master分支或者gh-pages分支都可以，然后在setting中配置，你的域名链接的源码的分支

![setting]({{ '/styles/images/doc_img/blog4.png' | prepend: site.baseurl  }} "setting")

8、输入你的域名就可以看到自己的blog了

![blog]({{ '/styles/images/doc_img/blog5.png' | prepend: site.baseurl  }} "blog")
