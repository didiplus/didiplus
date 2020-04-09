---
title: Hexo集成Travis CI实现自动化部署更新
date: 2020-04-09 10:28:53
tags:
  - [Hexo,Travis CI]
categories: Hexo 
keywords: Hexo集成Travis CI实现自动化部署,更新文章
description: Hexo集成Travis CI实现自动化部署,更新文章
cover: https://tvax3.sinaimg.cn/large/9fc55f55gy1gdncdjdll8j20xc0dwwhf.jpg
top_img:
copyright:
---

> Hexo 是一个快速、简洁高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

可以通过github或gitee等仓库免费搭建一个属于自己的博客网站，你没有听错啊，完全是免费的。

# hexo 发布一篇博文步骤：

1、进入博客源码目录，执行如下命令：

```shell
hexo new [layout] <title>
```

> 新建一篇文章。如果没有设置 `layout` 的话，默认使用 [_config.yml](https://hexo.io/zh-cn/docs/configuration) 中`default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。

2、生成静态文件，执行如下命令

```shell
hexo generate
```

3、把静态文件推送到git仓库上

```shell
hexo deploy
```

从上面可以看到，在发布一篇博客需要经历三个步骤，才能把文章发布成功。是不是觉得很麻烦呢？今天采用git仓库和Travis CI实现自动化部署，把文章发布的工作交给Travis CI去完成，我们只专心创作就行啦。

# 什么是Travis CI

> Travis CI 是目前新兴的开源持续集成构建项目，它与jenkins，GO的很明显的特别在于采用yaml格式，同时他是在线的服务,不像jenkins需要你本地打架服务器，简洁清新独树一帜。目前大多数的github项目都已经移入到Travis CI的构建队列中。



# 准备工作

##  博客项目托管git仓库

把博客源码托管到Github上，建议把博客源码和静态文件托管到同一个仓库的不同分支上。例如，我在github上创建一个仓库`didiplus`，把博客源码托管到这个仓库的`blog-source`分支上，静态页面托管到这个仓库的`gh-pages`分支上。如下图：

![gh-pages](https://tvax4.sinaimg.cn/large/9fc55f55gy1gdnbqrml1lj20ud0oq75m.jpg)

![blog-sourece](https://tva4.sinaimg.cn/large/9fc55f55gy1gdnbrr7sjyj20se0o70u3.jpg)

## TravisCI创建账户

打开[TravisCI](https://www.travis-ci.org/)网站，使用Github账户直接登录,登录后界面如下,勾选个人博客项目即可。如下图

![图片](https://tva4.sinaimg.cn/large/9fc55f55gy1gdnbvnnhjsj20vn0l2q45.jpg)

## 生成并配置Access Token

在GitHub生成Travis CI 的token，生成之后一定要保存好，因为只会出现一次，丢失了就只能再重新生成了。

![图片](https://tvax1.sinaimg.cn/large/9fc55f55gy1gdnbxm94f7j20t50om3zv.jpg)

##  在Travis配置token

把刚才生成的token值配置到Travis环境变量中。如下图

![图片](https://tva3.sinaimg.cn/large/9fc55f55gy1gdnc1nxtkuj217n0pigmv.jpg)

> 如果在码云中也想把静态文件部署，可以把码云的用户名和密码配置到Travis环境变量中。
>
> gitee的令牌暂时不支持像github的token那样有推送代码的权限，gitee的推送有两种方式：
>
> 1、通过https协议使用用户名、密码推送 `https://用户名:密码@gitee.com/用户/仓库名.git`
>
> 2、通过ssh协议，使用私钥文件免密推送 `git@gitee.com/用户/仓库名.git`

## 创建.travis.yml配置文件

在博客源码中创建.travis.yml配置文件，这个文件名一定是`.travis.yml`,不能是其他文件名。下面是我使用的配置文件

```yml
language: node_js #指定使用的语言

node_js: stable  #采用nodejs的最新版本

# 指定缓存模块，可选。缓存可加快编译速度。
cache:
  directories:
    - node_modules

before_install:
    - export TZ='Asia/Shanghai' # 更改时区    

# Start: Build Lifecycle
install:
  - npm install

# 执行清缓存，生成网页操作
script:
  - hexo clean
  - hexo generate

# 指定博客分支
branches:
  only:
    - blog-source #触发持续集成的分支

# 指定博客的仓库地址
env:
 global:
    - GH_REF: git@github.com:youname/youname.git
    - GE_REF: git@gitee.com:youname/youname.git

# 设置git提交名，邮箱；替换真实token到_config.yml文件，最后depoy部署
after_script:
  - cd ./public
  - git init
  - git config user.name "youname"
  - git config user.email "youname@qq.com"
  - git add .
  - git commit -m "TravisCI 自动部署"
  - git push --force --quiet "https://${GITHUB_TOKEN}@${GH_REF}" master:gh-pages
  - git push --force --quiet "https://${GE_USERNAME}:${GE_PASSWORD}@${GE_REF}" master:master
```

>GITHUB_TOKEN、GE_USERNAME和GE_PASSWORD这几个参数是在Travis网页中配置的环境变量。

# 发布新博客

将博客内容推送到blog-source分支上,就会触发Travis CI 的自动构建.

![图片](https://tva2.sinaimg.cn/large/9fc55f55gy1gdnceupdulj21gj0dqdgi.jpg)

