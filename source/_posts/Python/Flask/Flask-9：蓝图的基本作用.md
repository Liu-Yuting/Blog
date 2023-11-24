---
title: Flask-9：蓝图的基本作用
date: 2023-11-26 19:27:18
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-9：蓝图的基本作用

## 一、前言

- 在前面的例子中，所有的页面处理逻辑都是放在同一个文件中，随着业务代码的增加，将所有代码都放在单个程序文件中是非常不合理的；
- 不仅会让阅读代码变的困难，而且会给后期维护带来麻烦；
- Flask中使用蓝图，提供了模块化管理程序路由的功能，使程序结构更加清晰。

## 二、蓝图简介

- 随着Flask程序越来越复杂，需要对程序进行模块化的处理；
- 蓝图（Blueprint）是Flask程序的模块化处理机制；
- 它是一个存储视图方法的集合；
- Flask程序通过Blueprint来组织URL以及处理请求。

### Blueprint具有以下属性

- 一个项目可以具有多个Blueprint；
- Blueprint可以单独拥有自己的模板、静态文件的目录；
- 在应用初始化时，注册需要使用的Blueprint。

## 三、基本用法

### 1、功能概述

假设包含以下4个页面：

| 页面            | 功能           | 处理函数      |
| --------------- | -------------- | ------------- |
| /news/society/  | 社会新闻模块   | society_nows  |
| /news/tech/     | IT新闻模块     | tech_nows     |
| /products/car/  | 汽车产品模块   | car_products  |
| /products/baby/ | 婴幼儿产品模块 | baby_products |

- 前两个都是/news前缀，可以组成一个蓝图news。
- 后两个都是/products前缀，可以组成一个蓝图products。
- 相当于四个视图函数，两个蓝图。

程序中包含4个视图函数，根据页面路径，Flask将请求转发给对应的视图函数，从浏览器发送过来的请求处理过程如下：

{% asset_img "0.png" %}

### 2、使用蓝图后，路由匹配流程

1. 浏览器访问路径 /products/car ；
2. Flask框架在蓝图 news 和蓝图 products 中查找匹配该页面路径的路由；
3. 发现在蓝图 products 中，存在和路径 /products/car 匹配的视图函数 car_products ；
4. 最后将请求转发给函数 car_products 处理。

## 三、实战小栗子

### 1、目录结构

栗子程序包含2个蓝图，由3个文件构成：

- app.py，程序的主文件；
- news.py，实现蓝图news；
- products.py，实现蓝图products。

{% asset_img "1.png" %}

### 2、app.py代码

```python
# 导入 Flask 和 蓝图 Blueprint
from flask import Flask, Blueprint
# 导入蓝图类
from seventhFlask import news
from seventhFlask import products
app = Flask(__name__)
# 注册蓝图
app.register_blueprint(news.blueprint)
app.register_blueprint(products.blueprint)
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

### 3、news.py代码

```python
# 导入蓝图
from flask import Blueprint
"""
实例化蓝图对象
第一个参数：蓝图名称
第二个参数：导入蓝图的名称
第三个参数：蓝图前缀，该蓝图下的路由规则前缀都需要加上这个
"""
blueprint = Blueprint('news', __name__, url_prefix="/news")
# 用蓝图注册路由
@blueprint.route("/society/")
def society_news():
    return "社会新闻板块"
@blueprint.route("/tech/")
def tech_news():
    return "新闻板块"
```

### 4、products.py代码

```python
from flask import Blueprint
blueprint = Blueprint("products", __name__, url_prefix="/product")
@blueprint.route("/car")
def car_products():
    return "汽车产品版块"
@blueprint.route("/baby")
def baby_products():
    return "婴儿产品版块"
