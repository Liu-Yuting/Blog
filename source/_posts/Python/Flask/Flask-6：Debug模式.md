---
title: Flask-6：Debug模式
date: 2023-11-23 19:26:24
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-6：Debug模式

## 一、使用Flask开发过程中存在两个常见的问题

1. 当Flask程序出错时，没有提示错误的详细信息。
2. 修改Flask源代码后需要重启Flask程序。

这两个问题非常影响开发效率，因此Flask引入了debug模式解决以上问题。

### 错误示例

```python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def hello_world():
    1 / 0
    return '<b>hello world</b>'
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8888)
```

第五行，存在一个除以零的错误，在浏览器中访问Flask，会报错

{% asset_img "0.png" %}

浏览器中提示 Internal Server Error，表示服务端程序出现错误，但是没有给出错误的详细信息，即产生错误的文件、函数、行号等位置信息，排查错误非常不方便。

### 修改源代码后需要重启

开发Flask程序有如下3个步骤：

1. 编辑Flask源程序；
2. 在命令行中启动Flask程序；
3. 在浏览器中访问Flask程序；

每次对Flask源程序进行修改后，都需要重启Flask程序。如下，将上述函数修改为

```python
@app.route('/')
def hello_world():
    return '<b>hello world</b>'
```

程序的功能：访问页面/时，返回报错，要想正常返回文本'hello world'，需要做如下工作：

1. 切换到编辑器，编辑Flask源程序，修改函数；
2. 切换到终端，终止原先运行的Flask程序，再次运行Flask程序；
3. 切换浏览器，访问/页面。

在开发过程中，需要在编辑器、终端、浏览器3个程序之间来回切换，操作繁琐。这时，我们需要使用Debug模式来快速解决上面的问题。

## 二、Flask的Debug模式

Flask程序可以运行在Debug模式下，Debug模式提供了如下功能：

1. 当Flask程序出现错误时，在浏览器中提示错误的详细信息；
2. 修改Flask源码后，Flask程序会自动重新加载，不需要重启Flask程序，即可在浏览器中看到修改后的效果。

### 开启Debug模式

```python
# 在run中加 , debug=True 即可
app.run(host='0.0.0.0', port=8888, debug=True)
```

报错从页面查看

{% asset_img "1.png" %}

控制台中会显示

{% asset_img "2.png" %}

`Debug mode: on`，表示Flask程序已进入调试模式；修改代码后不需要重启。

{% asset_img "3.png" %}