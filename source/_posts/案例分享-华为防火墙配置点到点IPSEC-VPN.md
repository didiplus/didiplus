---
title: '案例分享:华为防火墙配置点到点IPSEC VPN'
tags:
  - IPSec VPN
categories: 数通
keywords: ':华为防火墙 IPSEC VPN 点到点'
description: 华为防火墙 IPSEC VPN 点到点
cover: 'https://tva2.sinaimg.cn/large/9fc55f55gy1gd9jjzqibfj20sg0lc4qp.jpg'
abbrlink: f1eed45f
date: 2020-03-28 12:37:57
top_img:
copyright:
---

# 实验目的
本文通过一个案例简单来了解一下Site-to-Site IPSec VPN 的工作原理及详细配置。
# 组网设备
USG6630防火墙两台，AR2220路由一台，PC机两台
# 实验拓扑
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328121820384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)
# 实验步骤
## 步骤1：FW1 和 FW2 配置接口 IP 地址和安全区域，完成网络基本参数配置 。
1、配置FW1接口IP地址，并且把GE1/0/1加入到untrust区域，GE1/0/6加入到trust。
```shell
[FW1] interface GigabitEthernet 1/0/1
[FW1-GigabitEthernet1/0/1] ip address 202.1.1.1 255.255.255.0
[FW1-GigabitEthernet1/0/1] service-manage ping permit
[FW1] interface GigabitEthernet 1/0/6
[FW1-GigabitEthernet1/0/6] ip address 10.91.74.254 255.255.255.0
[FW1-GigabitEthernet1/0/6] service-manage ping permit
[FW1] firewall zone untrust
[FW1-zone-untrust] add interface GigabitEthernet 1/0/1
[FW1] firewall zone trust
[FW1-zone-trust] add interface GigabitEthernet 1/0/6
```
2、配置 FW2 接口 IP 地址，并且 GE1/0/2 加入 Untrust 区域，GE1/0/6 加入Trust 区域。
```shell
[FW2] interface GigabitEthernet 1/0/1
[FW2-GigabitEthernet1/0/1] ip address 202.1.1.1 255.255.255.0
[FW2-GigabitEthernet1/0/1] service-manage ping permit
[FW2] interface GigabitEthernet 1/0/6
[FW2-GigabitEthernet1/0/6] ip address 10.91.74.254 255.255.255.0
[FW2-GigabitEthernet1/0/6] service-manage ping permit
[FW2] firewall zone untrust
[FW2-zone-untrust] add interface GigabitEthernet 1/0/1
[FW2] firewall zone trust
[FW2-zone-trust] add interface GigabitEthernet 1/0/6
```
## 步骤2：FW1 和FW2 配置到达对端的路由。
配置FW1和FW2的默认路由。
```shell
[FW1] ip route-static 0.0.0.0 0.0.0.0 202.1.2.1
[FW2] ip route-static 0.0.0.0 0.0.0.0 202.1.1.1 
```
## 配置 FW1和FW2 的 IPSec 隧道。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328122321409.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)1）、定义需要保护的数据流
```shell
[FW1]acl number 3000
[FW1-acl-adv-3000] rule 5 permit ip source 10.91.74.0 0.0.0.255 destination 10.91.65.0 0.0.0.255
```
2）、配置 IKE 安全提议
```shell
[FW1]ike proposal 1 #采用默认配置，
[FW1-ike-proposal-1] display this #查看默认的加密算法
```

3）、配置 IKE 对等体
```shell
[FW1]ike peer FW1
[FW1-ike-peer-FW1]exchange-mode auto
[FW1-ike-peer-FW1]pre-shared-key  huawei@123 #定义域共享密钥，两边要一样的
[FW1-ike-peer-FW1]ike-proposal 1  #关联IKE安全提议
[FW1-ike-peer-FW1]remote-address 202.1.2.1 #配置对端全局地址
```
4）、配置 IPSec 安全提议
```shell
[FW1]ipsec proposal 10 #采用默认值
```
5）、配置 IPSec 策略
```shell
[FW1]ipsec policy FW1 1 isakmp 
[FW1-ipsec-policy-isakmp-FW1-1]security acl 3000
[FW1-ipsec-policy-isakmp-FW1-1] proposal 10
[FW1-ipsec-policy-isakmp-FW1-1] ike-peer FW1
```
6）、应用 IPSec 安全策略到出接口
```shell
[FW1]interface GigabitEthernet 1/0/1
[FW1-GigabitEthernet1/0/1]ipsec policy  FW1
```
>防火墙FW2也是按照上面的步骤配置，中间只需要修改一下IKE 对等体的`remote-address IP地址`。

