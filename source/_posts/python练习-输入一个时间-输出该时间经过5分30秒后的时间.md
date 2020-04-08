---
title: 'python练习:输入一个时间,输出该时间经过5分30秒后的时间'
tags:
  - python
categories: python
keywords: 'python小案例，输入一个时间,输出该时间经过5分30秒后的时间'
description: 'python小案例，输入一个时间,输出该时间经过5分30秒后的时间'
cover: 'https://tva3.sinaimg.cn/large/9fc55f55gy1gd3q6jfx53j20sf0iywwf.jpg'
abbrlink: f56acc9
date: 2020-03-23 11:46:34
top_img:
copyright:
---

题目：输入一个时间(时:分:秒),输出该时间经过5分30秒后的时间

思考：

- 判断输入的时间格式是否正确。
- 秒满60，分加1.秒归零。分满60，时加1，分归零。时满24，时归零

详细代码如下

```python
input_time="24:58:59"
time_list = input_time.split(':')
time_data = [int(x) for x in time_list]

H =  time_data[0]
M =  time_data[1]
S = time_data[2]

if ((H >= 0) and (H <24) ) and ((M>= 0) and (M <=60)) and ((S>= 0) and (S <=60)):
	S +=30
	if S >= 60:
		S = S-60
		M+=1

	M+=5
	if M >= 60:
		M = M-60
		H+=1

	if H ==24:
		H = 0
	print ('next time:%02d:%02d:%02d'%(H,M,S))
else:
	print ('你输入的时间格式有问题，请重新输入！')


```



