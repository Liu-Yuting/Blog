---
title: Flask-8：jinja模板入门
date: 2023-11-25 19:24:58
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---

# Flask-8：jinja模板入门

## 一、前言

- 之前的文章有个栗子，视图函数可以直接返回一段HTML代码，浏览器可以自动渲染；
- 但是当你的HTML非常复杂的话，也要整串写在代码里面，这显然不合理，可阅读性也非常差；
- 所以，就诞生了Jinja2这种模板引擎来解决需要返回的复杂Jinja2模板代码问题。

## 二、一个简单栗子

下面是Jinja2的模板，他对登录和未登录用户显示不同的信息

```jinja
<html>
	{% if login %}
	<p>您好，{{name}}</p>
	{% else %}
	<a href='/login'>登录</a>
	{% endif %}
</html>
```

如果用户已经登录：变量login为真、变量name为Tom，模板被渲染成如下的HTML文件：

```html
<html>
    <p>
        您好，Tom
    </p>
</html>
```

如果用户没有登录：变量login为假，模板被渲染成如下HTML文件：

```html
<html>
    <a href='/login'>登录</a>
</html>
```

## 三、Flask中使用模板

### 1、目录结构

{% asset_img "0.png" %}

一般来说templates就是存放模板的目录

### 2、jinja2模板代码

```jinja2
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h2>My name is {{ name }}, I'm {{ age }} years old.</h2>
</body>
</html>
```

### 3、flask代码

- 首先，需要import render_template；
- 然后，视图调用render_template，对模板 templates/index.html进行渲染；
- 最后，render_template包含有2个命名参数：name和age，模板引擎将模板templates/index.html中的变量进行替换。

```python
from flask import Flask, render_template
app = Flask(__name__)
@app.route('/')
def index():
    return render_template('index.html', name='tom', age=10)
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

#### 请求结果

{% asset_img "1.png" %}

## 四、分界符

jinja2模板文件混合html语法与jinja2语法，使用分界符区分HTML语法与jinja2语法。有5中常见的分界符：

- `{{ 变量 }}`，将变量放置在 `{{ 和 }}` 之间；
- `{% 语句 %}`，将语句放置在 `{% 和 %}` 之间；
- `{# 注释 #}`，将注释放置在 `{# 和 #}` 之间；
- `##` 注释， 将注释放置在 # 之后。

### 1、变量

jinja2 模板中，使用 {{ var }} 包围的标识符称为变量，模板渲染会将其替换为Python中的变量。

```jinja2
{{ 变量 }}
```

### 2、jinja2模板

```jinja2
<html>
	{{ string }}
	<ul>
		<li> {{ list[0] }}
		<li> {{ list[1] }}
		<li> {{ list[2] }}
		<li> {{ list[3] }}
	</ul>
	<ul>
		<li> {{ dict['name'] }}
		<li> {{ dict['age'] }}
	</ul>
</html>
```

包含有3种类型的变量：字符串、列表、字典，他们会被替换为同名的Python变量。

### 3、Flask代码

```python
STRING = 'www.baidu.com'
LIST = ['www', 123, (1, 2, 3), {"name": "Tom"}]
DICT = {'name': 'TOM', 'age': True}
@app.route('/2')
def index2():
    return render_template('01-jinja2_2.html', string=STRING, list=LIST, dict=DICT)
```

列表的值包含字符串、数字、元组、字典，字典的值包含字符串、布尔值

#### 请求结果

{% asset_img "2.png" %}

## 五、for语句

### 1、语法

jinja2模板中，使用 `{% 语句 %}` 包围的语法块称为语句，jinja2支持类似于Python的for循环语句：

```jinja2
{% for item in iterable %}
{% endfor %}
```

### 2、模板代码

```jinja
<h1>Members</h1>
<ul>
	{% for user in users %}
    <li>{{ user }}</li>
    {% endfor %}
    # for item in iterable
    <li>{{ user }}</li>
    # endfor
</ul>
```

### 3、Flask代码

```python
users = ['tom', 'jerry', 'mike']
@app.route('/3')
def index3():
    return render_template('jinja2_3.html', users=users, iterable=users)
```

#### 请求结果

{% asset_img "3.png" %}

## 六、if语句

### 1、语法

jinja2模板中，使用 `{% 语句 %}` 包围的语法块称为语句，jinja2支持类似于Python的if-else判断语句：

```jinja2
{% if cond %}
{% elif cond %}
{% else %}
{% endif %} 
```

### 2、模板代码

```jinja2
<html>
{% if a %}
    <p>a is True</p>
{% else %}
    <p>a is False</p>
{% endif %}

{% if b %}
    <p>b is True</p>
{% elif c %}
    <p>b is False, and c is True</p>
{% endif %}
</html>
```

### 3、Flask代码

```python
@app.route('/4')
def index4():
    a = False
    b = False
    c = True
    return render_template('01-jinja2_4.html', a=a, b=b, c=c)
```

#### 请求结果

{% asset_img "4.png" %}

## 七、tests

### 1、语法

jinja2 提供的 tests 可以用来在语句里对变量或表达式进行测试

```jinja2
{% variable is test %}
```

完整的 test 请参考 https://jinja.palletsprojects.com/en/latest/templates/#builtin-tests，部分test如下：

| test 名称 | 功能                     |
| :-------- | :----------------------- |
| defined   | 变量是否已经定义         |
| boolean   | 变量的类型是否是 boolean |
| integer   | 变量的类型是否是 integer |
| float     | 变量的类型是否是 float   |
| string    | 变量是否是 string        |
| mapping   | 变量的类型是否是字典     |
| sequence  | 变量的类型是否是序列     |
| even      | 变量是否是偶数           |
| odd       | 变量是否是奇数           |
| lower     | 变量是否是小写           |
| upper     | 变量是否是大写           |

### 2、jinja2模板

```jinja2
{% if number is odd %}
    <p> {{ number }} is odd
        {% else %}
    <p> {{ number }} is even
{% endif %}

{% if string is lower %}
    <p> {{ string }} is lower
        {% else %}
    <p> {{ string }} is upper
{% endif %}
```

### 3、jinja2模板输入

```jinja2
number = 404
string = 'HELLO'
```

### 4、渲染后

```html
<html>
  <p> 404 is even
  <p> HELLO is upper
</html>
```

## 八、过滤器

### 1、语法

jinja2 过滤器的是一个函数

```jinja2
{{ variable | filter }}
```

- 执行函数调用 filter(varialbe)，把函数返回值作为这个代码块的值;
- 暂时不举具体的栗子了，只做简单介绍，目测后面我会出详细文章讲解 jinja2;

### 2、模板

```jinja2
{{ string | upper }}
```

### 3、jinja2模板输入

```jinja2
string = 'hello'
```

### 4、渲染后

```html
<html>
HELLO
</html>
```