```

### 5、请求结果

{% asset_img "2.png" %}

## 四、更具扩展性的架构

### 概述

随着业务代码的增加，需要为Flask程序提供一个具备扩展性的架构，根据Flask程序的扩展性分为如下三种类型：

#### 1、所有的页面逻辑放在同一个文件中

- 在这种架构中，程序完全不具备扩展性；
- 在初学Flask时，使用的栗子都是这种类型。

#### 2、使用一个独立的Python文件实现蓝图

- 在这种架构中，程序具备一定的扩展性
  - 程序由主程序和多个蓝图构成；
  - 每个蓝图对应一个Python文件；
  - 所有的蓝图共享相同的模板文件目录；
  - 所有的蓝图共享相同的静态文件目录。
- 上面的栗子就是采用这种架构
- 程序包含两个蓝图：news和products，由3个文件构成：app.py、news.py、products.py，其中news.py实现新闻模块，products.py实现产品模块。

#### 3、使用一个独立的目录实现蓝图

这种架构中，程序的扩展性最好：

- 程序由主程序和多个蓝图构成；
- 每个蓝图对应一个独立的目录，存储与这个蓝图相关的文件；
- 每个蓝图有一个独立的模板文件目录；
- 每个蓝图有一个独立的静态文件目录。

### 1、模板文件寻找规律

每个蓝图可以拥有独立的模板文件目录，模板文件寻找规律如下：

- 如果项目中的templates文件夹存在相应的模板文件，则使用templates文件夹下的模板文件；
- 如果项目中的templates文件夹中没有相应的模板文件，则使用定义的蓝图的时候指定的templates文件夹下的模板文件；
- 项目中的templates文件夹优先级大于指定的templates文件夹；

### 2、静态文件寻找规律

每个蓝图可以独立的静态文件目录，静态文件寻找规则如下：

- 如果项目中的 static 文件夹中存在相应的静态文件，则使用 static 文件夹下的静态文件
- 如果项目中的 static 文件夹中没有相应的静态文件，则使用定义蓝图的时候指定的 static 文件夹下的静态文件
- 项目中的 templates 文件夹优先级大于指定的 templates 文件夹

## 五、究极扩展性的栗子

### 1、目录结构

{% asset_img "3.png" %}

### 2、目录功能描述

| 路径               | 功能描述                       |
| :----------------- | :----------------------------- |
| templates          | 项目默认的模板文件夹           |
| static             | 项目默认的静态文件夹           |
| news               | 蓝图 news 的相关文件           |
| news/templates     | 蓝图 news 的私有模板文件夹     |
| news/static        | 蓝图 news 的私有静态文件夹     |
| products           | 蓝图 products 的相关文件       |
| products/templates | 蓝图 products 的私有模板文件夹 |
| products/static    | 蓝图 products 的私有静态文件夹 |

### 3、文件功能描述

| 路径                           | 功能描述                         |
| :----------------------------- | :------------------------------- |
| `app.py`                       | 主程序                           |
| `news/__init.py__`             | 蓝图 news 的实现                 |
| `news/templates/society.html`  | 属于蓝图 news 的一个模板文件     |
| `news/static/news.css`         | 属于蓝图 news 的一个静态文件     |
| `products/__init.py__`         | 蓝图 products 的实现             |
| `products/static/products.css` | 属于蓝图 products 的一个静态文件 |
| `products/templates/car.html`  | 属于蓝图 products 的一个模板文件 |

### 4、app的代码

```python
from flask import Flask
from eighthFlask import news, products
app = Flask(__name__)
app.register_blueprint(news)
app.register_blueprint(products)
app.run(host='0.0.0.0', port=8888, debug=True)
```

### 5、`news/__init__.py`代码

```py
# 导入蓝图
from flask import Blueprint, render_template
'''
实例化蓝图对象：
第一个参数：蓝图名称；
第二个参数：导入蓝图的名称；
第三个参数：蓝图的前缀，该蓝图下的路由规则前缀都需要加上这个。
'''
blueprint = Blueprint('news', __name__, url_prefix='/news', template_folder='templates', static_folder='static')
# 用蓝图注册路由
@blueprint.route('/society/')
def society_news():
    return render_template('society.html')
@blueprint.route('/tech/')
def tech_news():
    return 'IT 新闻模块'
```

- 蓝图中页面的 URL 前缀为 /news；
- 蓝图的模板目录为 templates，绝对路径为 ‘项目目录 /news/templates’；
- 蓝图的静态文件目录为 static，绝对路径为 ‘项目目录 /news/static’
- 调用 render_template (‘society.html’) 渲染模板文件 society.html，根据模板文件的查找规则，最终在 ‘项目目录 /news/templates’ 目录下找到模板文件

###  6、news/templates/society.html代码

```html
<link rel="stylesheet" href="{{ url_for('news.static',filename='news.css')}}">
<h1>社会新闻</h1>
```

在模板文件中引用了静态文件 news.css。`{{url_for ('news.static',filename=‘news.css’) }}` 的输出为 news/static/news.css，其中 news.static 表示蓝图 news 的 static 目录

### 7、news/static/news.css代码

```css
h1 {
    color: red;
}
```

### 8、`products/__init__.py`代码

```python
from flask import Blueprint
blueprint = Blueprint('products', __name__, url_prefix='/products')
@blueprint.route("/car")
def car_products():
    return "汽车产品版块"
@blueprint.route("/baby")
def baby_products():
    return "婴儿产品版块"
```

### 9、访问结果

{% asset_img "4.png" %}

## 六、验证目录优先级

在根目录下的 templates 目录下也添加一个 society.html 文件，在根目录下的 static 目录下添加一个 project.css

### 1、html代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HelloWord</title>
</head>
<body>
    <link rel="stylesheet" href="{{ url_for('static', filename='news.css')}}">
    <h1>社会新闻！！！！！</h1>
</body>
</html>
```

### 2、css代码

```css
h1 {
    color: pink;
}
```

### 3、预期结果

- 根据 templates、static 的查找规则，会优先查找项目根目录的 templates、static 目录下是否有对应的模板文件、静态文件
- 这里 society.html 同时出现在根目录的 templates 和蓝图目录的 templates，应该优先返回根目录的 templates 下的 society.html

### 4、访问结果

{% asset_img "5.png" %}

符合预期

## 七、Blueprint源码解析

### 类初始化`__init__`方法参数列表

{% asset_img "6.png" %}

- name：蓝图名称，将会被添加到每个 endpoint 
- import_name：蓝图包的名称，通常是 `__name__`，有助于找到 root_path 蓝图
- static_folder：包含静态文件的文件夹，由蓝图的静态路由提供服务，路径以蓝图文件为根路径开始找
- static_url_path：提供静态文件的 url，默认就是 static_folder，如果蓝图没有 url_prefix，应用程序的静态路由将优先，并且蓝图的静态文件将无法访问
- template_folder：包含模板文件的文件夹，路径以蓝图文件为根路径开始找
- url_prefix：会作为蓝图所有路由的前缀路径
- subdomain：蓝图路由将匹配的子域
- url_defaults：蓝图路由的默认值字典
- root_path：默认情况下，蓝图会自动设置这基于“import_name”
