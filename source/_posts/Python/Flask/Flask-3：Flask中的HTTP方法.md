---
title: Flask-3：Flask中的HTTP方法
date: 2023-11-20 22:58:55
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-3：Flask中的HTTP方法

## 一、查看app.route()源代码

{% asset_img "0.png" %}

### 1、重点

- Calls :meth:`add_url_rule`，需要关注一下这个方法；
- endpoint如果未传递endpoint参数，则路由的端点名称为视图函数的名称，如果已为注册函数，则会引发错误；
- methods参数默认值是["GET"]，所以当你不传methods参数时，只有发送GET请求才会匹配上对应的路由。

### 2、add_url_rule()源代码

{% asset_img "1.png" %}

- self：就是Flask类的实例；
- rule：就是路由规则；
- endpoint：函数名；
- methods：没有传递，那么会先通过view_func获取methods属性，如果还是没有，那么就是GET，注意这是个列表[]。

### 3、结论

默认的app.route()是仅支持GET请求的，如果是通过POST、PUT、DELETE等方法正常请求的话，需要添加methods参数。

## 二、GET请求的栗子

```python
from flask import Flask
app = Flask(__name__)
# 不指定 methods，默认就是 GET
@app.route('/')
def hello_world():
    # 返回字符串
    return '<b>Hello World</b>'
@app.route('/get', methods=["GET"])
def get_():
    # 返回字符串
    return '这是get请求'
if __name__ == '__main__':
    app.run(port=8080, debug=True)
```

### 请求结果：

{% asset_img "2.png" %}

{% asset_img "3.png" %}

## 三、POST请求的栗子

```python
@app.route('/post', methods=["post"])
def post_():
    # 返回字符串
    return {'message': '这是POST请求'}
# 返回结果是一个字典，最后的请求会得到什么响应呢
```

### 使用GET请求得到的结果

{% asset_img "4.png" %}

使用GET发送求求会报405，请求方法不允许！！！

### 使用POST请求

{% asset_img "5.png" %}

## 四、PUT、DELETE请求的栗子

```python
@app.route('/pd', methods=["PUT", "DELETE"])
def pd():
    # 返回列表
    return ["delete", "put"]
```

### 请求结果

{% asset_img "6.png" %}

{% asset_img "7.png" %}