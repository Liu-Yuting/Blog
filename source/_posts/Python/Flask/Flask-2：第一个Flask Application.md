---
title: Flask-2：第一个Flask Application
date: 2023-11-19 22:53:07
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-2：第一个Flask Application

## 一、安装Flask

Flask是一个web框架，使用它首先要安装

```shell
pip install flask
```

导入Flask模块

```python
import flask
```

## 二、最简单的栗子

### 主代码

```python
"""导入类flask.Flask"""
from flask import Flask
"""创建实例解析"""
"""
实例化创建一个Flask应用，第一个参数是Flask应用的名称。
__name__是一个标识Python模块的名字的变量：
	·如果当前模块是主模块，那么此模块名字就是__main__；
	·如果当前模块是被import的，则此模块名字为文件名。
"""
app = Flask(__name__)
"""装饰器解析"""
"""
	·定义hello_world函数，它返回一段html文本；
	·app.route("/")返回一个装饰器，装饰器来为函数hello_world绑定对应的URL（路由）；
	·当用户在浏览器访问这个URL的时候，就会出发这个函数，获取返回值。
"""
@app.route('/')
def hello_world():
    return "Hello World!"
"""主函数解析"""
"""
如果当前模块儿是主模块，则变量__name为'__main__'，此时调用run()方法启动Flask应用
"""
if __name__ == '__main__':
    app.run(port=8000)
```

### 运行报错：

```python
"""以一种访问权限不允许的方式做了一个访问套接字的尝试。"""
# 原因：
# --端口被占用
# 切换端口：app.run(port=8080)
```

### 运行输出：

{% asset_img "0.png" %}

### 浏览器访问：

{% asset_img "1.png" %}

遇到设置不生效：[https://www.cnblogs.com/poloyy/p/14993520.html ](https://www.cnblogs.com/poloyy/p/14993520.html)

