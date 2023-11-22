---
title: Flask-5：动态路由
date: 2023-11-22 17:26:45
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-5：动态路由

## 一、前言

- 前面几篇文章讲的路由路径（rule）都是固定的，就是一个路径和一个视图函数绑定，当访问这条路径时会触发响应的处理函数；
- 这样无法处理复杂的情况，比如常见的一个课程分类下有很多个课程，那么他们的path可能是`/course/class_1,/course/class_2,/course/class_3,...`仅最后的序号不同，其他部分都是相同的，如果每一条path都写一个单独的视图函数来处理，那样复用性会很差，代码量也会很多；
- 所以咱们要是用动态路由，路由中的路径是一个包含有参数的模板，这样就可以匹配多条路径。

## 二、静态路由的栗子

网站中有三个用户a，b，c，提供了3个路由访问这3个用户的信息

| 路由    | 视图函数      |
| ------- | ------------- |
| /user/a | show_user_a() |
| /user/b | show_user_b() |
| /user/c | show_user_c() |

```python
from flask import Flask
app = Flask(__name__)
@app.route('/user/a')
def show_user_a():
    return 'My name is a'
@app.route('/user/b')
def show_user_b():
    return 'My name is b'
@app.route('/user/c')
def show_user_c():
    return 'My name is c'
if __name__ == '__main__':
    app.run()
```

### 静态路由存在的问题

三个视图函数的功能逻辑是相同的，存在明显的逻辑代码重复

## 三、动态路由

Flask中动态路由是指带有参数的页面路径，大概格式：`/prefix/<参数>`，他是一个模板，可以匹配多条路径，将参数放置在符号<>之间。

### 将上面的静态路由栗子优化成动态路由

```python
@app.route('/user/<name>')
def show_user(name):
    return 'My name is {0}'.format(name)
```

- 匹配所有以/user/开头的路径；
- 视图函数show_user有一个参数name；
- 假设实际路径是/user/x，那么与/user/<name>匹配成功，并且将x存储到参数name中。

### 实际请求结果

{% asset_img "0.png" %}

## 四、转换器

在Flask中，动态路由的参数类型默认是string，但也可以指定其他类型，比如数字int等

| 类型   | 说明                       |
| ------ | -------------------------- |
| string | 默认，可以不用写           |
| int    | 整数                       |
| float  | 仅接收浮点数               |
| path   | 和string相似，但是接受斜线 |

```python
@app.route('/user/<name>')
def show_user(name):
    return 'My name is {0}'.format(name)
@app.route('/age/<int:age>')
def show_age(age):
    return 'age is %d' % age
@app.route('/price/<float:price>')
def show_price(price):
    return 'price is %f' % price
@app.route('/path/<path:name>')
def show_path(name):
    return 'path is %s' % name
```

### 上述代码定义了四条动态路由

| 动态路由               | 参数类型 | 参数  | 视图函数     |
| ---------------------- | -------- | ----- | ------------ |
| /user/<name>           | string   | name  | show_user()  |
| /age/<<int:age>>       | int      | age   | show_age()   |
| /price/<<float:price>> | float    | price | show_price() |
| /path/<<path:name>>    | path     | name  | show_path()  |

### 请求结果

如果<name>传了包含/的话，会报404

{% asset_img "1.png" %}

#### /user/<name>上面有就不截图了

#### /age/<int:age>

{% asset_img "2.png" %}

#### /price/<float:price>

传递整数（会报404）

{% asset_img "3.png" %}

传递浮点

{% asset_img "4.png" %}

#### /path/<path:name>

不包含/和字符串一样

{% asset_img "5.png" %}

包含/也可以正常请求

{% asset_img "6.png" %}

### 一个动态路由包含多个参数

```python
@app.route('/all/<path:path>/name/<string:name>/age/<int:age>/price/<float:price>')
def show_all(name, path, age, price):
    return f"path is {path}\nname is {name}\nage is {age}\nprice is {price}"
```

请求

{% asset_img "7.png" %}