## 步骤4：FW1和FW2 配置安全策略，允许私网指定网段进行报文交互 。
1）、配置从 Trust 到 Untrust 的域间策略
```shell
防火墙FW1
[FW1-policy-security]rule name trust_untrust
[FW1-policy-security-rule-trust_untrust]source-zone trust 
[FW1-policy-security-rule-trust_untrust]destination-zone untrust 
[FW1-policy-security-rule-trust_untrust]source-address 10.91.74.0 24
[FW1-policy-security-rule-trust_untrust]destination-address 10.91.65.0 24
[FW1-policy-security-rule-trust_untrust]action permit

防火墙FW2
[FW2-policy-security]rule name trust_untrust
[FW2-policy-security-rule-trust_untrust]source-zone trust
[FW2-policy-security-rule-trust_untrust]destination-zone untrust
[FW2-policy-security-rule-trust_untrust]source-address 10.91.65.0 24
[FW2-policy-security-rule-trust_untrust]destination-address 10.91.74.0 24
```
2)、配置从 Untrust 到Trust 的域间策略
```shell
防火墙FW1
[FW1-policy-security]rule name untrust_trust
[FW1-policy-security-rule-untrust_trust]source-zone untrust 
[FW1-policy-security-rule-untrust_trust]destination-zone trust 
[FW1-policy-security-rule-untrust_trust]source-address 10.91.65.0 24
[FW1-policy-security-rule-untrust_trust]destination-address 10.91.74.0 24
[FW1-policy-security-rule-untrust_trust]action permit

防火墙FW2
[FW2-policy-security]rule name untrust_trust
[FW2-policy-security-rule-untrust_trust]source-zone untrust 
[FW2-policy-security-rule-untrust_trust]destination-zone trust 
[FW2-policy-security-rule-untrust_trust]source-address 10.91.74.0 24
[FW2-policy-security-rule-untrust_trust]destination-address 10.91.65.0 24
[FW2-policy-security-rule-untrust_trust]action permit 
```
到此两台防火墙已经放通了从Trust 到 Untrust和从Untrust到 trust的域间策略，现在从PC1发ping包看看吧。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032812285657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)
从上图看到PC1和PC2是不能连通的，这是为啥呢？首先，在FW1防护墙上看看会话表，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328122913397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)
从上图看到FW1防火墙上出现了会话表，但是，数据包只有出去方向的，回来的数据包却没有。我们再到FW2看看是否有会话表项。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328122927343.png)
从上图可以看到FW2上没有发现会话表项。说明数据包没有到达FW2上。这是为什么呢？
**因为我们只放通了业务上策略，而没有放通ipsec VPN协商所需要的策略。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328123000674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)从上图看到IPsec VPN没有协商成功，导致数据包没有发送到FW2上。

## 步骤5:配置IPsec VPN 的域间策略
1)、定义服务集
```shell
[FW1]ip service-set  ike type object
[FW1-object-service-set-ike]service 0 protocol udp destination-port 500
[FW2]ip service-set  ike type object
[FW2-object-service-set-ike]service 0 protocol udp destination-port 500
```
2)、在防火墙上放通策略
```shell
[FW1-policy-security]rule name ike_out
[FW1-policy-security-rule-ike_out] source-zone local
[FW1-policy-security-rule-ike_out] destination-zone  untrust
[FW1-policy-security-rule-ike_out] source-address 202.1.1.1 mask 255.255.255.255
[FW1-policy-security-rule-ike_out] destination-address 202.1.2.1 mask 255.255.255.255
[FW1-policy-security-rule-ike_out]service ike
[FW1-policy-security-rule-ike_out]service esp
[FW1-policy-security-rule-ike_out] action permit

[FW1-policy-security]rule name ike_in
[FW1-policy-security-rule-ike_in] source-zone untrust
[FW1-policy-security-rule-ike_in] destination-zone local
[FW1-policy-security-rule-ike_in] source-address 202.1.2.1 mask 255.255.255.255
[FW1-policy-security-rule-ike_in] destination-address 202.1.1.1 mask 255.255.255.255
[FW1-policy-security-rule-ike_out]service ike
[FW1-policy-security-rule-ike_out]service esp
[FW1-policy-security-rule-ike_in] action permit

[FW2-policy-security] rule name ike_out
[FW2-policy-security-rule-ike_out]source-zone local
[FW2-policy-security-rule-ike_out]source-zone untrust
[FW2-policy-security-rule-ike_out] destination-address 202.1.1.1 mask 255.255.255.255
[FW2-policy-security-rule-ike_out] source-address 202.1.2.1 mask 255.255.255.255
[FW2-policy-security-rule-ike_out]service ike
[FW2-policy-security-rule-ike_out]service esp
[FW2-policy-security-rule-ike_out] action permit

[FW2-policy-security]rule name ike_in
[FW2-policy-security-rule-ike_in] source-zone untrust
[FW2-policy-security-rule-ike_in] destination-zone local
[FW2-policy-security-rule-ike_in] source-address 202.1.1.1 mask 255.255.255.255
[FW2-policy-security-rule-ike_in] destination-address 202.1.2.1 mask 255.255.255.255
[FW2-policy-security-rule-ike_out]service ike
[FW2-policy-security-rule-ike_out]service esp
[FW2-policy-security-rule-ike_in] action permit
```
到此，所有需要的策略都放通了，接着来验证一下吧，首先还是从PC1发起PING包，接着，在FW1查看会话表项，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032812323797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)
从上图可以看到，FW1上的会话表项数据包已经出现有去有回的，再看看IKE协商情况吧，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328123251587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)从上图可以看到，FW1上的IKE已经协商成功了。再看看FW2防火墙上的会话表项吧，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328123303501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)
从上图可看到，FW2上的会话表项也出现了PING包，证明了PC1到PC2已经连通了，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200328123316242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzI4NTIxNDg3,size_16,color_FFFFFF,t_70)
更多精彩内容请关注我的[头条号](https://www.toutiao.com/c/user/68783357974/#mid=1609422238702596)