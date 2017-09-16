---
layout: post
title:  用devstack安装openstack
date:   2017-08-09 16:12:01 +0800
categories: openstack
tag: 环境
---

* content
{:toc}
#### 1、创建stack用户

    useradd -s /bin/bash -d /opt/stack -m stack

#### 2、写文件

    echo "stack ALL=(ALL) NOPASSWD: ALL" | tee /etc/sudoers.d/s

#### 3、配置pip源和yum源

    mkdir .pip  （如果没有这个文件夹）
    vim /root/.pip/pip.conf
    [global]
    index-url = http://pypi.douban.com/simple
    trusted-host = pypi.douban.com
    disable-pip-version-check = true
    timeout = 120
    # yum源
    yum install wget  （如果没有wget）
    cd /etc/yum.repos.d
    sudo mv CentOS-Base.repo CentOS-Base.repo.bak
    sudo wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
    yum clean all
    yum makecache

#### 4、切换用户

    su - stack

#### 5、下载devstack

    sudo yum install git (如果没有git)
    git clone https://git.openstack.org/openstack-dev/devstack
    #切换到m版本
    cd devstack
    git checkout mitaka-eol  （可以用git branch -a  和git tag 查看所有的版本和tag）

#### 6、创建配置文件  local.conf

或者直接下载

    wget -O - https://raw.githubusercontent.com/shake/devstack/gh-pages/local.conf-sample-mitaka > ./local.conf

然后修改（修改后的见local.conf)

    [[local|localrc]]

    # use TryStack git mirror
    GIT_BASE=http://git.trystack.cn
    NOVNC_REPO=http://git.trystack.cn/kanaka/noVNC.git
    SPICE_REPO=http://git.trystack.cn/git/spice/spice-html5.git

    #OFFLINE=True
    RECLONE=True

    # Define images to be automatically downloaded during the DevStack built process.
    DOWNLOAD_DEFAULT_IMAGES=False
    IMAGE_URLS="http://download.cirros-cloud.net/0.3.4/cirros-0.3.4-x86_64-disk.img"

    HOST_IP=192.168.6.70


    # Credentials
    DATABASE_PASSWORD=pass
    ADMIN_PASSWORD=pass
    SERVICE_PASSWORD=pass
    SERVICE_TOKEN=pass
    RABBIT_PASSWORD=pass

    HORIZON_BRANCH=mitaka-eol
    KEYSTONE_BRANCH=mitaka-eol
    NOVA_BRANCH=mitaka-eol
    NEUTRON_BRANCH=mitaka-eol
    GLANCE_BRANCH=mitaka-eol
    CINDER_BRANCH=mitaka-eol


    #keystone
    KEYSTONE_TOKEN_FORMAT=UUID

    ##Heat
    #HEAT_BRANCH=mitaka-eol
    #enable_service h-eng h-api h-api-cfn h-api-cw



    ## Swift
    #SWIFT_BRANCH=mitaka-eol
    #ENABLED_SERVICES+=,s-proxy,s-object,s-container,s-account
    #SWIFT_REPLICAS=1
    #SWIFT_HASH=011688b44136573e209e


    # Enabling Neutron (network) Service
    disable_service n-net
    enable_service q-svc
    enable_service q-agt
    enable_service q-dhcp
    enable_service q-l3
    enable_service q-meta
    enable_service q-metering
    enable_service neutron

    ## Neutron options
    Q_USE_SECGROUP=True
    FLOATING_RANGE="192.168.6.0/24"
    FIXED_RANGE="10.0.0.0/24"
    Q_FLOATING_ALLOCATION_POOL=start=192.168.6.102,end=192.168.6.254
    PUBLIC_NETWORK_GATEWAY="192.168.6.1"
    Q_L3_ENABLED=True
    PUBLIC_INTERFACE=eth0
    Q_USE_PROVIDERNET_FOR_PUBLIC=True
    OVS_PHYSICAL_BRIDGE=br-ex
    PUBLIC_BRIDGE=br-ex
    OVS_BRIDGE_MAPPINGS=public:br-ex

    # #VLAN configuration.
    #Q_PLUGIN=ml2
    #ENABLE_TENANT_VLANS=True

    # Logging
    LOGFILE=/opt/stack/logs/stack.sh.log
    VERBOSE=True
    LOG_COLOR=True
    SCREEN_LOGDIR=/opt/stack/logs

    #Enable senlin
    #enable_plugin senlin http://git.trystack.cn/openstack/senlin
    #Enable senlin-dashboard
    #enable_plugin senlin-dashboard http://git.trystack.cn/openstack/senlin-dashboard

#### 7、安装

    ./stack.sh

安装过程

    1.下载并安装Openstack运行所需要的系统软件，大概包括一些python的组件、mysql、rabbitmq-server等

    2.下载openstack组件，包括nova 、keystone、glance、noVNC、horizon等

    3.下载并安装openstack源码所依赖的python库和框架

    4.安装openstack各组件

    5.启动各项服务

安装成功

#### 8、使用

进入控制台

    screen -x stack

启动控制台

    screen -c stack-screenrc

#### 9、问题

1）libvirt安装不上

解决：

修改/opt/stack/devstack/lib/nova_plugins/functions-libvirt的代码，注释这一行

![Alt text]({{ '/styles/images/doc_img/code1.png' | prepend: site.baseurl  }} "libvirt")

用root用户安装libvirt

    pip install 'libvirt-python===2.1.0'

重新运行

    ./stack.sh

3) devstack安装openstack之后无法进去网址
解决：

关掉防火墙

查看防火墙状态

    service iptables status
    1) 重启后生效
         开启： chkconfig iptables on
         关闭： chkconfig iptables off
     2) 即时生效，重启后失效
          开启： service iptables start
          关闭： service iptables stop