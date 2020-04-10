---
title: '原来CSS也能写出这么漂亮的登录页面,涨知识了'
date: 2020-04-10 11:55:31
tags:
  - CSS
categories: 前端
keywords: CSS3 login页面 原来CSS也能写出这么漂亮的登录页面,涨知识了
description: CSS3 login页面 原来CSS也能写出这么漂亮的登录页面,涨知识了
cover: https://imgconvert.csdnimg.cn/aHR0cHM6Ly90dmExLnNpbmFpbWcuY24vbGFyZ2UvOWZjNTVmNTVneTFnZG9qMXFsaDh0ajIwdXQwa2M3NG4uanBn?x-oss-process=image/format,png
top_img:
copyright:
---

>以前在学习网页设计的时候，没有很深入了解CSS样式，今天通过观看一个视频，发现原来CSS也能写出很美的登录页面。

先看看最终的效果吧
![图片](https://imgconvert.csdnimg.cn/aHR0cHM6Ly90dmExLnNpbmFpbWcuY24vbGFyZ2UvOWZjNTVmNTVneTFnZG9qMXFsaDh0ajIwdXQwa2M3NG4uanBn?x-oss-process=image/format,png)

### 步骤1：把登录页面的整体框架搭建出来，代码如下
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>login</title>
    <link rel="stylesheet" href="style.css"> 
</head>
<body>
    <div class="container">
        <div class="login-wrapper">
            <div class="header">Login</div>
            <div class="form-wrapper">
                <input type="text" name="username" placeholder="username" class="input-item">
                <input type="password" name="password" placeholder="password" class="input-item">
                <div id="login" class="btn">Login</div>
            </div>
            <div class="msg">
                Don't have account? <a href="">Sing up</a>
            </div>
        </div>
    </div>
</body>
</html>
```
### 步骤2：使用CSS给登录页面添加样式
```css
* {
    padding: 0;
    margin: 0;
    font-family: 'Open Sans Light';
    letter-spacing: .05em;
}
html {
    height: 100%;
}

body {
    height: 100%;
}

.container {
    height: 100%;
    background-image: linear-gradient(to right,#fbc2eb,#a6c1ee);
}

.login-wrapper {
    background-color: #fff;
    width: 250px;
    height: 500px;
    padding: 0 50px;
    position: relative;
    left: 50%;
    border-radius: 15px;
    top:50%;
    transform: translate(-50%,-50%);
}
.login-wrapper .header {
    font-size:  30px;
    font-weight: bold;
    text-align: center;
    line-height: 200px;
}

.login-wrapper .form-wrapper .input-item {
    display: block;
    width: 100%;
    margin-bottom: 20px;
    border: none;
    padding: 10px;
    border-bottom: 2px solid rgb(128,125,125);
    font-size: 15px;
    outline: none;
}

.login-wrapper .form-wrapper .input-item::placeholder {
    text-transform: uppercase;
}

.login-wrapper .form-wrapper .btn {
    text-align: center;
    padding: 5px;
    margin-top: 40px;
    background-image: linear-gradient(to right,#fbc2eb,#a6c1ee);;
    color: #fff;
}

.login-wrapper  .msg {
    text-align: center;
    line-height: 80px;
}

.login-wrapper  .msg a  {
    text-decoration-line:  none;
    color: #a6c1ee;
}
```