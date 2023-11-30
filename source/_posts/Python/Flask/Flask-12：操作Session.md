---
title: Flask-12：操作Session
date: 2023-11-29 10:40:30
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-12：操作Session

## 一、前言

session详解：<a href="https://www.cnblogs.com/poloyy/p/12513247.html">Session</a>

这一节来瞧一瞧如何用Flask操作Session

## 二、功能list

提供操作Session的4项功能

| 页面路径 | 功能                                             |
| -------- | ------------------------------------------------ |
| /set     | 在Session中存储一个名称为'user'、值为'Tom'的变量 |
| /get     | 获取Session中名称为'user'的变量                  |
| /del     | 删除Session中名称为'user'的变量                  |
| /clear   | 清除Session中所有的变量                          |

## 三、项目构成

程序由两个源文件构成

| 源文件               | 描述                                                   |
| -------------------- | ------------------------------------------------------ |
| app.py               | Flask后端程序，提供操作Session的接口                   |
| templates/query.html | 查询Session中名称为'user'和'pwd'的变量，并返回给客户端 |

## 四、模板文件

用户的数据存储在 Session 中，服务端程序使用页面模板 query.html 展示 Session 中的数据

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>显示 session 中的变量</title>
</head>
<body>
    <h1>显示 session 中的变量</h1>
    <h2>session.get('user') = {{ user }}</h2>
    <h2>session.get('pwd') = {{ pwd }}</h2>
</body>
</html>
```

## 五、app.py代码

### 1、引入模块

```python
from flask import Flask, render_template, session
import os
app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)
```

- Flask 有个配置属性叫 SECRET_KEY
- SECRET_KEY 是一个密钥，Flask 以及相关的扩展 extension 需要进行加密时需要使用这个密钥
- 使用 Session 存储数据时，Flask 在内部需要进行加密处理，所以要配置这个 KEY
- 这边用 Python 的 os.random() 生成一个包含 24 个字符的随机字符串

### 2、设置Session

```python
@app.route("/set/")
def set_session():
    session["user"] = "poloyy"
    session["pwd"] = "password"
    return render_template('query.html', user=session.get("user"), pwd=session.get("pwd"))
```

### 3、获取Session

```python

```

### 4、删除Session

```python
@app.route("/del")
def del_session():
    session.pop("user")
    return render_template('query.html', user=session.get("user"), pwd=session.get("pwd"))
```

### 5、清空Session全部变量

```python
@app.route("/clear")
def clear_session():
    session.clear()
    return render_template('query.html', user=session.get("user"), pwd=session.get("pwd"))
```

### 6、浏览器运行结果

#### 设置session

{% asset_img "0.png" %}

#### 获取session

{% asset_img "1.png" %}

#### 删除session

{% asset_img "2.png" %}

#### 清空session

{% asset_img "3.png" %}

