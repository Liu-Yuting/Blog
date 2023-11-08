---
title: FastAPI-2：快速入门
date: 2023-11-06 16:12:24
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-2：快速入门

## 安装

```shell
pip install fastapi
# 将来需要将应用程序部署到生产环境可以安装 uvicorn 作为服务器
pip install uvicorn
```

## 最简单的代码栗子

```python
from fastapi import FastAPI
app = FastAPI()
@app.get('/')
async def first():
    return {'message': "Hello World"}
```

## 运行 `uvicorn` 命令，启动服务器

进入 `.py` 文件所属目录的命令行

```python
uvicorn main:app --reload
```

- **main** ：main.py文件（一个Python[模块]）；
- **app** ：在 main.py 文件中通过 app = FastAPI() 创建的对象；
- **--reload** ：让服务器在更新代码后自动重新启动，仅在开发时使用该选项。

### 服务启动示例

{% asset_img FastAPI-2：快速入门-1.png %}

### 浏览器访问

{% asset_img FastAPI-2：快速入门-2.png %}

### 查看交互式文档

{% asset_img FastAPI-2：快速入门-3.png %}

### 查看可选的API文档

{% asset_img FastAPI-2：快速入门-4.png %}

## OpenAPI

FastAPI使用API的OpenAPI标准为所有API生成schema

### schema

- 是对事务的一种定义或描述；
- 它并非具体的实现代码，只是抽象描述；

### API Schema

- OpenAPI 是一种规定如何定义API Schema的规范；
- 定义的OpenAPI Schema将包括API路径，以及它们肯能使用的参数等等；
- 比如：这个API的作用是什么，需要必传哪些参数，请求方法是什么。

### Data Schema

- 指的是某些数据比如JSON的结构；
- 它可以表示JSON的属性及其具有的数据类型；
- 比如：某个属性的数据类型是什么，有没有默认值，是不是必填，作用是什么。

### JSON Schema

- OpenAPI会为API定义API Schema，一般会包括API发送和接收的数据的定义，比如：发送的数据的类型，是否必填；
- 这些定义会以JSON数据格式展示出来，所以都会称为JSON Schema。

### 查看 openapi.json

原始的OpenAPI Schema，其实它只是一个自动生成的包含了所有API描述的JSON数据结构。

{% asset_img FastAPI-2：快速入门-5.png %}

## 拆分代码详情

```python
### 第一步
from fastapi import FastAPI
# 1、FastAPI 是一个为API提供了所有功能的Python类，必写就对了；
# 2、FastAPI 是直接从 Starlette 继承的类，可以通过FastAPI使用所有的Starlette的功能。

### 第二步
app = FastAPI()
# 1、app就是FastAPI类的一个实例对象啦；
# 2、重点：app 将是创建所有API的主要交互对象；
# 3、要点：uvicorn 执行命令时也会用到app。
# 将app变量名换一下：
my_app = FastAPI()
# 那么运行时也需要换
uvicorn main:my_app --reload

### 第三步
# 创建一个路径操作
# 路径
# 1、指的是URL中从第一个 / 起的后半部分，即常说的path
# 2、比如：https://example.com/items/foo 的路径就是 /items/foo
# 3、路径也称为：端点路由
# 操作：就是HTTP请求方式
    # 1、POST
    # 2、GET
    # 3、PUT
    # 4、DELETE
    # 5、OPTIONS
    # 6、HEAD
    # 7、PATCH
    # 8、TRACE
# 在 HTTP 协议中，可以使用以上的其中一种（或多种）与每个路径进行通信
# 遵守RESTFul风格的话
# 通常使用：
    # 1、POST：新建数据
    # 2、GET：获取数据
    # 3、PUT：更新数据
    # 4、DELETE：删除数据
# 定义一个路径操作装饰器
@app.get('/')
# 有两点含义
    # 1、请求路径为'/'
    # 2、使用 get 请求
# 其它请求方法的装饰器
    # 1、@app.post()
    # 2、@app.put()
    # 3、@app.delete()
    # 4、@app.potions()
    # 5、@app.head()
    # 6、@app.patch()
    # 7、@app.trace()
    
### 第四步
async def first():
# 1、这就是一个普通的Python函数；
# 2、每当FastAPI接收一个使用 GET 方法访问路径为 / 的请求时这个函数会被调用；
# 3、在这个栗子中，它是一个 async 函数（异步处理函数）。
# 可以不加 async
def first():

### 第五步
return {'message': 'Hello World'}
# 1、可以返回一个 dict、list，也可是 str、int单个值；
# 2、还可以返回 Pydantic 模型；
# 3、还可以是其他会自动转换为JSON的对象和模型（包括ORM对象等）
```



## FastAPI入门总结

编写一个最简单的FastAPI应用程序五部曲

1. 导入FastAPI
2. 创建一个app实例
3. 编写一个路径操作装饰器，如 `@app.get('/')`
4. 编写一个路径操作函数，如 `def first():`
5. 运行开发服务器，如 `uvicron main:app --reload`

