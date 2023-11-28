---
title: Flask-11：操作Cookie
date: 2023-11-28 17:42:36
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-11：操作Cookie

## 一、前言

Cookie详解：https://www.cnblogs.com/poloyy/p/12513247.html

现在来瞧瞧如何用Flask操作Cookie，接下来就是实战栗子！！！

## 二、功能list

提供操作Cookie的3项功能

| 页面路径    | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| /set_cookie | 设置一个名为poloyy、值为https://www.cnblogs.com/poloyy的Cookie |
| /get_cookie | 在服务端获取名称为'poloyy'的Cookie，并将其值返回给客户       |
| /del_cookie | 删除名称为'poloyy'的Cookie                                   |

## 三、项目构成

程序有3个源文件构成

| 源文件                    | 描述                                 |
| ------------------------- | ------------------------------------ |
| app.py                    | Flask后端程序，提供操作Cookie的接口  |
| templates/get_cookie.html | 在服务端获取Cookie，显示Cookie的值   |
| templates/js_cookie.html  | 在客户端通过JavaScript显示Cookie的值 |

## 四、模板文件get_cookie.html

浏览器访问网站时，每次都会把Cookie发送给服务端，在服务端获取Cookie并返回给浏览器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>在服务端获取 cookie</title>
</head>
<body>
    <h2>在服务端获取 cookie: <b>{{cookie}}</b></h2>
</body>
</html>
```

## 五、模板文件js_cookie.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>在服务端设置 cookie</title>
</head>
<body>
    <h2>在服务端设置 cookie</h2>
    <h2>在客户端通过 Javascript 读取 cookie: <b id='cookie'></b></h2>
</body>
<script>
    cookie = document.getElementById('cookie');
    cookie.innerHTML = document.cookie;
</script>
</html>
```

document.cookie 是浏览器端保存的 cookie 值，在 id=cookie 中显示 Cookie 值

## 六、app.py代码

### 1、引入模块

```python
from flask import Flask, request, Response, render_template
app = Flask(__name__)
```

request 对象详解：<a href="./Flask-7：request对象.md">request</a>

request.cookies 就是获取客户端发送的 Cookie

### 2、获取Cookie

```python
@app.route("/get_cookies")
def get_cookies():
    cookie = request.cookies.get('poloyy')
    return render_template('get_cookie.html', cookie=cookie)
```

### 3、设置Cookie

```python
@app.route("/set_cookie")
def set_cookie():
    html = render_template("js_cookie.html")
    response = Response(html)
    response.set_cookie("poloyy", "https://www.cnblogs.com/poloyy")
    return response
```

### 4、删除Cookie

```python
@app.route("/del_cookie")
def del_cookie():
    html = render_template("js_cookie.html")
    response = Response(html)
    response.delete_cookie("poloyy")
    return response
```

### 5、浏览器运行结果

#### 设置cookie

{% asset_img "0.png" %}

#### 获取cookie

{% asset_img "1.png" %}

#### 删除cookie

{% asset_img "2.png" %}

#### 再次获取cookie

{% asset_img "3.png" %}