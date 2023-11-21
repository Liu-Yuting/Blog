---
title: Flask-4：URL组成部分详解
date: 2023-11-21 22:02:08
author: 刘宇亭
category:
    - Python
    - Flask
tag:
    - Python
    - Flask
---
# Flask-4：URL组成部分详解

## 一、URL

- Uniform Resource Locator的简写，中文名叫统一资源定位符；
- 用于表示服务端各种资源，例如网页；
- 下面将讲解Flask中如何提取组成URL的各个部分。

## 二、URL组成详解

一个常见的URL： https://www.baidu.com/1/

由以下几部分组成： scheme://host:port/path?key=value

- scheme：代表的是访问的协议，一般为http或https。
- host：主机名、域名。
- port：端口号，http协议默认使用80端口，https协议默认使用443端口。通常情况下，使用默认值，不需要显示的写明端口号。
- path：页面路径。
- key=value：查询字符串。

## 三、在Flask中分析URL参数

- 服务端收到向客户端发送的数据后，封装形成一个请求对象，在FLask中，请求对象是一个模块变量flask.request；

- request对象包含了众多的属性；

- 假设URL等于http://localhost/query?userId=123，则与URL参数相关的属性如下：

  | 属性      | 说明                              |
  | --------- | --------------------------------- |
  | url       | http://localhost/query?userId=123 |
  | base_url  | http://localhost/query            |
  | host      | localhost                         |
  | host_url  | http://localhost/                 |
  | path      | /query                            |
  | full_path | /query?userId=123                 |

### 实际栗子

```python
from flask import Flask, request
app = Flask(__name__)
def echo(key, value):
    print('%-10s = %s' % (key, value))
@app.route('/query')
def query():
    echo('url', request.url)
    echo('base_url', request.base_url)
    echo('host', request.host)
    echo('host_url', request.host_url)
    echo('path', request.path)
    echo('full_path', request.full_path)
    print(request.args)
    print('userId = %s' % request.args['userId'])
    return 'hello'
if __name__ == '__main__':
    app.run(port=8080, debug=True)
```

浏览器访问

{% asset_img "0.png" %}

控制台输出结果

{% asset_img "1.png" %}