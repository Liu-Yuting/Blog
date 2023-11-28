---
title: Flask-10：标准类视图
date: 2023-11-27 17:39:44
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-10：标准类视图

## 一、前言

- 前面文章讲解Flask路由的时候，都是将URL路径和一个视图函数关联
- 当Flask框架接收到请求后，会根据请求URL，调用响应的视图函数进行处理
- Flask不仅提供了视图函数来处理请求，还提供了视图类；可以将URL路径和一个视图类关联

## 二、标准视图函数

- 将 URL 路径和一个函数关联，这个函数又被称为视图函数，Flask 框架会根据请求的 URL 调用相应的视图函数进行处理
- 当访问 127.0.0.1:5000/ 时，index() 函数就会处理该请求，并返回 hello world 字符串

```python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def index():
    return 'hello world'
app.run(debug = True)
```

## 三、标准视图类

Flask.views.View 是 Flask 的标准视图类，用户定义的视图类需要继承于  Flask.views.View 。使用视图类的步骤如下：

1. 用户定义一个视图类，继承于 Flask.views.View；
2. 在视图类中定义方法 dispatch_request ，处理请求、返回 HTML 文本给客户端；
3. 使用 app.add_url_rule (rule, view_func) 将 URL 路径和视图类绑定

### 最简单的栗子

```py
from flask import Flask, views
from flask.typing import ResponseReturnValue
app = Flask(__name__)
# 自定义视图类，继承 views.View
class view_test(views.View):
    # 返回一个字符串给客户端
    def dispatch_request(self) -> ResponseReturnValue:
        return "hello world"
# 将路由规则 / 和视图类 view_test 进行绑定
app.add_url_rule(rule="/", view_func=view_test.as_view("view"))
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

### 重点as_view

- view_test.as_view("view") 代表创建了一个名称为 view 的视图函数
- app.add_url_rule 实际上是将路由规则和视图函数（由视图类的 as_view 方法转换而来）绑定

### 访问效果

{% asset_img "0.png" %}

### as_view函数

视图类本质是视图函数，函数View.as_view()会返回一个视图函数

### 简化版

为了更清晰理解 as_view 函数的功能，自行实现一个简化版本的 as_view 函数

```python
# 将路由规则 / 和视图类 view_test 进行绑定
app.add_url_rule(rule="/view1", view_func=view_test.as_view("view"))
class view_test2(views.View):
    def dispatch_request(self) -> ResponseReturnValue:
        return {"msg": "success", "code": 0}
    @staticmethod
    def as_view(name, **kwargs):
        view = view_test2()
        return view.dispatch_request
# 将路由规则 / 和视图类 view_test 进行绑定
app.add_url_rule(rule="/view2", view_func=view_test2.as_view("view"))
```

1. 定义了一个静态方法 as_view，它首先创建一个实例 view
2. 然后返回实例 view 的 dispatch_request 方法
3. 即 view_func 指向了实例 view 的方法 dispatch_request
4. 当访问页面路径 /view2/ 时，最终会调用 index.dispatch_request ()

## 四、继承

使用类视图的好处是支持继承，可以把一些共性的东西放在父类中，其他子类可以继承

### 1、父类BaseView

```python
class BaseView(views.View):
    # 如果子类忘记定义 get_template 就会报错
    def get_template(self):
        raise NotImplementedError()
    # 如果子类忘记定义 get_data 就会报错
    def get_data(self):
        raise NotImplementedError()
    def dispatch_request(self):
        # 获取模板需要的数据
        data = self.get_data()
        # 获取模板文件路径
        template = self.get_template()
        # 渲染模板文件
        return render_template(template, **data)
```

### 2、子类UserView

```python
class UserView(BaseView):
    def get_template(self):
        return "user.html"
    def get_data(self):
        return {
            'name': 'Tom',
            'gender': 'male',
        }
```

### 3、app.py 应用主入口

```python
app.add_url_rule('/user/', view_func=UserView.as_view('UserView'))
```

### 4、user.html代码

```html
<html>
<body>
<h2>name = {{ name }}</h2>
<h2>gender = {{ gender }}</h2>
</body>
</html>
```

## 五、使用装饰器

在视图函数、视图类中使用装饰器还是一大杀器

### 1、检查登录功能

不使用装饰器前的代码

```py
def check_login():
    if 用户已经登录：
        return True
    else:        
        return False
@app.route('/page1', page1)
def page1():
    if not check_login():
        return '请先登录'
    执行 page1 的功能
@app.route('/page2', page2)
def page2():
    if not check_login():
        return '请先登录'
    执行 page2 的功能
```

- 在处理 /page1 和 /page2 时需要检查登录，在函数 page1 () 和 page2 () 的头部调用 check_login 函数
- 这种方法虽然实现了功能，但不够简洁

### 2、检查登录的装饰器

使用装饰器实现登录的功能，定义检查登录的装饰器 check_login

```py
from flask import request
from functools import wraps
def check_login(original_function):
    @wraps(original_function)
    def decorated_function(*args, **kwargs):
        user = request.args.get("user")
        if user and user == "zhangsan":
            return original_function(*args, **kwargs)
        else:
            return "请登录"
    return decorated_function()
```

- 装饰器 check_login 本质是一个函数
- 它的输入是一个函数 original_function
- 它的输出也是一个函数 decorated_function
- original_function 是原先的处理 URL 的视图函数,它不包含检查登录的功能逻辑，就是到时候需要添加装饰器的函数
- decorated_function 是在 original_function 的基础上进行功能扩充的函数（这就是装饰器的功能），它首先检查是否已经登录，如果已经登录则调用 original_function，如果没有登录则返回错误
- 使用 functools.wraps (original_function) 保留原始函数 original_function 的属性

### 3、在视图函数中使用装饰器

```python
@app.route("/page1")
@check_login
def page1():
    return "page1"
@app.route("/page2")
@check_login
def page2():
    return "page2"
```

- page1、page2 两个视图函数更关注请求处理，而检查登录的功能交给装饰器去负责
- 这样，检查登录的功能与 page1 和 page2 本身的功能是分离的

### 4、在视图类中使用装饰器

```python
def check_login(original_function):
    @wraps(original_function)
    def decorated_function(*args, **kwargs):
        user = request.args.get("user")
        if user and user == 'zhangsan':
            return original_function(*args, **kwargs)
        else:
            return '请先登录'
    return decorated_function
class Page1(views.View):
    decorators = [check_login]
    def dispatch_request(self):
        return 'Page1'
class Page2(views.View):
    decorators = [check_login]
    def dispatch_request(self):
        return 'Page2'
app.add_url_rule(rule='/page1', view_func=Page1.as_view('Page1'))
app.add_url_rule(rule='/page2', view_func=Page2.as_view('Page2'))
```

decorators = [check_login] 设定视图类的装饰器