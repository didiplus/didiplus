---
title: Flask 项目目录结构设计
date: 2020-04-19 09:28:20
tags:
 - Flask
categories: Flask 
keywords: Flask,python 创建目录
description: Flask,python 创建目录
cover:
top_img:
copyright:
---

# 什么是Flask？

Flask是一个使用 [Python](https://baike.baidu.com/item/Python) 编写的轻量级 Web 应用框架。其 [WSGI](https://baike.baidu.com/item/WSGI) 工具箱采用 Werkzeug ，[模板引擎](https://baike.baidu.com/item/模板引擎/907667)则使用 Jinja2 。

# Flask 与django有啥区别？

django走的是大而全的路线，是重量型的框架，Flask是一轻量级的框架。

django事模块式的开发方式：

- 有完善的ORM模型，评价略高于sqlAlchemy，和模板引擎（强大程度略低于jinja）
- 非常适合企业级的开发（高效，稳定，）
- 开发文档比较完善

Flask走的是灵活多变的路线:

- 有各种第三方的插件支持，可拓展性强
- flask与关系型数据库的配合不弱于django，但与Nosql非关系型数据库的配合由于django

# 创建第一个Flask项目

> 新建python虚拟隔离环境，这样每个项目互不干扰。

```shell
$ virtualenv.exe flaskenv
```

> 安装Flask

```shell
$ pip install Flask
```

> 新建一个app.py文件

```shel
$ touch.exe app.py #@app.route('/')
```

>创建第一Flask项目

```shell
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'hello,wold!'

if __name__ == "__main__":
    app.run(host='0.0.0.0',port=5000,debug=True)
```

> 启动Flask项目

```shell
$ python app.py 
```

这样就启动了一个简单的Flask项目，通过浏览器访问本机5000端口，就会返回一个字符串`hello,wold`。是不是很简单呢。

但是，随着项目的功能越来越大，为了更加高效得管理，需要把优化项目结构的设计。

