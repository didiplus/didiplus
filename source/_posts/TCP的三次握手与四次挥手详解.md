---
title: TCP的三次握手与四次挥手详解
tags:
  - 数通
categories: 数通
keywords: TCP三次握手 四次挥手
description: 详解TCP三次握手与四次挥手
cover: 'https://tvax4.sinaimg.cn/large/9fc55f55gy1gcdlwfmyjvj20n50f8gn4.jpg'
abbrlink: 2c9e1e4
date: 2020-03-02 09:50:37
top_img:
copyright:
---

在了解三次握手和四次挥手之前，先要知道TCP报文内部包含了那些东西。熟悉了解TCP报文对日后学习网络和排除方面有很大的帮助，所以，今天为了加深对三次握手的理解，从新去认识TCP报文格式。

![TCP报文格式](https://tvax4.sinaimg.cn/large/9fc55f55gy1gcdlwfmyjvj20n50f8gn4.jpg)

### TCP报文详解

TCP报文由首部和数据两部分组成。首部一般由**20-60字节(Byte)，**长度可变。其中前20B格式固定，后40B为可选。

因为，TCP报文还得传给下层网络层，封装成IP包，而一个IP包最大长度为65535，同时IP包首部也包含最少20B，所以一个IP包或TCP包可以包含的数据部分最大长度为65535-20-20=65495B。

> 1、源端口号(Source Port)
>
> ​		长度为16位，指明发送数据的进程
>
> 2、目的端口(Destination Port)
>
> ​		长度为16位，指明目的主机接收数据的进程
>
> 3、序号（Sequence Number）
>
> ​		也称为序列号，长度为32位，序号用来标识从TCP发送端向接入端发送的数据字节流进行编号，可以理解成对字节流的计数。
>
> 4、确认号
>
> ​		长度为32位，确认号包含发送确认的一端所期望收到的下一个序号。确认号只有在ACK标志为1时才有效。
>
> 5、首部长度
>
> ​		长度为4位，用于表示TCP报文首部的长度。用4位（bit）表示，十进制值就是[0,15]，一个TCP报文前20个字节是必有的，后40个字节根据情况可能有可能没有。如果TCP报文首部是20个字节，则该位应是20/4=5
>
> 6、保留位(Reserved)
>
> ​		长度为6位，必须是0，它是为将来定义新用途保留的。
>
> 7、标志(Code Bits）
>
> ​		长度为6位，在TCP报文中不管是握手还是挥手还是传数据等，这6位标志都很重要。6位从左到右依次为：
>
> ​		URG：紧急标志位，说明紧急指针有效；
>
> ​		ACK：确认标志位，多数情况下空，说明确认序号有效
>
> ​		PSH：推标志位，置位时表示接收方应立即请求将报文交给应用层
>
> ​		RST：复位标志，用于重建一个已经混乱的连接；
>
> ​		SYN：同步标志，该标志仅在三次握手建立TCP连接时有效
>
> ​		FIN：结束标志，带该标志位的数据包用于结束一个TCP会话。
>
> 8、窗口大小(window Size)
>
> ​		长度为16位，TCP流量控制由连接的每一端通过声明的窗口大小来提供。
>
> 9、检验和（Checksum）
>
> ​		长度为16位，该字段覆盖整个TCP报文端，是个强制性的字段，是由发送端计算和存储，到接收端后，由接收端进行验证。
>
> 10、紧急指针(Urgent Pointer)
>
> ​		长度为16位，指向数据中优先部分的最后一个字节，通知接收方紧急数据的长度，该字段在URG标志置位时有效。
>
> 11、选项（Options)
>
> ​		长度为0-40B（字节），必须以4B为单位变化，必要时可以填充0。
>
> 12、数据

在了解TCP报文格式，通过捕捉一个TCP报文直观感受一下。

![TCP报文](https://tva3.sinaimg.cn/large/9fc55f55ly1gcfco0s2poj20hs08j3z5.jpg)

### 三次握手

建立TCP连接时，需要客户端和服务器共发送3个包。

![三次握手](https://tva3.sinaimg.cn/large/9fc55f55ly1gcfcpd1s0hj20hs08cq38.jpg)

1、客户端主动打开，发送连接请求报文段，将SYN标识位置为1，Sequence Number置为x（TCP规定SYN=1时不能携带数据，x为随机产生的一个值），然后进入SYN_SEND状态。

2、服务器收到SYN报文段进行确认，将SYN标识位置为1，ACK置为1，Sequence Number置为y，Acknowledgment Number置为x+1，然后进入SYN_RECV状态，这个状态被称为半连接状态。

3、客户端再进行一次确认，将ACK置为1（此时不用SYN），Sequence Number置为x+1，Acknowledgment Number置为y+1发向服务器，最后客户端与服务器都进入ESTABLISHED状态

为了加深对TCP三次握手的理解，抓包看一下TCP三次握手的过程。

![三次握手](http://p1.pstatp.com/large/pgc-image/5c7b3a7f8a9d4edeaedb5a798b24b334)

从抓包结果看来，整个过程符合TCP三次握手的预期：

1. 客户端发送SYN给服务端
2. 服务端返回SYN+ACK给客户端
3. 客户端确认，返回ACK给服务端

### TCP四次挥手

TCP四次挥手则是TCP连接释放的过程**。**下面是TCP四次挥手的流程图：

![四次挥手](http://p1.pstatp.com/large/pgc-image/5f70d7d11ec1474395a58959fb9cbc92)



当客户端没有数据再需要发送给服务端时，就需要释放客户端的连接，这整个过程为：

1、客户端发送一个报文给服务端（没有数据），其中FIN设置为1，Sequence Number置为u，客户端进入FIN_WAIT_1状态。

2、服务端收到来自客户端的请求，发送一个ACK给客户端，Acknowledge置为u+1，同时发送Sequence Number为v，服务端年进入CLOSE_WAIT状态。

3、服务端发送一个FIN给客户端，ACK置为1，Sequence置为w，Acknowledge置为u+1，用来关闭服务端到客户端的数据传送，服务端进入LAST_ACK状态。

4、客户端收到FIN后，进入TIME_WAIT状态，接着发送一个ACK给服务端，Acknowledge置为w+1，Sequence Number置为u+1，最后客户端和服务端都进入CLOSED状态。

为了加深对TCP四次挥手的理解，抓包看一下TCP四次挥手的过程。

![加深理解TCP的三次握手与四次挥手](http://p9.pstatp.com/large/pgc-image/985ff495ae6e44d5b1f2db4ee7ed7f44)



### 为什么三次握手和四次挥手？

三次握手时，服务器同时把ACK和SYN放在一起发送到了客户端那里。

四次挥手时，当收到对方的 FIN 报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方是否现在关闭发送数据通道，需要上层应用来决定，因此，己方 ACK 和 FIN 一般都会分开发送。