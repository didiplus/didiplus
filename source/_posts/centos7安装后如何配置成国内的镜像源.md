---
title: centos7安装后如何配置成国内的镜像源
tags:
  - centos7
categories: linux
keywords: centos7配置国内yum源
description: centos7配置国内yum源
cover: 'https://tva2.sinaimg.cn/large/9fc55f55gy1gdj61d71khj20xc0ozu08.jpg'
abbrlink: 1660888c
date: 2020-04-05 20:26:51
top_img:
copyright:
---

> centos 7 安装完后采用的是国外的镜像仓库，在下载或者更新软件相对慢，所以，在安装完centos 7 系统后，要把镜像源更换成国内的，相对比较好的。

国内比较好的镜像源有：

 1. 阿里云（http://mirrors.aliyun.com/repo/Centos-7.repo）
 2. 网易     (http://mirrors.163.com/.help/CentOS6-Base-163.repo)
 3. 华为（https://repo.huaweicloud.com/repository/conf/CentOS-7-reg.repo ）

我这里选择华为镜像源，作为例子，其他类型的。
使用说明：
1、备份配置文件：
```shell
cp -a /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```
2、下载新的CentOS-Base.repo文件到/etc/yum.repos.d/目录下
```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo https://repo.huaweicloud.com/repository/conf/CentOS-7-reg.repo
```
3、执行yum clean all清除原有yum缓存
```shell
yum clean all
```
4、执行yum makecache（刷新缓存）或者yum repolist all（查看所有配置可以使用的文件，会自动刷新缓存）。
```shell
yum makecache // yum repolist all
```

相关网址:

[CentOS官方地址](http://www.centos.org/)：
[CentOS邮件列表地址](http://www.centos.org/modules/tinycontent/index.php?id=16)
[CentOS论坛地址](http://www.centos.org/modules/newbb/)
[CentOS文档地址](http://www.centos.org/docs/)
[CentOS Wiki地址](http://wiki.centos.org/)