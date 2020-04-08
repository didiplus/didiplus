---
title: 'Docker实战:基于centos7镜像创建ssh容器'
tags:
  - Docker
categories: Docker
keywords: Docker Dockerfile创建ssh容器
description: Dockerfile创建ssh容器
cover: 'https://tva1.sinaimg.cn/large/9fc55f55ly1gcet3611zgj209c07zt96.jpg'
abbrlink: 77f24681
date: 2020-03-01 20:51:12
top_img:
copyright:
---

有些时候需要多台机器去测试脚本，利用传统的虚拟机技术，不仅开销大，而且一台物理机虚拟出的机器是有限的。采用Docker技术不仅开销小，而且虚拟出的主机比利用虚拟机虚拟的更多。

今天通过两种为容器添加SSH服务并保存为镜像的方式。

### docker commit

```commit```命令，支持用户提交自己对容器的修改，从而生成一个新的镜像。

#### 1、启动基础镜像

``` shell
[root@izt4nh30l604g7q40vzsglz /]# docker run -it centos:7
```

> -i   以交互模式运行容器，通常与 -t 同时使用；
>
> -t  为容器重新分配一个伪输入终端，通常与 -i 同时使用；

#### 2、容器中安装openssh-server net-tools

```shell
[root@462b591af4a6 /]# yum install -y openssh-server net-tools
```

#### 3、修改root用户密码

```shell
[root@462b591af4a6 /]# echo '123123456' | passwd --stdin root
```

#### 4、生成密钥

```
[root@462b591af4a6 /]# ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
[root@462b591af4a6 /]# ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key
[root@462b591af4a6 /]# ssh-keygen -t rsa -f /etc/ssh/ssh_host_ed25519_key
```

#### 7、编写启动脚本

```shell
[root@462b591af4a6 /]# vi /run.sh
[root@462b591af4a6 /]# chmod +x /run.sh 
```

其中 run.sh的内容为

```shell
#!/bin/bash
/usr/sbin/sshd -D
```

#### 8、退出容器，保存镜像

```shell
[root@462b591af4a6 /]# exit
[root@462b591af4a6 /]# docker ps -al 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
46d6949f23b3        centos              "/bin/bash"         11 minutes ago      Exited (0) 47 seconds ago                       practical_kilby

[root@462b591af4a6 /]#docker commit -m 'openssh-server'  46d6949f23b3  sshd:centos7
sha256:fa665548b8186c9b656a145ff9beaae1847d183dd405eba25888066e85ca10fc
```

#### 9、启动容器

```shell
[root@izt4nh30l604g7q40vzsglz ~]#  docker run -d --name ssh -p 10022:22 sshd:centos /run.sh
334c4330a56bfe5d9e87c35747ef604da60c5aa84fdae427ca30bbdab2592d37
[root@izt4nh30l604g7q40vzsglz ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                   NAMES
334c4330a56b        sshd:centos7         "/run.sh"           55 seconds ago      Up 54 seconds       0.0.0.0:10022->22/tcp   ssh
```

#### 10、远程连接测试

```shell
[root@izt4nh30l604g7q40vzsglz ~]# ssh root@192.168.0.3 -p 10022
The authenticity of host '[192.168.0.3]:10022 ([192.168.0.3]:10022)' can't be established.
RSA key fingerprint is e1:95:09:40:48:8e:13:94:ca:73:15:e7:7b:37:2d:6c.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[192.168.0.3]:10022' (RSA) to the list of known hosts.
root@192.168.0.3's password: 
[root@334c4330a56b ~]# logout
Connection to 192.168.0.3 closed.
```



### Dockerfile

使用`docker commit` 手动构建一个新的镜像，虽然步骤清晰，但是镜像分发起来比较不方便。Dockerfile 就是最优替代方案。

#### 1、创建Dockerfile文件。

```shell
[root@izt4nh30l604g7q40vzsglz home]# cat Dockerfile 
FROM centos:7
MAINTAINER 972479352@qq.com
ENV ROOTPASSWORD  123456
RUN yum install -y openssh-server net-tools\
    && ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key \
    && ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key \
    && ssh-keygen -t rsa -f /etc/ssh/ssh_host_ed25519_key \
    && echo $ROOTPASSWORD | passwd --stdin root 
EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
```

#### 2、构建镜像文件

```shell
[root@izt4nh30l604g7q40vzsglz home]# docker build -t sshd:dockerfile .
[root@izt4nh30l604g7q40vzsglz home]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
sshd                dockerfile          c619f06bde24        3 minutes ago       313MB
sshd                centos7             2f4be89a1626        5 hours ago         313MB
centos              7                   5e35e350aded        3 months ago        203MB
mysql               5.7.17              9546ca122d3a        2 years ago         407MB
[root@izt4nh30l604g7q40vzsglz home]# 
```

#### 3、启动容器

```shell
[root@izt4nh30l604g7q40vzsglz home]# docker run  -d  -e ROOTPASSWORD='09876543' -p 10023:22 sshd:dockerfile
```

> -e 为环境变量赋值
>
> -d 后台运行容器，并返回容器ID；
>
> -p 指定端口映射，格式为：主机(宿主)端口:容器端口



#### Dockerfile遇到的问题

【Q1】没有解决在启动容器时通过环境变量赋值的方式，动态设置root密码



