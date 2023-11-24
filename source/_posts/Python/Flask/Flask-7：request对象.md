---
title: Flask-7：request对象
date: 2023-11-24 19:18:52
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-7：request对象

## 一、Flask中很重要的request对象

- 浏览器访问服务端时，向服务端发送请求。
- Flask程序使用request对象描述请求信息。
- 当你想获取请求体、请求参数、请求头数据的时候，就需要靠request对象了。
- 这篇会用结果驱动源码解析的方式来讲解。

## 二、真实使用场景

浏览器访问服务端，需要将相应的数据发送给服务端，可能有如下场景：

1. 通过URL参数进行查询，浏览器需要将查询参数发送给服务端。
2. 提交表单Form进行查询，浏览器需要将表单Form中的字段发送给服务端。
3. 上传文件，浏览器需要将文件发送给服务端。

服务端收到将客户端发送的数据后，封装形成一个请求对象，在Flask中，请求对象是一个模块变量flask.request

### request包含常用的属性

| 属性    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| method  | 当前的请求方法                                               |
| form    | 表单参数及其值的字典对象                                     |
| args    | 查询字符串的字典对象                                         |
| values  | 包含所有数据的字典对象                                       |
| json    | 如果mimetype是application/json，这个参数将会解析JSON数据，如果不是则返回None |
| headers | http协议请求头                                               |
| cookies | cookie名称和值的字典对象                                     |
| files   | 与上传文件有关的数据                                         |

### request对象也能获取url相关参数吗？

获取URL请求参数的栗子

```python
from flask import Flask, request
app = Flask(__name__)
@app.route('/query')
def query():
    return {"name": request.args['name'], "age": request.args['age']}
@app.route('/query2')
def query2():
    print('args =', request.args)
    print('form =', request.form)
    return "form"
@app.route('/query3')
def query3():
    print('args =', request.args)
    print('json =', request.json)
    return "json"
@app.route('/query4')
def query4():
    return {"name": request.values['name'], "age": request.values['age']}
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8888, debug=True)
```

下面我会用url请求参数传数据：

（在Flask里面，把四种获取请求数据的属性都写一遍，然后看看最后的结果，提前踩坑）

### 请求结果

/query

{% asset_img "0.png" %}

/query2 控制台输出

{% asset_img "1.png" %}

用form属性的话得到是一个空字典哦！！！

/query3 控制台输出

{% asset_img "2.png" %}

用JSON属性的话会报错哦，所以无论如何都不要使用json获取url请求参数！！！

/query4

{% asset_img "3.png" %}

可以看到values属性也能拿到url请求参数哦！！！

### 获取表单参数的栗子

```python
@app.route('/addUser', methods=['POST'])
def check_login():
    return {"name": request.form['name'], "age": request.form['age']}
@app.route('/addUser2', methods=['POST'])
def check_login2():
    print('form =', request.form)
    print('args =', request.args)
    return "good"
@app.route('/addUser3', methods=['POST'])
def check_login3():
    print('form =', request.form)
    print('json =', request.json)
    return "good"
@app.route('/addUser4', methods=['POST'])
def check_login4():
    return {"name": request.values['name'], "age": request.values['age']}
```

下面我会用表单格式来传递数据：

/addUser

{% asset_img "4.png" %}

/addUser2 控制台输出

{% asset_img "5.png" %}

/addUser3 控制台输出

{% asset_img "6.png" %}

用JSON属性的话会报错，所以无论如何不要使用json获取form-data！！！

/addUser4

{% asset_img "7.png" %}

可以看到values属性也能拿到form表单提交的数据哦！！！

### 获取JSON数据的栗子

```python
@app.route('/addJson', methods=['POST'])
def check_login():
    return {"name": request.json['name'], "age": request.json['age']}
@app.route('/addJson2', methods=['POST'])
def check_login2():
    print('json =', request.json)
    print('args =', request.args)
    return "good"
@app.route('/addJson3', methods=['POST'])
def check_login3():
    print('json =', request.json)
    print('form =', request.form)
    return "good"
@app.route('/addJson4', methods=['POST'])
def check_login4():
    print('json =', request.json, type(request.json))
    print('values =', request.values)
    return {"name": request.json['name'], "age": request.json['age']}
```

/addJson

{% asset_img "8.png" %}

/addJson2 控制台输出

{% asset_img "9.png" %}

/addJson3 控制台输出

{% asset_img "10.png" %}

/addJson4

{% asset_img "11.png" %}

控制台输出

{% asset_img "12.png" %}

要注意：当请求体是JSON时，不能通过values来获取请求数据哦！！！

上面我们可以看到request.json拿到的就是JSON格式的请求体，并且自动转化成字典了！！！

## 三、为什么request.values能获取form、args的数据，却拿不到JSON的数据呢？

request.value源码

{% asset_img "13.png" %}

- 能看到，他本质就是获取args、form的数据，但不包含JSON数据。
- 但是这里有个重点，只有请求方法不为GET的时候，发送form表单数据才能通过request.value拿到请求数据。

### 验证

```python
@app.route('/query5', methods=["GET", "POST"])
def query5():
    print(request.form)
    print(request.args)
    print(request.values)
    return {"name": request.values['name'], "age": request.values['age']}
```

{% asset_img "14.png" %}

直接报错，找不到对应的name和key，因为request.value是空的

控制台输出

{% asset_img "15.png" %}

看源码应该知道，当非GET请求的时候传递表单数据，request.values也能获取得到request.form的数据。