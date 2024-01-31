---
title: FastAPI-20：Response Status Code响应状态码
date: 2024-01-27 09:35:30
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-20：Response Status Code响应状态码

## 前言

和指定响应一样，可以在任何路径操作中添加参数status_code，用于声明**响应**的HTTP状态码。

- @app.get()
- @app.post()
- @app.put()
- @app.delete()

## 最简单的栗子

```python
from fastapi import APIRouter
router = APIRouter()
@router.get("/", status_code=201)
async def cat_status_code(name: str):
    return {"name": name}
```

### 重点

- status_code 接收一个带有 HTTP 状态代码的 number
- status_code 也可以接收一个 IntEnum
- 如果是 number，可以使用 from fastapi import status ，里面都是封装好的状态码变量，直接调用即可
- 如果是 IntEnum，可以使用 from http import HTTPStatus ，是一个 int 类型的枚举类

### 正确传参的响应结果

{% asset_img "1.png" %}

## status栗子

```python
from fastapi import status
@router.get("/status/", status_code=status.HTTP_202_ACCEPTED)
async def cat_status_code2(name: str):
    return {"name": name}
```

- **更推荐用这个**，因为变量名会包含状态码和含义
- **fastapi.status**是直接来自**starlette.status**，提供的东西都是一样的

### fastapi.status

{% asset_img "2.png" %}

### 正确传参的响应结果

{% asset_img "3.png" %}

## HTTPStatus栗子

```python
from http import HTTPStatus
@router.get("/http-status/", status_code=HTTPStatus.INTERNAL_SERVER_ERROR)
async def cat_https_status(name: str):
    return {"name": name}
```

### http.HTTPStatus

{% asset_img "4.png" %}

### 正确传参的响应结果

{% asset_img "5.png" %}

## status_code作用

- 在响应中返回该状态码
- 在 OpenAPI Schema 中记录它，也会显示在 Swagger API 文档中

### 查看Swagger

{% asset_img "6.png" %}
