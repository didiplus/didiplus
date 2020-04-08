---
title: OSi模型与TCP/IP模型
tags:
  - 数通
categories: 数通
keywords: OSI模型TCP/IP模型
description: OSI模型TCP/IP模型
cover: 'https://tvax2.sinaimg.cn/large/9fc55f55gy1gcjcswe25aj20ra0esabi.jpg'
abbrlink: '96494195'
date: 2020-03-05 21:35:46
top_img:
copyright:
---

> OSI模型和TCP/IP模型几乎应该说是网络通信领域中使用频率最高的两个术语。特别是TCP/IP，它几乎成了网络通信的代名词。从整体上先大致了解一下OSI模型和TCP/IP模型，对于深入学习各种网络通信知识，具有非常重要的指导性作用

# OSI模型

OSI（Open System Interconnect），即开放式系统互联。 一般都叫OSI参考模型，是ISO（国际标准化组织）组织在1985年研究的网络互连模型。

**OSI是一个7层功能协议模型，分别是物理层、数据链路层、网络层、传输层、会话层、表示层、应用层。**

1. **物理层：实现物理信号的发送、接收，以及该介质的传输过程。**
2. **数据链路层：建立逻辑意义上的数据链路，实现数据的点到点或点到多点方式直接通信。**
3. **网络层：实现数据从任何一个节点到任何另外一个节点的整个传输过程**
4. **传输层：建立、维护和取消一次端到端的数据传输过程，控制传输节奏的快慢，调整数据的排序**
5. **会话层：在通信双方之间建立、管理和终止会话，确定双方是否应该开始进行某一方发起的通信。**
6. **表示层:进行数据格式的转换，以确保一个系统生成的应用数据能够被另一个系统的应用层所识别和理解。**
7. **应用层：向用户应用软件提供丰富的系统应用接口**

![img](https://tvax2.sinaimg.cn/large/9fc55f55gy1gcjcswe25aj20ra0esabi.jpg)



从OSI模型的观点来看，计算机发送数据时，数据会从高层向底层逐层传递，在传递过程中进行相应的封装，并最终通过物理层转换为光/电信号发送出去。计算机接收到数据时，数据会从底层向高层传递，在传递过程中进行相应的解封装。

![img](http://p9.pstatp.com/large/pgc-image/1b6183ddc63047459ac1824d8d7a495b)



 

# TCP/IP协议簇

很多初学者误认为TCP/IP就是一种协议，其实它包含了很多的协议，只是TCP/IP是这个协议簇中的两个非常重要的协议，分别是TCP和IP。

下图给出了TCP/IP模型有两个不同的版本，以及它们与OSI模型的比较。

![img](http://p1.pstatp.com/large/pgc-image/bc53b3b74dc94cb39edbb2d617fe2a9b)



TCP/IP模型与OSI模型再层次的划分上稍有差异，单这种层次划分的差异并不是二者之间的主要差异。TCP/IP模型与OSI模型的主要差异在于二者所使用的的具体协议不同。下图列出TCP/IP模型与OSI模型各自所使用的部分协议。

![img](http://p1.pstatp.com/large/pgc-image/cc4620c319c8494dbbbaeb10d51b08e0)